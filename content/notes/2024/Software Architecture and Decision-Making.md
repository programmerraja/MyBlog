+++
title = 'Software Architecture and Decision-Making'
date  = 2024-01-22T08:19:32.3232+05:30
tags  = ['book']
draft = false
+++

## Book Core concepts

It explains principles and concepts I believe a senior architect must understand deeply and discusses how to employ those principles to manage uncertainty

 Books for Leadership

1. **The Hard Thing About Hard Things** by Ben Horowitz
2. **Trillion Dollar Coach** by Eric Schmidt et al.
3. **Team of Teams: New Rules of Engagement for a Complex World** by Stanley McChrystal
4. **Good Strategy, Bad Strategy** by Richard Rumelt

## System Architecture Approaches

**Two Prominent Approaches**
1. **Waterfall**: Identify the system’s requirements in full detail beforehand and start building.
2. **Agile**: Iterative approach, collaborating with users to refine requirements and construct a system that genuinely benefits the user.

### TOGAF Architecture Layers

**Three Layers of TOGAF Architecture**:
- **Business Architecture**
- **Information Systems Architecture**
- **Technology Architecture**

## Understanding Systems, Design, and Architecture

### Cloud App Design Choices

- **Single Cloud**: Leverage the unique strengths of one cloud provider.
- **Multi-Cloud**: Make the application portable across several cloud providers.

### How to Design a System

**Five Questions to Consider:**
1. **When is the best time to market?**
   - If a feature needs to go to market urgently, design it simply and quickly, with readiness to revise it later.
2. **What is the skill level of the team?**
3. **What is our system’s performance sensitivity?**
4. **When can we rewrite the system?**
5. **What are the hard problems?**

**Seven Principles of System Design:**

1. **Drive Everything from the User’s Journey**: Focus on the user experience.
2. **Use an Iterative Thin Slice Strategy**:
   - Start with simple architectural choices.
   - Measure the system, find bottlenecks, and refine later.
3. **Add the Most Value for the Least Effort**:
   - On each iteration, prioritize actions that offer the most value with minimal effort.
4. **Make Decisions and Absorb Risks**: Be prepared to make informed decisions and accept associated risks.
5. **Design Deeply for Hard-to-Change Aspects**:
   - Implement slowly and carefully for components that are difficult to alter.
6. **Eliminate Unknowns and Learn from Evidence**:
   - Tackle hard problems early and work in parallel to gain insights.
7. **Understand Trade-Offs**:
   - Balance between cohesion and flexibility in software architecture.



## Mental Models for Understanding and Explaining System Performance

Eight mental models that help us think about and understand performance

1. Cost of Switching to the Kernel Mode from the User Mode
	- Every time anapplication enters kernel mode, a context switch occurs, which adds nonessential costs to the system, such as time to save the stack and to rest the cache. To improve performance, we need to reduce the number of system calls.
2. Operations Hierarchy

| Operation | Time/Speed (Example) | Description |
| ---- | ---- | ---- |
| L1 Cache Reference | 0.5 ns | Accessing data from Level 1 cache in the CPU. |
| L2 Cache Reference | 7 ns | Accessing data from Level 2 cache in the CPU. |
| L3 Cache Reference | 20 ns | Accessing data from Level 3 cache in the CPU. |
| Main Memory (RAM) Access | 100 ns | Retrieving data from the system's RAM. |
| SSD Storage Access | 1,000,000 ns (1 ms) | Accessing data from Solid State Drive (SSD). |
| HDD Storage Access | 10,000,000 ns (10 ms) | Accessing data from Hard Disk Drive (HDD). |
| Network Packet Round Trip (US to India) | 150,000,000 ns (150 ms) | Sending a packet from the US to India and receiving the acknowledgment. |
| CPU Processing (Instruction) | 0.2 ns per instruction | Executing a single CPU instruction. |
| GPU Processing (Parallel) | 50 ns per parallel task | Executing parallel tasks on a Graphics Processing Unit (GPU). |
| PCIe Data Transfer (Device to CPU) | 2,000 ns (2 us) | Transferring data between a peripheral device and the CPU via PCIe. |
| Context Switch (Kernel) | 1,000 ns (1 us) | Switching between different processes in the kernel. |
| RAM-to-Cache Transfer | 20 ns | Copying data from RAM to cache in the CPU. |
| Database Query (Local) | 10,000,000 ns (10 ms) | Executing a database query on a local server. |
| Database Query (Remote) | 50,000,000 ns (50 ms) | Executing a database query on a remote server. |
| System Call (Linux) | 1,000 ns (1 us) | Initiating a system call in a Linux environment. |
| I/O Operation (Disk Write) | 10,000 ns (10 us) | Writing data to a disk. |
| I/O Operation (Network Send) | 1,000,000 ns (1 ms) | Sending data over a network. |
| I/O Operation (Network Receive) | 1,000,000 ns (1 ms) | Receiving data over a network. |

