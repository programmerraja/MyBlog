---
title: From Heroku to Kubernetes Lessons Learned in Our Migration Journey
date: 2024-10-30T20:38:12.1212+05:30
draft: true
tags:
---



We start moving our servers from heroku to kubernetes

Migrating from Heroku to Kubernetes is no small feat. While Heroku provided a straightforward platform-as-a-service (PaaS) environment that took care of many operational aspects for us, Kubernetes promised much more flexibility, scalability, and control. However, with great power comes great responsibility—and a host of new challenges. In this post, I’ll walk you through the lessons we learned along the way and how we overcame some common hurdles in our journey from Heroku to Kubernetes.

Probe

The first issue we face after migration was user was start reporting that they seeing couldfare error message but upon refreshing the page it went out and by invistigating we find out this happens due to our deployment if you would like more about the invesitigation you check out this blog where i have written very details 

so we need to have probe for the pod to get a zero downtime  so we created both readness probe which will take care 


restarting the server 

this is interseting and unknown issue for us because we have noticed that some of our server get in zomibie state when we have a not push any change or restarted the server for 2 days the server will became the unresponsible state we try our best to find out the RCA but we not able to so to fix that we have written a cron that will restart the specific server on daily .


Getting real user IP

so we have some service that depends on user Ip but after migrating to kuberntes we have seen that we not able to get actual real user ip we get the pod proxy ip so get actual user ip we need to set `externalTrafficPolicy`to to local 


Internal service communication 

one advantage of moving in to kunberentes was we dont want expose all service because some service only need internally for that we use pod internal dns to communicate between two service which greatly help us our service protection no one can access from outside


alerts 
have proper alerts such that you will get notify when the pod went to crash loop or pod get restarted etc

Taints
we used taints to use effective the nodes so basically taints help us to bind the pod with node which help us to bind high resources node and low resourcece usage node which make the node use effective

