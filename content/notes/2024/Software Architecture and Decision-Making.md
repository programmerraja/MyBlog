+++
title = 'Software Architecture and Decision-Making'
date  = 2024-01-22T08:19:32.3232+05:30
draft = false
+++

## Book Core concepts

It explains principles and concepts I believe a senior architect must understand deeply and discusses how to employ those principles to manage uncertainty

## Books for leadership

The Hard Things About Hard Things by Ben Horowitz, Trillion Dollar Coach by Eric Schmidt et al., Team of Teams: New Rules of Engagement for a Complex World by Stanley McChrystal, Good Strategy, Bad Strategy by Richard Rumel

Three layers of architecture of The Open Group Architecture Framework (TOGAF)

two prominent approaches to system architecture:
• Waterfall ->identify the system’s requirements in full detail beforehand and start building
• Agile -> Iterative way (collaborating with users to refine requirements and construct a system that can genuinely benefit the user)

## Understanding Systems, Design, and Architecture

When writing a cloud app, we have two choices: We can choose a single cloud, taking advantage of its unique strengths for our application, or we can make the application portable across several cloud providers

#### How to Design a System

Five questions and 7 Principles to understand the context of system we building

1. When is the best time to market?
	- If the feature need to go urgent becasue of market urgent. we can design the way simple and fast. and we also ready to re-write it.
2. What is the skill level of the team?
3. What is our system’s performance sensitivity?
4. When can we rewrite the system?
5. What are the hard problems?

Seven principles

1. Drive everything from the user’s journey.

2. Use an iterative thin slice strategy
      - Unless you have a specific reason, always start with simple architectural choices. Measure the system, find the bottlenecks, and improve the system later.

4. On each iteration, add the most value for the least effort to support more users

5. Make decisions and absorb the risks


6. Design deeply things that are hard to change but implement them slowly

7. Eliminate the unknowns and learn from the evidence by working on hard problems early and in parallel

8. Understand the trade-offs between cohesion and flexibility in the software architecture


## Mental Models for Understanding and Explaining System Performance

Eight mental models that help us think about and understand performance

1. Cost of Switching to the Kernel Mode from the User Mode
	- Every time anapplication enters kernel mode, a context switch occurs, which adds nonessential costs to the system, such as time to save the stack and to rest the cache. To improve performance, we need to reduce the number of system calls.

2. Operations Hierarchy

| Operation                        | Time/Speed (Example)       | Description                                      |
|----------------------------------|---------------------------|--------------------------------------------------|
| L1 Cache Reference               | 0.5 ns                    | Accessing data from Level 1 cache in the CPU.    |
| L2 Cache Reference               | 7 ns                      | Accessing data from Level 2 cache in the CPU.    |
| L3 Cache Reference               | 20 ns                     | Accessing data from Level 3 cache in the CPU.    |
| Main Memory (RAM) Access         | 100 ns                    | Retrieving data from the system's RAM.           |
| SSD Storage Access               | 1,000,000 ns (1 ms)       | Accessing data from Solid State Drive (SSD).     |
| HDD Storage Access               | 10,000,000 ns (10 ms)     | Accessing data from Hard Disk Drive (HDD).       |
| Network Packet Round Trip (US to India) | 150,000,000 ns (150 ms) | Sending a packet from the US to India and receiving the acknowledgment. |
| CPU Processing (Instruction)     | 0.2 ns per instruction    | Executing a single CPU instruction.              |
| GPU Processing (Parallel)        | 50 ns per parallel task   | Executing parallel tasks on a Graphics Processing Unit (GPU). |
| PCIe Data Transfer (Device to CPU) | 2,000 ns (2 us)         | Transferring data between a peripheral device and the CPU via PCIe. |
| Context Switch (Kernel)           | 1,000 ns (1 us)           | Switching between different processes in the kernel. |
| RAM-to-Cache Transfer            | 20 ns                     | Copying data from RAM to cache in the CPU.       |
| Database Query (Local)           | 10,000,000 ns (10 ms)     | Executing a database query on a local server.   |
| Database Query (Remote)          | 50,000,000 ns (50 ms)     | Executing a database query on a remote server.  |
| System Call (Linux)              | 1,000 ns (1 us)           | Initiating a system call in a Linux environment.|
| I/O Operation (Disk Write)       | 10,000 ns (10 us)         | Writing data to a disk.                         |
| I/O Operation (Network Send)     | 1,000,000 ns (1 ms)       | Sending data over a network.                    |
| I/O Operation (Network Receive)  | 1,000,000 ns (1 ms)       | Receiving data over a network.                  |

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

#### Optimization Techniques

To optimize, we need to decide where the bottlenecks are. Bottlenecks usually come in one of three forms:

- One of the resources (e.g., CPU, I/O, or memory) is the
bottleneck.

- Thread models are causing critical resources to be idle.

- Resources are wasted on nonessential tasks (e.g., context
switches, GC).

##### CPU Optimization Techniques
- Optimize Individual Tasks
- Optimize Memory
- Maximize CPU Utilization
##### I/O Optimization Techniques
- Avoid I/O (use a cache)
- Buffering
- Send Early, Receive Late, Don’t Ask but Tell
- Prefetching
- Append-Only Processing (Kafka)
#### Memory Optimization Techniques
- Too Many Cache Misses
#### Latency Optimization Techniques
- Do Work in Parallel
- Reduce I/O

