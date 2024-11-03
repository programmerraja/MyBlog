---
title: "Caught in the Crunch: My Journey from Snacks to 200,000 Exposed Users Privacy"
date: 2024-11-03T04:45:38.3838+05:30
draft: true
tags:
  - hacking
  - reverse_engineering
---

Hey there! Welcome back! Today, I want to share a surprising story about how a simple ₹10 cashback offer from a biscuit packet led to a big security mistake. It’s a wild ride, so let’s dig in!

## The Snack Adventure

it was a weekend, and I was working on my personal project, feeling a little hungry. I decided to grab some biscuits (I’ll keep the brand a secret). As I opened the packet, I discovered a QR code promising cashback if I scanned it. Of course, my curiosity got the better of me!

I grabbed my phone, scanned the code, and found myself on a website asking me to sign up with my mobile number. After verifying my number with an OTP (a code they texted me), I was prompted to scan a 16-digit number from the packet and upload it. Then came the fun part I had to cut the biscuit into a specific shape on a digital canvas on there website. Based on my creation, I earned 53 points and got a ₹10 cashback. They even had a leaderboard where I could see my score!

![](../../Images/leaderboard.png)

## A Developer’s Curiosity Kicks In

As I basked in my biscuit victory, my inner tech nerd started asking questions. How does this all work? I decided to do a little digging.

First, I tracked the OTP process. The app sent my name and phone number to an endpoint called `/v1/auth/register`, and just like that, I got my OTP. After I entered it, they hit another endpoint, `/v1/auth/verify-otp`, and I received this response:

```json
{
  "user": {
    "role": "user",
    "isVerified": true,
    "isBlocked": false,
    "bonusPoints": 0,
    "name": "xxxxxxx",
    "mobileNumber": "xxxxxxxxx",
    "id": "670bc1615a6eac9288930248"
  },
  "tokens": {
    "access": {
      "token": "JWT token",
      "expires": "date"
    },
    "refresh": {
      "token": "JWT token",
      "expires": "time"
    }
  },
  "score": { "userScore": 155, "userRank": 6895 }
}
```

Here’s the kicker: they were using JWT tokens for authentication and MongoDB as their database. Everything seemed fine until I went to the leaderboard page.

## The Shocking Discovery

When I checked the leaderboard at the `/v1/users/leaderboard` endpoint, I couldn’t believe my eyes. The response included users’ phone numbers! Check this out:

```json
{
  "message": "ok",
  "leaderboard": [
    {
      "rank": 1,
      "userId": "mongodb ID",
      "name": "name",
      "mobileNumber": "xxxxxxxx",
      "bonusPoints": 60,
      "totalMatchPercentage": 52367,
      "score": 52427
    },
    ....other users details
  ]
}
```

Can you believe it? They were exposing phone numbers right there! This was a huge security flaw.

After discovering this, I noticed that they followed a standard structure for their API endpoints, with each one starting with `version_number/module/`, where the module indicates components like `auth` and `users`.

Curious, I decided to check another endpoint, `/v1/users`. Initially, it responded with an "unauthorized" message. However, when I tried again using my token, I was shocked to find that it returned user data in a paginated format.

 Oh my God the data included more than 200,000 users an enormous amount of sensitive information exposed!
```json
{
  "results": [
    {
      "activationCodes": [],
      "role": "user",
      "isVerified": true,
      "isBlocked": false,
      "bonusPoints": 60,
      "name": "xxxxxxxxx",
      "mobileNumber": "xxxxxxxxx",
      "city": "xxxxxxxx",
      "dob": "xxxxxx",
      "email": "xxxxxxxxxxx",
      "gender": "",
      "pincode": "",
      "platform": "xxxxxxx",
      "id": "xxxxxxx"
    },
    ...
  ],
  "page": 1,
  "limit": 10,
  "totalPages": 23315,
  "totalResults": 233145
}
```

To download all user data i just add query params as `?limit=233145` I could easily download all user data just by tweaking a query parameter.

## Wrapping It Up

After uncovering these issues, I immediately contacted the company, explaining the problems and urging them to fix it. It’s vital for companies to protect user data, especially when they’re running promotions.

So, what’s the takeaway? Always be careful with the information you share online. And businesses need to make sure they have strong security measures in place.

What do you think about this situation? Have you ever encountered similar issues? I’d love to hear your thoughts in the comments!