+++
title = 'MeiliSearch'
date = 2024-02-20T07:17:12.1212+05:30
draft = true
tags =[]
+++ 


Documents in Meilisearch are loosely typed one field can have multiple data type

## indexing

Indexation is a memory-intensive and multi-threaded operation. This means that **the more memory and processor cores available, the faster MeiliSearch will index new documents**. In general, using a machine with many processor cores will be more effective than increasing RAM.
By default, all the fields of your documents are considered as "searchable".

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



## API
Index settings
 `/indexes/indexName/settings`  To get the setting for the index










## Resources
1. https://www.symas.com/post/understanding-lmdb-database-file-sizes-and-memory-utilization