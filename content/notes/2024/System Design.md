

 problems in a distributed system 
- maintaining the system state (liveness of nodes)
- communication between nodes
solutions
- centralized state management service (Apache Zookeeper)
- peer-to-peer state management service  (gossip protocol)

Gossip Protocol 
every node periodically sends out a message to a subset of other random nodes. The entire system will receive the particular message eventually with a high probability. In layman’s terms, the gossip protocol is a technique for nodes to build a global map through limited local interactions