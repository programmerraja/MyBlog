
+++
title = 'Sherlock Holmes: The Case of the Missing User IPs'
date = 2024-07-01T21:08:07.077+05:30
draft = false
tags =['debugging','ngnix','Kubernetes','Sherlock Holmes']
+++ 

#### A Sherlock Holmes Adventure in Kubernetes

It was a typical evening when an urgent message arrived. Our Nginx ingress in Kubernetes was logging the wrong IP addresses. Instead of showing the actual user IPs, it was recording the internal IPs of the Kubernetes nodes.So, I began my investigation as fllows

---

####  Examining the Headers

Before diving deeper, I tried logging the `X-Forwarded-For` header in Nginx, hoping it might reveal the true client IPs. However, the header was empty.

####  Investigating the Kubernetes Service

Next, we looked at the Kubernetes service configuration. Here, we found an important clue: the `externalTrafficPolicy` setting.

"Look, Watson," Holmes said, pointing to the configuration. "The `externalTrafficPolicy` is set to `Cluster`. This setting makes Kubernetes proxy the requests internally, hiding the real client IP."

"So, the real IPs are lost within the internal traffic. How do we fix this, Holmes?"

---

####  Solving the Mystery

Holmes suggested a straightforward solution: change the `externalTrafficPolicy` to `Local`.

"By setting `externalTrafficPolicy` to `Local`, Watson, Kubernetes will keep the original client IP. The requests won’t be proxied internally, allowing Nginx to log the real IPs."

---

####  The Fix

We updated the Kubernetes service configuration:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  externalTrafficPolicy: Local
  ...
```

After redeploying the service, we checked the Nginx logs again. This time, the true client IPs were there, just as we expected.

"Elementary, my dear Watson," Holmes declared. "The case of the missing IPs is solved."

Stay tuned for the next adventure in our series of infrastructure detective stories. Until then, keep your logs clear and your configurations correct!