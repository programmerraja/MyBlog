+++
title = 'Mongodb Notes'
date = 2024-07-07T09:06:30.3030+05:30
draft = true
tags =[]
+++ 


## Collection

Methods
- **Validate** :command checks a collection's data and indexes for correctness and returns the results.
- **Note:** Due to the performance impact of validation, consider running [`validate`](https://www.mongodb.com/docs/manual/reference/command/validate/#mongodb-dbcommand-dbcmd.validate) only on [secondary](https://www.mongodb.com/docs/manual/reference/glossary/#std-term-secondary) replica set nodes. and when running this cmd it will lock the collection

```json
{
    "ns" : "myCollection.questions",
    "nInvalidDocuments" : 0,
    "nrecords" : 1131552,
    "nIndexes" : 4,
    "keysPerIndex" : {
        "_id_" : 1131552, // NO of keys in index
    },
    "indexDetails" : {
        "_id_" : {
            "valid" : true
        }
    },
    "valid" : true,
    "repaired" : false,
    "warnings" : [],
    "errors" : [],
    "extraIndexEntries" : [],
    "missingIndexEntries" : [],
    "corruptRecords" : [],
    "ok" : 1.0
}
```
## Indexing

Index wil be used only if the key is in prefix of the index let say we have a index for `{a:1, b:1} ` where we query using `find({b:1})` it won't use the index becuase it same `COLSCAN` it need to scan whole index to filter out 

`db.find({b:100}).sort({a:-1}).explain("execuionStats")` -> in this query even though it used index the `totalDocsExamined` is same as the total document present in db so it similary to `COLSCAN`

```

{
            "stage" : "FETCH",
            "filter" : {
                "b" : {
                    "$eq" : 100.0
                }
            },
            "inputStage" : {
                "stage" : "IXSCAN",
                "keyPattern" : {
                    "a" : 1.0,
                    "b" : 1.0
                },
                "indexName" : "a_1_b_1",
                "isMultiKey" : false,
                "multiKeyPaths" : {
                    "a" : [],
                    "b" : []
                },
                "isUnique" : false,
                "isSparse" : false,
                "isPartial" : false,
                "indexVersion" : 2,
                "direction" : "backward",
                "indexBounds" : {
                    "a" : [ 
                        "[MaxKey, MinKey]"
                    ],
                    "b" : [ 
                        "[MaxKey, MinKey]"
                    ]
                }
            }
        }

"executionStats" : {
        "executionSuccess" : true,
        "nReturned" : 3,
        "executionTimeMillis" : 1580,
        "totalKeysExamined" : 1131552,
        "totalDocsExamined" : 1131552,
        "executionStages" : {
           ...
            }
        }
    },

```

Split compound indexes into multiple single-field indexes if the query pattern changes frequently.

Avoid creating indexes on fields with low cardinality or high update frequency.

Keep the low cardinality key on the last of then index let say we have **isDeleted**,**Name**,**age** where **isDeleted** will have only 2 value and 90% of the document have `false` then it better to put on last which make  the index them more selective so it need to scan only less no of doc in index.

**Index intersection** MongoDB scans each selected index to retrieve the relevant data. This is done in parallel, using multiple threads or processes. intersects the results from each index scan, combining the data to produce the final result set. This is done using a bitwise AND operation, where the resulting documents are those that exist in all the intersecting indexes.

```
{
  "stage": "FETCH",
  "filter": {
    "$and": [
      {
        "a": {
          "$eq": "hello"
        }
      },
      {
        "b": {
          "$eq": 100
        }
      }
    ]
  },
  "inputStage": {
    "stage": "AND_SORTED",
    "inputStages": [
      {
        "stage": "IXSCAN",
        "indexBounds": {
          "a": [
            "[\"hello\", \"hello\"]"
          ]
        }
        ...
      },
      {
        "stage": "IXSCAN",
        "indexBounds": {
          "b": [
            "[100.0, 100.0]"
          ]
        },
        ...
      }
    ]
  }
}
```

### Sparse and partial 
```shell
 db.restaurants.createIndex(
	{"address.city": 1, cuisine: 1},
	{partialFilterExpression: {"stars": {$gte: 3.5}}}
)
> exp.find({'address.city': 'New York', cuisine: 'Sichuan'})
```

After that last explain, would think partial index would be used but it's not, still doing COLLSCAN.

To use partial index, query must be guaranteed to match subset of docs specified by filter expression. Otherwise server might miss results where matching docs not indexed.

To make index used, need to include predicate that matches partial filter expression, stars in our example:

```shell
> exp.find({'address.city': 'New York', cuisine: 'Sichuan', stars: {$gt: 4.0}})
```

Now, IXSCAN is used.

### Index stats

```js
col.aggregate([{$indexStats: {}}])

{
    "name" : "a_1",
    "key" : {
        "a" : 1.0
    },
    "accesses" : {
        "ops" : NumberLong(1), //no of operation per sec 
        "since" : ISODate("2024-07-24T04:20:41.751Z") //since
    },
    "spec" : {
        "v" : 2,
        "key" : {
            "a" : 1.0
        },
        "name" : "a_1"
    }
}
```
## Query Plans

For any given query, the MongoDB query planner chooses and caches the most efficient query plan given the available indexes. To evaluate the efficiency of query plans, the query planner runs all candidate plans during a trial period. In general, the winning plan is the query plan that produces the most results during the trial period while performing the least amount of work.

The associated plan cache entry is used for subsequent queries with the same query shape.

**How is query plan selected?**

MongoDB has _empirical query planner_ - trial period where each candidate plan is executed over short period of time. Planner evaluates which performs the best:

**Query Shape**

To help identify slow queries with the same [query shape](https://www.mongodb.com/docs/v5.0/reference/glossary/#std-term-query-shape), each [query shape](https://www.mongodb.com/docs/v5.0/reference/glossary/#std-term-query-shape) is associated with a queryHash. The `queryHash` is a hexadecimal string that represents a hash of the query shape and is dependent only on the query shape.
To get the  information on the plan cache entries for the collection

`planCacheKey` is a hash of the key for the plan cache entry associated with the query.


```

db.collection.getPlanCache().clear()

db.collection.getPlanCache().listQueryShapes()

db.orders.aggregate( [
   { $planCacheStats: { } }
] )
```

- WiredTiger stores documents with the snappy compression algorithm by default. Any data read from the file system cache is first decompressed before storing in the WiredTiger cache

To just get the quer plan but not want to excute the query use below cmd

```js
db.collectionName.explain("queryPlanner")
```

Over time, collection and indexes may change, therefore plan cache will evict the cache occasionally. Plans can be evicted when:

- Server restarted.
- Amount of work performed by first portion of query exceeds amount of work performed by winning plan by factor of 10.
- Index rebuilt.
- Index created or dropped.

### Covered index perf tips

Add projection to query such that only index fields are included:

```js
db.restaurants.createIndex({name: 1, cuisine: 1, stars: 1})

db.restaurants.find({name: { $gt: 'L' }, cuisine: 'Sushi', stars: { $gte: 4.0 } }, { _id: 0, name: 1, cuisine: 1, stars: 1 })
```

Then all data to be returned exists in keys of index. Mongo can match query conditions _AND_ return results only using index.

Creating above index and running query in shell, executionStats show `totalDocsExamined: 0`.

Caveat, if run the same query but modify projection to omit fields that are not index (as opposed to explicitly asking for fields that are in index):

```js
db.restaurants.find({name: { $gt: 'L' }, cuisine: 'Sushi', stars: { $gte: 4.0 } }, { _id: 0, address: 0 })
```

executionStats shows `totalDocsExamined: 2870`, even though query is the same as before. Query planner does not know what other fields might be present, might have some docs with fields that are not covered by index, therefore it needs to examine the documents.

### Regex Performance

Performance can be improved by creating index: `db.users.createIndex({username: 1})`. Now only need to check regex against `username` index key instead of entire document.

However, still need to look at every single key of index, defeats purpose of index. Recall index stored as B-tree to _reduce_ search space to ordered subset of entire collection.

To take advantage of index when using regex, add `^` at beginning of regex to indicate - only want to return docs where the following chars _start_ at beginning of string.
```js
db.users.find({username: /^kirby/})
```

 Index files use prefix compression
	- use         0 => use
	- used        1 => 0d
	- useful      2 => 0ful
	- usefully    3 => 2ly
	- usefulness  4 => 2ness
	- useless     5 => 0less
	- uselessness 6 => 5ness

### Questions to ask before creating indexing
- Cardinatlity 
- how frequent the field get updated?
- Another exception is when have indexes on fields that grow monotonically - eg: counters, dates, incremental id's. Recall index is b-tree. Monotonically incrementing fields will cause index to become unbalanced on the right hand side -> grows with new data incoming.
## Debugging tools

- mongotop
- mongostat :  it built with go and under the hood it using `serverStatus` cmd to gather info

## Aggregation 

Start with step that filter out most of the data.
	- Two consecutive matches should be combined with `$and` clause
	- Consider project as last step in pipeline. Pipeline can work with minimal data needed for result. 
		- If we do project early in the pipeline chances are that pipeline may not have some variable that it needs.
		- If we need to have a calculated result `$set` (equivalent of `$addField`) stage is recommended.  
	- `$unwind`ing an array only to `$group` them using same field is anti-pattern.
		- Better to use accumulators on array.
	- Prefer streaming stages in most of the pipelines, use blocking stages at the end.
		- `$sort(withoutIndex)`,`$group`,`$bucket`,`$facet` are blocking stages.
		- Streaming stage will process documents as they come and send to the other side.
		- Blocking stages wait for all document to enter in the stage to start processing.

## System Info

`db.hostInfo()` : tell about mongodb host info like cpu, memory,os etc.


## Eviction

In MongoDB, eviction refers to the process of removing data from the in-memory cache

**Eviction Process:**
1. **Eviction Walk**: MongoDB performs an eviction walk, which is a process that scans the cache and identifies candidate pages for eviction.
2. **Page Selection**: MongoDB selects pages to evict based on factors such as:
    - Least Recently Used (LRU) pages
    - Pages with low access frequency
    - Pages that are not pinned in memory (e.g., not currently being accessed)
3. **Page Eviction**: The selected pages are evicted from the cache, and the corresponding data is written to disk.

**Eviction Metrics:**
1. **eviction walks**: The number of eviction walks performed.
2. **eviction walks gave up because they restarted their walk twice**: The number of eviction walks that restarted twice, indicating high eviction pressure.
3. **pages evicted**: The number of pages evicted from the cache.
4. **pages evicted by application threads**: The number of pages evicted by application threads, indicating eviction due to memory pressure.

### Cache eviction thresholds

- Read Cache
	- Below 80% utilisation: no cache eviction
	- 80%+ utilisation: evictions are done on background threads
	- 95%+ utilisation: evictions are done on application threads 
	- 100% utilisation: no new operations until some evictions occur
- Dirty Cache: (As of 3.2.0 )
	- Below 5%: No special policy
	- 5%+: Writes are done using background threads
	- 20%+: Start using application threads to help. 
	
**NOTE:** If some write happens to the document in the key, that page image is considered as dirty.All the consecutive changes are stacked on top of each other in a data structure called skip list.

## RAM and cache usage

The Wired Tiger cache will be set to either 50% of total memory minus 1GB or to 256MB use  ` db.serverStatus()`   to get more details

```js

wiredTiger.cache["maximum bytes configured"] // will tell how much allocated for wired tiger

db.serverStatus().globalLock 
{
"currentQueue" : {
        "total" : 0,
        "readers" : 0, 
        "writers" : 0
    },
    "activeClients" : {
        "total" : 0,
        "readers" : 0,//Number of clients with read operations in progress or queued
        "writers" : 0
    }
}

opcounters

{
    "insert" : NumberLong(0), //No of insert will be stored until system restart
    "query" : NumberLong(46),
    "update" : NumberLong(55),
    "delete" : NumberLong(2),
    "getmore" : NumberLong(60),
    "command" : NumberLong(669)
}

opLatencies 
 {
		 //Total latency statistics for read requests per second
        "reads" : {
            "latency" : NumberLong(30163526),
            "ops" : NumberLong(91)
        },
        "writes" : {
            "latency" : NumberLong(0),
            "ops" : NumberLong(0)
        },
        "commands" : {
            "latency" : NumberLong(250994),
            "ops" : NumberLong(697)
        }
}
```

REFER : https://www.datadoghq.com/blog/monitoring-mongodb-performance-metrics-wiredtiger/#storage-metrics
```js

function bytesToGB(bytes) {
    return (bytes / (1024 * 1024 * 1024)).toFixed(2);
}

function checkWiredTigerCacheStats() {
    
    var status = db.serverStatus();
    
    var cacheStats = status.wiredTiger.cache;
    
    // Extract relevant fields
    var maxBytesConfigured = cacheStats["maximum bytes configured"];
    var bytesInCache = cacheStats["bytes currently in the cache"];
    var dirtyBytesInCache = cacheStats["tracked dirty bytes in the cache"];
    var pagesReadIntoCache = cacheStats["pages read into cache"];
    var pagesWrittenFromCache = cacheStats["pages written from cache"];
    
    // Check if bytes in cache exceed maximum bytes configured
    var isCacheFull = bytesInCache >= maxBytesConfigured;
    
    // Calculate the threshold for dirty bytes (5% of max cache size)
    var dirtyBytesThreshold = maxBytesConfigured * 0.05;
    var isDirtyBytesHigh = dirtyBytesInCache > dirtyBytesThreshold;
    
    // Calculate per-second rates for page reads and writes
    var uptimeSeconds = status.uptime;
    var pagesReadPerSecond = pagesReadIntoCache / uptimeSeconds;
    var pagesWrittenPerSecond = pagesWrittenFromCache / uptimeSeconds;
    
    // Print the results
    print("WiredTiger Cache Stats:");
    print("Maximum Bytes Configured: " + bytesToGB(maxBytesConfigured) + " GB");
    print("Bytes Currently in Cache: " + bytesToGB(bytesInCache) + " GB");
    print("Dirty Bytes in Cache: " + bytesToGB(dirtyBytesInCache) + " GB");
    print("Dirty Bytes Threshold (5% of Max): " + bytesToGB(dirtyBytesThreshold) + " GB");
    print("Pages Read into Cache (Total): " + pagesReadIntoCache);
    print("Pages Written from Cache (Total): " + pagesWrittenFromCache);
    print("Pages Read per Second: " + pagesReadPerSecond.toFixed(2));
    print("Pages Written per Second: " + pagesWrittenPerSecond.toFixed(2));
    
    // Evaluate the cache status
    if (isCacheFull) {
        print("Warning: Cache is full or nearly full. Consider scaling out.");
    } else {
        print("Cache usage is within the configured limit.");
    }
    
    if (isDirtyBytesHigh) {
        print("Warning: Dirty bytes in cache exceed 5% of maximum cache size. Consider scaling out.");
    } else {
        print("Dirty bytes in cache are within acceptable limits.");
    }
}

// Run the function
checkWiredTigerCacheStats();

```

`db.collectioname.aggregate( [ { $collStats: { storageStats:{} } } ] )` -> To get collection level memory allocated detail 

**Note**: There will be rool level wired triger cache which will be cache allocated for collection data if we want index level cache check on each index level cache detail in **indeDetails**

```json
{
"storageStats" : {
	..
	size:"", //**`size`**: The `size` field represents the total uncompressed size of all documents in the collection,
	storagesize:""  //comperesed size including index size
	"wiredTiger" : {
		"cache" : {
			// how much of index is in RAM
			"bytes currently in the cache" : 3568, 
			//"cache miss," where the requested data is not found in the cache and must be fetched from disk so how much bytes read in to cache
			"bytes read into cache" : 1160,
			//the amount of bytes write from cache to disk this happen when the cache get udpate
			"bytes written from cache":0
			// use to determine hit and pass page ratios
			"pages read into cache" : 3,
			//the number of pages that were requested from the cache (i.e., from RAM) to satisfy queries 			
			"pages requested from the cache" : 2,
		
	}
},
"indexDetails": {
	"indexName":{
		"cache" : {
			"bytes currently in the cache" : 357142535,
		}
	 }
}
```




## Files Organization

Files in mongodb path `mongod --dbpath /ata/db`

```
WiredTiger                           diagnostic.data
WiredTiger.lock                      index-1-5808622382818253038.wt
WiredTiger.turtle                    index-3-5808622382818253038.wt
WiredTiger.wt                        index-5-5808622382818253038.wt
WiredTigerLAS.wt                     index-6-5808622382818253038.wt
_mdb_catalog.wt                      index-8-5808622382818253038.wt
collection-0-5808622382818253038.wt  journal
collection-2-5808622382818253038.wt  mongod.lock
collection-4-5808622382818253038.wt  sizeStorer.wt
collection-7-5808622382818253038.wt  storage.bson
```

For each collection and index, WiredTiger storage engine writes an individual `.wt file.
`_mdb_catalog` Catalog file contains catalog of all collections and indexes that mongod contains.

### Journal

Essential component of persistence. Journal file acts as safeguard against data corruption caused by incopmlete file writes. eg: if system sufferes unexpected shutdown, data stored in journal is used to recover to a consistent and correct state.

```shell
ls /data/db/journal
WiredTigerLog.0000000001      WiredTigerPreplog.0000000002
WiredTigerPreplog.0000000001
```

Journal file structure includes individual write operations. To minimize performance impact of journalling, flushes performed with group commits in compressed format. Writes to journal are atomic to ensure consistency of journal files.



### Cache Overflow and the LookAside Table

When WiredTiger's cache becomes full (cache pressure gets too high), it needs to free up space by evicting pages from the cache. However, if there are updates to those pages that haven't yet been written to disk, WiredTiger uses the LookAside (LAS) table to store these updates temporarily. This process ensures that the cache can continue operating efficiently without losing any updates.

- **Cache Pressure:**
    - When the cache usage approaches its limit, WiredTiger needs to make room for new data by evicting some existing pages.
- **Using the LookAside Table:**
    - If a page in the cache has been updated but can't be immediately written to disk (due to cache overflow), the updates are instead written to the LAS table (`WiredTigerLAS.wt`).
    - This LAS table serves as an overflow area to temporarily store updates.
- **Page Eviction:**
    - The updated page is then evicted from the cache.
    - A pointer is added in memory indicating that this page has updates stored in the LAS table.
- **Reading from LAS:**
    - If the page needs to be read again, WiredTiger will check the pointer and realize that it needs to fetch updates from the LAS table.
    - When this happens, the entire history of updates stored in the LAS table for that page is loaded back into memory.
    - This operation is "all or nothing"—the entire update history for the page is loaded, not just a portion of it.



db.serverStatus({tcmalloc:true})`

`tcmalloc` stands for Thread-Caching Malloc, which is a memory allocation library developed by Google. It's a drop-in replacement for the standard C `malloc` function and is designed to be highly efficient and scalable.

In the context of MongoDB, `tcmalloc` is used to manage memory allocation for the WiredTiger storage engine. When you run the command `db.serverStatus({tcmalloc:true})`, you're asking MongoDB to return statistics about the memory allocation and usage of the `tcmalloc` library.

The output will include information such as:

- `generic`: general memory allocation statistics
- `tcmalloc`: specific statistics about the `tcmalloc` library
- `pageheap_free_bytes`: the number of free bytes in the page heap
- `pageheap_unmapped_bytes`: the number of unmapped bytes in the page heap
- `max_total_thread_cache_bytes`: the maximum total thread cache bytes
- `current_total_thread_cache_bytes`: the current total thread cache bytes
- `total_free_bytes`: the total number of free bytes
- `central_cache_free_bytes`: the number of free bytes in the central cache
- `transfer_cache_free_bytes`: the number of free bytes in the transfer cache
- `thread_cache_free_bytes`: the number of free bytes in the thread cache


## Admin cmd
-  `db.adminCommand({buildInfo:true})`
- `db.adminCommand({hostInfo:true})`
-
- 
## Logs
`db.adminCommand({getLog:'global'})`Gets last 1024 entries (global :Combined output of all recent entries )


## Profiling

The MongoDB profiler is a useful tool for troubleshooting and optimizing database performance. When enabled, it records operations in the `system.profile` collection, which can help identify slow queries, inefficient indexing, and other performance bottlenecks.

enabling the profiler comes with a performance cost. It can cause:

- Twice the writes: Each operation is written to the `system.profile` collection, in addition to the original write operation.
- One write and one read for slow operations: When a slow operation is detected, the profiler writes the operation to the `system.profile` collection and also reads the operation from the collection to analyze it.

**Note :**  MongoDB 4.4 and newer versions provide an alternative to the profiler: logs. You can use logs to capture slow operations and other performance-related data without incurring the additional write and read overhead associated with the profiler
## MongoDB authentication mechanisms

#### Available in all versions

- `SCRAM`(`S`alted `C`hallenge `R`esponse `A`uthentication `M`echanism): Default authentication mechanism.
  - Here MongoDB provides some challenge that user must respond to.
  - Equivalent to Password Authentication
  - Client requests a unique value (Nonce) from server
  - Server sends nonce back
  - Client hashes password, adds nonce and hashes hashed password+nonce again
  - Server has hashed password saved
  - Upon receiving end, server picks hashed password and hashes it + nonce it has sent to client.
  - If both the hashes are equal, the login is successful.

- `X.509`: Uses X.509 certificate for authentication.

#### Available in enterprise versions

- `LDAP`(`L`ightweight `D`irectory `A`ccess `P`rotocall): Basis of Microsoft AD.
	- LDAP is sending plaintext data by default, TLS is required to make it secure.
- `KERBEROS`: Powerful authentication designed by MIT.
	- Like `x509` certificates but they are very short lived.


#### Available only in Atlas
 - `MONGODB-AWS`: Authenticate using AWS IAM roles.

### Intra-cluster Authentication

- Two nodes in a cluster authenticate themselves to a [[Replication|Replica Set]]. They can either use `SCRAM-SHA-1` by generating Key file and **sharing key file between the nodes**, (the approach used in replication labs).
	- If they use this key file mechanism user will be `__system@local` and password will be generated keyfile
- Or they can authenticate using `X.509` certificates which are issued by same authority and **can be separated from each other**.

## Authorization

- RBAC is implemented
  - Each user has one or more roles
  - Each role has one or more privileges
  - Each privilege represents a group of actions and the resources where those actions are applied
  - E.g. There are three roles, `Admin`, `Developer` and `Performance Inspector`
  - `Admin` has all the privileges.
  - `Developer` can create, update collection, indices and data. They can also delete indices and data. But can not change any performance tuning parameter.
  - `Performance Inspector` can view all the data and indices, can see and change performance tuning parameter.



## Resources
https://github.com/danielabar/mongo-performance?tab=readme-ov-file [ need to read from ## Performance on Clusters]

https://tech.oyorooms.com/mongodb-out-of-memory-kill-process-mongodb-using-too-much-memory-solved-44e9ae577bed


```
for(let i=0;i<100;i++){
      const largeObject = {
            name: 'Test Object',
            nested: {},
            array: []
        };

        for (let i = 0; i < 1000; i++) {
            largeObject.nested[`property${i}`] = 'x'.repeat(1000); // 1000 characters string
        }

       
        for (let i = 0; i < 1000; i++) {
            largeObject.array.push({ index: i, value: 'y'.repeat(1000) }); // 1000 characters string
        }

  
        const docs = Array(100).fill(largeObject);
   
    db.questions.insertMany(docs)
    }
```



To get DataSize 

This returns a document with the size in bytes for all matching documents mentioned in keypattern on collection mentioned in dataSize
```js
db.runCommand({ 
	dataSize: "database.collection",  //database -> databaseName
	keyPattern: { field: 1 },
	 min: { field: 10 }, 
	 max: { field: 100 } }
	)
```




Vertical sharding
- split by table 

Horzontal sharding
- split by collection level
- mongos
- config server
- shards





Idea 
- Try to convert index.wt file and print as json




## SRV

 SRV (Service) record is a type of DNS (Domain Name System) record that specifies the location of a server or service.

**SRV Record Format:**

An SRV record provides information about the hostnames and ports where the MongoDB instances are running. The format generally looks like this:

`_service._proto.name. TTL class SRV priority weight port target.`

For MongoDB, the connection string might look like this:
`mongodb+srv://<username>:<password>@cluster0.mongodb.net/<dbname>?retryWrites=true&w=majority`

**How SRV Works in MongoDB**

- **`mongodb+srv://`**: The `+srv` part in the connection string indicates that the driver should look for an SRV record to find the list of servers in the cluster.
- **Cluster Discovery**: When you use an SRV record in the connection string, MongoDB automatically discovers the primary and secondary nodes in your cluster, making it easier to manage connections, especially in a sharded or replicated environment.
- **Port Management**: SRV records include the port number, so you don’t need to specify it separately in the connection string.


## Full Time Diagnostic Data Capture

To help MongoDB engineers analyze server behavior, [`mongod`](https://www.mongodb.com/docs/manual/reference/program/mongod/#mongodb-binary-bin.mongod) and [`mongos`](https://www.mongodb.com/docs/manual/reference/program/mongos/#mongodb-binary-bin.mongos) processes include a Full Time Diagnostic Data Capture (FTDC) mechanism. FTDC is enabled by default.
FTDC collects statistics produced by the following commands on file rotation or startup:
- [`getCmdLineOpts`](https://www.mongodb.com/docs/manual/reference/command/getCmdLineOpts/#mongodb-dbcommand-dbcmd.getCmdLineOpts)
- [`buildInfo`](https://www.mongodb.com/docs/manual/reference/command/buildInfo/#mongodb-dbcommand-dbcmd.buildInfo)
- [`hostInfo`](https://www.mongodb.com/docs/manual/reference/command/hostInfo/#mongodb-dbcommand-dbcmd.hostInfo)

`mongod` processes store FTDC data files in a `diagnostic.data` directory under the instances `storage.dbPath`

FTDC runs with the following defaults:
- Data capture every 1 second
- 200MB maximum `diagnostic.data` folder size.


## Keyhole
A [tool](https://github.com/simagix/keyhole) to quickly collect statistics from a MongoDB cluster and to produce performance analytics summaries in a few minutes.


## Security

- https://github.com/stampery/mongoaudit 