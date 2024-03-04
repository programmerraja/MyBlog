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

example -> envoy(sidecar),istio(control tower)


Envoy -> are used ton communication between service it have features like circuit breaker,load balancing,retires,...etc
- it run as sidecar in container

ISTIO- > mangae all  the envoy 
- 




Find 
Domain story telling


API gateway
1. AWS API 
2. TAG (Tinder API Gateway)
3. Krakend

james lewis head ->microservice []()

Antipatterns


ways to find bounded context
 
 Domain story telling
 - A pictorial represnetation of the domain story



