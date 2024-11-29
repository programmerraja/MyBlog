---
title: "Sherlock Holmes: The Mystery of the"
date: 2024-11-25T06:52:20.2020+05:30
draft: true
tags:
---

If you've been following our recent Kubernetes migration adventures, you might have noticed a few bumps along the way. From configuring pods to troubleshooting networking issues, we've been on quite the journey. In previous blogs, we've tackled some tricky problems, and today we’re diving into yet another Kubernetes-related mystery.

Recently, while using Kubernetes observability tools, we noticed something odd: **over a million NXDomain errors**. Naturally, our curiosity was piqued, and we set off to investigate what was going on. If you've ever encountered NXDomain errors, you know the drill—they essentially mean that the domain doesn't exist. But when we started looking closer, we realized the domains involved were quite unusual.

### The Case of the Mysterious Domains

The NXDomain errors we were seeing were associated with external domains like:

- `gmail.googleapis.com.cluster.local`
- `oauth2.googleapis.com.es52e2p4cate3lv5fzg4m1it5a.bx.internal.cloudapp.net`

Wait a minute—something was off here. If you look closely, you'll notice a strange pattern: the external domains had extra words attached to them, like `.cluster.local` or `.internal.cloudapp.net`. So what was causing this?

### How Kubernetes DNS Resolves Domains

To understand the root cause, we first need to look at how **DNS resolution works in Kubernetes**. When a pod performs a DNS lookup, it doesn’t always behave the same way as a typical DNS query. By default, Kubernetes uses the `/etc/resolv.conf` file provided by the Kubelet to forward DNS queries to the cluster's DNS server.

Here’s the breakdown:

- **DNS Server:** All DNS queries are forwarded to the cluster’s DNS server (e.g., `10.123.0.10` in our case).
- **Search Domains:** Kubernetes defines a list of search domains and the `ndots` option, which governs how incomplete domain names are handled.
    - **Search Domains** are suffixes appended to incomplete domain names.
    - **NDots** is a setting that determines how many dots (`.`) are in a domain name before Kubernetes decides whether to append search domains or query directly.

For example, imagine a pod named `foo` wants to look up `bar.other-ns`. If the `ndots` option is set to 5 (the default), Kubernetes counts the number of dots in the domain. If there are fewer than 5 dots, Kubernetes appends the search domains to the query before making a lookup. If there are 5 or more dots, the query is sent directly as an absolute domain name.

### The NXDomain Mystery Solved

So, what was causing those NXDomain errors? Let’s break it down:

When the pod queries an external domain like `gmail.googleapis.com`, Kubernetes sees the query as incomplete because it doesn't have enough dots. It then appends the default search domains, such as `.svc.cluster.local` or `.cluster.local`, and tries to resolve something like `gmail.googleapis.com.svc.cluster.local` or `gmail.googleapis.com.cluster.local`.

As you might have guessed, **these domains don’t exist**—hence the NXDomain error.

### How to Fix It: Custom DNS Configuration for Pods

Now that we’ve solved the mystery, it’s time to fix it. The solution is to modify the DNS configuration for the affected pods so that Kubernetes doesn't append its default search domains to external queries.

To do this, you can specify a custom DNS configuration for your pods. Here’s how you can change the DNS behavior in your pod definition:

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

- **`dnsPolicy: "None"`**: By setting this, you tell Kubernetes not to use the default DNS settings. This gives you full control over DNS resolution within the pod.
- **`nameservers`**: Specifies the DNS servers to use for resolving domain names.
- **`searches`**: Defines the search domains that Kubernetes will use for incomplete domain names.
- **`options`**: Allows you to set additional DNS resolver options, such as:
    - **`ndots`**: In this case, we set it to `2`, which reduces the number of dots required for Kubernetes to append search domains.
    - **`edns0`**: Enables EDNS (Extension Mechanisms for DNS) support, which can improve performance and allow for larger DNS messages.

