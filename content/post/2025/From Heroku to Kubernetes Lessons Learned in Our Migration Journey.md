---
title: From Heroku to Kubernetes Lessons Learned in Our Migration Journey
date: 2024-10-30T20:38:12.1212+05:30
draft: true
tags:
  - devops
  - kubernetes
---

Migrating from Heroku to Kubernetes is no small feat. While Heroku provided a straightforward platform as a service (PaaS) environment that took care of many operational aspects for us, Kubernetes promises much more flexibility, scalability, and control. However, with great power comes great responsibility and a host of new challenges. In this post, I'll walk you through some of the lessons we learned along the way and how we overcame common hurdles during our migration.

### 🚨 **Probe Issue: Cloudflare Errors**

After migrating to Kubernetes, one of the first issues we encountered was user reports of **Cloudflare error messages**. These errors would resolve after refreshing the page, but it was a consistent problem. After investigating, we traced the issue back to our **deployment configuration**.

We realized the way the pods were being deployed didn’t allow Cloudflare to properly check pod health, causing these timeouts.

Here’s how we fixed it:

1. **Implementing Pod Probes:** We created both **readiness** and **liveness probes** in Kubernetes to ensure that pods were always healthy before routing traffic to them.
    
2. **Adding Resilience:** This ensured that our Kubernetes deployment automatically restarted unhealthy pods, preventing downtime.
    

Here's a sample YAML for configuring the readiness and liveness probes in Kubernetes:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  template:
    spec:
      containers:
      - name: my-app-container
        image: my-app:latest
        ports:
        - containerPort: 8080
        readinessProbe:
          httpGet:
            path: /health
            port: 8080
          initialDelaySeconds: 5
          periodSeconds: 10
        livenessProbe:
          httpGet:
            path: /health
            port: 8080
          initialDelaySeconds: 15
          periodSeconds: 20
```

In this YAML, the readiness probe ensures the pod is ready to accept traffic, while the liveness probe ensures the pod is running properly and restarts it if needed.

For a detailed breakdown of this investigation and solution, you can check out my full blog post [here](https://dev.to/programmerraja/sherlock-holmes-and-the-case-of-the-cloudflare-timeout-mystery-3d61).

---

### 🧐 **Retrieving Real User IPs**

Another issue we faced was the inability to retrieve the **real IP address of users** after migrating to Kubernetes. Instead, we were seeing the pod proxy IP, which wasn’t helpful for tracking user data or managing logs.

**Solution:** By configuring the `externalTrafficPolicy` to `Local`, Kubernetes ensures that the real client IP is passed to your services, even when traffic goes through a load balancer.

Here’s a sample YAML snippet to achieve this:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 8080
  type: LoadBalancer
  externalTrafficPolicy: Local
```

This configuration helps restore the real client IP by making sure traffic is routed only to local nodes.

For a detailed walk-through of the solution, check out my blog post [here](https://dev.to/programmerraja/sherlock-holmes-the-case-of-the-missing-user-ips-5a81).

---

### 💀 **Zombie State Issue: Servers Becoming Unresponsive**

One of the more unexpected challenges we faced was our servers entering a “**zombie**” state. After running smoothly for a couple of days without a restart or code push, some of our servers became unresponsive.

Even after extensive troubleshooting, we couldn’t pinpoint the root cause. After some experimentation, we implemented a simple solution: a **cron job** to restart servers automatically every 24 hours. This approach helped keep our services responsive and ensured we wouldn’t face any prolonged downtime.

Here’s an example of how you could set up a cron job in Kubernetes using a **CronJob resource**:

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: restart-my-server
spec:
  schedule: "0 0 * * *" # Runs every day at midnight
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: restart-container
            image: my-app:latest
            command: ["sh", "-c", "echo Restarting server... && kill -HUP 1"]
          restartPolicy: OnFailure
```

This CronJob will restart your container at midnight every day, ensuring the server stays responsive.

---

### 🔐 **Internal Service Communication: Keeping It Secure**

One of the major advantages of moving to Kubernetes was the ability to **enhance our security** by restricting the visibility of internal services. We didn’t want all of our services to be exposed externally, so we configured Kubernetes to allow internal communication without exposing these services to the public.

We leveraged **Kubernetes' internal DNS system** to facilitate communication between internal services, ensuring that only internal services can access one another.

Example: A service running in the namespace `my-namespace` can be accessed via `my-service.my-namespace.svc.cluster.local`.

This setup helped us isolate critical services, reducing the attack surface and increasing overall security.

---

### 🚨 **Setting Up Alerts: Proactive Monitoring**

Proper **alerting** is critical to keeping your Kubernetes environment healthy. Without it, problems like **crash loops** or **unexpected pod restarts** can go unnoticed until they cause major downtime.

In our case, we set up alerts using **Prometheus** and **Alertmanager**, notifying us whenever a pod enters a crash loop or when specific conditions (e.g., high CPU usage) are met.

For example, you can use this Prometheus alerting rule to notify you when a pod enters a crash loop:

```yaml
groups:
- name: crash-loop-alerts
  rules:
  - alert: PodInCrashLoop
    expr: kube_pod_container_status_restarts_total{job="kubelet",container="my-app"} > 5
    for: 5m
    labels:
      severity: critical
    annotations:
      summary: "Pod {{ $labels.pod }} in namespace {{ $labels.namespace }} is in a crash loop"
```

This rule triggers an alert when the number of restarts for a container exceeds 5 within 5 minutes.

---

### ⚙️ **Taints and Tolerations: Optimizing Node Utilization**

We used **taints and tolerations** to manage pod placement on nodes based on resource requirements. By doing so, we ensured that high-resource pods ran on nodes with sufficient resources, while low-resource pods were allocated to less powerful nodes.

Here’s a simple YAML example to apply a taint to a node:

```yaml
kubectl taint nodes node1 key=value:NoSchedule
```

This command prevents any pod that doesn't have the corresponding toleration from running on `node1`. To allow a pod to be scheduled on that node, you would add a toleration in the pod specification:

```yaml
spec:
  tolerations:
  - key: "key"
    operator: "Equal"
    value: "value"
    effect: "NoSchedule"
```

By using this strategy, we optimized resource distribution and avoided overloading any one node.

---


### 📝 **Share Your Experience!**

What challenges did you face when migrating to Kubernetes? Or perhaps you’ve got some other great tips to share? Drop a comment below, and let’s discuss! 😊
