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

## FoundationDB
[FoundationDB](https://github.com/apple/foundationdb) is an open-source, distributed, transactional key-value store. It’s designed to handle large volumes of data and works well for both read/write workloads and write-heavy workloads. It’s also [ACID-compliant](https://www.swequiz.com/learn/acid-properties).

#### Vitess 
[Vitess](https://vitess.io/) Scalable. Reliable. MySQL-compatible. Cloud-native. Database.used by hubspot,flipkart,

#### Citus
[Citus](https://www.citusdata.com/) gives you the Postgres you love, plus the superpower of distributed tables. 100% open source. Now with schema-based and row-based sharding—plus Postgres 16 support!



## Tools

#### [Ingestr](https://github.com/bruin-data/ingestr) 
ingestr is a CLI tool to copy data between any databases with a single command seamlessly.

#### [Debezium](https://debezium.io/)
Debezium is an open source distributed platform for change data capture. Start it up, point it at your databases, and your apps can start responding to all of the inserts, updates, and deletes that other apps commit to your databases. 