### The Outcome

By adjusting the DNS configuration for your pods, you ensure that Kubernetes only tries to resolve domains as they are, without mistakenly appending internal search domains for external queries. This resolves the NXDomain errors we were seeing and ensures smooth DNS resolution for external services.

---

And there you have it—the mystery is solved! With a little bit of DNS sleuthing, we uncovered the root cause of those pesky NXDomain errors and put a plan in place to fix it.

### Stay Tuned for More Kubernetes Mysteries

As always, there are more Kubernetes mysteries waiting to be cracked. Keep your magnifying glass handy and stay tuned for more insights and troubleshooting tips from our ongoing adventure in the world of Kubernetes.

**If this post helped you solve a problem or gave you some clarity, don’t forget to clap 👏 and follow for more updates. Thanks for reading!**

---

This should give you a blog post that's informative, easy to follow, and maintains the engaging, Sherlock Holmes-style tone. Let me know if you'd like to tweak any parts!

































As you have gone through my pervious blog you may get to know that we are recently migrated to the kubernetes after migration we face few that all are discussed on the previous blogs. today also we going to discuss about the kubernetes related error  so lets get started


We are using kuberentes observablity tool in that we have seen more then 1 Milions of NXdomain error which caught our attentaion and asked me to invistigate why

Basically NXdomain error mean the domain not exists and i checked for the domains it looks like `gmail.googleapis.com.cluster.local` ,  `oauth2.googleapis.com.es52e2p4cate3lv5fzg4m1it5a.bx.internal.cloudapp.net` etc so if you noticied there is some patterns are following all the external domain have attached extra word as which  make 

TO move further you need to understand how kubernetes dns works so here a small 


By default, the `/etc/resolv.conf` file provided by the Kubelet will forward all DNS queries to the cluster's DNS server (10.123.0.10 in the example above). The Kubelet also defines search domains and the `ndots` option for DNS queries.

The search domains specify which domain suffixes should be searched when incomplete domains (non-FQDNs) are given. The `ndots` option determines when a query for the absolute domain is made directly instead of first appending the search domains.

To better understand how this works, let's look at an example. Suppose a pod named foo performs a DNS lookup for `bar.other-ns`. If the `ndots` option is set to 5 (the default value — [here's why](https://github.com/kubernetes/kubernetes/issues/33554#issuecomment-266251056)), the resolver will count the number of dots in the domain.

If there are fewer than 5 dots, the search domains will be appended before the DNS lookup is performed on the DNS server. If there are 5 or more dots, the domain will be queried as-is without appending the search domains. In this example, `bar.other-ns` has less than 5 dots, so the search domains will be appended before the DNS lookup is performed.

By default, the search domains are:

- (requester namespace).svc.cluster.local
- svc.cluster.local
- cluster.local

Until a valid response is found, these search domains are appended to the domain and queried. The resolver will try the following queries one by one:

- bar.other-ns.(requester namespace).svc.cluster.local
- bar.other-ns.svc.cluster.local (⇐ match found!)

The bar service will be listening on `bar.other-ns.svc.cluster.local`, so a match is found and the proper A-record is returned.


SO you find the answer why that is hapenning so lets see how we fix this

To change the behavior of a pod's DNS resolver, you can change the DNS config of a pod

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

In the example above, the `dnsPolicy` is set to "None", which means that the pod will not use the default DNS settings provided by the cluster. Instead, the `dnsConfig` field is used to specify custom DNS settings for the pod.

The `nameservers` field is used to specify the DNS servers that the pod should use for DNS lookups. The searches field is used to specify the search domains that should be used for incomplete domains.

The `options` field is used to specify custom options for the DNS resolver, such as the `ndots` and `edns0` options in the example above.

These settings will be used by the pod's DNS resolver instead of the default settings provided by the cluster. For more information on pod DNS configuration, see the [official docs](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/#pod-dns-config).
