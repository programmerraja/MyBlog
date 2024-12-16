---
title: "Sherlock Holmes: The Case Of Debugging ECONNRESET"
date: 2024-10-31T20:52:25.2525+05:30
draft: false
tags:
  - Sherlock_holmes
  - debugging
---

When we migrated our application to Kubernetes, we encountered an unexpected issue that severely impacted user experience: frequent `ECONNREFUSED` errors. These errors were happening on our proxy server, which was supposed to forward requests to other services. Naturally, I began investigating the root cause to identify why this was happening.

### Understanding the Problem

The first step in troubleshooting any error is determining what it means and why it occurs. The `ECONNREFUSED` error generally indicates that a connection attempt was made to a server that wasn't accepting connections. This usually happens when a server is either down or not listening on the expected port. However, in our case, our servers were running just fine, so I knew something else had to be at play.

### Pinpointing the Timing of the Error

After some deeper investigation, I found an interesting pattern: the `ECONNREFUSED` errors occurred **only after deploying new code**. The timing was too consistent to ignore. The servers would respond properly before the deployment, but after a deployment, requests started getting rejected. This led me to suspect that the issue had to do with the deployment process itself.

### Kubernetes Deployment Process and the Issue

If you're familiar with Kubernetes, you'll know that it’s designed to ensure zero downtime during deployments. By default, Kubernetes first starts the new pod, checks its readiness, and only then removes the old one. This prevents downtime, but there's a subtle nuance in how this works.

When Kubernetes deploys a new version of the application, it spins up new pods. However, it doesn’t immediately route traffic to these new pods. Kubernetes waits for the readiness probe to succeed before it starts sending traffic to the new pod. In the meantime, the old pods are still serving requests.

Here’s where the issue lies: the new pod’s readiness probe may succeed, but Kubernetes still needs a few more seconds to update the **Kube Proxy’s IP table**, which keeps track of where to route traffic. During this brief window (typically 10–15 seconds), Kubernetes may terminate the old pod before the Kube Proxy has updated, causing incoming requests to hit a non-existent server. This results in the dreaded `ECONNREFUSED` errors.

 **Flowchart Visualization**
 
```
[ Start Deployment ]
         |
         v
 [ Deploy New Version ]
         |
         v
  [ Spin Up New Pods ]
         |
         v
[ Are New Pods Ready? ]
       /   \
     Yes     No
      |       |
      v       |
[ Update Kube-Proxy IP Tables ] <------+
      |                                  |
      v                                  |
[ Send SIGTERM to Old Pods ]             |
      |                                  |
      v                                  |
[ Are Old Pods Done Handling Requests? ] |
       /   \                             |
     Yes     No                          |
      |       |                          |
      v       v                          |
[ Remove Old Pods ] <-------------------+
      |
      v
[ Deployment Complete ]

```

### The Key to Fixing It: Graceful Shutdown

Now, you might be thinking: “But doesn’t Kubernetes handle this automatically with its readiness and liveness probes?” Well, it does—sort of. Kubernetes will only kill the old pod **after** the new pod has successfully passed its readiness probe. However, there’s still a small gap in time when the old pod is still processing traffic but has already been marked for termination.

To solve this, the solution is simple: we need to delay the termination process for a few extra seconds after receiving the `SIGTERM` signal. This delay gives Kubernetes enough time to update the IP table and ensure that all requests are properly routed to the new pod before the old pod is shut down.

### The Solution: Modifying the Code for Graceful Shutdown

Here’s the fix I implemented in our server code:

```js
process.on("SIGTERM", async function () {
    // Wait for 15 seconds to allow time for Kube Proxy to update the routing table
    await new Promise((res) => setTimeout(res, 15000));
    
    // Continue with the rest of the cleanup process (close DB, shut down server, etc.)
});
```

This ensures that the old pod waits for 15 seconds after receiving the `SIGTERM` signal, allowing enough time for the Kube Proxy to finish updating the IP table and properly route traffic to the new pod. Once the delay is over, the pod gracefully shuts down.

### The Result: No More `ECONNREFUSED` Errors

After making this change, the issue was resolved. No more `ECONNREFUSED` errors. The system became stable, and user experience improved significantly. This fix also clarified how Kubernetes handles traffic routing during deployments and reinforced the importance of understanding its internals.















We have a server which will proxy the request to other service but after we migrating to kuberentes we saw the `ECONNREFUSED` error happening frequently on our proxing server which affect our user expereince so i am start invesitigation why this happening there is one possibel reason `ECONNREFUSED` error will occured 
when the server is not listening on the PORT but our server are running properly

after some invesitagtion i find one intersting thing is the error accoured on the server when we deploying our code the server which are accepting the request and giving response so it is clear there is some thing wrong when we deploying 

As you have expirence with kubernetes how it handling the when we deploy. by default kubernetes will kill the old pod until new pod get up so there is no chance for downtime  but there is problem there is alaways server take few sec to server to accept the request which is technically called cold start time so it take few sec but our kubernetes will not wait untill that to fix that we need to have readineess probe you may think we dont have it but you are wrong we have probe so kubenetes will kill the our old pod when our new pod are reddy to accept 

so there is no chance for downtime but we facing issue so i started digging dive in to the issue and explored how under the hood kuberntes proxy the request and how it works and find out this articel which is really give me a insigts about how it works and i got to know what might be the reason

so i will try to explain as short as possibel when we create a deploy a new code kubernetes will start creating new container and keep the old container until the new pod redniess probe get sucessied so until that any request came it will be processed by the old pod but when the new pod is up it will update the new Ip address on kube proxy ip table such that kube proxy will know here after i need to proxy the request to new pod so the issue is here it take more 10 to 15s the ip table get updated so when new pod readness probe get sucessed it will send the `SIGTERM` single to the old and and it get killed before the ip table get updated so the request that coming mean while are not able to processed becuase the pod get killed 

So to solve this we need to wait for 10 15 sec after receviing `SIGTERM` singal and process the recevied request so far once it done close the server and exit the process 

so i made the changes in a code as below 

```js
process.on("SIGTERM", async function () {
//waited for 15 sec
await new Promise((res) => setTimeout(res, 15000));
//do the rest of thing closing db ,server and other things
});
```

After the changes it got fixed so finaly the case came to the end hope you learned something today
