---
title: MeiliSearch
date: 2024-02-20T07:17:12.1212+05:30
draft: false
tags:
  - meilisearch
  - logging
---


Documents in Meilisearch are loosely typed one field can have multiple data type

## indexing

 index scheduler responsible for managing the indexes of a Meilisearch

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

Meilisearch creates approximately 20 inverted indexes per document index,
Below 
```rust
  pub word_docids: Database<Str, RoaringBitmapCodec>,

    /// A word and all the documents ids containing the word, from attributes for which typos are not allowed.
    pub exact_word_docids: Database<Str, RoaringBitmapCodec>,

    /// A prefix of word and all the documents ids containing this prefix.
    pub word_prefix_docids: Database<Str, RoaringBitmapCodec>,

    /// A prefix of word and all the documents ids containing this prefix, from attributes for which typos are not allowed.
    pub exact_word_prefix_docids: Database<Str, RoaringBitmapCodec>,

    /// Maps a word and a document id (u32) to all the positions where the given word appears.
    pub docid_word_positions: Database<BEU32StrCodec, BoRoaringBitmapCodec>,

    /// Maps the proximity between a pair of words with all the docids where this relation appears.
    pub word_pair_proximity_docids: Database<U8StrStrCodec, CboRoaringBitmapCodec>,
    /// Maps the proximity between a pair of word and prefix with all the docids where this relation appears.
    pub word_prefix_pair_proximity_docids: Database<U8StrStrCodec, CboRoaringBitmapCodec>,
    /// Maps the proximity between a pair of prefix and word with all the docids where this relation appears.
    pub prefix_word_pair_proximity_docids: Database<U8StrStrCodec, CboRoaringBitmapCodec>,

    /// Maps the word and the position with the docids that corresponds to it.
    pub word_position_docids: Database<StrBEU16Codec, CboRoaringBitmapCodec>,
    /// Maps the word and the field id with the docids that corresponds to it.
    pub word_fid_docids: Database<StrBEU16Codec, CboRoaringBitmapCodec>,

    /// Maps the field id and the word count with the docids that corresponds to it.
    pub field_id_word_count_docids: Database<FieldIdWordCountCodec, CboRoaringBitmapCodec>,
    /// Maps the word prefix and a position with all the docids where the prefix appears at the position.
    pub word_prefix_position_docids: Database<StrBEU16Codec, CboRoaringBitmapCodec>,
    /// Maps the word prefix and a field id with all the docids where the prefix appears inside the field
    pub word_prefix_fid_docids: Database<StrBEU16Codec, CboRoaringBitmapCodec>,

    /// Maps the script and language with all the docids that corresponds to it.
    pub script_language_docids: Database<ScriptLanguageCodec, RoaringBitmapCodec>,

    /// Maps the facet field id and the docids for which this field exists
    pub facet_id_exists_docids: Database<FieldIdCodec, CboRoaringBitmapCodec>,
    /// Maps the facet field id and the docids for which this field is set as null
    pub facet_id_is_null_docids: Database<FieldIdCodec, CboRoaringBitmapCodec>,
    /// Maps the facet field id and the docids for which this field is considered empty
    pub facet_id_is_empty_docids: Database<FieldIdCodec, CboRoaringBitmapCodec>,

    /// Maps the facet field id and ranges of numbers with the docids that corresponds to them.
    pub facet_id_f64_docids: Database<FacetGroupKeyCodec<OrderedF64Codec>, FacetGroupValueCodec>,
    /// Maps the facet field id and ranges of strings with the docids that corresponds to them.
    pub facet_id_string_docids: Database<FacetGroupKeyCodec<StrRefCodec>, FacetGroupValueCodec>,

    /// Maps the document id, the facet field id and the numbers.
    pub field_id_docid_facet_f64s: Database<FieldDocIdFacetF64Codec, Unit>,
    /// Maps the document id, the facet field id and the strings.
    pub field_id_docid_facet_strings: Database<FieldDocIdFacetStringCodec, Str>,

    /// Maps the document id to the document as an obkv store.
    pub(crate) documents: Database<OwnedType<BEU32>, ObkvCodec>,
```

