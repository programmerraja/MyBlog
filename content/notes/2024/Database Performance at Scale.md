---
title : Database Performance at Scale
date : 2024-03-18T08:27:02.022+05:30
draft : true
tags :  
    - 'books'
---

Repo ­ https://github.com/Apress/db-performance-at-scale.

https://rust-lang.github.io/mdBook/  mdBook “is a command line tool to create books with Markdown.


## Your Project, Through the Lens of Database Performance

#### Write-Heavy Workloads

Write-heavy workload,strongly recommend a database that stores data in immutable files (e.g., Cassandra, ScyllaDB, and others that use LSM trees). These databases optimize write speed because)Database Performance a writes are sequential, which is faster in terms of disk I/O ) writes are performed immediately, without first worrying about reading or updating existing values (like databases that rely on B trees do). As a result, you can typically write a lot of data with very low latencies.

`Compaction is a background process that databases with an LSM tree storage backend use to merge and optimize the shape of the data. Since files are immutable, the process essentially involves picking up two or more pre-existing files, merging their contents, and producing a sorted output file`

**Note:** Writes cost around five times more than reads under some vendors’ pricing models

#### Read-Heavy Workloads

B-tree databases (such as DynamoDB) are optimized for reads

cold data vs hot data

cold data: data that not acessing frequently
hot data: data that are accessing frequently

If your ratio of cache misses is higher than hits, this means that reads need to
frequently hit the disks in order to look up your data

Competing Workloads (Real-Time vs Batch) -> need to look again page no 21

#### Item Size

The size of each document is matter let assume the default page cache size is 4kb the document size is 1MB it cannot be stored in cache it need more I/O 
but large number of doc with small size may introduce CPU overhead

Most write-optimized databases will store your writes in memory before persisting that information to the disk (in fact, that’s one of the reasons why they are write-optimized). Larger payloads deplete the available cache space more frequently, and this incurs a higher flushing activity to persist the information on disk in order to release space for more incoming writes. Therefore, more disk I/O is needed to persist that information. If you don’t size this properly, it can become a bottleneck throughout this repetitive process

#### Item Type

item type has a large impact on compression.if you need to frequently process JSON data that you can’t easily transform, a document database like `MongoDB might be a better option than a Cassandra-compatible database.`

Some databases also support user created fields, such as` User-Defined Types `(UDTs) in Cassandra. UDTs can be a great ally for reducing the de-serialization overhead when you combine several columns into one.



https://raymondjones.dev/en/system-design-notes/