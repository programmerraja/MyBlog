+++
title = 'MicroServices'
date  = 2024-01-12T08:19:32.3232+05:30
draft = false
+++

Questions to be asked
1. How will new service be deployed and upgraded
2. how will it be consumed by the rest of the system

Service registery -> Eureka
- Where single service when new service it inform to service regis and who want to lookup it ask for service regis
- single point of failure
- we need to  write code on each service



Service Mesh -> TO solve the above problem
- use side car and control tower (mange the sidecar )
- no need to write a single code

Service mesh typically consists of two components 
- Data Plane
- Control Plane
### **Data Plane**
The data plane consists of lightweight proxies that are deployed alongside every instance for all of your microservices (i.e. the [sidecar pattern](https://link.mail.beehiiv.com/ss/c/Ioed2HDTdYAXh1G-wub8Z3krL9lvWCVrc470Urpa-8YY6XxTNk0XwMExcAy8BoUo10IIq_HWxvSMZJnxVz71QFZCL3YMiIE7T9cXcwPNe7kr67TCyeSwFbwZY8hLWIt58P3w8DtPe5L4wgvqxYWjqDUm8i8ZrSCG0t9rWH62MAWAT-OrYHMaek1gG4WKNqLjVd1LZIYxd7sVKes-ziMiYP_dya9NA8BPsmiYJ61pGqdYM0TEjSVEDDWZ6Ur20LuYDfIMVnNdHtuJho8qV62q2g/446/OZ639r3dT0SOCEiZ4U-qBQ/h9/lgZgQP2VyiCrl2EX-BP83niKssZsun4FPP7CItzNk7w)). This service mesh proxy will handle all outbound/inbound communications for the instance.

So, with Istio (a popular service mesh), you could install the [Envoy Proxy](https://link.mail.beehiiv.com/ss/c/GNU-s309YMfVomuE3zs8pzuyRe_bSWNSNSWbQIaMTCRhgsUtFHn_oOniNih4OkBxqLT6NyuLrCL66ZGoBEJcEXzJNStR7C3Fn1inBBs2Rff-qL9bLn0dump4uVDNZPTipSiMgbmJVBdw9Wwu4doOM_s4u3UV4Sei-1vg7bpf_2NNCAr2IdMpQCumyv3laONXsBmi1EcEyBB8nUtP95w6tNkq97LifIyCLeHOseR9_Ugqgw1xTzeLfl6GIkXYh6SI/446/OZ639r3dT0SOCEiZ4U-qBQ/h10/k0BaFw1hdYGNTL9Y53c1-05U-qImInasrRcwCKXwufY) on all the instances of all your microservices.
### **Control Plane**
The control plane manages and configures all the data plane proxies. So you can configure things like retries, rate limiting policies, health checks, etc. in the control plane.

The control plane will also handle service discovery (keeping track of all the IP addresses for all the instances), deployments, and more.

https://quoraengineering.quora.com/Building-a-Service-Mesh-in-a-Hybrid-Environment

example -> envoy(sidecar),istio(control tower)


Envoy -> are used ton communication between service it have features like circuit breaker,load balancing,retires,...etc
- it run as sidecar in container

ISTIO- > mangae all  the envoy 
- 

https://cilium.io/  (best handle in kernel level)
1. uses eBPF
2. They not using iptables and kubproxy in kubernetes 


Find 
Domain story telling


API gateway
1. AWS API 
2. TAG (Tinder API Gateway)
3. Krakend

james lewis head ->microservice [

Antipatterns


ways to find bounded context
 
 Domain story telling
 - A pictorial represnetation of the domain story



Event drive architecture 


## Caching
- https://readyset.io/  Real-time SQL caching for Postgres and MySQL which will take care everything
- They using streams and watch for the changes