---
title: Databases
date: 2024-03-03T07:34:21.2121+05:30
draft: false
tags:
  - database
---


## ZippyDB
ZippyDB is the largest strongly consistent, geographically distributed key-value store at Facebook.ZippyDB uses [RocksDB](https://www.facebook.com/notes/facebook-engineering/under-the-hood-building-and-open-sourcing-rocksdb/10151822347683920/) as the underlying storage engine

### **Cassandra**
[Cassandra](https://cassandra.apache.org/_/index.html) is a wide-column NoSQL database management system. It was originally developed at Facebook to power the Facebook inbox search feature
build in java 
- https://blog.stackademic.com/architecture-of-cassandra-6d9d248a7463

## FoundationDB
[FoundationDB](https://github.com/apple/foundationdb) is an open-source, distributed, transactional key-value store. It’s designed to handle large volumes of data and works well for both read/write workloads and write-heavy workloads. It’s also [ACID-compliant](https://www.swequiz.com/learn/acid-properties).

#### Vitess 
[Vitess](https://vitess.io/) Scalable. Reliable. MySQL-compatible. Cloud-native. Database.used by hubspot,flipkart,

#### Citus
[Citus](https://www.citusdata.com/) gives you the Postgres you love, plus the superpower of distributed tables. 100% open source. Now with schema-based and row-based sharding—plus Postgres 16 support!

## Cockroach DB
SQL distributed database 

## [fauna](https://fauna.com/)
Fauna is a distributed document-relational database that combines the flexibility of documents with the power of a relational, ACID compliant database that scales across regions, clouds or the globe.


## Tools

#### [Ingestr](https://github.com/bruin-data/ingestr) 
ingestr is a CLI tool to copy data between any databases with a single command seamlessly.

#### [Debezium](https://debezium.io/)
Debezium is an open source distributed platform for change data capture. Start it up, point it at your databases, and your apps can start responding to all of the inserts, updates, and deletes that other apps commit to your databases. 

## [Risingwave](https://risingwave.com/)
Scalable Postgres for stream processing, analytics, and management. KsqlDB and Apache Flink alternative. 🚀 10x more productive. 🚀 10x more cost-efficient.

## [JunoDB’s](https://github.com/paypal/junodb)
- JunoDB is a distributed key-value store. highly concurrent architecture implemented in Go to efficiently handle hundreds of thousands of connections.
- JunoDB uses a proxy-based architecture.
- JunoDB uses RocksDB as the storage engine.
- use quorum protocol [[Distributed System#quorum-based protocol]]  for read and write among region


## Triplit
[Triplit](https://www.triplit.dev/) is an open-source database that syncs data between server and browser in real-time.

## Redis

It is single threaded
#### Types of Redis Architecture

The three main types of Redis Architecture are

- Redis Standalone
- Redis Sentinel
- Redis Cluster (Clustering is a way to share data automatically across multiple cluster nodes (Horizontal Scaling). The cluster will be able to continue operations when some nodes fail or not able to communicate with each other.)

####  Redis Sentinel
The Redis Sentinel comes up with a master-slave architecture. With this architecture, we will be able to avoid the Single point of failure which was a major concern with the Redis Standalone. Additionally, it comes up with other sets of features, which are

- **Monitoring** — constantly checking whether the Master and slave instances are working properly or not.
- **Notification** — Notify systems in case of failure of instances.
- **Automatic Failover** — In case of a master node failure, the slave node will be promoted to master.
- **Configuration Provider** — Sentinel nodes also serve as a point of discovery of the current main Redis instance.

Redis server can be run in two modes:
- Master Mode (Redis Master)
- Slave Mode (Redis Slave or Redis Replica)

We can configure which mode to write and read from. It is recommend to serves writes through Redis Master and reads through Redis Slaves.

Redis Master does replicate writes to one or more Redis Slaves. The master-slave replication is done asynchronously.

Redis is **AP** ( Availability and Partition Tolerance.) system. 

What happens when Redis Master receives write request from Client:

- It does acknowledge to Client.
- Redis Master replicates the write request to 1 or more slaves. (Depends on Replication factor).

Here you can see, Redis Master does not wait for replication to be completed on slaves and does acknowledgment to client immediately.

Now lets us assume, Redis Master acknowledged to client and then got crashed. Now one of the Redis Slave (that did not receive the write) will get promoted to Redis Master, loosing the write forever.

In Redis, the automatic promotion of a slave (now called a _replica_) to master upon the master's failure does **not** happen automatically by default. However, you can achieve automatic failover using **Redis Sentinel**, which is designed to monitor your Redis servers and handle automatic promotion when the master fails.

**Redis Persistence Models**
- No persistence
- RDB Files: The RDB persistence performs point-in-time snapshots of your dataset at specified intervals.
- **AOF** (Append Only File):The AOF persistence logs every write operation the server receives that will be played again at server startup, reconstructing the original dataset.

**How redis doing snapshot with single thread**

Redis leverages forking and copy-on-write to enable efficient data persistence. Forking creates a new process (child) that shares memory with the original (parent) process. Redis, which manages large amounts of memory, uses this mechanism to snapshot data without consuming additional memory unless changes are made. Through copy-on-write, memory pages are only duplicated when modified, allowing the child process to work with a consistent snapshot while keeping memory usage low. This approach enables Redis to capture snapshots of gigabytes of memory quickly and efficiently.


- **Atomic Operations:** Every Redis command is atomic, meaning that when a command is executing, other commands cannot interrupt it. This ensures data correctness, even with multiple clients connected to the Redis server.
    - This atomicity is especially beneficial for operations like incrementing values, as it avoids concurrency issues where multiple clients trying to increment a value simultaneously could lead to incorrect results.
- **In-Memory Data Storage:** Redis stores data in memory, making it extremely fast for read and write operations. This is why Redis is often used as a cache.
    - However, Redis also offers configurable persistence options to prevent data loss in case of a crash. These options include:
        - **Periodic Disk Dumping:** Data is periodically written to disk without deleting it from memory. Upon restarting, Redis loads the last dump.
        - **Write-Ahead Logging (AOF):** Every update command is logged to an append-only file, allowing for data reconstruction.
        - **Asynchronous Replication:** Data is replicated to another Redis server.
- **Single-Threaded Event Loop:** Redis utilizes a single-threaded event loop for handling concurrent client requests, unlike multi-threaded approaches commonly used in databases like MySQL and PostgreSQL.
    - **Redis's Approach:** Redis leverages the fact that network I/O operations (like reading data from a socket) are generally slow compared to in-memory operations. It uses IO multiplexing to efficiently monitor multiple sockets and only reads data when it's available, avoiding unnecessary blocking.
        - This approach allows Redis to handle many concurrent connections on a single thread, as it spends minimal time waiting for I/O operations to complete.
    - **Speed and Simplicity:** The single-threaded model, combined with in-memory data storage, makes Redis extremely fast. By avoiding multi-threading complexities, Redis also maintains code simplicity and reduces the risk of concurrency-related bugs.

https://mattermost.com/blog/lessons-learned-running-redis-at-scale/

## StarRocks
[StarRocks](https://www.starrocks.io/) is an open-source, OLAP (_analytics-focused_) database that’s designed for running low-latency queries on data in real-time

## Influx DB
  
InfluxDB is an open-source time-series database designed to handle high write and query loads for time-stamped data it uses column oriented

#### Concepts

Stroage Engine : LevelDB
```
7gy8H2ZmWzcb0YrWhzlbksyB9vkJjdp8mGvj6trn-USBhYFuf8ZMhQbPsI2fHcQoUOKGUA0WMtlKRmW48K04oQ==
```


InfluxDB is schemaless, there is still a conceptual schema that includes:

1. **Measurement**: We'll have a measurement called "environment" to represent the environmental data recorded by the sensors.
2. **Tag sets**: We'll use tags to identify each room. For example, we'll have tags like "room_id" and "building_floor" to uniquely identify each room. Tags are indexed and are useful for filtering and grouping data efficiently.
3. **Field set**: The fields will contain the actual data values recorded by the sensors. We'll have fields for "temperature" and "humidity". Fields are where the data resides and are not indexed like tags.
4. **Timestamp**: Each data point will have a timestamp representing when the data was recorded.

**example**
```
Measurement: environment

Tags: room_id=101, building_floor=1
Fields: temperature=23.5°C, humidity=45%
Timestamp: 2024-05-15T12:00:00Z

Tags: room_id=102, building_floor=1
Fields: temperature=24.0°C, humidity=50%
Timestamp: 2024-05-15T12:00:00Z

Tags: room_id=201, building_floor=2
Fields: temperature=22.0°C, humidity=40%
Timestamp: 2024-05-15T12:00:00Z

INSERT environment,room_id=101,building_floor=1 temperature=23.5,humidity=45 1645660800000000000

```

**Flux**
Flux is a powerful data scripting and query language.It's designed specifically for working with time-series data and is the primary query language for InfluxDB 2.0 and later versions.

Downsampling is a technique used in time-series databases like InfluxDB to reduce the resolution of data by aggregating multiple data points into larger time intervals.

  
InfluxQL is the query language used with InfluxDB versions prior to 2.0. It's specifically designed for querying time-series data stored in InfluxDB

  
In InfluxDB, a "bucket" is a logical container that holds time-series data. It's essentially a storage abstraction used to organize and manage data within the database. Buckets serve as a way to group related data together and define the retention policy for that data.


LSM  delete are expensive





#### Resources
- https://nakabonne.dev/posts/write-tsdb-from-scratch/ 




#### Resources
- https://redis.io/docs/latest/operate/oss_and_stack/management/optimization/benchmarks/
- https://techcommunity.microsoft.com/t5/azure-database-for-postgresql/understanding-partitioning-and-sharding-in-postgres-and-citus/ba-p/3891629


#### Tools
- [The swiss army knife of live data migrations by shopify](https://github.com/Shopify/ghostferry ) [[System Design Case study#[Shopfiy](https //shopify.engineering/horizontally-scaling-the-rails-backend-of-shop-app-with-vitess)]]
- [GitHub's Online Schema-migration Tool for MySQL ](https://github.com/github/gh-ost)
- [Postgres Sharding](https://www.citusdata.com/) 
- [Change Data Capture Tool ](https://debezium.io/)
- https://github.com/mongodb/mongo-snippets
- [Database strees testing tool](https://github.com/adaptive-scale/dbchaos)
- https://www.uber.com/en-IN/blog/schemaless-sql-database


**Sharding** -involves distributing Partitioning data across multiple independent databases or shards. replica are also called sharding because the shard the same data in both server

## Database Partitioning

#### **Vertical Partitioning** 

 - In vertical partitioning, data within a table is divided vertically based on columns in to multiple database.
 - In here we splitting the table in to two based on requirement let say you have user table which has name,email,etc if we split basic user info in single table and other info as another table
#### **Horizontal Partitioning**

- In this we split the data in two database based on shard key let say from user name start with A to F will be in one database and other's in new database


