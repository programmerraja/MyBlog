+++
title = "The Day a DDOS Attack Led to the World's Most Awkward 'Hello World'"
date = 2024-10-02T06:57:18.1818+05:30
draft = true
tags =[]
+++ 

Hey everyone, grab a coffee because I’ve got a funny (and slightly embarrassing) story to share! It’s all about how I turned our landing page and application into a personal "Hello World" greeting – but not for the world... for our own team.

It was a perfectly normal afternoon until *BAM!* Our marketing team burst into our Slack like they’d just seen a ghost: **“The landing page is down!”**

Naturally, I did what any developer does in this situation internally panicked while pretending to stay calm.

#### **Cue the investigation montage:**  

I dove into the server logs, expecting to find something dramatic, and there it was: a DDOS attack. Someone, somewhere, decided to unleash the fury of a thousand requests per second on our poor, unsuspecting landing page

The server, as you can imagine, was not happy about it and promptly crashed.

#### **Time for Action!** 

We tried the classic “Have you tried turning it off and on again?” method. Spoiler: didn’t work. The DDOS was still going strong. So, we switched gears and decided to block the incoming IP addresses. 

In my **‘urgent superhero mode,’** I pulled all the suspicious IPs from the logs and tossed them into our blocklist like they were confetti at a parade. The attack slowed down. Mission accomplished! 

Or so I thought...

### The Plot Twist 

A few minutes later, I started hearing complaints from the team:  
“Uh... I’m only seeing ‘Hello World’ on the website... what’s going on?”  
“Wait, why am I being greeted with ‘Hello World’ instead of our homepage?”

**The Moment of Realization** 

My heart dropped. I knew it instantly. *It was me.* I was the culprit! While hastily blocking IPs like a digital bouncer, I had accidentally blocked **our own internal network’s IP** too. because some one from our company visited the website during the same period.

And to add some flair to this epic mistake, I had set up a default “Hello World” message to appear whenever an IP was blocked. 

I quickly unblocked our internal IP and asked to check them it starts working 

I hope this gave you a chuckle! 😅 Stay tuned for more techy blunders and the lessons I (painfully) learn along the way.


