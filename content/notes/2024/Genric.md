---
title : Genric
date : 2024-03-17T06:43:12.1212+05:30
draft : true
tags : 
---
## API Preparation Questions

#### Technical Aspects
- Are there any rate limits or quotas?
#### Performance and Scalability
- What is the expected response time?
- What mechanisms are in place for scaling?
- Is caching applicable? If so, what are the appropriate caching strategies?
#### Security Considerations
- What security measures are implemented?
- How is authentication and authorization handled?
#### Support
- What support options are available?
- Are there SLAs (Service Level Agreements) in place?

#### Testing and Monitoring
- What tools are used for testing the API?
- How is the API monitored in production?


## Software Design Principles

### High Cohesion
- Concept: A well-organized toolbox where each drawer is dedicated to a specific type of tool (e.g., one drawer for screwdrivers, another for hammers).

### Low Coupling
- Concept: Reducing dependencies between different modules or components to enhance maintainability and flexibility.

## Team Topologies
- Resource: [Team Topologies with James Lewis](https://www.guidefari.com/tt-jl/)

## HyperMedia in REST

### Purpose
- To make the client loosely coupled from the backend by explicitly sending the operations that can be performed for an endpoint in the response.

### Example: Toaster API Endpoints
- `GET /toaster`: Retrieves the current status of the toaster.
- `POST /toaster/on`: Turns the toaster on.
- `POST /toaster/off`: Turns the toaster off.

#### Sample Responses:
1. **Toaster Off:**
   ```json
   {
     "status": "off",
     "_links": {
       "self": { "href": "/toaster" },
       "turn_on": { "href": "/toaster/on" }
     }
   }
   ```
2. **Toaster On:**
   ```json
   {
     "status": "on",
     "_links": {
       "self": { "href": "/toaster" },
       "turn_off": { "href": "/toaster/off" }
     }
   }
   ```

- Clients can control the toaster by following the hypermedia links provided in the API responses, without needing to know specific endpoints beforehand.

---

## System Monitoring Commands

- `vmstat`: Reports information about processes, memory, paging, block IO, traps, and CPU activity.
- `iostat`: Reports CPU and input/output statistics of the system.
- `netstat`: Displays statistical data related to IP, TCP, UDP, and ICMP protocols.
- `lsof`: Lists open files of the current system.
- `pidstat`: Monitors the utilization of system resources by processes, including CPU, memory, device IO, task switching, and threads.



## gRPC
- Overview of gRPC and its benefits for high-performance, cross-platform communication.

---

## Virtual Memory Concepts
- Understanding of registers (e.g., a 32-bit register can store addresses up to 2GB).
- Memory fragmentation and page tables.



## Goodhart's Law
**Definition:** "When a measure becomes a target, it ceases to be a good measure."

### Example
- Companies may track the number of bugs fixed as a measure of progress. If developers are incentivized to fix as many bugs as possible, they might focus on easy, minor bugs rather than critical or difficult ones. This can distort the original intent of improving software quality.

- The focus on specific metrics can overshadow broader goals, leading to behaviors that do not address underlying problems effectively.