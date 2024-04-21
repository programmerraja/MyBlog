+++
title = 'Distributed System'
date = 2024-04-06T18:43:35.3535+05:30
draft = true
tags =[]
+++ 


## CRDT
  
CRDT stands for Conflict-Free Replicated Data Type. It's a type of data structure designed for distributed systems where multiple replicas of data exist across different nodes, and these replicas can be modified independently.
[Resources](https://github.com/alangibson/awesome-crdt) 


#### quorum-based protocol 

The quorum-based protocol is the key to reaching a consensus on a value within a distributed database. You’ve two quorums:

- **The Read Quorum**: When a client wants to read data, it needs to receive responses from a certain number of zones (known as the read quorum). This is to make sure that it gets the most up-to-date data.
    
- **The Write Quorum**: When the client wants to write data, it must receive acknowledgment from a certain number of zones to make sure that the data is written to a majority of the zones.




#### Resources

1. [https://www.allthingsdistributed.com/](https://www.allthingsdistributed.com/) [_Werner Vogels on building scalable and robust distributed systems]_

#### Tools to monitor

1. StatsD is originally a simple daemon developed and [released by Etsy](https://codeascraft.com/2011/02/15/measure-anything-measure-everything/) to aggregate and summarize application metrics

Product [sass]

1. [https://dapr.io/](https://dapr.io/)