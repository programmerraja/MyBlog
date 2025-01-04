---
title: "Sherlock Holmes: The Case Redis Overload During a DDoS Attack"
date: 2024-10-08T06:42:19.1919+05:30
draft: false
tags:
  - Sherlock_holmes
  - redis
  - webdev
---

It was a calm day until disaster struck. We received alerts about DDoS and brute-force attacks originating from random bot IPs. Our team quickly mobilized to mitigate the attacks. Just when we thought the situation was under control, another alarming message appeared: our Redis database was at **80% capacity**! This was particularly shocking, given that our Redis DB typically stays under 20MB.

### Investigation Phase: The Redis Mystery

Before addressing the Redis issue, we focused on halting the DDoS and brute-force attacks. We implemented rate limiting for specific endpoints via Cloudflare.

Now, let’s explore what we store in Redis. We use it for user session management in our Node.js application with Passport.js. Here’s an example of what a session looks like:

```json
{
  "cookie": {
    "originalMaxAge": number,
    "expires": "date",
    "secure": true,
    "httpOnly": false,
    "domain": "domain",
    "path": "/",
    "sameSite": false
  },
  "passport": { 
    "user": // Actual user data here
  }
}
```

With that context, we began our investigation. I queried all data in Redis to check the validity of the sessions. To my surprise, only **10%** of the sessions contained valid user data. The rest were invalid, lacking the **user** key.

### The Code Unveiled: Why So Many Invalid Sessions?

I dug into how these invalid sessions were being generated and discovered that by default, **express-session** creates a session for every request that doesn’t have an attached cookie (you can refer [here](https://www.passportjs.org/concepts/authentication/sessions/)). During the DDoS attack, this resulted in a new session being generated for each request, which was then stored in Redis.

To resolve this, we set the `saveUninitialized: false` option:

```javascript
const session = require('express-session');

app.use(session({
  secret: 'keyboard cat',
  resave: false,
  saveUninitialized: false,
  cookie: { secure: true }
}));
```

### Fixing the Issue

After implementing the code change, I wrote a script to remove invalid sessions from Redis. While we hoped this would resolve the issue, the Redis storage continued to grow at an alarming rate.

### The Investigation Continues

Despite the initial fix, we noticed that the Redis database was still expanding rapidly. Something was still amiss.

I dug deeper into the codebase and discovered the **[flash](https://www.npmjs.com/package/connect-flash)** package, which we used to transfer messages between servers. Upon further inspection, I found that when accessing the messages:

```javascript
const { msg } = req.flash();
```

The package assigns an empty object to `req.session.flash` if one doesn’t already exist:

```javascript
var msgs = this.session.flash = this.session.flash || {};
```

This modification of `req.session` triggered **express-session** to store the session in Redis. The flash functionality was being used on public endpoints, which caused user sessions to remain empty and led to new sessions being inserted with each request.

### Path to Resolution

To resolve this, I modified the code to destroy the session after reading the message, as shown below:

```javascript
const { msg } = req.flash();

req.session.destroy();
```

And so, the detective journey came to an end.
