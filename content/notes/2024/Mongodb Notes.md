+++
title = 'Mongodb Notes'
date = 2024-07-07T09:06:30.3030+05:30
draft = true
tags =[]
+++ 

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



## Query Plans

For any given query, the MongoDB query planner chooses and caches the most efficient query plan given the available indexes. To evaluate the efficiency of query plans, the query planner runs all candidate plans during a trial period. In general, the winning plan is the query plan that produces the most results during the trial period while performing the least amount of work.

The associated plan cache entry is used for subsequent queries with the same query shape.

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


## Memory Profiling

```js
// Function to convert bytes to gigabytes
function bytesToGB(bytes) {
    return (bytes / (1024 * 1024 * 1024)).toFixed(2); // Converts bytes to GB and rounds to 2 decimal places
}

// Function to check WiredTiger cache stats
function checkWiredTigerCacheStats() {
    // Get the server status
    var status = db.serverStatus();
    
    // Extract WiredTiger cache stats
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
## Debugging tools

- mongotop
- mongostat