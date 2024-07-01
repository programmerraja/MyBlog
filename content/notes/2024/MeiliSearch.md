+++
title = 'MeiliSearch'
date = 2024-02-20T07:17:12.1212+05:30
draft = true
tags =[]
+++ 


Documents in Meilisearch are loosely typed one field can have multiple data type

## indexing

Indexation is a memory-intensive and multi-threaded operation. This means that **the more memory and processor cores available, the faster MeiliSearch will index new documents**. In general, using a machine with many processor cores will be more effective than increasing RAM.By default, all the fields of your documents are considered as "searchable".

In Meilisearch, document storage involves creating specialized inverted indices to facilitate fast responses to search queries. These indices map words or prefixes to the document IDs containing them. While storing documents in raw form is simple, building these indices takes time, especially since some depend on others (e.g., prefix indices depend on word indices).

Suppose we have a collection of documents:

1. Document 1: "The quick brown fox jumps over the lazy dog."
2. Document 2: "A quick brown dog jumps over the lazy cat."
3. Document 3: "The lazy dog sleeps all day."

To respond quickly to search queries, we create inverted indices. For simplicity, let's focus on word-based indices:

Word -> Document IDs containing the word

For the above documents, the word indices would look something like this:

- "The" -> [1, 3]
- "quick" -> [1, 2]
- "brown" -> [1, 2]
- "fox" -> [1]
- "jumps" -> [1, 2]
- "over" -> [1, 2]
- "lazy" -> [1, 2, 3]
- "dog" -> [1, 3]
- "A" -> [2]
- "cat" -> [2]
- "sleeps" -> [3]
- "all" -> [3]
- "day" -> [3]

Now, let's say we want to create prefix indices:

These depend on the word indices. For example, if we want to create the prefix index for "qu", we need to know which documents contain words starting with "qu", which we can get from the word index.

- "q" -> [1, 2]
- "qu" -> [1, 2]
- "qui" -> [1]
- "quic" -> [1, 2]

Building the prefix index for "quic" requires knowing the document IDs containing words starting with "qu", which means we need the word index to be built first.

Additionally, LMDB's single-writing-thread limitation means we can't parallelize updates to these indices without auto-batching, which further complicates the process.

Note:  When doing indexing first create setting then do indexing because by default all fileds are searchable which consume more time on indexing
Send payload in ndjson or CSV format for faster indexing 

## Ranking rules
Ranking rules are built-in rules that ensure relevancy in search results. MeiliSearch applies ranking rules in a default order which can be changed in the settings. You can add or remove rules and change their order of importance.

MeiliSearch employs the following order for ranking rules:
1. typo
2. words
3. proximity
4. attribute
5. wordsPosition
6. exactness

Update setting give priority to wordsPosition

```js
const index = client.getIndex('blogs')
await index.updateSettings({
    rankingRules:
        [
            "wordsPosition",
            "typo", 
            "words", 
            "proximity", 
            "attribute",
            "exactness"
        ]
})
```


### filterable attributes

filterable are stroed in **Lightning Memory-Mapped Database** LMDB uses a B+ tree data structure, which is stored in a single-level store, meaning it doesn't require additional layers or caching mechanisms to manage the data.

### Storing Faceted/Filtered Values:

 **Key-Value Structure:**
 - **Key:** Faceted/filtered value.
 - **Value:** Internal document ID stored in a `RoaringBitmap`, forming an inverted index.
 
 **Example:** For a document with a field `color` and value `blue`, the key is the field ID followed by the string `blue`, and the value is the document ID.

##  Roaring bitmap
Inverted index need to store the index Id of the document if we have 5 id for each integer 5 * 32 = 160 bytes of space need to optimize this meili using Roaring bitmap for storing the doc id.

Roaring bitmaps breaks up all integers in buckets of 2¹⁶ integers(65535). These buckets are called container. It stores individual container data in 3 different formats based on heuristics. Hence its hybrid compression.

Three different type of storage containers used:

> **Array container**

Store as simple sorted array of integers, no bitmaps used at all, [1,13,45,100]. However it will not store common prefix, hence saving space. Searches are done through binary search. This sorted array grows dynamically based on heuristics. If number of elements in array container exceeds 4096, then its converted to bitmap container.

> **Bitmap container**

Stored as 1024 * Word of 64bit , no compression on bitmap. Its fixed size on allocation and does not dynamically grow. It uses 64 bit instructions on processor, hence fast.
- **Range:** [1, 1024]
- **Bitmap:** A 1024-bit long bitmap where each bit represents the presence of a number.
- **Stored As:** Bitmap of 1024 bits, e.g., 000000...1111...00000 (with 1s indicating presence of numbers)

> **Run container**

It is made of a packed array of pairs of 16-bit integers. The first value of each pair represents a starting value, whereas the second value is the length of a run

- **Integers:** [11, 12, 13, 14, 15]
- **Stored As:** (11, 4) (start value 11 and length 4 indicating the run so we can increament +1 4 times for each)

Since 2¹⁶ integers are stored in containers, when index operations are performed, it only need to do against 2 containers at a time. Hence never running out of space.

## API
Index settings
 `/indexes/indexName/settings`  To get the setting for the index










## Resources
1. https://www.symas.com/post/understanding-lmdb-database-file-sizes-and-memory-utilization