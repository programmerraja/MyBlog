---
title: Tech stack of popular companies
date: 2024-01-12T08:19:32.3232+05:30
draft: false
tags:
  - system_design
---


 problems in a distributed system 
- maintaining the system state (liveness of nodes)
- communication between nodes
solutions
- centralized state management service (Apache Zookeeper)
- peer-to-peer state management service  (gossip protocol)

Gossip Protocol 
every node periodically sends out a message to a subset of other random nodes. The entire system will receive the particular message eventually with a high probability. In layman’s terms, the gossip protocol is a technique for nodes to build a global map through limited local interactions



Load balancer anycast

## Strangler Fig pattern

Helps for legacy system migrations.

The first step (Stage 0) is to create a Facade that intercepts requests to the backend legacy system. Now, you can use this facade to route the requests to the legacy application or the new services



1. For a Read-Heavy System — Consider using a Cache.  
2. For a Write-Heavy System — Use Message Queues for async processing  
3. For a Low Latency Requirement — Consider using a Cache and CDN.  
4. Need 𝐀tomicity, 𝐂onsistency, 𝐈solation, 𝐃urability Compliant DB — Go for RDBMS/SQL Database.  
5. Have unstructured data — Go for NoSQL Database.  
6. Have Complex Data (Videos, Images, Files) — Go for Blob/Object storage.  
7. Complex Pre-computation — Use Message Queue & Cache.  
8. High-Volume Data Search — Consider search index, tries, or search engine.  
9. Scaling SQL Database — Implement Database Sharding.  
10. High Availability, Performance, & Throughput — Use a Load Balancer.  
11. Global Data Delivery — Consider using a CDN.  
12. Graph Data (data with nodes, edges, and relationships) — Utilize Graph Database.  
13. Scaling Various Components — Implement Horizontal Scaling.  
14. High-Performing Database Queries — Use Database Indexes.  
15. Bulk Job Processing — Consider Batch Processing and Message Queues.  
16. Server Load Management & Preventing DOS Attacks- Use a Rate Limiter.
17. Microservices Architecture — Use an API Gateway.  
18. For Single Point of Failure — Implement Redundancy.  
19. For Fault-Tolerance and Durability — Implement Data Replication.  
20. For User-to-User fast communication — Use Websockets.  
21. Failure Detection in Distributed Systems — Implement a Heartbeat.  
22. Data Integrity — Use Checksum Algorithm.  
23. Efficient Server Scaling — Implement Consistent Hashing.  
24. Decentralized Data Transfer — Consider Gossip Protocol.  
25. Location-Based Functionality — Use Quadtree, Geohash, etc.  
26. Avoid Specific Technology Names — Use generic terms.  
27. High Availability and Consistency Trade-Off — Eventual Consistency.  
28. For IP resolution and domain Name Query — Mention DNS.  
29. Handling Large Data in Network Requests — Implement Pagination.  
30. Cache Eviction Policy — Preferred is LRU (Least Recently Used) Cache.  
31. To handle traffic spikes: Implement Autoscaling to manage resources dynamically  
32. Need analytics and audit trails — Consider using data lakes or append-only databases  
33. Handling Large-Scale Simultaneous Connections — Use Connection Pooling and consider using Protobuf to minimize data payload**

##  Cell-Based Architecture

