---
title: PostgreSQL
date: 2024-07-08T06:39:04.044+05:30
draft: true
tags:
  - database
---


## Internal
- everything in append even when we update
- Every connection is seprate process (max 100 connection default) because mostly people used PGBoucer as proxy in front of PostgreSQL
- they go with this approach for fault tolrance so when one process get crashed it wont affect other but in thread it will 
- WAL shared memory used to communicate between process


## Storage

Postgres stores all its data in a directory sensibly called `/var/lib/postgresql/data`


|Directory|Explanation|
|---|---|
|`  base/`|Contains a subdirectory for each database. Inside each sub-directory are the files with the actual data in them. We’ll dig into this more in a second.|
|`  global/`|Directly contains files for cluster-wide tables like `pg_database`.|
|`  pg_commit_ts/`|As the name suggests, contains timestamps for transaction commits. We don’t have any commits or transactions yet, so this is empty.|
|`  pg_dynshmem/`|Postgres uses multiple processes (not multiple threads, although there has been [discussion around it](https://www.postgresql.org/message-id/31cc6df9-53fe-3cd9-af5b-ac0d801163f4%40iki.fi)) so in order to share memory between processes, Postgres has a dynamic shared memory subsystem. This can use [`shm_open`](https://man7.org/linux/man-pages/man3/shm_open.3.html), [`shmget`](https://man7.org/linux/man-pages/man2/shmget.2.html) or [`mmap`](https://man7.org/linux/man-pages/man2/mmap.2.html) on Linux – by default it uses `shm_open`. The shared memory object files are stored in this folder.|
|`  pg_hba.conf`|This is the [Host-Based Authentication (HBA) file](https://www.postgresql.org/docs/current/auth-pg-hba-conf.html) which allows you to configure access to your cluster based on hostname. For instance, by default this file has `host all all 127.0.0.1/32 trust` which means “trust anyone connecting to any database without a password if they’re connecting from localhost”. If you’ve ever wondered why you don’t need to put your password in when running `psql` on the same machine as the server, this is why.|
|`  pg_ident.conf`|This is a [user name mapping file](https://www.postgresql.org/docs/current/auth-username-maps.html) which isn’t particularly interesting for our purposes.|
|`  pg_logical/`|Contains status data for logical decoding. We don’t have time to talk about how the Write-Ahead Log (WAL) works in full, but in short, Postgres writes changes that it’s going to make to the WAL, then if it crashes it can just re-read and re-run all the operations in the WAL to get back to the expected database state. The process of turning the WAL back into the high-level operations – for the purposes of recovery, replication, or auditing – is called logical decoding and Postgres stores files related to this process in here.|
|`  pg_multixact/`|”xact” is what the Postgres calls transactions so this contains status data for multitransactions. Multitransactions are a [thing that happens when you’ve got multiple sessions](https://www.highgo.ca/2020/06/12/transactions-in-postgresql-and-their-mechanism/) who are all trying to do a row-level lock on the same rows.|
|`  pg_notify/`|In Postgres you can [listen for changes on a channel and notify listeners of changes](https://tapoueh.org/blog/2018/07/postgresql-listen-notify/). This is useful if you have an application that wants to action something whenever a particular event happens. For instance, if you have an application that wants to know every time a row is added or updated in a particular table so that it can synchronise with an external system. You can set up a trigger which notifies all the listeners whenever this change occurs. Your application can then listen for that notification and update the external data store however it wants to.|
|`  pg_replslot/`|Replication is the mechanism by which databases can synchronise between multiple running server instances. For instance, if you have some really important data that you don’t want to lose, you could set up a couple of replicas so that if your main database dies and you lose all your data, you can recover from one of the replicas. This can be physical replication (literally copying disk files) and logical replication (basically copying the WAL to all the replicas so that the main database can eb reconstructed from the replica’s WAL via logical decoding.) This folder contains data for the various replication slots, which are a way of ensuring WAL entries are kept for particular replicas even when it’s not needed by the main database.|
|`  pg_serial/`|Contains information on committed serialisable transactions. Serialisable transactions are the highest level of strictness for transaction isolation, which you can read more about [in the docs](https://www.postgresql.org/docs/current/transaction-iso.html).|
|`  pg_snapshots/`|Contains exported snapshots, used e.g. by `pg_dump` which can dump a database in parallel.|
|`  pg_stat/`|Postgres calculates statistics for the various tables which it uses to inform sensible query plans and plan executions. For instance, if the query planner knows it needs to do a sequential scan across a table, it can look at approximately how many rows are in that table to determine how much memory should be allocated. This folder contains permanent statistics files calculated form the tables. Understanding statistics is really important to analysing and fixing poor query performance.|
|`  pg_stat_tmp/`|Similar to `pg_stat/` apart from this folder contains temporary files relating to the statistics that Postgres keeps, not the permanent files.|
|`  pg_subtrans/`|Subtransactions are another kind of transaction, like multitransactions. They’re a way to split a single transaction into multiple smaller subtransactions, and this folder contains status data for them.|
|`  pg_tblspc/`|Contains symbolic references to the different tablespaces. A [tablespace](https://www.postgresql.org/docs/current/manage-ag-tablespaces.html) is a physical location which can be used to store some of the database objects, as configured by the DB administrator. For instance, if you have a really frequently used index, you could use a tablespace to put that index on a super speedy expensive solid state drive while the rest of the table sits on a cheaper, slower disk.|
|`  pg_twophase/`|It’s possible to [“prepare”](https://www.postgresql.org/docs/current/sql-prepare-transaction.html) transactions, which means that the transaction is dissociated from the current session and is stored on disk. This is useful for two-phase commits, where you want to commit changes to multiple systems at the same time and ensure that both transactions either fail and rollback or succeed and commit|
|`  PG_VERSION`|This one’s easy – it’s got a single number in which is the major version of Postgres we’re in, so in this case we’d expect this to have the number `16` in.|
|` pg_wal/`|This is where the Write-Ahead Log (WAL) files are stored.|
|`  pg_xact/`|Contains status data for transaction commits, i.e. metadata logs.|
|`  postgresql.auto.conf`|This contains server configuration parameters, like `postgresql.conf`, but is automatically written to by `alter system` commands, which are SQL commands that you can run to dynamically modify server parameters.|
|`  postgresql.conf`|This file contains all the possible server parameters you can configure for a Postgres instance. This goes all the way from `autovacuum_naptime` to `zero_damaged_pages`. If you want to understand all the possible Postgres server parameters and what they do in human language, I’d highly recommend checking out [postgresqlco.nf](https://postgresqlco.nf/)|
|`  postmaster.opts`|This simple file contains the full CLI command used to invoke Postgres the last time that it was run.|

Source https://drew.silcock.dev/blog/how-postgres-stores-data-on-disk/



## Search

Postgres to create a robust search engine. We’ll combine three techniques:

1. Full-text search with `tsvector`
2. Semantic search with `pgvector`
3. Fuzzy matching with `pg_trgm`
4. Bonus: BM25



## Procedural languages

PostgreSQL supports multiple procedural languages that allow you to write more complex logic and functions directly within the database. These procedural languages expand the capabilities of SQL by allowing you to use control structures like loops, conditionals, and variables,

### 1. PL/pgSQL (Procedural SQL)

- **Description**: PL/pgSQL (Procedural Language/PostgreSQL) is the default and most widely used procedural language in PostgreSQL. It is specifically designed for PostgreSQL and is similar to Oracle’s PL/SQL.
- **Features**:
    - Supports control structures (like loops, `IF` statements).
    - Allows declaring variables and creating complex expressions.
    - Supports exception handling, allowing you to catch and manage errors.
    - Able to work closely with SQL, making it ideal for data-centric applications.
- **Use Cases**:
    - Creating complex stored procedures and triggers that need to perform conditional logic.
    - Optimizing operations that need to be done frequently and should be close to the data.

```sql
CREATE FUNCTION calculate_discount(price NUMERIC, discount NUMERIC) RETURNS NUMERIC AS $$
BEGIN
    RETURN price * (1 - discount);
END;
$$ LANGUAGE plpgsql;

```
### 2. PL/Python

- **Description**: PL/Python allows you to use Python to write functions in PostgreSQL. It’s useful for leveraging Python's libraries and for users familiar with Python.
- **Features**:
    - Enables using Python's extensive library ecosystem for math, data processing, etc., within PostgreSQL.
    - Allows creating custom data processing functions and leveraging Python’s syntax.
    - Includes the ability to work with arrays and other complex data types.
- **Use Cases**:
    - Ideal for advanced data processing, statistical operations, or custom business logic where Python's libraries are helpful.
    - Useful when machine learning models need to be integrated or called from within PostgreSQL.

```sql
CREATE FUNCTION python_greeting(name TEXT) RETURNS TEXT AS $$
return f"Hello, {name}!"
$$ LANGUAGE plpythonu;

```


## Extensions

PostgreSQL has a rich ecosystem of extensions that enhance its functionality.

Exmaple
`pg_stat_statements`:  Tracks execution statistics of all SQL statements executed by the server.

```sql
SELECT * FROM pg_available_extensions;
```



NOTES

Parameter placeholders are a key feature in SQL statements used with database libraries (like `psycopg2` for PostgreSQL in Python) to safely incorporate user input or variable data into SQL queries. They help prevent SQL injection attacks by ensuring that input values are treated as data rather than executable code. Here’s a deeper look into how they work, their syntax, and other available options:

### How Parameter Placeholders Work

1. **Preventing SQL Injection**: 
   - By using placeholders, the database library automatically handles the escaping of special characters, ensuring that user input cannot manipulate the SQL command structure. For example, if a user input includes a single quote, it won't break the SQL syntax.

2. **Syntax**:
   - In Python's `psycopg2` library (for PostgreSQL), the typical syntax for parameter placeholders is:
     - Named placeholders: `%(key)s`
     - Positional placeholders: `%s`

3. **Named Placeholders**:
   - You can use named placeholders (as seen in your example):
     ```python
     cur.execute(
         """
         INSERT INTO documents (title, content)
         VALUES (%(title)s, %(content)s)
         """,
         {'title': 'My Title', 'content': 'Some content here.'}
     )
     ```
   - Here, you pass a dictionary where keys correspond to the placeholders in the SQL statement.

4. **Positional Placeholders**:
   - You can also use positional placeholders, which are represented as `%s`:
     ```python
     cur.execute(
         """
         INSERT INTO documents (title, content)
         VALUES (%s, %s)
         """,
         ('My Title', 'Some content here.')
     )
     ```
   - In this case, the values are passed as a tuple, and their positions in the tuple correspond to the placeholders in the SQL query.

### Other Available Options

1. **Type Casting**:
   - In some databases, you might need to specify the type explicitly. For example, in PostgreSQL, you can cast using `::type`:
     ```sql
     SELECT * FROM users WHERE id = %(id)s::integer
     ```

2. **Array and JSON Types**:
   - If you're working with array or JSON data types, you can also use placeholders:
     ```python
     cur.execute(
         """
         INSERT INTO settings (options)
         VALUES (%(options)s)
         """,
         {'options': [1, 2, 3]}  # Example of an array
     )
     ```

3. **Multiple Statements**:
   - Placeholders can be used across multiple statements within a single `execute` call if they are well-structured:
     ```python
     cur.execute(
         """
         UPDATE users SET last_login = NOW() WHERE id = %(id)s;
         INSERT INTO logs (user_id, action) VALUES (%(id)s, 'login');
         """,
         {'id': user_id}
     )
     ```

### Summary

Parameter placeholders are essential for secure and efficient database interactions. They come in named and positional formats and can be used with various data types. By using placeholders, you ensure that your SQL queries are safe from injection attacks and maintain clean, readable code. This practice is standard across many database libraries and programming languages, promoting best practices in application development.