| Inverted Index Name                 | What it Stores                                                                                                      | Example                                                                 |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| `word_docids`                       | A word and all the document IDs containing the word                                                                 | `{"hello": [1, 2, 3]}`                                                  |
| `exact_word_docids`                 | A word and all the document IDs containing the word, from attributes for which typos are not allowed                | `{"hello": [1, 2]}`                                                     |
| `word_prefix_docids`                | A prefix of a word and all the document IDs containing this prefix                                                  | `{"hel": [1, 2, 3]}`                                                    |
| `exact_word_prefix_docids`          | A prefix of a word and all the document IDs containing this prefix, from attributes for which typos are not allowed | `{"hel": [1, 2]}`                                                       |
| `docid_word_positions`              | Maps a word and a document ID (u32) to all the positions where the given word appears                               | `{(1, "hello"): [0, 5]}`                                                |
| `word_pair_proximity_docids`        | Maps the proximity between a pair of words with all the document IDs where this relation appears                    | `{("hello", "world"): [1, 2]}`                                          |
| `word_prefix_pair_proximity_docids` | Maps the proximity between a pair of word and prefix with all the document IDs where this relation appears          | `{("hello", "wor"): [1, 2]}`                                            |
| `prefix_word_pair_proximity_docids` | Maps the proximity between a pair of prefix and word with all the document IDs where this relation appears          | `{("hel", "world"): [1, 2]}`                                            |
| `word_position_docids`              | Maps the word and the position with the document IDs that corresponds to it                                         | `{("hello", 0): [1, 2]}`                                                |
| `word_fid_docids`                   | Maps the word and the field ID with the document IDs that corresponds to it                                         | `{("hello", 1): [1, 2]}`                                                |
| `field_id_word_count_docids`        | Maps the field ID and the word count with the document IDs that corresponds to it                                   | `{(1, 2): [1, 2]}`                                                      |
| `word_prefix_position_docids`       | Maps the word prefix and a position with all the document IDs where the prefix appears at the position              | `{("hel", 0): [1, 2]}`                                                  |
| `word_prefix_fid_docids`            | Maps the word prefix and a field ID with all the document IDs where the prefix appears inside the field             | `{("hel", 1): [1, 2]}`                                                  |
| `script_language_docids`            | Maps the script and language with all the document IDs that corresponds to it                                       | `{("en", "US"): [1, 2]}`                                                |
| `facet_id_exists_docids`            | Maps the facet field ID and the document IDs for which this field exists                                            | `{1: [1, 2]}`                                                           |
| `facet_id_is_null_docids`           | Maps the facet field ID and the document IDs for which this field is set as null                                    | `{1: [1, 2]}`                                                           |
| `facet_id_is_empty_docids`          | Maps the facet field ID and the document IDs for which this field is considered empty                               | `{1: [1, 2]}`                                                           |
| `facet_id_f64_docids`               | Maps the facet field ID and ranges of numbers with the document IDs that corresponds to them                        | `{1: [(0, 10), (20, 30)]}`                                              |
| `facet_id_string_docids`            | Maps the facet field ID and ranges of strings with the document IDs that corresponds to them                        | `{1: [("a", "z"), ("x", "y")]}`                                         |
| `field_id_docid_facet_f64s`         | Maps the document ID, the facet field ID and the numbers                                                            | `{(1, 1): [1.0, 2.0]}`                                                  |
| `field_id_docid_facet_strings`      | Maps the document ID, the facet field ID and the strings                                                            | `{(1, 1): ["hello", "world"]}`                                          |
| `documents`                         | Maps the document ID to the document as an obkv store                                                               | `{1: {"title": "Hello World", "content": "This is a sample document"}}` |

there are three that take the longest time to build: `WORD_DOCIDS`, `WORD_POSITION_DOCID`, and `WORD_PAIR_PROXIMITY_DOCIDS`.

Meilisearch indexes documents in two main phases

- the first phase is about calculating for filtering for example -> this phase is multi-threaded
- the second phase is about writing the information on disk -> this phase is NOT multi-threaded, only one thread does the job, since LMDB (internal DB) does not allow concurrent writings

