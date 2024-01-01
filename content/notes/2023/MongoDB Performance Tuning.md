+++
title = 'MongoDB Performance Tuning Book Notes'
date = 2023-12-03T09:44:50+05:30
draft = false
+++

  1. if the `write concern is set to “majority”,` then the database

will not complete the write operation until a majority of secondaries

receive the write. We can also set the write concern to wait until all

secondaries or a specific number of secondaries receive the write

operation.

  

2. `Oplog` →The primary node stores information about document changes in a collection within

its local database called the Oplog. The primary will continuously attempt to apply these

changes to secondary instances.

<details>

<summary> Oplog Collection contain for update</summary>

  

{

"ts": Timestamp(1560494765, 2), // Timestamp of the operation

"t": NumberLong(1), // Term

"h": NumberLong("9876543210987654321"), // Hash

"v": NumberInt(2), // Version

"op": "u", // Operation type (update)

"ns": "mydb.mycollection", // Namespace (database.collection)

"ui": UUID("12345678-1234-1234-1234-1234567890AB"), // User ID

"wall": ISODate("2019-06-14T11:25:30.456Z"), // Wall clock time

"o2": {

"_id": ObjectId("5d02b016f3e0eab1c3fcd741") // Unique identifier of the document being updated

},

"o": {

"$set": {

"age": 35

},

"$unset": {

"email": ""

}

}

}

  

</details>


3. ExcutionStats → takes more time compare to explain() and take a load high

4. # Profiler

Setting Profiling Level

```javascript

db.setProfilingLevel(

level, {

slowms: slowMsThreshold,

sampleRate: samplingRate

});

```

  

- `samplingRate` can be:

1. `0`: Disabled

2. `1`: Collects only queries taking more time than `slowms`

3. `2`: Collects all queries

  

# Sampling Rate

- Sampling rate determines a random sampling level. For example, if the sampling rate is set to 0.5, then half of all statements will be traced.

# Profiling Information Retrieval

- `db.getProfilingStatus()`

- Use this to retrieve profiling information stored in the `system.profile` collection.

- `system.profile` is a circular collection. When the collection reaches its fixed size, older entries are removed to accommodate new ones.

- The default size for `system.profile` is only 1MB.

  

5. ### MongoDB Indexing Guidelines

  

1. **Index Efficiency Consideration**:

- Index scans aren't always advantageous. For extensive ranges, an index scan might perform worse than not using an index at all.

  

2. **Selective Index Usage**:

- Using an index may not be suitable if all documents need to be read. However, MongoDB allows specifying a collection scan (`hint({$natural:1})`) to bypass index usage when necessary.

  

3. **Optimizing $or Queries**:

- For optimal performance in an $or query, it's recommended to index all attributes present in the $or array.

  

4. **$exists Optimization**:

- Optimization for `$exists:true` queries can be achieved through a sparse index on the concerned attribute. However, the same optimization doesn't apply for `$exists:false` queries.

  

5. **Case-Insensitive Searches**:

- To conduct case-insensitive searches, creating an index with a case-insensitive collation sequence is suggested. This involves specifying a collation sequence with a particular strength.

- Example:

```jsx

db.customers.createIndex(

{ LastName: 1 },

{ collation: { locale: 'en', strength: 2 } }

);

```

  

6. **Index Merges**:

- When multiple indexes (e.g., `a` and `b`) are involved in a search query, MongoDB performs index merges. This process combines the index values during the search. The Query plan might involve steps like `IXSCAN`, `AND_SORTED`, and `FETCH`, indicating the utilization of index intersections for `$and` conditions.

  

- Query Plan Example:

```jsx

IXSCAN a_1

2 IXSCAN b_1

3 AND_SORTED

4 FETCH

```

- The AND_SORTED step indicates that an index

intersection has been performed.

Index intersections for $and conditions are unusual.

  

7. **Types of Indexes**:

- **Partial Index**:

- A partial index covers only a subset of information based on a specified filter expression. Example: `{ partialFilterExpression: { retweet_count: { $gt: 0 } } }`

- **Sparse Index**:

- A sparse index excludes documents that lack the indexed attributes. It doesn't support an `$exists:true` search on the indexed attribute. In contrast, non-sparse indexes contain all documents, storing null values for fields absent in documents.

- `sparse: true` is used to ignore documents without the indexed field.

  

8. **Index Statistics**:

- Retrieving index statistics via aggregation:

```javascrpit

db.collection.aggregate([{$indexStats: {}}])

```

- The result will provide relevant index-related statistics.

```json

{

"name": "test",

"key": {

"key": 1

},

"host": "xxx:27017",

"accesses": {

"ops": "145",

"since": "2017-11-25T16:21:36.285Z"

}

}

```

- Ops ->This is the number of operations that used the index.Since

This is the time from which MongoDB gathered stats, and is typically the last start time.

  

6. ### MongoDB Replica Sets

  

1. **Read Preference**:

- By default, all reads are directed to the primary node. However, setting a read preference allows directing read requests to secondary nodes based on specific options:

Options:

- `primary`: Default setting directing all reads to the replica set primary.

- `primaryPreferred`: Direct reads to the primary; if unavailable, direct reads to a secondary.

- `secondary`: Direct reads to a secondary node.

- `secondaryPreferred`: Direct reads to a secondary; if unavailable, direct reads to the primary.

- `nearest`: Direct reads to the replica set member with the lowest network round trip time to the calling program.

  

2. **maxStalenessSeconds**:

