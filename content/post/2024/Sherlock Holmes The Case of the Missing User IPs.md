---
title: "Sherlock Holmes:  The Case of the Missing User IP's"
date: 2024-07-01T21:08:07.077+05:30
draft: false
tags:
  - debugging
  - ngnix
  - Kubernetes
  - Sherlock_holmes
---
Welcome to our series of infrastructure detective stories, where we unravel the mysteries lurking within our systems In this episode, we tackle a perplexing problem: our Nginx server inside Kubernetes was logging an IP address that didn't match the actual user IP. Join me as we unravel the mystery and uncover the truth behind the missing IP.

### The Mysterious IP

It all started when we noticed something unusual in our logs. The IP addresses recorded by Nginx were not matching the actual IPs of our users. Every log entry showed the same IP, regardless of the user. Clearly, something was amiss. The first clue led us to believe that a proxy was involved, obscuring the true origin of the requests.

### The Proxy Puzzle

Determined to get to the bottom of this, I decided to log the `X-Forwarded-For` header, hoping it would reveal the original IP. However, to my surprise, the header was empty. This deepened the mystery. What could be interfering with our ability to see the user IPs?

### The Gateway Revelation

Next, I examined the IP that was consistently logged. It turned out to be an address ending with ".1", which matched the default gateway of our Nginx service. This pointed the investigation towards Kubernetes. There was something about the Kubernetes configuration that was hidding the user ip.

### Kubernetes Configuration Conundrum

Digging into the Kubernetes documentation, I discovered that our nginx service's `externalTrafficPolicy` was set to `Cluster`. By default, Kubernetes balances the load among different Nginx pods, proxying requests and masking the original IP. This explained why the user IPs were not appearing in our logs.

### The Final Piece

To solve the problem, I changed the `externalTrafficPolicy` to `Local`. This configuration ensures that all requests are sent directly to the pods without being proxied by Kubernetes. While this meant giving up on load balancing, it allowed us to log the true user IPs.

With the mystery solved, our logs now accurately reflect the IP addresses of our users. This journey through the depths of Kubernetes and Nginx configurations highlights the importance of understanding the tools we use and their impact on our systems. Stay tuned for the next adventure, where we continue to uncover and solve the hidden challenges in our infrastructure.
