---
title: "Sherlock Holmes: The Case Redis Overload During a DDoS Attack"
date: 2024-10-08T06:42:19.1919+05:30
draft: false
tags:
  - Sherlock_holmes
  - redis
  - webdev
---
Welcome to our Sherlock Holmes-inspired tech adventure series! Imagine each technical challenge as a thrilling mystery waiting to be solved. Just like Sherlock, we’ll tackle these problems with sharp attention to detail. Grab a cup of coffee, and let’s dive in!

##  The Case Begins: DDoS and Brute Force Attacks

It was a fine day until disaster struck. We received alerts about DDoS and brute force attacks from random bot IPs. Our team sprang into action to mitigate the attacks. But just when we thought we had things under control, another alarming message popped up: our Redis database was at **80% capacity**! This was shocking, especially since our Redis DB usually stays under 20MB.

###  Investigation Phase: The Redis Mystery

Before diving into the Redis issue, we focused on stopping the DDoS and brute force attacks. We implemented rate limiting for specific endpoints using Cloudflare. 

Now, let’s explore what we store in Redis. We use it for user session management in our Node.js application, utilizing Passport.js. Here’s a snapshot of what a session looks like:

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
  "passport": { // Only for logged-in users
    "user": // Actual user data here
  }
}
```

With that context, we started our investigation. I queried all data in Redis to check the validity of the sessions. Shockingly, only **10%** of the sessions contained valid user data! The rest were useless and not required.

###  The Code Unveiled: Why So Many Invalid Sessions?

I needed to find out how these sessions were being populated. After some digging, I discovered that by default, **express-session** creates a session for every request that doesn’t have a cookie attached. This means that during the DDoS attack, each request was generating a new session, which was stored in Redis.

To prevent this, we needed to set the option `saveUninitialized: false`. Here’s what that does:

```javascript
saveUninitialized: false // Prevents unmodified sessions from being saved
```

###  Fixing the Issue

After making the code change, I also wrote a script to remove the invalid sessions from Redis. You might think that resolved the issue, but the Redis storage continued to increase rapidly. 

### The Investigation Continues

You might think we had resolved the issue after our initial code changes. However, as we continued to monitor the Redis database, we noticed that the storage size kept increasing at an alarming rate. Clearly, something was still amiss in our code.

After digging deeper for another day, I discovered we were using the **flash** package to store temporary messages . This package is useful for sharing messages between server requests using sessions, but it can inadvertently lead to session bloat if not handled carefully.

Here’s how we were retrieving messages:

```javascript
const { msg } = req.flash();
```

When I looked into the `req.flash()` implementation, I found that it assigns an empty object to `req.session.flash` if it doesn’t already exist:

```javascript
var msgs = this.session.flash = this.session.flash || {};
```

###  The Problem Unveiled

The issue here was that every time we modified `req.session`, it triggered **express-session** to store the current session in Redis. This meant that when unauthenticated users were hitting our API—especially during the DDoS attacks—we were creating new sessions unnecessarily. Each request was leading to a new session being stored in our database, further exacerbating the storage problem.
	
###  Path to Resolution

To mitigate this, we needed to ensure that we weren't unintentionally creating sessions for unauthenticated users. I worked on modifying the logic around message retrieval and session handling to prevent this session bloat. 















this going to be a long detective case so grap a cup coffe to move further

It was fine day until this happens where we received DDOS and brute force attack from some random bot IP's immdentitly we jumped in to that and we started working on stoping that mean while we get another scray alert from redis where it said redis DB is 80%  Fill which make all us to shock becuase most of the time our redis DB will not have go more then 20MB and we have 1GB of storage but we getting alert of 80% full first we want to stop the DDOS and brute force attack to invistigate the redis issue so we added rate limit for the  specific endpoint in cloudfare and then  i started looking on the redis issue

Before we dive let me tell what we store on redis we used to store the user session on the redis for authtencation where we used passport js for that if your are nodejs developer you may know how the session will be stored by the passport js like below

```
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
"passport": { //will have for only logged in user
"user": //actual user data will be here
},
}
```
So the valid session will have the user object in the session

so now you have some idea so we can get started our invistigation first i try to query all the data from and check for how many valid session are exists and and i find only 10% of the session is valid session other dont have user object which mean it is not required for us 

Next i started invistigation how these session are populated in redis that are not valid it does not have `passport object` i started looked on the code and after some googling i find out that  by default for all request that dont have cookie attached to that request express-session will create session to that request and by default passsport will store that in store (redis db). if we not set the option `saveUninitialized: false,` which will make sure 

```
saveUninitialized

Forces a session that is "uninitialized" to be saved to the store. A session is uninitialized when it is new but not modified. Choosing `false` is useful for implementing login sessions, reducing server storage usage, or complying with laws that require permission before setting a cookie. Choosing `false` will also help with race conditions where a client makes multiple parallel requests without a session.

```

so you may find out why the redis get 80% full becuase of the DDOS and brute force attack where each request does not have cookie so express session  created session for all request and passport stored that in our Redis db
so i first  changed the code and pushed and write a script that will remove the invaild session from redis db 

so you may think issue has been resolved even though after did the code change we continously monitor the redis db storage size it keep increasing but fast as before so still some thing wrong in our code so started looking again for  a day and find out that we have using flash pkg 

which will use session for storing any msg and it will be usefull if we want to share msg between two server in session

So in that to retrive the msg we have passed we have written a code like below

```js
const {msg} = req.flash()
```

so i checked inside code of req.flash where they doing in where they assing the empty object to `req.session.flash` if it `req.session.flash` is don't have any value as it below

```js
var msgs = this.session.flash = this.session.flash || {};
```

So the problem was we altering the this.session which will trigger the express-session to store the current session in db.  so when ever unauth user hitting or doing DDOS on the API we are storing creating new session and storing on our db 

that what make our DB to fill fast so to fix that once we read the msg i destroyed the session as  below if the session is not valid 

```js
req.session.destroy();
```

finally we rolled out the changes and it get fixed