3. Context Switching Overhead
	- Switching processes adds an overhead cost of about 5–7 microseconds
4. Amdahl’s Law
	- is used to predict speed up of a task execution time when it’s scaled to run on multiple processors. It simply states that the maximum speed up will be limited by the serial fraction of the task execution as it will create resource contention.
	- `one woman nine months to make one baby, “nine women can’t make a baby in one month`
	- Assume if have 3 thread that doing a single task  it take 2 sec but the most of the time will spend on maintining the 3 thread and resources sharing
	- Parellel process are efficient when they are independt
5. Universal Scalability Law
	- says that actual speedup is even worse than Amdahl’s law due to shared variables. USL defines a new parameter coherency, which is the overhead added by communication between multiple processes, threads, or nodes.
6. Latency and Utilization Trade-offs
	 - Only a single thread can use the most resources at a given time, which forces threads to wait and take turns
7. Designing for Throughput with the Maximal Useful  Utilization (MUU) Model
8. Adding Latency Limits

---
## Optimization Techniques

To effectively optimize, it's crucial to identify where the bottlenecks occur. Bottlenecks typically manifest in one of three forms:

1. **Resource Bottleneck**: One of the resources (e.g., CPU, I/O, or memory) is the limiting factor.
2. **Thread Model Bottleneck**: Thread models are causing critical resources to be idle.
3. **Resource Waste**: Resources are being wasted on nonessential tasks (e.g., context switches, garbage collection).

### CPU Optimization Techniques

- **Optimize Individual Tasks**: Improve the efficiency of specific tasks that use CPU resources.
- **Optimize Memory**: Ensure that memory usage is efficient to reduce the CPU's burden.
- **Maximize CPU Utilization**: Ensure that the CPU is used to its fullest potential without unnecessary idling.

### I/O Optimization Techniques

- **Avoid I/O**: Use caching to minimize direct I/O operations.
- **Buffering**: Implement buffering to handle I/O operations more efficiently.
- **Send Early, Receive Late, Don’t Ask but Tell**: Design systems to send data early and receive it late, and use push-based communication rather than pull-based.
- **Prefetching**: Load data before it’s needed to reduce wait times.
- **Append-Only Processing**: Use append-only structures (e.g., Kafka) to improve I/O performance.

### Memory Optimization Techniques

- **Reduce Cache Misses**: Minimize the number of cache misses to improve memory access efficiency.

### Latency Optimization Techniques

- **Do Work in Parallel**: Utilize parallel processing to reduce overall latency.
- **Reduce I/O**: Minimize I/O operations to lower latency.

#### CPU Utilization is Wrong 

- CPU utilization is a metric that measures the percentage of time the CPU spends executing non-idle tasks. It doesn't necessarily mean the CPU is busy with computation; it's more about the time the CPU is not running the idle thread.
- The idle thread is a special task that runs when the CPU has no other tasks to perform. The operating system kernel tracks CPU utilization during context switches.
- So  high %CPU to mean that the processing unit is the bottleneck, which is wrong because CPU is capable of doing the process it may wait for I/O or something .
- `Source: https://opensource.com/article/18/4/cpu-utilization-wrong`

## The USE Method

The **Utilization Saturation and Errors (USE) Method** is a methodology for analyzing the performance of any system.

**For every resource, check the following:**

- **Utilization**: The average time that the resource was busy servicing work.
- **Saturation**: The degree to which the resource has extra work it can't service, often resulting in a queue.
- **Errors**: The count of error events associated with the resource.

**Resources**: This includes all physical server functional components, such as CPUs, disks, buses, etc.

### References

