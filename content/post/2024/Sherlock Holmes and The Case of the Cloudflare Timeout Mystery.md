+++
title = 'Sherlock Holmes and The Case of the Cloudflare Timeout Mystery'
date = 2024-09-14T18:57:23.2323+05:30
draft = true
tags =[]
+++ 

Welcome to our Sherlock Holmes-inspired tech adventure Series! Imagine each technical challenge as a thrilling mystery waiting to be solved. Like Sherlock Holmes with his sharp eye for detail, I'll tackle the problem with wit and precision. Let's dive in and crack these cases together!

## The Case: Cloudflare Gateway Timeout Errors

We recently embarked on a journey to migrate our application to Kubernetes. The migration process went smoothly, and we successfully moved the majority of our main application. However, shortly after, our clients started experiencing issues. They reported seeing Cloudflare's `Gateway Timeout` error, though the issue seemed to resolve itself upon refreshing.

### Initial Investigation

Given that the error message indicated a timeout, it was clear that our application was not responding within the specified time limit. We started our investigation:

1. **Server Logs Review**: We reviewed the server logs but found no obvious issues. Everything seemed to be running fine, and we couldn't pinpoint the exact times when users were facing problems due to the randomness of the complaints.

2. **Correlated with Migration**: One clear observation was that these issues began occurring right after our migration to Kubernetes. This led us to believe that something related to the Kubernetes setup might be causing the problem.

### Discovery and Analysis

The breakthrough came when one of our colleagues encountered the same error. We checked the server logs again, and while they appeared normal, a deeper analysis revealed a critical insight:

- We had recently pushed code to our live server, which triggered a server restart (or, more specifically, a pod restart in Kubernetes).

- To confirm if the restart was the issue, we manually restarted the pod and observed that the error could be reproduced until the server was fully up and running.

### Root Cause: Cold Start Time

Our initial assumption was that Kubernetes would handle the transition smoothly, keeping the old server down only once the new server was up and running. While Kubernetes does handle this, we overlooked one crucial detail: **cold start time**.

**Cold start time** refers to the period required for a new container to start up, connect to the database, and be fully operational. During this time, Kubernetes might route requests to the new container before it's ready, leading to failures and timeouts.

### The Solution: Kubernetes Probes

To address the issue, we delved into Kubernetes features and discovered the concept of **Probes**. Probes are essential for managing the state of containers and ensuring that they are ready to handle traffic. Kubernetes offers three types of probes:

1. **Liveness Probe**: This probe indicates whether a container is still running. If the probe fails, Kubernetes will kill and restart the container. It helps catch issues like deadlocks and improves application availability.

2. **Readiness Probe**: This probe checks whether the application in the container is ready to accept requests. If the probe fails, Kubernetes will remove the pod from the service's endpoints, preventing traffic from being sent to it. This probe is crucial for handling the cold start issue because it ensures that requests are only sent to containers that are fully up and running.

    **Note**: If you don't set a readiness probe, Kubernetes assumes that the application is ready to handle traffic as soon as the container starts. This can lead to request failures during the container's startup period.

3. **Startup Probe**: This probe determines whether the application has started. Once the startup probe succeeds, other probes begin to function. If the probe fails, Kubernetes will restart the container.

### Implementing the Solution

To resolve our issue, we implemented the **Readiness Probe** in our Kubernetes configuration. This adjustment ensured that traffic was only directed to containers that were fully operational, thus eliminating the `Gateway Timeout` errors our clients were experiencing.

Stay tuned for our next adventure, where we continue to unravel the mysteries of the infrastructure world, one case at a time. Until then, keep your magnifying glasses handy and your curiosity alive.

**Finally, if the article was helpful, please clap 👏and follow, thank you!**















Welcome to our Sherlock Holmes-inspired tech adventure Series! Imagine each technical challenge as a thrilling mystery waiting to be solved. Like Sherlock Holmes with his sharp eye for detail, I'll tackle the problem with wit and precision. Let's dive in and crack these cases together!


We are in progress of migrating our application to kubernetes we mostly moved our most of the main application and after we moved few days later our clients starts complining that they seeing below cloudfare error gatewaytimeout but upon refreshing it works 

we started our invsitigation so based on the error message we clearly know that our application is not responding within specfic period of the time limit so cloudfare is showing that error message so we checked the server logs and it looks fine and we not able get excat time when user is facing because multiple user are complling randomly 

but the one clear thing we have is this only happening after we migrate to kuberenetes so there is some thing wrong with that or we may did something wrong but we not able to find why it's happening 

so one day after suddenly our college also see the same error and they told me and show me the error i checked server logs it looks fine  but after doing deep analysis i find that we pushed the code to our live server and our server get restarted on the same time to confirm is that causing the issue we manully restarted the server (pod in kubneretes) and checked our website it is reproduced until the server get up atlast we find out the root cause 

but what we thaugh when we migrate that kuberentes will take care that it will down the old server until new server is up but it is correct kuberentes is doing that but the issue is cold start time kubernetes we redirect the new request as soon as continer status get running but our code will few sec to get started becuase it need to connect with db and other things that amount time it taken is called cold time

so to avoid this we did some research on kubernetes and findout  that is concept Probe in kuberentes which help to resolve the problem

1. **Liveness Probe** indicates if the container is operating. If so, no action is taken. If not, the kubelet kills and restarts the container. Liveness probes are used to tell kubernetes to restart a container. If the liveness probe fails, the application will restart. This can be used to catch issues such as a deadlock and make your application more available.
2. **Readiness Probe** indicates whether the application running in the container is ready to accept requests. If so, Services matching the pod are allowed to send traffic to it. If not, the endpoints controller removes the pod from all matching Kubernetes Services.`Note:`**If you don't set the readiness probe, the kubelet assumes that the app is ready to receive traffic as soon as the container starts. If the container takes 2 minutes to start, all the requests to it will fail for those 2 minutes.**
3. **Startup Probe** indicates whether the application running in the container has started. If so, other probes start functioning. If not, the kubelet kills and restarts the container.

So solve our issue we introduced Readiness probe which wil solve our issue 