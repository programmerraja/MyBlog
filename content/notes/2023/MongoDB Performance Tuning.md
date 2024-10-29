---
title: MongoDB Performance Tuning Book Notes
date: 2023-12-03T09:44:50+05:30
tags:
  - mongodb
  - database
  - book
---

## MongoDB Architecture and Concepts

if the **write concern is set to “majority”,** then the database will not complete the write operation until a majority of secondaries receive the write. We can also set the write concern to wait until all secondaries or a specific number of secondaries receive the write operation.

 **Oplog** The primary node stores information about document changes in a collection within its local database called the Oplog. The primary will continuously attempt to apply these changes to secondary instances.

## Tools of the Trade
 
 In order to obtain the execution statistics, explain("executionStats") will fully execute the MongoDB statement concerned. This means that it may take much longer to complete than a simple explain() and place significant load on the MongoDB server.

 **ExcutionStats** Takes more time compare to explain().

## Profiling
 
 Profiling information in MongoDB is stored within the `system.profile` collection. This collection follows a circular structure, meaning it has a fixed size. When this size is surpassed, older entries are automatically removed to accommodate new ones. The default size for `system.profile` is set to 1MB, but it might be advisable to consider increasing it based on your needs. To achieve this, one can halt profiling, drop the existing collection, and recreate it with an expanded size.

Adjusting the profiling level is facilitated through the `db.setProfilingLevel(level, {slowms: slowMsThreshold, sampleRate: samplingRate})` command. The `level` parameter can be set to:

	- 0 to disable profiling.
	- 1 to collect only queries surpassing a specified time threshold (`slowms`).
	- 2 to collect all queries indiscriminately.

 The `samplingRate` parameter, which determines the random sampling level, is a crucial aspect. For instance, if set to 0.5, approximately half of all statements will undergo profiling, providing a representative sample. 

This feature is instrumental in managing the trade-off between profiling accuracy and performance overhead. Adjusting these parameters enables a fine-tuned control over the profiling process, catering to specific optimization and analysis requirements.

#### Server Status

