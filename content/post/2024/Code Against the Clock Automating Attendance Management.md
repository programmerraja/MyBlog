+++
title = 'Code Against the Clock: Automating Attendance Management'
date = 2024-08-31T12:13:53.5353+05:30
draft = true
tags =[]
+++ 

Welcome back to **"Code Against the Clock!"** – the blog series where I transform mundane tasks into streamlined, time-saving marvels. Today, I'm thrilled to share a project where I turned a repetitive, manual chore into an automated powerhouse. Ready to see how you can save time and add a touch of excitement to your workflow? Let’s dive in!

## The Backstory

As many of you know, I work as a full-stack developer at a startup. We use Keka for managing employee attendance, which requires manually clocking in and out each day when entering and leaving the office. The issue? Sometimes, I forget to clock in or out, which results in my attendance being marked as not present. This means I have to raise a ticket in Keka to correct it – a tedious task that I wanted to automate.

## The Problem Breakdown

Initially, I looked for an API provided by Keka for this purpose, but unfortunately, they don't offer one. No problem! As a developer, I took on the challenge of solving this myself. I started by analyzing Keka’s website to understand how it works. Using the network tab in my browser’s developer tools, I identified the endpoint triggered when clocking in and out.

I wrote a simple Node.js script using `fetch` to make requests with a Bearer token in the header, and it worked. However, there was a catch: the Bearer token expired daily. I discovered that the website retained the refresh token in local storage, which was used to obtain a new Bearer token when the old one expired.

After adapting my script to handle this, I faced a few more challenges:
- **How would the script know when I entered the office?**
- **How would it determine when to clock out?**
- **How would I be notified of any errors and be able to manually clock in?**

## The Solution

To tackle these issues, I devised the following solutions:

1. **Office Entry Detection:** I configured the script with specific office hours. The script would then start attempting to clock in when these hours were reached.
2. **Clock-Out Timing:** I set a configurable duration in the script for how many hours after clocking in it should automatically clock out.
3. **Error Notification and Manual Clock-In:** I integrated Slack notifications into the script. This way, I would receive alerts for successful clock-ins and outs, as well as errors.

```
[CRON Job (Every 15 minutes)] ---> [Node.js Script]
                         \-------> [Check Time] ---> [Clock In/Out]
                         \-------> [Notify (Slack)]
```

Once everything was in place, I scheduled a cron job to run my script every 15 minutes. This setup worked flawlessly for a year, automating my attendance management efficiently.

## Transition to a Chrome Extension

After a year, I decided to enhance the solution by converting the script into a Chrome extension. This made it easier to share with colleagues. Here’s how the extension works:

1. **Setup:** After installing the extension, you’re prompted to enter your office in and out times and specify the duration after which you want to clock out.
2. **Alarm Mechanism:** The extension sets a Chrome alarm to run every 15 minutes. When the clock-in time is reached, it opens the Keka website with a query parameter (`?CLK_IN=true`).
3. **Content Script:** The extension includes a content script that parses the query parameters and triggers the clock-in or clock-out process. Once successful, it sends a success message to the background service, which records the clock-in time and schedules the clock-out accordingly.

```
[User Setup (Times & Duration)] ---> [Chrome Extension]
                                    |   \--> [Alarm Mechanism]
                                    |   \--> [Content Script]
                                    \--> [Keka Website]
```

This streamlined approach made managing my attendance even easier!

**Note:** If you’re interested in the source code, feel free to reach out to me!

## Your Turn!

Have you ever automated a task using code? Share your experiences and tips in the comments below! What tasks do you wish you could automate? Let's discuss!

Thanks for joining me on this automation journey. Don’t forget to subscribe to my blog for more tips and updates. Happy coding!