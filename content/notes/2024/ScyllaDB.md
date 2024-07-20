+++
title = 'ScyllaDB'
date = 2024-07-08T07:51:40.4040+05:30
draft = true
tags =[]
+++ 


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