A cell-based architecture comes from the concept of a Bulkhead pattern. it simllar to that it was used on[ AWS cloud.](https://docs.aws.amazon.com/wellarchitected/latest/reducing-scope-of-impact-with-cell-based-architecture/what-is-a-cell-based-architecture.html) 

Bulkhead pattern
-  In a bulkhead architecture, elements of an application are isolated into pools so that if one fails, the others will continue to function.

## Concurrency Patterns
#### Request Coalescing
The idea of request collapsing is simple: if you have multiple requests for the same resource, allow only one to pass through but use its result for all responses.

#### **Request Hedging**.

Lets say we have cache and we recevied the query that not in cache so it request DB for data but similarly there are more request for same resource all request will hit the DB

To avoid this problem, we use a technique called **Request Collapsing**.

1. The first read request acquires a lock.
2. All subsequent requests are made to wait on this lock.
3. Once the DB call returns and populates the cache, the lock is opened.
4. All dependent requests read the result from the cache.

What happens if a DB call fails? All request waiting for lock will request DB to solve this we send two duplicate requests. The first successful response is used to answer all dependent requests. is called **Request Hedging**.



Resources
1. https://read.engineerscodex.com/p/meta-xfaas-serverless-functions-explained 




How uber implements caching

They have build own database top of SQL called docstore in that query engine they have introduced caching layer. 

This caching layer is built on top of Redis and uses a _cache-aside_ strategy.
So first it check cache and pick data from there

**Cache Invalidation**
- They add TTL 5 min and use CDC (change data capture) based on the MySQL binlog. Whenever there’s a data update, Uber will take the change (_and its associated timestamp_) and check if there should be any modification to a cached value in Redis.

**Circuit Breakers**
implement a circuit breaker that will cut off requests to Redis nodes with a high error rate. in redis


## CRDT
Conflict-free Replicated Data Type 
https://jakelazaroff.com/words/an-interactive-intro-to-crdts/  
## Resources
1.  https://github.com/JohnCrickett/SystemDesign/tree/main/engineering-blogs
2. https://github.com/Coder-World04/Complete-System-Design?tab=readme-ov-file
3. https://systemdesign.one/leaderboard-system-design/
4. https://engineering.grab.com/frequency-capping
5. https://www.primevideotech.com/video-streaming/how-prime-video-ingests-processes-and-distributes-live-tv-to-millions-of-customers-around-the-world-while-reducing-costs
6. https://highscalability.com/



How Canva Supports Real-Time Collaboration for 135 Million Monthly Users

They using  [RSocket](https://rsocket.io/) for real-time collaboration. which better then WS



Panic mode 
multi cdn
kafa


https://blog.bytebytego.com/p/how-do-we-design-for-high-availability


 Never fail a system design interview again. A collection of [Quiz](https://www.swequiz.com/)


## CQRS

command query responsibility seggregation. where it will use full when we need to more read optimitic and write optimistic There is two opporach in impelement this
- Using same database
- Using different database 


## Event sourcing

**Event sourcing** is a Microservice design pattern that involves capturing all changes to an application’s state as a **sequence of events, rather than simply updating the state itself**. Each event represents a discrete change to the system and is stored in **an event log,** which can be used to reconstruct the system’s state at any point in time. it will be usefull when we want the history of updation record
	1. **Event Generation:** An event is generated whenever a change occurs in the system.
	2. **Persistence in Event Store:** The event is persisted to an event store, which is essentially a log of all events that have occurred in the system.
	3. **Reconstruction of current state by replaying events:** The current state of the system can be reconstructed at any time by replaying all of the events in the event store, in the order that they occurred.
	4. **Service wise event store** : Each service in the microservice architecture can have its own event store, which can be used to maintain its own state.
	5. **Subscription to event store:** Services can subscribe to events that are relevant to them and update their own state accordingly.

we can combine CQRS with event sourcing to handle update and read

## SAGA
 
 Saga pattern for acid among microservices
    
A Saga is a design pattern used in distributed systems and microservices architecture to manage a sequence of related, independent transactions or operations. The primary goal of a Saga is to ensure that all these transactions are completed successfully or, if an error occurs, to provide a mechanism to compensate for the operations that have already occurred.
    - Two ways to implement SAGA
        - [**Orchestration-Based Saga](https://blog.bitsrc.io/how-to-use-saga-pattern-in-microservices-9eaadde79748#:~:text=1.%20Orchestration%2DBased%20Saga):** A single orchestrator (arranger) manages all the transactions and directs services to execute local transactions.
        - [**Choreography-Based Saga**](https://blog.bitsrc.io/how-to-use-saga-pattern-in-microservices-9eaadde79748#:~:text=2.%20Choreography%2DBased%20Saga): All the services that are part of the distributed transaction publish a new event after completing their local transaction.
    
-  [https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/saga/saga](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/saga/saga)
- https://blog.bitsrc.io/implementing-saga-pattern-in-a-microservices-with-node-js-aa2faddafac3](https://blog.bitsrc.io/implementing-saga-pattern-in-a-microservices-with-node-js-aa2faddafac3)

My notes
- After implementing a system make to add monitoring for the system
	- How many request processed/per second etc..






