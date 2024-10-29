---
title : ScyllaDB
date : 2024-07-08T07:51:40.4040+05:30
draft : true
tags : 
---


ScyllaDB is a high-performance, highly available NoSQL database that is compatible with Apache Cassandra. It is designed to handle large-scale, real-time workloads with minimal latency.

#### **Key Concepts**

- **Keyspace**: A namespace that groups tables together, similar to a database in relational systems.
- **Table**: A collection of data organized in rows and columns.
- **Column**: The smallest unit of data storage in a table.
- **Primary Key**: Uniquely identifies a row in a table, consisting of partition key and optional clustering columns.

#### **Basic Commands**

##### **Keyspace Operations**

- **Create Keyspace**
  ```sql
  CREATE KEYSPACE keyspace_name WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 3};
  ```

- **List Keyspaces**
  ```sql
  DESCRIBE KEYSPACES;
  ```

- **Use Keyspace**
  ```sql
  USE keyspace_name;
  ```

- **Drop Keyspace**
  ```sql
  DROP KEYSPACE keyspace_name;
  ```

##### **Table Operations**

- **Create Table**
  ```sql
  CREATE TABLE table_name (
      id UUID PRIMARY KEY,
      column1 text,
      column2 int
  );
  ```

- **List Tables**
  ```sql
  DESCRIBE TABLES;
  ```

- **Describe Table**
  ```sql
  DESCRIBE TABLE table_name;
  ```

- **Drop Table**
  ```sql
  DROP TABLE table_name;
  ```

##### **Data Manipulation**

- **Insert Data**
  ```sql
  INSERT INTO table_name (id, column1, column2) VALUES (uuid(), 'value1', 123);
  ```

- **Select Data**
  ```sql
  SELECT * FROM table_name;
  SELECT column1, column2 FROM table_name WHERE id = uuid_value;
  ```

- **Update Data**
  ```sql
  UPDATE table_name SET column1 = 'new_value' WHERE id = uuid_value;
  ```

- **Delete Data**
  ```sql
  DELETE FROM table_name WHERE id = uuid_value;
  ```

##### **Indexing**

- **Create Index**
  ```sql
  CREATE INDEX index_name ON table_name (column_name);
  ```

- **Drop Index**
  ```sql
  DROP INDEX index_name;
  ```

##### **Batch Operations**

- **Batch Insert/Update/Delete**
  ```sql
  BEGIN BATCH
  INSERT INTO table_name (id, column1, column2) VALUES (uuid(), 'value1', 123);
  UPDATE table_name SET column1 = 'new_value' WHERE id = uuid_value;
  DELETE FROM table_name WHERE id = uuid_value;
  APPLY BATCH;
  ```

#### **Advanced Features**

- **Materialized Views**
  ```sql
  CREATE MATERIALIZED VIEW mv_name AS
  SELECT column1, column2 FROM table_name
  WHERE column1 IS NOT NULL AND column2 IS NOT NULL
  PRIMARY KEY (column1, column2);
  ```

- **User-Defined Types (UDTs)**
  ```sql
  CREATE TYPE address (
      street text,
      city text,
      zip_code int
  );

  CREATE TABLE users (
      id UUID PRIMARY KEY,
      name text,
      address address
  );
  ```

- **User-Defined Functions (UDFs)**
  ```sql
  CREATE FUNCTION keyspace_name.function_name (arg1 int, arg2 int)
  RETURNS int LANGUAGE java AS 'return arg1 + arg2;';
  ```

- **User-Defined Aggregates (UDAs)**
  ```sql
  CREATE AGGREGATE keyspace_name.aggregate_name (int)
  SFUNC sumState STYPE int INITCOND 0
  FINALFUNC finalFunc;
  ```



## Internals
- All nodes are read and write 


Write will be written in commit log like journal in mongodb and it using memtable (lsm tree)


built with c++ seastar framework follows shard per core architecture

schedulling groups  
Io schduler -> build there own IO  schduler
custom cache instead of linux
leader less architecure

each shard has independent scheduler 

casandra cannot specify timeout for per query

in sycalldb we can control does we need to put this data in caching or not




You was mongodb architecture i want to you to help me solve the problem my problem is i have a mongodb database with 16Gb Ram and 2Tb of storage and 4 core processor i want to calulate how much spec ram,cpu and storage used by the cuurent mongodb how i can calculate this and i want to bring new collection from different database to this database how can i caluculation that this current resources is good enough for the new collection and how i can find this


**ScyllaDB Overview**

- ScyllaDB is compatible with Apache Cassandra and DynamoDB
- It supports multiple consistency models, including eventual consistency and lightweight transactions
- It is designed for multi-terabyte or petabyte workloads and high-performance applications

**Architecture**

- ScyllaDB has a symmetric architecture, with each node divided into two layers: coordination and storage
- Each node has a full connection mesh, allowing for efficient communication between nodes
- The database uses a thread-per-core architecture, with each thread having its own scheduler

**Performance**

- ScyllaDB is designed to be highly efficient, with a focus on minimizing coordination and locking overhead
- It uses asynchronous I/O and networking to improve performance
- The database is self-tuning, with a feedback loop that adjusts the scheduler to optimize performance

**Concurrency**

- ScyllaDB is designed to handle high concurrency, with a focus on minimizing latency
- It uses a scheduling algorithm to manage concurrency and prioritize requests
- The database tracks metadata about each request, including the originating process and priority

**I/O and Networking**

- ScyllaDB uses Linux AIO and epoll for I/O and networking
- It has a custom TCP/IP stack, but it is not currently used due to deployment issues
- The database is designed to work with iouring, but it is not currently supported

**Cooperative Scheduling**

- ScyllaDB uses a cooperative scheduling model, where tasks yield control back to the scheduler voluntarily
- This approach allows for efficient scheduling and minimizes latency
- The database uses a preemption timer to ensure that tasks do not run for too long without yielding control



there is primary seconday nodes all are master when the write request send to one node it will replicate quorum 

partion key -> convert to number
cluster key
Tablets