+++
title = 'Genric'
date = 2024-03-17T06:43:12.1212+05:30
draft = true
tags =[]
+++ 

list of questions need to ask before API
- Technical Aspects (Are there any rate limits or quotas?,)
-  Performance and Scalability (expected response time?,mechanisms for scaling?,Is caching applicable? What are the appropriate caching strategies?)
- Security Considerations 
- Support 
- Testing and Monitoring

#### High Cohesion
Think of a well-organized toolbox where each drawer is dedicated to a specific type of tool - one drawer for screwdrivers, another for hammers, etc.

#### Low Coupling
Low Coupling is about reducing dependencies between different modules or components.


# Team topologies with James Lewis
https://www.guidefari.com/tt-jl/



## HyperMedia in REST

To make client loosly coupled from backend we can explictly send the operation that can do for this endpoint in reponse.
The operatio need to move like state machine we can send the operation that can be performed after this state.

**Toaster API Endpoints**:
- `GET /toaster`: Retrieves the current status of the toaster.
- `POST /toaster/on`: Turns the toaster on.
- `POST /toaster/off`: Turns the toaster off.
```
{
  "status": "off",
  "_links": {
    "self": { "href": "/toaster" },
    "turn_on": { "href": "/toaster/on" }
  }
}

{
  "status": "on",
  "_links": {
    "self": { "href": "/toaster" },
    "turn_off": { "href": "/toaster/off" }
  }
}


```
- Clients can control the toaster by following the hypermedia links provided in the API responses. For example, they can turn the toaster on or off without needing to know the specific endpoints beforehand.



vmstat’ - reports information about processes, memory, paging, block IO, traps, and CPU activity.  
![:small_blue_diamond:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/google-medium/1f539.png)‘iostat’ - reports CPU and input/output statistics of the system.  
![:small_blue_diamond:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/google-medium/1f539.png)‘netstat’ - displays statistical data related to IP, TCP, UDP, and ICMP protocols.  
![:small_blue_diamond:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/google-medium/1f539.png)‘lsof’ - lists open files of the current system.  
![:small_blue_diamond:](https://a.slack-edge.com/production-standard-emoji-assets/14.0/google-medium/1f539.png)‘pidstat’ - monitors the utilization of system resources by all or specified processes, including CPU, memory, device IO, task switching, threads, etc.


GRPC


virtual memory

register is 32 bit so it can only store 2gb of ram address

memory fragmention
page table



