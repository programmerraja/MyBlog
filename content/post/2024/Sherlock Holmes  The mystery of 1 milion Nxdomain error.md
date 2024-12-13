---
title: "Sherlock Holmes: The Mystery of the"
date: 2024-11-25T06:52:20.2020+05:30
draft: true
tags:
---

If you’ve been following our recent Kubernetes migration saga, you’re probably already aware that the journey has been full of challenges. From configuring pods to tackling networking issues, it’s been a rollercoaster. We've already explored some tricky problems in previous blogs, and today, we’re diving into another Kubernetes-related mystery that recently caught our attention.
### The Mysterious Case of NXDomain Errors

While using Kubernetes observability tools, we stumbled upon something unusual: **over a million NXDomain errors**. For those who might be unfamiliar, NXDomain errors essentially indicate that a domain doesn’t exist. Naturally, we were curious and decided to dig deeper into what could be causing these errors.

At first glance, the domains involved seemed fairly typical. However, upon closer inspection, we noticed something odd: these external domains had extra words appended to them, like `.cluster.local` or `.internal.cloudapp.net`. For example, one error was tied to:

- `gmail.googleapis.com.cluster.local`
- `oauth2.googleapis.com.es52e2p4cafzg4m1it5a.bx.internal.cloudapp.net`

If you're familiar with how DNS works, you’d recognize that something is off here. External domains usually don't come with those extra suffixes, so why were they showing up?

### How Kubernetes DNS Resolves Domains: A Deeper Look

To get to the bottom of this mystery, we need to understand how **DNS resolution works in Kubernetes**. When a pod tries to perform a DNS lookup, the process doesn’t exactly mirror that of a typical DNS query. By default, Kubernetes uses the `/etc/resolv.conf` file provided by the Kubelet to forward DNS queries to the cluster’s DNS server.

Here’s how the process works:

- **DNS Server**: All DNS queries are sent to the cluster’s DNS server, which in our case is something like `10.123.0.10`.
- **Search Domains**: Kubernetes defines a list of search domains and the `ndots` option, which governs how incomplete domain names are handled. 

   - **Search Domains**: These are suffixes appended to domain names when they’re incomplete (i.e., non-fully qualified domain names or non-FQDNs).
   - **NDots**: This setting determines how many dots must appear in a domain name before Kubernetes sends a query directly (i.e., without appending search domains).

For example, let’s say a pod named `foo` wants to look up `bar.other-ns`. If the `ndots` option is set to 5 (which is the default), Kubernetes will count the number of dots in the domain name. If there are fewer than 5 dots, Kubernetes appends the search domains to the query. If there are 5 or more dots, it treats the domain as fully qualified and sends the query directly.

### The Mystery Unravels

Now we can connect the dots pun intended! The NXDomain errors occurred because when a pod queried an external domain like `gmail.googleapis.com`, Kubernetes saw the domain as incomplete due to the lack of sufficient dots. As a result, it appended the default search domains, such as `.svc.cluster.local` or `.cluster.local`, before making the query. This transformed the original query into something like:

- `gmail.googleapis.com.svc.cluster.local`
- `gmail.googleapis.com.cluster.local`

As you might have guessed, **these domains don’t exist**, which is why we were seeing those pesky NXDomain errors.

### How to Fix It: Custom DNS Configuration for Pods

Now that we’ve identified the root cause of the issue, it’s time to address it. The solution lies in modifying the DNS configuration for the affected pods so that Kubernetes doesn’t append its default search domains to external queries.

Here’s an example of how you can customize the DNS configuration for your pods:

```yaml
apiVersion: v1
kind: Pod
metadata:
  namespace: default
  name: dns-example
spec:
  containers:
    - name: test
      image: nginx
  dnsPolicy: "None"
  dnsConfig:
    nameservers:
      - 1.2.3.4
    searches:
      - ns1.svc.cluster-domain.example
      - my.dns.search.suffix
    options:
      - name: ndots
        value: "2"
      - name: edns0
```

### What’s Happening Here?

- **`dnsPolicy: "None"`**: This setting disables the use of Kubernetes' default DNS settings, allowing you to fully control the DNS resolution behavior within the pod.
- **`nameservers`**: This specifies the DNS servers to be used for resolving domain names, ensuring that external queries are handled appropriately.
- **`searches`**: Here, you define the search domains to be used for incomplete domain names. By specifying this, you ensure that Kubernetes doesn’t append unnecessary internal search domains for external queries.
- **`options`**: The `ndots` option is crucial—it tells Kubernetes how many dots need to be present before the DNS query is sent directly. In our example, we’ve set it to `2` to prevent Kubernetes from mistakenly appending internal suffixes. Additionally, the `edns0` option enables EDNS (Extension Mechanisms for DNS), improving performance and supporting larger DNS messages.

### The Outcome: A Smooth DNS Experience

By adjusting the DNS configuration for your pods, you prevent Kubernetes from erroneously appending internal search domains to external queries. This fixes the NXDomain errors and ensures that DNS resolution for external services works as expected. The error that caused confusion and disruption is now resolved, and the system can communicate more efficiently with external resources.


If this post helped solve a problem or provided some clarity, don’t forget to clap 👏 and follow for more updates. Thanks for reading!

