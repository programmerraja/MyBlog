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