Meilisearch uses roaring bitmaps widely throughout its inverted indexes to represent document IDs. Roaring bitmaps offer a space-efficient way to store large sets of integers and perform set operations like [union, intersection, and difference](https://en.wikipedia.org/wiki/De_Morgan%27s_laws?ref=blog.meilisearch.com). These operations play a crucial role in refining search results by allowing the inclusion or exclusion of specific documents based on their relationship with others.
### Finite-state transducer

A finite-state transducer is also called a _word dictionary_ because it contains all words present in an index. Meilisearch relies on the utilization of two FSTs: one for storing all words of the dataset, and one for storing the most recurrent prefixes.

FSTs are useful because they support compression and lazy decompression techniques, optimizing memory usage and storage. Additionally, by using automata such as regular expressions, they can retrieve subsets of words that match specific rules or patterns. Moreover, this enables the retrieval of all words that begin with a specific prefix, empowering fast, comprehensive search capabilities.
### R-tree

R-trees are a type of [tree](https://en.wikipedia.org/wiki/Tree_(data_structure)?ref=blog.meilisearch.com) used for indexing multi-dimensional or spatial information. Meilisearch leverages R-trees to power its geo search functionality.
### Tokenziation

**Tokenization** involves two main processes: segmentation and normalization.

**Segmentation** consists in splitting a sentence or phrase into smaller units called tokens.

Meilisearch uses (and maintains) the open-source tokenizer named [Charabia](https://github.com/meilisearch/charabia?ref=blog.meilisearch.com). The tokenizer is responsible for retrieving all the words (or tokens) from document fields.
### Query graph

Each time it receives a search query, Meilisearch creates a query graph representing the query and its possible variations.

To compute these variations, Meilisearch applies [concatenation](https://www.meilisearch.com/docs/learn/advanced/concat?ref=blog.meilisearch.com#concatenated-queries) and [splitting](https://www.meilisearch.com/docs/learn/advanced/concat?ref=blog.meilisearch.com#split-queries) algorithms to the query terms. The variations considered also include potential typos and synonyms–if the user set them. As an example, let’s examine the query: `the sun flower`. Meilisearch will also search for the following queries:

- `the sunflower`: concatenation
- `the sun flowe**d**`: substitution
- `the sun flower**s**`: addition

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

Note:Modifying [ranking rules](https://www.meilisearch.com/docs/learn/core_concepts/relevancy?ref=blog.meilisearch.com#ranking-rules) may trigger a reindexing process. Consider the impact and plan accordingly. and we can get the ranking score on search API by passing`showRankingScoreDetails` 

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

### Auto Batching
	
Auto-batching is a feature that will batch together consecutive document additions and perform them together to improve indexing speed. For document addition to being added to the same batch, they need to:

- target the same index
- have the same update method (`POST /documents` or `PUT /documents`, i.e update or replace documents)
- be immediately consecutive

Auto-batching can be enabled by passing the `--enable-auto-batching` flag to MeiliSearch. This, in turn, gives you access to three new parameters:

- `--max-batch-size <NUM>`: set the maximum number of tasks that can be processed in a batch to NUM. This is set to 1 if NUM is 0, since such a value would prevent any task from ever being processed. If not specified, this is unlimited.
- `--max-documents-per-batch <NUM>`: Set a limit to the maximum number of documents that can be indexed together in a single batch. Since the batch must contain at least one update, this value can be exceeded. If not specified, this is unlimited.
- `--debounce-duration-sec <SECS>`: When about to prepare a batch, wait at least `SECS` seconds before processing it. defaults to 0secs (process immediately). This feature may be useful when you want to send large payloads, but don't want to increase the maximum payload size for the server, you can instead split the payload, send each chunk, and given that you have a debounce duration set high enough, all the chunks will be processed together.
- `--max-indexing-threads` CLI option that would permit you to limit the number of threads the indexing process is using.


## How the avoided the reindexing whole on update or delete

The indexing in Meilisearch is split into 3 main phases:

1. The computing or the extraction of the data (Multi-threaded)
2. The writing of the data in LMDB (Mono-threaded)
3. The processing of the prefix databases (Mono-threaded)

Because the writing is mono-threaded, it represents a bottleneck in the indexing, reducing the number of writes in LMDB will reduce the pressure on the main thread and should reduce the global time spent on the indexing.

Differential indexing ("diff indexing") is a new way of indexing that can selectively re-index the parts of documents that were modified when updating documents.
## API
Index settings
 `/indexes/indexName/settings`  To get the setting for the index










## Resources
1. https://blog.meilisearch.com/how-full-text-search-engines-work/
2. https://www.symas.com/post/understanding-lmdb-database-file-sizes-and-memory-utilization
















MISC

**Index Scheduler**

The Index Scheduler is a system that manages tasks related to indexing data in a database. It's responsible for scheduling, prioritizing, and executing tasks such as adding, deleting, or updating documents, as well as managing index metadata.

**Task Scheduler Struct**

The `TaskScheduler` struct is a singleton that manages the scheduling of tasks. It has several methods:

- `register`: registers a new task with the scheduler
- `get_next_batch`: returns the next batch of tasks to be executed
- `abort_processing_tasks`: aborts the current task being processed if it's in the list of tasks to be aborted
- `scheduler_worker`: a worker function that runs in a separate thread and executes the tasks in the scheduler

**Task Types**

There are several types of tasks that can be scheduled:

- `DumpImport`: imports data from a file
- `DumpExport`: exports data to a file
- `DocumentAddition`: adds a new document to the index
- `DocumentDeletion`: deletes a document from the index
- `ClearAllDocuments`: clears all documents from the index
- `Settings`: updates the settings of an index
- `RenameIndex`: renames an index
- `CreateIndex`: creates a new index
- `DeleteIndex`: deletes an index
- `SwapIndex`: swaps two indexes
- `CancelTask`: cancels a task

**Internal Task**

An `InternalTask` is a representation of a task that's stored in the database. It contains additional metadata such as the task's status, enqueued time, processed time, and error messages.

**Task View**

A `TaskView` is a representation of a task that's returned to the user. It contains a subset of the information in the `InternalTask`, such as the task ID, status, enqueued time, and processed time.

**Index Task Store**

The `IndexTaskStore` is a database that stores information about tasks and indexes. It has several databases:

- `all_tasks`: stores all tasks
- `enqueued_tasks`: stores tasks that are currently enqueued
- `finished_tasks`: stores tasks that have been completed
- `index_name_mapper`: maps index names to index UUIDs
- `index_tasks`: stores tasks associated with each index

**Prioritizer and Auto-Batcher**

The prioritizer and auto-batcher is a system that determines which tasks to execute next. It takes into account the task type, priority, and dependencies between tasks. It returns a `BatchOperation` that contains the tasks to be executed.

**Batch Operation**

A `BatchOperation` is a representation of a batch of tasks to be executed. It can contain multiple tasks, and each task can have additional metadata such as content files or document IDs.

**Index Access**

The `IndexScheduler` struct is responsible for managing access to indexes. It uses a `RwLock` to protect a `HashMap` that maps `IndexUuid` to `Index` objects. The `index` method allows retrieving an `Index` object by its name, and the `swap_indexes` method swaps the names of two indexes.

**Task Prioritization**

The system uses two types of prioritizers: a global one and an index-level one. The global prioritizer retrieves important tasks to do first, such as cancel tasks and dumps. The index-level prioritizer manages tasks for a single index and auto-batches them. Task cancelation is processed one by one in reverse order of enqueuing, ensuring a correct and logical way of processing cancelation.

**Prioritized Task Types**

There are two enums: `PrioritizedTaskType` and `IndexTaskType`. `PrioritizedTaskType` represents global operations, while `IndexTaskType` represents index-level operations.

**Global Prioritizer**

The `global_prioritizer` function takes a list of tasks with their types and returns a `BatchOperation` that the scheduler must process.

**Index Prioritizer and Auto-Batcher**

The `index_prioritizer_auto_batcher` function takes a list of tasks with their types and tries to auto-batch tasks to speed up processing.

**Cron Tasks**

The system plans to implement a mechanism to remove old tasks by creating a loop in a thread that enqueues a new `CleanOldTasks` task into the task queue. This task will be processed by the scheduler, and users can also request clearing the queue.

This is the definition of the `IndexScheduler` struct in Rust. It's a complex struct with many fields, which I'll break down into categories:

**Databases**

- `env`: an LMDB environment
- `all_tasks`: a database of all tasks, keyed by their IDs
- `status`: a database of task IDs grouped by their status
- `kind`: a database of task IDs grouped by their kind
- `index_tasks`: a database of tasks associated with an index
- `canceled_by`: a database of tasks canceled by a task UID
- `enqueued_at`, `started_at`, `finished_at`: databases of task IDs grouped by their enqueue, start, and finish dates

**Task Management**

- `must_stop_processing`: a boolean to stop processing tasks
- `processing_tasks`: a list of tasks currently being processed
- `file_store`: a store of files referenced by tasks

**Index Management**

- `index_mapper`: an `IndexMapper` responsible for creating, opening, storing, and returning indexes

**Features and Configuration**

- `features`: a `FeatureData` instance for managing experimental features
- `autobatching_enabled`, `cleanup_enabled`: booleans for auto-batching and cleanup
- `max_number_of_tasks`, `max_number_of_batched_tasks`: configuration for task limits
- `webhook_url`, `webhook_authorization_header`: webhook configuration
- `dumps_path`, `snapshots_path`, `auth_path`, `version_file_path`: paths for dumps, snapshots, auth, and version files