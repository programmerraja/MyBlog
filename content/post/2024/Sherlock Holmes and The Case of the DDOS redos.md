+++
title = 'Sherlock Holmes and The Case of the Redis get fulled'
date = 2024-10-08T06:42:19.1919+05:30
draft = true
tags =[]
+++ 

Welcome to our Sherlock Holmes-inspired tech adventure Series! Imagine each technical challenge as a thrilling mystery waiting to be solved. Like Sherlock Holmes with his sharp eye for detail, I'll tackle the problem with wit and precision. Let's dive in and crack these cases together!

this going to be a long detective case so grap a cup coffe to move further

It was fine day until this happens where we received DDOS and brute force attack from some random bot IP's immdentitly we jumped in to that and we started working on stoping that mean while we get another scray alert from redis where it said redis DB is 80%  Fill which make all us to shock becuase most of the time our redis DB will not have go more then 20MB and we have 1GB of storage but we getting alert of 80% full first we want to stop the DDOS and brute force attack to invistigate the redis issue so we added rate limit for the main specific endpoint in cloudfare and then  i started looking on the redis issue

Before we dive let me tell what we store on redis we used to store the user session on the redis for authtencation where we used passport js for that if your are nodejs developer you may know how the session will be stored by the passport js like below


So the valid session will have the user object in the session


so now you have some idea so we can get started our invistigation first i try to query all the data from and check for how many valid session are exists and and i find only 10% of the session is valid session other dont have user object which mean it is not required for us 

Next i started invistigation how these session are populated in redis that are not valid i started looked on the code and after some googling and checked out the code of passport js and express-session  i found that 

by default for all request that dont have cookie attached to that request express-session will create session to that request and by default passsport will store that in store (redis db). if we not set the option `saveUninitialized: false,` which will make sure 

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