The db.serverStatus() command is a quick and powerful way to get a lot of high-level information about your MongoDB server [DOC](https://www.mongodb.com/docs/manual/reference/command/serverStatus/#std-label-server-status-output) will return all server detail example how read and updated happens and so many thing how many connections are active

 However, there are two problems with using db.serverStatus() in this way. Firstly,
these counters don’t tell us much about what’s happening on the server right now,
making it difficult to identify which of the metrics may be impacting the performance of
our application
#### Examining Current Operations

Another useful tool when tuning performance in MongoDB is the `db.currentOp()`
command. This command works as you might imagine – it returns information about
operations that are currently running on the database
## Indexing

Index scans are not always a good thing. If the range is extensive, then the index scan might be worse than not using an index at all.

 Index is not suitable if we going to read all docs but mongo use index u can specify to use col scan hint({$natural:1}),

 To fully optimize an $or query, index all the attributes in the $or array.

 An $exists:true lookup can be optimized by a sparse index on the attribute concerned. However, such an index cannot optimize an $exists:false query.

 If you want to do case-insensitive searches, then there is a trick you can use. First, create an index with a case-insensitive collation sequence. This is done by specifying a collation sequence with a strength `db.customers.createIndex({ LastName: 1 },{ collation: { locale: 'en', strength: 2 } });`	

#### Index Merges

if we have 2 index one for a and b if search of a and b mongodb will merges the index value on search

```plaintext
1  IXSCAN a_1
2  IXSCAN b_1
3  AND_SORTED
4  FETCH
```

The **AND_SORTED** step indicates that an index intersection has been performed.
Index intersections for $and conditions are unusual

But this not efficient as compound index and very rarely this will be used this approach need to scan more doc compared to compound index
#### Type of index

1. **Compound**: A compound index is simply an index comprising more than one attribute. The most significant advantage of a compound index is that it is usually more selective than a single key index. The combination of multiple attributes will point to a smaller number of documents than indexes composed of singular attributes. A compound index that contains all of the attributes contained within the find() or $match clauses will be particularly effective  `createIndex({key1:1,key2:1})`

2. **Partial**:  A partial index is one which is only maintained for a subset of information `createIndex({key1:{$exists:true}})`

3. **Sparse**:  sparse index doesn’t include documents that don’t contain the indexed attributes. a sparse index cannot support an **$exists:true** search on the indexed attribute: By contrast, non-sparse indexes contain all documents in a collection, storing null values for those documents that do not contain the indexed field. `createIndex( { "key": 1 }, { sparse: true } )`

5. **Geospatial Indexes**: Typically need to perform searches across map data

6. **Text Index**: MongoDB uses a method called suffix stemming to build a search index. Suffix stemming involves finding a common element (prefix) at the start of each word which forms the root of a search tree. Each divergent suffix “stems” off into its own node that may stem further. This process creates a tree that can be efficiently searched from the root (the most common shared element) down to the leaf node, with the path from the root to the leaf node forming a complete word. 

	For example, suppose we have the words “**finder**,” “**finding**,” and “**findable**” somewhere in our documents. 
	By using suffix stemming, we could find a common root in these words of “**find**,” and then the suffixes stemming from this term would be “**er**,” “**ing**,” and “**able**.” `createIndex({description: "text"})`

#### Finding Unused Indexes

 we can take a look at index utilization by using the `$indexStats` aggregation command:
```js
{
"name": "test",
"key": {
	"key": 1
},
"host": "xxx:27017",
"accesses": {
	"ops": NumberLong(145),
	"since": ISODate("2017-11-25T16:21:36.285Z")
 }
}
```
- **OPS** :This is the number of operations that used the index
- **Since** : This is the time from which MongoDB gathered stats, and is typically the last start time.
## Query Tuning
####  Optimizing Network Round Trips

1. Projections
2. Batch Processing
3. Bulk Inserts

**Batch Processing**

When retrieving data using a cursor, you can specify the number of rows fetched
in each operation using the batchSize clause. For instance, in the following we have a
cursor where the variable batchSize controls the number of documents retrieved from
the MongoDB database in each network request:

```js
db.millions.find({},{n:1,_id:0}).batchSize(batchsize);
```

#### Overriding the Optimizer with Hints

We can change force this query to use a collection scan by adding a hint. If we
append .`hint({$natural:1})`, we instruct MongoDB to perform a collection scan to
resolve the query:

#### Optimizing Sort Operations

**Blocking sort**

The sorting that doing without using any index is called blocking sort because the it need fetch all doc to memory and need to do sorting and return the doc to the user

if you are going to to do a blocking sort on a large amount of data, you may need to
allocate more memory for the sort. You can do this by adjusting the internal parameter
`internalQueryExecMaxBlockingSortBytes`.

```js
db.getSiblingDB("admin").runCommand(
{ setParameter: 1, 
 internalQueryExecMaxBlockingSortBytes:1001048576 
});
```

___
**Hint**: Beware of index-supported **$ne** queries. They resolve to multiple index
range scans which might be less effective than a collection scan.
___
## Tuning AggregationPipelines

The method `mongoTuning.aggregationExecutionStats()` will provide a top-level summary of the time taken byeach step.

```js
var exp = db.customers.explain('executionStats').aggregate(query)
mongoTuning.aggregationExecutionStats(exp)
```

#### Optimizing Aggregation Ordering

MongoDB will automatically resequence the order of operations in a pipeline to optimize performance. one such case where automatic reordering is not possible is aggregations using `$lookup`. The $lookup stage allows you to join two collections

#### Automatic Pipeline Optimizations

- $sort and $limit stages will be merged, allowing the $sort to only maintain the limited number of documents instead of its entire input.
- If your aggregation only requires a subset of document attributes,MongoDB may add a projection to remove all unused fields.when we use group mongodb will add projection before the group stage

#### Indexed Aggregation Sorts

- `$sort` can be utilized the index only if it in early enough the pipeline to be rolled into the initial data access operation.  
```js
aggregate([
{ $sort:{  d:1 }},
{$addFields:{x:0}}
],
{allowDiskUse: true}
);
```
- The above query will use the index but if we swap the order it won't use index because When MongoDB executes `$addFields`, it applies the specified expression to each document in the aggregation pipeline, introducing a modification to the document structure. This modification, in turn, may invalidate certain optimization opportunities associated with the index.
- When we first add the fields to the doc and if we do sorting we cannot use index because the when we index lookup and sort it we again need lookup on disk to get the document which will not have the newly added field.

## Inserts, Updates, and Deletes

Indexes always add to the overhead of insert and delete statements and may add to the overhead of update statements. Avoid over-indexing, especially on columns which are frequently updated.

 **Write Concern** (waiting for replicas to write/update happen):  Adjusting writeConcern can improve performance, but it may come at the expense of data integrity or safety. Don't adjust `writeConcern` to improve performance unless you are fully aware of these trade-offs.

 MongoDB 4.2, we have the ability to embed aggregation framework pipelines within an update statement. These pipelines allow us to set a value that is derived from, or dependent on, other values in the document. For instance, we could populate the viewCount attribute with this single query
```js
db.collection.update({},[{ $set: { viewCount: { $size: '$views' } } }])
```

## Server Monitoring

- **Host-Level Monitoring**
	- Top, uptime,vmstat,netstat,etc..
- **Network**
	- **bwn-ng** -> You can monitor the amount of data transferring 
	- **traceroute** mongors03.koreacentral.cloudapp.azure.com --port=27017 -T -> to find how many miilisec it take to reach  make sure it is less then 100ms
- **Memory**
	- vmstat -s : print active, inactive ,swap memory etc..
	- iostat : for I/O

## Memory Tuning

#### MongoDB memory architecture

It uses WiredTiger storage engine.Each connection uses up to 1 megabyte of RAM.By default, WiredTiger uses Snappy block compression for all collections and prefix compression for all indexes and  we can have up to 100 open connections in the pool

The WiredTiger storage engine maintains lists of empty records in data files as it deletes documents. This space can be reused by WiredTiger, but will not be returned to the operating system unless under very specific circumstances.

The amount of empty space available for reuse by WiredTiger is reflected in the output of [db.collection.stats()](https://www.mongodb.com/docs/manual/reference/method/db.collection.stats/#mongodb-method-db.collection.stats) under the heading `wiredTiger.block-manager.file bytes available for reuse`.

 To allow the WiredTiger storage engine to release this empty space to the operating system, you can de-fragment your data file. This can be achieved using the [compact](https://www.mongodb.com/docs/manual/reference/command/compact/#mongodb-dbcommand-dbcmd.compact) command. For more information on its behavior and other considerations, see [compact](https://www.mongodb.com/docs/manual/reference/command/compact/#mongodb-dbcommand-dbcmd.compact)[.](https://www.mongodb.com/docs/manual/reference/command/compact/#mongodb-dbcommand-dbcmd.compact)``
 
The `db.serverStatus()` command provides details of how much memory MongoDB is using. The following script prints out a top-level summary of memory utilization.

```js
let serverStats = db.serverStatus();
print('Mongod virtual memory ', serverStats.mem.virtual);
print('Mongod resident memory', serverStats.mem.resident);
print('WiredTiger cache size',
...     Math.round(
...       serverStats.wiredTiger.cache
             ['bytes currently in the cache'] / 1048576
...     )
...   );
```

**Virtual Memory** 
 - This value includes all memory that the MongoDB process can access, including both physical RAM and swap space.
  - Virtual memory represents the total memory space allocated by the operating system to the process. It includes not only the resident memory but also any memory that may have been swapped out to disk or is reserved but not currently in use.
  - Virtual memory can be much larger than physical memory, as it represents the total address space allocated to the process.
  
 **Resident Memory** 
 - is the amount of physical RAM, in mebibytes (MiB), that the MongoDB process is currently using.
  
  **Note :** The Wired Tiger cache will be set to either 50% of total memory minus 1GB or to 256MB

#### The Database Cache "Hit" Ratio

The database cache hit ratio is a somewhat notorious metric with a long history.Simplistically, the cache hit ratio describes how often you find a block of data you want in memory:

 `CacheHitRatio = Number of IO requests that were satisfied in the cache / Total IO requests`

 The cache hit ratio represents the proportion of block requests that are satisfied by the database cache without requiring a disk read. Each “hit” – when the block is found in memory – is a good thing, since it avoids a time-consuming disk IO.
 
  Script to calculate 
```js
var cache=db.serverStatus().wiredTiger.cache;
var missRatio=cache['pages read into cache']*100/cache['pages requested from the cache'];
var hitRatio=100-missRatio;
print(hitRatio);
```

It will return the cache hit since server started.To calculate for specfic peroid of the time use mongo tunning `mongoTuning.monitorServerDerived(5000,/cacheHitRate/)` 

#### Evictions (putting data to disk from cache)

MongoDB doesn’t wait until the cache is completely full before performing evictions.By default, MongoDB will try and keep 20% of the cache free for new data and will startto restrict new pages from coming into cache when the free percentage hits 5%

MongoDB tries to keep the percentage of modified – “**dirty**” – blocks under 5%. If the percentage of modified blocks hits 20%, then operations will be blocked until the target value is achieved.

 When the number of clean blocks or dirty blocks hits the higher threshold values, then sessions that try to bring new blocks into the cache will be required to perform an eviction before the read operation can complete

 To get total blocking evictions so far `db.serverStatus().wiredTiger["thread-yield"]["page acquire eviction blocked"]

 To get ratio
```js
var wt=db.serverStatus().wiredTiger;
var blockingEvictRate=wt['thread-yield']['page acquire eviction blocked'] *100 / wt['cache']['eviction server evicting pages'];
```


- **wiredTiger.cache("application threads page read from disk to cache count")**: This records the number of reads from disk into the Wired Tiger cache.
- **application threads page read from disk to cache time (usecs)**: This records the number of microseconds spent moving data from disk to cache
- If we divide the count/time we get average time to read a page into the cache it must need between 1ms to 2ms
- **application threads page write from cache to disk count**: The number of writes from the cache to disk
- **application threads page write from cache to disk time (usecs)**: The time spent writing from cache to disk

#### Checkpoints

When ever data updated the data won't get updated in disk it will updated in cache first then only it will be updated bulk to disk to avoid **I/O**
 
 Mongodb periodically ensures that the data files are synchronized with the changes in the cache. These checkpoints involve writing out the modified “**dirty**” blocks to disk. By default, checkpoints occur every 60 sec.

 The **eviction_dirty_trigger** and **eviction_dirty_target settings** –  control how many modified blocks are allowed in the cache before eviction processing kicks in. These can be adjusted to reduce the number of modified blocks in the cache, reducing the amount of data that must be written to disk during a checkpoint.

## Disk IO
#### Latency and Throughput

**Latency** describes the time it takes to retrieve a single item of information from the disk
 
 **Throughput** describes the number of IOs that can be performed by the disk devices in a given unit of time. Throughput is generally expressed in terms of IO operations per second, often abbreviated as IOPS.
 
 `TIP:` The throughput of an IO system is primarily determined by the number of physical disk devices it contains. To increase IO throughput, increase the number of physical disks in disk volumes.
 
#### MongoDB IO

MongoDB performs three major types of IO operations:
1. `Temporary file IO` involves reads and writes to the “_tmp” directory within the dbPath directory. These IOs occur when a disk sort or disk-­based aggregation operation occurs

2.  `Datafile IO` occurs when WiredTiger reads from and writes to collection and index files in the db Path directory

3. `Journal file IO ` occurs as the WiredTiger storage engine writes to the“write-ahead” journal file.
	- db.serverStatus().wiredTiger.log
		- **log bytes written**: The amount of data written to the journal. 
		- **log sync operations**: The number of log “sync” operations. A sync occurs when journal information held in memory is flushed to disk. 
		- **log sync time duration** (μsecs): The number of microseconds spent in sync operations.

## Replica Sets

A replica set following best practice consists of a primary node together with two or more secondary nodes. It is recommended to use three or more nodes, with an odd number of total nodes. The primary node accepts all write requests which are propagated synchronously or asynchronously to the secondary nodes. In the event of a failure of a primary, an election occurs to which a secondary node is elected to serve as the new primary and database operations can continue

 **Read Preference** → By default, all reads are directed to the primary node. However, we can set a read preference which directs the MongoDB drivers to direct read requests to secondary nodes.

|                    | Read Preference                                                                                        |
| ------------------ | ------------------------------------------------------------------------------------------------------ |
| primary            | This is the default. All reads are directed to the replica set primary.                                |
| primaryPreferred   | Direct reads to the primary, but if no primary is available, direct reads to a secondary.              |
| secondary          | Direct reads to a secondary.                                                                           |
| secondaryPreferred | Direct reads to a secondary, but if no secondary is available, direct reads to the primary.            |
| nearest            | Direct reads to the replica set member with the lowest network round trip time to the calling program. |

**maxStalenessSeconds** : can be added to a read preference to control the tolerable lag in data. When picking a secondary node, the MongoDB driver will only consider those nodes who have data within `maxStalenessSeconds` seconds of the primary. The minimum value is 90 seconds. For instance, this URL specified a preference for secondary nodes, but only if their data timestamps are within 5 minutes (300 seconds) of the primary:

 **Replica Set Tag Sets**: Tag sets can be used to direct read requests to specific nodes. You can use tag sets to nominate nodes for special purposes such as analytics or to distribute read workload more evenly across all nodes in the cluster.

## Sharding
- https://www.percona.com/blog/when-should-i-enable-mongodb-sharding/
- Need to study :)