Refer
1. https://fs.blog/mental-models/ 
2. http://highscalability.com/blog/2012/5/16/big-list-of-20-common-bottlenecks.html
3. https://opensource.com/article/18/4/cpu-utilization-wrong
4. http://www.cs.cornell.edu/projects/ladis2009/talks/dean-keynote-ladis2009.pdf
5. https://en.wikipedia.org/wiki/Queueing_theory
6. https://www.brendangregg.com/usemethod.html [bottleneck]
7. https://medium.freecodecamp.org/what-makes-apache-kafka-so-fast-a8d4f94ab145
8. An Analysis of Web Servers Architectures Performances on Commodity Multicore https://hal.inria.fr/hal-00674475/document 
9. https://ocw.mit.edu/courses/6-851-advanced-data-structures-spring-2012/video_galleries/lecture-videos/

## Understanding User Experience (UX)

Few concepts or principles that help us to design a good UX.

- Understand the Users
- Do as Little as Possible
- Good Products Do Not Need a Manual: Its Use Is Self-Evident
- Think in Terms of Information Exchange 
	- Users come to our system to get something done. The faster they can find what they need to do and do it, the happier they will be. If we can provide that UX without asking them anything, that’s even better.
- Make Simple Things Simple
- Design UX Before Implementation

`Note:` having UX expertise on the team is a must

## Macro Architecture

Spliting for serivce is macro Architecture
Macro Architectural Building Blocks
- Data Management  (DB,)
- Routers and Messaging (API Gateway,loadbalancer,message broker)
- Executors (Actual server)
- Security
- Communication (Distributed hash tables,Gossip architectures,Tree of responsibility patterns)

## Macro Architecture: Coordination

#### Drive flow from the client

Call All API calls to service from client

#### Another service

Have seprate service which will cordinate with other and return the result.
#### Implement Choreography

Event driven system, where each participant in the process listens to different events and carries out their individual part. Each action generates asynchronous events, which trigger participants downstream

Refer
1. https://www.thoughtworks.com/insights/blog/scaling-microservices-event-stream

## Macro Architecture: Preserving Consistency of State

#### Two-phase commit protocol

#### Approaches to going beyond transactions

1. Redefining the Problem to Require Lesser Guarantees
    - Figure out a way to resolve complex situations.
    - Provide a button for users to forcefully refresh the page if they can tell that it is outdated

2. Using Compensations
     - Starbucks `Does Not Use Two-Phase Commit`
     - The key idea is that if an action fails, you can compensate.
     - Use compenstations if below 3 have statisfy
         1. Each individual operation can be verified.
         2. The operation is idempotent. If we repeat the operation with the same data, no additional side effects occur.
         3. We can handle the failure or take compensative actions

Refer
1. https://en.wikipedia.org/wiki/Consistency_model#Client-centric_consistency_models
2. http://www.allthingsdistributed.com/2007/12/eventually_consistent.html
3. https://www.enterpriseintegrationpatterns.com/ramblings/18_starbucks.html [Must need to check and he suggest one book need to check]
4. [Life Beyond Distributed Transactions: An Apostate’s Opinion] https://queue.acm.org/detail.cfm?id=3025012
5. https://etcd.io/  []

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


Refer
1. Scalability! But at What COST? (https://www.usenix.org/system/files/conference/hotos15/hotos15-paper-mcsherry.pdf)

## Macro Architecture: Microservices Considerations

Microservices let us split the structure into many loosely connected parts that can be developed, released, improved, and managed independently.

The decisions to make before going microservice
1. Handling Shared Database(s)
    - Each microservice should have its own database, and two microservices must not share data via the same database. This rule removes a common cause that leads to tight coupling between services
    - If we want to share then make sure only one Service do update to avoid data reduntency 
    - Use transaction if we want both service to update

2. Securing Microservices

3. Coordinating Microservices

4. Avoiding Dependency Hell
     - One service depolyment not to break other service if we have dependency use **Backward Compatibility** or **Forward Compatibility**
     - **Backward Compatibility:** If we have updated the API to v2 which will accpet query params but make sure that API support without query params that's how before it work's.
     - **Forward Compatibility:** If V2 not working try V1 (it just temporary solution)
     - Avoid dependency hell by `Be conservative in what you do, be liberal in what you accept from others`

Refer
1. http://martinfowler.com/articles/microservices.html
2. http://martinfowler.com/articles/microservice-trade-offs.html
3. http://martinfowler.com/bliki/MicroservicePremium.html
4. https://cramonblog.wordpress.com/2014/02/25/micro-services-its-not-only-the-size-that-matters-its-also-how-you-use-them-part-1/
5. https://www.infoq.com/news/2015/06/taming-dependency-hell/
6. https://news.ycombinator.com/item?id=9705098

## Server Architectures

some guidelines for writing efficient and simple services
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



Refer
1. https://stackoverflow.com/questions/3570610/what-is-seda-staged-event-driven-architecture.
2. https://berb.github.io/diploma-thesis/
3. https://mechanical-sympathy.blogspot.com/2011/09/single-writer-principle.html.
4. https://mechanical-sympathy.blogspot.com/2011/07/memory-barriersfences.html.
5. https://martinfowler.com/bliki/CQRS.html

## Building Stable Systems

**scenarios that affect stability:**
- Unexpected workloads
- Resource and dependency failures, plus service-level agreement violations
- Software bugs in both ours and borrowed code

#### Handling Unexpected Load
- understand the load and set up a system that can keep up with the load most of the time. This is called **capacity planning**
- Autoscaling