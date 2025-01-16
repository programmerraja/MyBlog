---
title: From Heroku to Kubernetes Lessons Learned in Our Migration Journey
date: 2024-10-30T20:38:12.1212+05:30
draft: true
tags:
  - devops
  - kubernetes
---

Migrating from Heroku to Kubernetes is no small feat. While Heroku provided a straightforward platform as a service (PaaS) environment that took care of many operational aspects for us, Kubernetes promised much more flexibility, scalability, and control. However, with great power comes great responsibility and a host of new challenges. In this post, I’ll walk you through the lessons we learned along the way and how we overcame some common hurdles in our journey from Heroku to Kubernetes.

### Probe

The first issue we encountered after migration was user reports of encountering Cloudflare error messages. These errors would resolve upon refreshing the page. After investigating, we determined that this issue occurred due to our deployment configuration. If you would like more information on the investigation, I’ve written a detailed blog post on it [here](https://dev.to/programmerraja/sherlock-holmes-and-the-case-of-the-cloudflare-timeout-mystery-3d61)

To address this, we implemented pod probes to ensure zero downtime. Specifically, we created both a readiness probe and a liveness probe to help manage pod health and restart the server as needed.

### Getting real user IP

After migrating to Kubernetes, we noticed that we could no longer retrieve the actual IP address of users. Instead, we were getting the pod proxy IP. To restore access to the real user IP, we had to configure the `externalTrafficPolicy` setting to `Local`. for this also i have written detail blog you can check out [here](https://dev.to/programmerraja/sherlock-holmes-the-case-of-the-missing-user-ips-5a81)


## Zombie State

An interesting and unexpected issue we faced was some of our servers entering a "zombie" state. We observed that after two days without a push or server restart, some servers would become unresponsive. Despite our best efforts, we couldn't identify the root cause (RCA). To resolve this, we set up a cron job that automatically restarts specific servers daily.


### Internal service communication 

One of the key advantages of moving to Kubernetes was the ability to avoid exposing all of our services. Some services needed to be accessible only internally. To facilitate this, we leveraged Kubernetes' internal DNS system for communication between services, which enhanced our security by ensuring that internal services were not exposed externally.

### Alerts 

Proper alerting is essential to ensure that we are notified when a pod enters a crash loop or when a pod is unexpectedly restarted. Setting up accurate and timely alerts has been crucial in maintaining the health of our system.

### Taints

We used taints to optimize node utilization. Taints allow us to bind specific pods to specific nodes, ensuring that high-resource pods run on nodes with the required resources, while low-resource pods are allocated to less powerful nodes. This approach has helped us maximize the efficiency of our Kubernetes infrastructure.

