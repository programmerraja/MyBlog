---
title : Distributed System
date : 2024-04-06T18:43:35.3535+05:30
draft : true
tags : 
---


## CRDT
  
CRDT stands for Conflict-Free Replicated Data Type. It's a type of data structure designed for distributed systems where multiple replicas of data exist across different nodes, and these replicas can be modified independently.
[Resources](https://github.com/alangibson/awesome-crdt) 


#### quorum-based protocol 

The quorum-based protocol is the key to reaching a consensus on a value within a distributed database. You’ve two quorums:

- **The Read Quorum**: When a client wants to read data, it needs to receive responses from a certain number of zones (known as the read quorum). This is to make sure that it gets the most up-to-date data.
    
- **The Write Quorum**: When the client wants to write data, it must receive acknowledgment from a certain number of zones to make sure that the data is written to a majority of the zones.


## Raft Consensus Protocol

Raft is a consensus protocol designed for distributed systems, particularly for achieving consensus in a fault-tolerant manner. It's widely used in modern distributed systems, including cloud-native applications, databases, and file systems.

**What is Consensus?**

In a distributed system, consensus refers to the process of agreeing on a single value or state among multiple nodes or replicas. This is crucial in ensuring data consistency and availability, even in the presence of failures or network partitions.

**Raft Consensus Protocol:**

Raft is a consensus protocol developed by Diego Ongaro and John Ousterhout in 2013. It's designed to be more understandable, scalable, and fault-tolerant than its predecessors, such as Paxos.

Raft works by electing a leader node, which is responsible for managing the replication of data across the cluster. The protocol ensures that the leader node is always up-to-date and that all nodes agree on the same state.

**Key Components:**

1. **Leader Node**: The leader node is responsible for managing the replication of data and ensuring consistency across the cluster.
2. **Follower Nodes**: Follower nodes replicate the data from the leader node and participate in the consensus process.
3. **Term**: A term is a period of time during which a leader node is elected and serves the cluster.
4. **Log**: The log is a sequence of entries that represent the state of the system. Each entry is assigned a unique index and term.

**Raft Protocol Steps:**

1. **Leader Election**: Nodes in the cluster vote for a leader node. The node with the most votes becomes the leader.
2. **Log Replication**: The leader node replicates its log to the follower nodes.
3. **Heartbeats**: The leader node sends periodic heartbeats to the follower nodes to maintain its leadership.
4. **Log Consistency**: Follower nodes verify the consistency of their logs with the leader node's log.
5. **Conflict Resolution**: In case of conflicts, the leader node resolves them by rolling back the conflicting entries and re-applying the correct ones.

**Raft's Advantages:**

1. **Fault Tolerance**: Raft can tolerate up to (n-1)/2 node failures, where n is the total number of nodes in the cluster.
2. **Scalability**: Raft is designed to scale horizontally, making it suitable for large distributed systems.
3. **Understandability**: Raft's design is more intuitive and easier to understand than other consensus protocols.

#### Resources

1. [https://www.allthingsdistributed.com/](https://www.allthingsdistributed.com/) [_Werner Vogels on building scalable and robust distributed systems]
2. https://muratbuffalo.blogspot.com/2020/06/learning-about-distributed-systems.html _
3. https://www.somethingsimilar.com/2013/01/14/notes-on-distributed-systems-for-young-bloods/

#### Tools to monitor

1. StatsD is originally a simple daemon developed and [released by Etsy](https://codeascraft.com/2011/02/15/measure-anything-measure-everything/) to aggregate and summarize application metrics

Product [sass]

1. [https://dapr.io/](https://dapr.io/)