1. [Farnam Street: Mental Models](https://fs.blog/mental-models/)
2. [High Scalability: Big List of 20 Common Bottlenecks](http://highscalability.com/blog/2012/5/16/big-list-of-20-common-bottlenecks.html)
3. [OpenSource.com: CPU Utilization Misconceptions](https://opensource.com/article/18/4/cpu-utilization-wrong)
4. [Wikipedia: Queueing Theory](https://en.wikipedia.org/wiki/Queueing_theory)
5. [Brendan Gregg: USE Method](https://www.brendangregg.com/usemethod.html)
6. [Medium: What Makes Apache Kafka So Fast](https://medium.freecodecamp.org/what-makes-apache-kafka-so-fast-a8d4f94ab145)
7. [An Analysis of Web Servers Architectures Performances on Commodity Multicore](https://hal.inria.fr/hal-00674475/document)
8. [MIT OpenCourseWare: Advanced Data Structures](https://ocw.mit.edu/courses/6-851-advanced-data-structures-spring-2012/video_galleries/lecture-videos/)

## Understanding User Experience (UX)

Few concepts or principles that help us to design a good UX:

- Understand the Users
- Do as Little as Possible
- Good Products Do Not Need a Manual: Its Use Is Self-Evident
- Think in Terms of Information Exchange
    - Users come to our system to get something done. The faster they can find what they need to do and do it, the happier they will be. If we can provide that UX without asking them anything, that’s even better.
- Make Simple Things Simple
- Design UX Before Implementation

`Note:` Having UX expertise on the team is a must

## Macro Architecture

Spliting for serivce is macro Architecture.

Macro Architectural Building Blocks are
- Data Management  (DB,)
- Routers and Messaging (API Gateway,loadbalancer,message broker)
- Executors (Actual server)
- Security
- Communication (Distributed hash tables,Gossip architectures,Tree of responsibility patterns)

## Macro Architecture: Coordination

#### Drive Flow from the Client
- Call all API calls to the service from the client.
#### Another Service
- Have a separate service that coordinates with others and returns the result.
#### Implement Choreography
- Event-driven system where each participant listens to different events and carries out their individual part. Each action generates asynchronous events, triggering participants downstream.

Refer:
1. [Scaling Microservices with Event Streams](https://www.thoughtworks.com/insights/blog/scaling-microservices-event-stream)

---
## Macro Architecture: Preserving Consistency of State

#### Two-Phase Commit Protocol

#### Approaches to Going Beyond Transactions

1. **Redefining the Problem to Require Lesser Guarantees**
    - Resolve complex situations with reduced guarantees.
    - Provide a button for users to forcefully refresh the page if it appears outdated.
2. **Using Compensations**
    - Starbucks `Does Not Use Two-Phase Commit`
    - The key idea is to compensate if an action fails.
    - Use compensations if the following are satisfied:
        1. Each individual operation can be verified.
        2. The operation is idempotent (repeating it with the same data has no additional side effects).
        3. Failure can be handled or compensatory actions can be taken.

**Resources**
1. [Consistency Models](https://en.wikipedia.org/wiki/Consistency_model#Client-centric_consistency_models)
2. [Eventually Consistent Systems](http://www.allthingsdistributed.com/2007/12/eventually_consistent.html)
3. [Starbucks and Two-Phase Commit](https://www.enterpriseintegrationpatterns.com/ramblings/18_starbucks.html)
4. [Life Beyond Distributed Transactions: An Apostate’s Opinion](https://queue.acm.org/detail.cfm?id=3025012)
5. [etcd](https://etcd.io/)
## Macro Architecture: Handling Security

#### Interaction Security

**Authentication Techniques**
1. https://github.com/ory/keto. 

## Macro Architecture: Handling High Availability and Scale

Before going to scale the system to N Do the POC of single system capacity how much they can handle and if possible can tweak that to improve performance.

There are four tactics (techniques) to keep communication low, and they help us scale.

1. Share Nothing
2. Distribution
3. Caching
4. Async Processing

**Resources**
1. Scalability! But at What COST? (https://www.usenix.org/system/files/conference/hotos15/hotos15-paper-mcsherry.pdf)

## Macro Architecture: Microservices Considerations

Microservices allow us to split the structure into many loosely connected parts that can be developed, released, improved, and managed independently.

### Decisions to Make Before Adopting Microservices

1. **Handling Shared Database(s)**
    - Each microservice should have its own database; avoid sharing databases to reduce tight coupling between services.
    - If sharing is necessary, ensure only one service performs updates to prevent data redundancy.
    - Use transactions if both services need to perform updates.
2. **Securing Microservices**
3. **Coordinating Microservices**
4. **Avoiding Dependency Hell**
    - Ensure that one service deployment does not break others. Use **Backward Compatibility** or **Forward Compatibility** to manage dependencies.
        - **Backward Compatibility**: If updating the API to v2, it should still support the previous version’s functionality without query params.
        - **Forward Compatibility**: If V2 fails, fall back to V1 as a temporary solution.
    - Avoid dependency hell by being conservative in what you do and liberal in what you accept from others.

**Resources**
1. [Microservices](http://martinfowler.com/articles/microservices.html)
2. [Microservice Trade-Offs](http://martinfowler.com/articles/microservice-trade-offs.html)
3. [Microservice Premium](http://martinfowler.com/bliki/MicroservicePremium.html)
4. [Microservices: Size and Use](https://cramonblog.wordpress.com/2014/02/25/micro-services-its-not-only-the-size-that-matters-its-also-how-you-use-them-part-1/)
5. [Taming Dependency Hell](https://www.infoq.com/news/2015/06/taming-dependency-hell/)
6. [Hacker News Discussion](https://news.ycombinator.com/item?id=9705098)

## Server Architectures

Some guidelines for writing efficient and simple services
- Do not reinvent the wheel
- Use pools to reuse complex objects (need to see how moongose have pool of connections what they doing)
- Make service operations idempotent whenever possible
#### Service Architecture
- Thread-per-Request Architecture
- Event-Driven (Nonblocking) Architecture
- Staged Event-Driven Architecture (SEDA)

classification of applications based on their resource usage
- CPU-bound applications (CPU >> Memory and no I/O)
- Memory-bound applications (CPU + Bound Memory and no I/O)
- Balanced applications (CPU + Memory + I/O)
- I/O-bound applications (I/O > CPU)

**Resources**
- [What is SEDA (Staged Event-Driven Architecture)?](https://stackoverflow.com/questions/3570610/what-is-seda-staged-event-driven-architecture)
- [Diploma Thesis by Berb](https://berb.github.io/diploma-thesis/)
- [Single Writer Principle](https://mechanical-sympathy.blogspot.com/2011/09/single-writer-principle.html)
- [Memory Barriers/Fences](https://mechanical-sympathy.blogspot.com/2011/07/memory-barriersfences.html)
- [CQRS (Command Query Responsibility Segregation)](https://martinfowler.com/bliki/CQRS.html)

## Building Stable Systems

**scenarios that affect stability:**
- Unexpected workloads
- Resource and dependency failures, plus service-level agreement violations
- Software bugs in both ours and borrowed code

#### Handling Unexpected Load

- understand the load and set up a system that can keep up with the load most of the time. This is called **capacity planning**
- **Autoscaling**
- **Admission Control:** set of policies, procedures, and checks that regulate the admission or acceptance of incoming entities or requests into a system example: Assume server is already overloaded instead of accepting new connection we through error.
- **Noncritical Functionality:** Turn off the Noncritical Functionality
- Disaster recovery (have a plan for what to when X go down)

#### Handling Human Changes
- Blue and green Deployment  (Having two system one with new updated one and one with old one if new one fails redirect traffic to old one)
- Canary deployments (small percentage of users to the new system and gradually increase the load to that system)

#### Handle Unknown Errors
- Observability (Application Performance Monitoring (APM) tool, Open telementry)

## Building and Evolving the Systems

- Get the Basics Right
- Understand the Design Process
- Conway law: organizations design systems that mirror their own communication structure rather than fighting it
- Make Decisions and Absorb Risks
- Create checklists of questions that are useful for different situations and use them.
- Communicating the Design
- Growth hacking funnel (find where we stuck does in user side or in development are we not pusing not much feature etc..)

**Resources**
- [Always Be Hacking](https://blog.thinkst.com/2022/08/always-be-hacking.html)

---

Book
- The Coaching Habit: Say Less, Ask More & Change the Way You Lead Forever
- Hacking Growth: How Today’s Fastest-Growing Companies Drive Breakout Success
- Design-Driven Growth: Strategy & Case Studies for Product Shapers


## My Learnings
- 