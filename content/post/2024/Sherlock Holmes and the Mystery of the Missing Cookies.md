+++
title = 'Sherlock Holmes and the Mystery of the Missing Cookies'
date = 2024-09-03T05:57:21.2121+05:30
draft = true
tags =[]
+++ 

Welcome to our Sherlock Holmes-inspired tech adventure Series! Imagine each technical challenge as a thrilling mystery waiting to be solved. Like Sherlock Holmes with his sharp eye for detail, I'll tackle the problem with wit and precision. Let's dive in and crack these cases together!

### Case File: Migration to K8

Our mission began with migrating a test application from Heroku to Kubernetes (K8). The plan seemed straightforward, but soon we hit a snag. After migrating the test application, I attempted to log in, only to be redirected back to the login page. Oddly enough, the same application worked flawlessly on Heroku. Clearly, something was amiss in the K8 migration.

#### Clues from Authentication

you’re probably familiar with how authentication works. Here’s a quick refresher:

1. **Credentials Submission:** You enter your credentials.
2. **Backend Validation:** The server validates your credentials.
3. **Cookie Handling:** If all’s good, the server sends cookies to your browser. Your browser then stores these cookies and uses them for subsequent requests.

With this in mind, I took a closer look at the login endpoint using the network tab. My first clue was that the `set-cookie` header was missing from the response, despite entering the correct credentials. This seemed suspicious. Could something specific to K8 be causing this issue.

#### Investigating the Middleware

I started by checking if there were any proxies involved, but there were none. My next move was to add logs to the login endpoint to confirm if the server was sending the `set-cookie` header. The goal was to determine if something in between was stripping out this header.

Once the logs were in place, it became clear that the server wasn’t generating the `set-cookie` header, even though the credentials were valid.

#### Delving into the Code

To get to the bottom of this, I dug into the code where we were using Node.js along with Passport and Express-session for authentication. I focused on how `set-cookie` is handled. Specifically, I examined the Passport.js and Express-session libraries. Here’s the critical snippet I found:

```js
// only send secure cookies via https
if (req.session.cookie.secure && !issecure(req, trustProxy)) {
    debug('not secured');
    return;
}
if (!touched) {
    // touch session
    req.session.touch();
    touched = true;
} 
// set cookie
setcookie(res, name, req.sessionID, secrets[0],req.session.cookie.data);
```

**Aha Moment!** Did you spot the issue? The problem became clear: The `secure` flag was set to `true` during the initialization of Express-session. This flag ensures that cookies are only sent over HTTPS connections. However, in our K8 setup, we were using HTTP internally, with Nginx handling HTTPS termination. This setup meant that by the time Nginx communicated with our test server, it was over HTTP, which led to the absence of the `set-cookie` header.

#### Solving the Case

The solution was to switch from Nginx to Ingress for handling HTTPS. Ingress seamlessly manages HTTPS, solving our issue. Once the change was made, our application began to work perfectly.

Stay tuned for our next adventure, where we continue to unravel the mysteries of the infrastructure world, one case at a time. Until then, keep your magnifying glasses handy and your curiosity alive.

**Finally, if the article was helpful, please clap 👏and follow, thank you!**
