+++
title = 'Databases'
date = 2024-03-03T07:34:21.2121+05:30
draft = false
tags =[]
+++ 


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
- use quorum protocol[[Distributed System#quorum-based protocol]]  for read and write among region

## Redis


#### Resources
- https://redis.io/docs/latest/operate/oss_and_stack/management/optimization/benchmarks/
- https://techcommunity.microsoft.com/t5/azure-database-for-postgresql/understanding-partitioning-and-sharding-in-postgres-and-citus/ba-p/3891629


#### Tools
- [The swiss army knife of live data migrations by shopify](https://github.com/Shopify/ghostferry ) [[System Design Case study#[Shopfiy](https //shopify.engineering/horizontally-scaling-the-rails-backend-of-shop-app-with-vitess)]]
- [GitHub's Online Schema-migration Tool for MySQL ](https://github.com/github/gh-ost)
- [Postgres Sharding](https://www.citusdata.com/) 
- [Change Data Capture Tool ](https://debezium.io/)


**Sharding** -involves distributing Partitioning data across multiple independent databases or shards. replica are also called sharding because the shard the same data in both server

## Database Partitioning

#### **Vertical Partitioning** 

 - In vertical partitioning, data within a table is divided vertically based on columns in to multiple database.
 - In here we splitting the table in to two based on requirement let say you have user table which has name,email,etc if we split basic user info in single table and other info as another table
#### **Horizontal Partitioning**

- In this we split the data in two database based on shard key let say from user name start with A to F will be in one database and other's in new database
 