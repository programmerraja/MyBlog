---
title : MicroServices
date : 2024-01-12T08:19:32.3232+05:30
draft : true
---

## Questions to Be Asked

1. How will the new service be deployed and upgraded?
2. How will it be consumed by the rest of the system?

## Service Registry

**Eureka**

- When a new service is introduced, it registers with the service registry.
- Services can look up other services through the registry.
- There is a single point of failure.
- Code needs to be written for each service to interact with the service registry.

## Service Mesh

**Purpose:** To address the limitations of service registries by managing service-to-service communication in a more streamlined way.

### Components of a Service Mesh
1. **Data Plane**
    - Consists of lightweight proxies deployed alongside every instance of your microservices (sidecar pattern).
    - Handles all outbound/inbound communications for the instance.
    - Example: Using Istio, a popular service mesh, you could deploy the Envoy Proxy on all microservice instances.
2. **Control Plane**
    - Manages and configures the data plane proxies.
    - Configures policies such as retries, rate limiting, health checks, etc.
    - Handles service discovery (tracking IP addresses of instances), deployments, and more.
    **Example:**
    - Envoy (sidecar) manages communication between services, featuring capabilities like circuit breaking, load balancing, and retries.
    - Istio (control plane) manages all the Envoy proxies.
    **Resources:**
    - [Quora Engineering - Building a Service Mesh in a Hybrid Environment](https://quoraengineering.quora.com/Building-a-Service-Mesh-in-a-Hybrid-Environment)
    - [Cilium](https://cilium.io/) (handles networking at the kernel level using eBPF, avoids iptables and kube-proxy in Kubernetes)

## API Gateway

1. AWS API Gateway
2. TAG (Tinder API Gateway)
3. Krakend

## Domain-Driven Design

### Antipatterns

- Recognizing and avoiding common antipatterns in microservices and domain-driven design.

### Ways to Find Bounded Context

- Domain Storytelling: A pictorial representation of the domain story to define bounded contexts.

## Event-Driven Architecture

- An architectural pattern where services communicate through events rather than direct calls.
- https://hookdeck.com/blog/event-driven-architectrure-fundamentals-*pitfalls*
## Caching

- **ReadySet**: Real-time SQL caching for Postgres and MySQL that manages everything internally.
    - Uses streams and watches for changes.