- `maxStalenessSeconds` can be added to a read preference to control acceptable data lag. When selecting a secondary node, the MongoDB driver considers nodes whose data timestamps are within `maxStalenessSeconds` seconds of the primary. The minimum value is 90 seconds.

Example: Specifying a preference for secondary nodes with data within 5 minutes (300 seconds) of the primary.

  

3. **Replica Set Tag Sets**:

	- Tag sets enable directing read requests to specific nodes. These tags can designate nodes for specific purposes, such as analytics or distributing read workload evenly across the cluster.
	
	- **Write Concern**:
	
	By default, MongoDB considers a write request complete when the change is journaled on the primary. Write concern settings allow variation from this default with three components:
	
	- `w`: Specifies the number of nodes to receive the write before completion (can be a number or "majority").
	
	- `j`: Controls whether write operations necessitate a journal write before completion (true or false).
	
	- `wtimeout`: Specifies the time allowed to achieve the write concern before returning an error.
	
	**Caution:** Manually setting the write concern based on the number of nodes in the cluster might cause read timeouts in case of cluster failures.

  

7. ### Disk IO

	1. IO throughput
	
	- Throughput, on the other hand, refers to the rate at which data can be transferred from a source to a destination. It's a measure of how much data can be processed in a given amount of time. In the context of disk devices, throughput is often measured in terms of data transferred per second (e.g., megabytes per second).
	
	  
	
	2. TIP
	
	- The throughput of an IO system is primarily determined by the number of physical disk devices it contains. To increase IO throughput, increase the number of physical disks in disk volumes.
	
	3. MongoDB performs three major types of IO operations:
	
	1. Temporary file IO
	
	Involves reads and writes to the “_tmp” directory within the dbPath directory. These IOs occur when a disk sort or disk-based aggregation operation occurs.
	
	1. Temporary file IO occurs when a MongoDB aggregation request cannot be performed in memory, and the allowDiskUse clause has been set to true. In this case, excess data will be written to temporary files in the “_tmp” directory within the dbPath directory.
	
	2. Datafile IO
	
	Occurs when WiredTiger reads from and writes to collection and index files in the dbPath directory.
	
	3. Journal file IO
	
	Occurs as the WiredTiger storage engine writes to the “write-ahead” journal file.

  

4. The Journal

	- When MongoDB changes a document image in the WiredTiger cache, the modified “dirty” copy is not written to disk immediately. The modified pages are only written to disk when a checkpoint occurs.
	
	- db.serverStatus().wiredTiger.log`
		- Within this section, the following statistics are the most useful:
		- **log bytes written:** The amount of data written to the journal.
		- **log sync operations:** The number of log “sync” operations. A sync occurs when journal information held in memory is flushed to disk.
		- **log sync time duration (μsecs):** The number of microseconds spent in sync operations.
	
	  
	
	- In a database, journaling is the process of recording changes before they are applied to the actual data files. This helps ensure data durability in case of crashes. Journal disk contention occurs when multiple write operations are trying to write their changes to the journal at the same time. This competition for access to the journal space can lead to delays in write operations, affecting the performance of the database.
	
	- When WiredTiger creates a new journal file. Because MongoDB uses a journal file size limit of 100 MB, WiredTiger creates a new journal file approximately every 100 MB of data.
	
	
	1. **the journaling write concerns only apply to the primary**
	
		- By default, the journal is committed to disk every 100ms. When you specify a write with the journaled option, the journal is committed to disk in 30ms. So, if you specify j:true for every write, your throughput will be a maximum of 1000/30 = 33.3 writes/sec. If you want better throughput, you’ll need to batch your updates and set j:true for the last update of the batch.
	
	2. **Default Journal Commit Interval**
	
		- MongoDB is a NoSQL database that supports various write operations, such as inserts, updates, and deletes. When you perform these write operations, MongoDB stores the data in memory and asynchronously writes it to the disk to ensure durability (data won't be lost in case of a system crash). The default journal commit interval in MongoDB is set to 100 milliseconds (ms).
	
	3. **Committing Data**
	
		- With a 100ms commit interval, MongoDB will wait for data to accumulate for up to 100ms before committing it to disk. This means that data changes made during this interval are grouped together and written to the disk as a batch.
	
	4. **Implication on Write Throughput**
	
		- Now, let's consider the impact on write throughput. In the context of a single thread (a single operation handler), during the 100ms commit interval, MongoDB will acknowledge the write operation once 30ms have passed. This acknowledgment doesn't mean the data is on disk; it just means that MongoDB has accepted the data and will ensure it gets to disk eventually.
	
	5. **Calculation of Write Operations per Second**
	
		- Given that MongoDB waits 30ms before acknowledging a write operation, you can calculate the maximum number of writes per second achievable on a single thread. The calculation is:
		
				`1 / (commit interval in seconds) = 1 / (0.1 seconds) = 10 writes per second`
		
		- So, on a single thread, you can achieve approximately 10 writes per second with the default settings.
	
	6. **Batching Writes**
	
		- To improve write throughput, MongoDB recommends batching your writes. Instead of setting the "j": true (journaling) option on every write operation, you can set it to true only on the last write operation within a batch. This tells MongoDB to acknowledge that all the previous writes within the same batch are also committed to disk.
		
		- For example, if you have 50 write operations to perform, you can set "j": true only on the last write operation. This means that MongoDB will acknowledge all 50 writes once the last one is committed to disk, which can significantly improve write throughput.