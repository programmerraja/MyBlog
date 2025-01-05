---
title : Appache Kaffa
date : 2024-02-25T12:50:53.5353+05:30
draft : true
tags : 
---

internal archi course
https://developer.confluent.io/courses/architecture/get-started/


### Kafka's Zero-Copy Optimization: Simplified

If you've come across Kafka, you might have heard about its zero-copy optimization, a technique aimed at reducing unnecessary data copies. Let's break it down:

**What is Zero-Copy?**
Zero-copy operations minimize unnecessary data duplication, although they don't literally make zero copies.

**Kafka's Use of Zero-Copy**
Kafka leverages the OS's zero-copy optimization to bypass the Kafka broker Java program entirely when data is transferred from the page cache to the socket buffer.

### Traditional Data Transfer (Without Zero-Copy)

1. **Read buffer (OS page cache)** - Stores data for quick access.
2. **Socket buffer** - Manages data packets.
3. **NIC buffer** - Network card's byte buffer.
4. **DMA (Direct Memory Access)** - Allows hardware to access memory without the CPU.

#### Steps:
1. Disk to OS buffer (DMA copy, user to kernel mode).
2. OS buffer to app buffer (kernel to user mode).
3. App buffer to socket buffer (user to kernel mode).
4. Socket buffer to NIC buffer (DMA copy, kernel to user mode).

### Optimized Data Transfer (With Zero-Copy)

Kafka stores data in a binary format compatible with its responses, skipping unnecessary steps:
- The read buffer copies data directly to the NIC buffer.
- The socket buffer stores read buffer pointers, enabling the DMA engine to read directly from memory.

### Benefits of Zero-Copy
- Fewer user/kernel mode switches (reduced from 4 to 2).
- Same number of DMA copies (2).
- One minimal CPU copy of pointers (2 fewer CPU copies).

### The Reality Check

Despite the efficiency gains, zero-copy might not significantly impact most Kafka deployments:
- The network often saturates before CPU becomes a bottleneck.
- Encryption and SSL/TLS prevent zero-copy use.

**Kafka remains performant even without zero-copy optimization.**



1. What is Kafka?  
    Kafka is a distributed event store and a streaming platform. It began as an internal project at LinkedIn and now powers some of the largest data pipelines in the world in orgs like Netflix, Uber, etc.  
    
2. Kafka Messages  
    Message is the basic unit of data in Kafka. It’s like a record in a table consisting of headers, key, and value.  
    
3. Kafka Topics and Partitions  
    Every message goes to a particular Topic. Think of the topic as a folder on your computer. Topics also have multiple partitions.  
    
4. Advantages of Kafka  
    Kafka can handle multiple producers and consumers, while providing disk-based data retention and high scalability.  
    
5. Kafka Producer  
    Producers in Kafka create new messages, batch them, and send them to a Kafka topic. They also take care of balancing messages across different partitions.  
    
6. Kafka Consumer  
    Kafka consumers work together as a consumer group to read messages from the broker.  
    
7. Kafka Cluster  
    A Kafka cluster consists of several brokers where each partition is replicated across multiple brokers to ensure high availability and redundancy.  
    
8. Use Cases of Kafka  
    Kafka can be used for log analysis, data streaming, change data capture, and system monitoring.


## Resources
- https://towardsdev.com/kafka-101-a-beginners-guide-to-understanding-kafka-2cd797864614