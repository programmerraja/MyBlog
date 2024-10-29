---
title: Elastic search
date: 2024-03-30T20:20:04.044+05:30
draft: false
tags:
  - elastic_search
  - notes
---

## Indexing

It is like a collection name in a database.

### **Index-level shard allocation filtering**

Index-level shard allocation filtering in Elasticsearch is a feature that allows you to control which nodes in your Elasticsearch cluster are eligible to store the primary and replica shards of specific indices. This feature gives you fine-grained control over shard placement based on node attributes and conditions, helping you optimize data distribution, resource utilization, and cluster stability.

Here's an explanation of index-level shard allocation filtering:

**Scenario:** Consider a multi-node Elasticsearch cluster where each node has different hardware specifications or specific roles, such as hot, warm, or cold data nodes. You want to ensure that certain indices are allocated only to nodes that meet specific criteria.

#### **Node Attributes:**

Elasticsearch allows you to assign custom attributes to nodes in your cluster. These attributes can describe node characteristics such as hardware capabilities, roles, geographic location, or any other relevant information.

#### **Index Settings:**

You can define shard allocation rules at the index level using index settings. Specifically, you can use the `index.routing.allocation.include` and `index.routing.allocation.exclude` settings to specify conditions that nodes must meet or avoid in order to be eligible for storing the shards of a particular index.

- `index.routing.allocation.include`: This setting specifies conditions that nodes must meet to be eligible for shard allocation. For example, you can include nodes with specific attributes, roles, or other criteria.
- `index.routing.allocation.exclude`: This setting specifies conditions that nodes must avoid to be eligible for shard allocation. It allows you to exclude nodes based on attributes, roles, or other criteria.

**Example:**

Let's say you have an Elasticsearch cluster with nodes tagged with attributes like "datacenter" and "node_type," and you want to ensure that a particular index, "logs," is only allocated to nodes in the "us-west" datacenter with the "hot" node type.

```json
PUT /logs/_settings
{
  "settings": {
    "index.routing.allocation.include.datacenter": "us-west",
    "index.routing.allocation.include.node_type": "hot"
  }
}

```

In this example:
- The `index.routing.allocation.include.datacenter` setting ensures that shards of the "logs" index will be allocated only to nodes in the "us-west" datacenter.
- The `index.routing.allocation.include.node_type` setting further restricts allocation to nodes with the "hot" node type.

By applying these index-level shard allocation filtering settings, you can control where your data is stored based on your cluster's node attributes and criteria. This helps you optimize resource utilization, balance workloads, and ensure that data is placed on nodes that meet specific requirements for your use case.

#### **Array of objects are stored as seprate**

In Lucene, arrays of objects are stored by flattening them into a sequence of individual terms. Lucene doesn't natively support complex nested structures like arrays of objects or nested arrays as you would find in some other document-oriented databases. Instead, Lucene is designed to work with flat fields, and it treats arrays of objects as a collection of separate terms.

```
{
      "title": "The Great Book",
      "authors": [
        {"name": "Author 1", "country": "USA"},
        {"name": "Author 2", "country": "UK"}
      ]
}
```

The "authors" field would be indexed as multiple terms, with each term representing one author object. For example:
- [authors.name]: ["Author 1", "Author 2"]
- authors.country: ["USA", "UK"]

#### **Index patterns**

Index patterns are a fundamental concept when working with Kibana, as they allow you to search, analyze, and visualize data stored in Elasticsearch.

An index pattern specifies a wildcard pattern that matches one or more Elasticsearch indices. Indices are where your data is stored in Elasticsearch. By defining an index pattern, you determine the scope of data that Kibana will work with.

#### **Index life cycle mangement**

`GET /my-index-000001/_ilm/explain` → to get llm

`POST /my-index-000001/_ilm/retry` → to retry the index

**[Aliases](https://www.elastic.co/guide/en/elasticsearch/reference/current/aliases.html) →**An alias is a secondary name for a group of data streams or indices. Most Elasticsearch APIs accept an alias in place of a data stream or index name. it used when the index is rollover it write the old data in this index name and new data will come to the old name.

#### **Index templates**

that defines settings, mappings, and other index-related configurations for newly created indices that match a specified pattern or criteria. Index templates are useful when you want to ensure consistent settings and mappings for indices created dynamically or periodically, such as daily or monthly time-based indices.

How to Create templates

```
PUT /_template/my_template
    {
     // Match indices starting with "log-" then apply this template
      "index_patterns": ["log-*"], 
    
      "settings": {
        "number_of_shards": 1,
        "number_of_replicas": 1
      },
      "mappings": {
        "properties": {
          "timestamp": {
            "type": "date"
          },
          "message": {
            "type": "text"
          },
          // Other field mappings
        }
      }
    }
```

#### Reindexing

[https://blog.stackademic.com/optimizing-elasticsearch-reindex-without-downtime-0f70cb4949d6](https://blog.stackademic.com/optimizing-elasticsearch-reindex-without-downtime-0f70cb4949d6)

####  **Concurrent updates on documents**

Elasticsearch uses a versioning mechanism to handle concurrent updates to the same document. When a document is updated, its version is incremented. If two parallel queries attempt to update the same document simultaneously, the following general sequence of events occurs:

1. **Read and Retrieve the Document:**
    - Both queries read the document with its current version.
2. **Update Operation:**
    - Both queries attempt to update the document simultaneously.
    - Elasticsearch uses the version number to determine the order of updates.
    - Only one of the updates is accepted based on the version number.
3. **Version Conflict:**
    - If the second update arrives with an outdated version number, Elasticsearch detects a version conflict.
    - The update with the outdated version is rejected, and the client needs to retry the update with the correct version.

## Shard

"shard" is the basic unit of data storage and distribution. It's essentially a smaller, manageable piece of an index it contain lucene index.

**what is Lucence ?**

**Lucene.**

Lucene is the base of ElasticSearch, but you don’t interract directly with him, as you drive your car, but you don’t ask direct to your engine to start. But what if your car break, don’t you think is a good idea to know how your engine works?

> Lucene Indexing

You have a big a mount of files, and you need to find a specificy file, wich contais a certain word, how to be quickly to do this? how to be scalable? Here’s where indexing comes in: to search large amounts of text quickly, you must first index that text and convert it into a format that will let you search it rapidly. This conversion process is called indexing, and its output is called an index.

> Lucene Index.

The index is composed of one or more segments, and each segment is composed of several indexes, confusing, right? When it is created, it is separated into smaller segments, or you can see it as sub-indices, where each index is not completely independent.


> Lucene Segments.

Segments are immutable .Each segment contains one or more Lucene Documents

At some point, on your journey through ES, you have been in the situation of having to delete a document. And apparently that was no problem, but what is going on behind the scenes?

When you delete a document, it is only “marked” as deleted and a new version of the document is added to the segment. Its real execution is only carried out from time to time when a “joining” of larger segments occurs.

In the meantime, documents continue to take up disk space.

> Lucene Merge.

Over time the index will accumulate many segments. Periodically, segments are merged into a single new segment and removing the old segments.

But wait, whats it the benefits Merge my segments?

Basically, because two important things, discard old documents and as result reduce our index space on disk, and the second one is the old segments are remove, and a new bigger segment are create, incresing your search speed.

From elastic search < 7.0 we have default 5 shard will be created now 1
    
The core structure of a Lucene index is the `inverted index.` It's like a dictionary where terms (words or tokens) are mapped to the documents that contain them. In your example, the terms come from the "title" and "content" fields. "Document 1 Title"  "This is the content of Document 1.will be indexed as 

```
"This" -> [doc2]
"Title" -> [doc1]
"is" -> [doc2]
"the" -> [doc2]
"content" -> [doc2]
"of" -> [doc2]
"Document" -> [doc1,doc2]
"1" -> [doc1,doc2]
```

There are 2 shard `primary` and `replica` shards both are fixed at index creation. replica of primary shard won’t store on the same node where primary exists
To find no of shards
```
GET /your_index_name/_settings
GET _/_cat/_shard/?v 
```

## Segments

Elastic search buffers the documents for some time, and then creates one inverted index for all those documents. _**This “inverted index” is called Segment and this “sometimes” is called Refresh Time.**_ This refreshing time is usually 1 second. Since every document takes the minimum of refresh time to get indexed, that’s why Elastic Search is called _**near-real-time analytics**._ The above optimization might save us some cost, but again, will it be scalable?

### **Nodes**

Node is a single instance of the Elasticsearch server. Nodes are responsible for various tasks such as data storage,      indexing, searching, and responding to client requests. There are two main types of nodes in Elasticsearch:

- **Data Node:** A data node stores data and performs tasks related to data indexing and searching. It holds a portion of the index data and is responsible for the distributed storage and retrieval of documents.
    
- **Master Node:** A master node is responsible for cluster coordination and management tasks. It maintains the cluster state, controls index creation and deletion, and manages the allocation of shards (the smallest unit of data storage in Elasticsearch) across data nodes.
    
- **coordination Node →** distribute the query for the data node (The API sends a search or a get query to any of these nodes. The node it sends the query to becomes the “coordinator” node.)
    
- **Ingest Node →** The Ingest Node Pipeline is a sequence of predefined or custom processors that operate on the incoming documents. These processors allow you to manipulate, enrich, or modify the data before it is indexed. Common tasks performed by processors include: After the data has been processed by the Ingest Node Pipeline, the transformed documents are indexed in Elasticsearch. The indexing process includes storing the documents in a suitable format and creating inverted indexes for efficient searching.
    
    - **Grok Processor:** Parsing unstructured log data into structured fields.
    - **Date Processor:** Parsing and formatting date fields.
    - **Remove Processor:** Removing unwanted fields from documents.
    - **Set Processor:** Setting values for specific fields.
    - **GeoIP Processor:** Adding geo-location information based on IP addresses.
    
To define node type we need to give in elasticsearc.yml file when startingnode

## **Cluster:**

 In Elasticsearch, one or more nodes can form a cluster. A cluster is a group of interconnected Elasticsearch nodes that work together to store and manage data. It provides redundancy, fault tolerance, and the ability to distribute workloads across multiple nodes. create nodes with same cluster name in elasticsearch.yml on each node elastic search will take care to communicate with them using discovery.seed_hosts
```
  cluster.name: your-cluster-name
    node.name: Node1
    network.host: 0.0.0.0
    discovery.seed_hosts: ["Node1_IP:9300", "Node2_IP:9300", "Node3_IP:9300"]
    cluster.initial_master_nodes: ["Node1", "Node2", "Node3"]
```

## **Mapping**

It defines the structure of the database schema, including tables, columns, data types, constraints, and indexes. This ensures that your application's data aligns with the database's expectations.Proper mapping can optimize data retrieval and storage. For example, mapping may dictate the use of indexing or the choice of data types, which can impact query performance. `GET /my-index-000001/_mapping` → to see mapping of the index

Explict → defining mapping on creating index with filed and types and `we can add "index": false to avoid indexing the document which we will not used for search which saves the memory`

**Data types**

1. **Text Data Types**:
    - **Text:** Use the **`text`** data type for **full-text search fields**. This data type tokenizes text into terms, making it suitable for free-text search queries. It's commonly used for fields like article content, product descriptions, and user comments.
    - **Keyword:** Use the **`keyword`** data type for exact matching or sorting fields. Keywords are not tokenized and are typically used for fields like tags, categories, or IDs. They are excellent for filtering and aggregating.
2. **Numeric Data Types**:
    - **Integer:** Use the **`integer`** data type for whole numbers. It's suitable for fields like age, price, and quantity when you need to perform mathematical operations or range queries.
    - **Long:** Use the **`long`** data type for large integer values. It's suitable for fields like timestamps, unique IDs, and counters.
    - **Float/Double:** Use the **`float`** or **`double`** data types for decimal numbers. Use **`float`** for less precision and storage, and **`double`** for higher precision. These are appropriate for fields like product prices, scores, and coordinates.
3. **Date Data Type**:
    - **Date:** Use the **`date`** data type for fields representing dates or timestamps. Elasticsearch can parse and store dates in a specific format, making it efficient for date range queries, date-based aggregations, and date math operations.
4. **Boolean Data Type**:
    - **Boolean:** Use the **`boolean`** data type for fields with true/false values, such as flags or checkboxes.
5. **Geo Data Types**:
    - **Geo-point:** Use the **`geo_point`** data type for storing latitude and longitude coordinates. It's essential for location-based searches, such as finding nearby stores or places.
6. **Binary Data Type**:
    - **Binary:** Use the **`binary`** data type for fields containing binary data, such as images, documents, or multimedia files. Elasticsearch can store and retrieve binary data, but it doesn't index the content for full-text searches.
7. **IP Data Type**:
    - **IP:** Use the **`ip`** data type for fields containing IP addresses. It allows efficient searching for IP ranges and aggregations based on IP addresses.
8. **Other Data Types**:
    - **Nested:** Use the **`nested`** data type to work with arrays of objects or complex data structures where individual elements need to be indexed and searched independently.
    - **Object:** Use the **`object`** data type for JSON-like objects that don't require structured searching within the object fields.

Choosing the right data type depends on your specific use case and how you intend to query and aggregate the data. Consider the following factors:

- **Search Requirements:** Determine whether you need full-text search, exact matching, filtering, sorting, or aggregations for a particular field.
- **Data Volume:** Consider the volume of data and the potential impact on storage and indexing performance, especially for large fields.
- **Analyzers:** Choose appropriate analyzers and tokenization for text fields to ensure effective full-text searching.
- **Sorting and Aggregations:** Use **`keyword`** fields for fields you need to sort or aggregate on.
- **Numeric Precision:** Choose the appropriate numeric data type (e.g., **`float`** vs. **`double`**) based on precision requirements.

**Dynamic option in mapping**

if you set `false` then If there's a mismatch (e.g., a field is of the wrong data type or has a different analyzer), Elasticsearch will reject the document.

**Dynamic option in mapping**

if you set `runtime` then New fields are added to the mapping as [runtime fields](https://www.elastic.co/guide/en/elasticsearch/reference/current/runtime.html). These fields are not indexed, and are loaded from `_source` at query time.

Because runtime fields aren’t indexed, adding a runtime field doesn’t increase the index size. You define runtime fields directly in the index mapping, saving storage costs and increasing ingestion speed. You can more quickly ingest data into the Elastic Stack and access it right away. When you define a runtime field, you can immediately use it in search requests, aggregations, filtering, and sorting.

The "freshness_score" field is defined as a **`double`** data type and is a runtime field. It calculates the freshness score on-the-fly using a script that considers the timestamp of the review.
```

```

**Mapping parameters**

1. **`type`**:
    - **Description:** Specifies the data type of the field. Common types include **`text`**, **`keyword`**, **`integer`**, **`date`**, and more.
    - **Example:** **`"type": "text"`**
2. **`analyzer`**:
    - **Description:** Specifies the text analysis process (analyzer) to use for tokenization and text processing. Applicable to text fields.
    - **Example:** **`"analyzer": "standard"`**
3. **`index`**:
    - **Description:** Controls whether and how a field is indexed. Options include **`"true"`** (default), **`"false"`** (not indexed), and **`"false"`** with **`"doc_values"`** (indexed for sorting and aggregations, but not full-text search).
    - **Example:** **`"index": "true"`**
4. **`norms`**:
    - **Description:** Determines whether field length normalization is enabled. Norms are used for scoring and relevance calculations. if we set false for the filed that we not going to use for search to save space
        
    - **Example:** **`"norms": false`**
        
Relevance calculations.
Relevance calculation, often referred to as scoring or relevance scoring, is a fundamental concept in information retrieval and search engines, including Elasticsearch. It refers to the process of determining how well a document matches a user's query based on various factors and assigning a numerical score to each document. This score reflects the document's relevance to the query, allowing search engines to rank and return the most relevant documents to users.
        
In Elasticsearch, relevance calculations are primarily driven by the following key components:
        
1. **Term Frequency (TF):** This measures how often a term appears in a document. Documents that contain the query terms multiple times are considered more relevant.
2. **Inverse Document Frequency (IDF):** IDF assesses the importance of a term within a corpus of documents. Terms that appear in many documents are considered less important, while terms that appear in a smaller number of documents are considered more important.
3. **Field Length Normalization:** Longer documents tend to have more terms than shorter ones. Field length normalization adjusts the relevance score to account for the document's length, ensuring that longer documents are not unfairly favored.
4. . **Coordination Factor:** This factor accounts for how many of the query terms are present in a document. Documents that contain more query terms are typically more relevant.
5. **Term Boosting:** Elasticsearch allows you to assign different levels of importance (boosts) to individual query terms or fields, which can influence the relevance score.
6. . **BM25 Algorithm:** Elasticsearch uses the BM25 ranking algorithm, which is a variation of TF-IDF with term frequency saturation and term weighting. BM25 is designed to be effective for full-text search and is the default scoring algorithm in Elasticsearch.
7. **Scripting:** Custom scripts can be used to calculate relevance scores based on complex criteria, such as business rules, personalization, or dynamic factors. This is particularly useful when standard scoring mechanisms are insufficient.

**Metadata fields of the documents**

Each document has metadata associated with it, such as the `_index` [The index to which the document belongs.] and `_id` metadata fields. The behavior of some of these metadata fields can be customized when a mapping is created.

`_Source` →The original JSON representing the body of the document. when passing on creating of doc

`_field_names` field used to index the names of every field in a document that contains any value other than `null`.

`_routing` →A document is routed to a particular shard in an index using the following formulas: The default `_routing` value is the document’s `[_id](<https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-id-field.html>)`

When you index a document into an Elasticsearch index, the document is first routed to a primary shard. The calculation for routing a document is performed as follows:

Elasticsearch calculates a hash of the routing value (in your example, the "product_id"). This hash value is typically a number.

The hash value is then divided by the total number of primary shards (the shard count you configured when creating the index). The result of this division determines which primary shard the document should be routed to.

we can user routing key on search to search on only specfic shard

```
GET my-index-000001/_search
        {
          "query": {
            "terms": {
              "_routing": [ "key" ] 
            }
          }
        }
```

For example, if you have an index with 5 primary shards and the hash value of your routing field is 123, Elasticsearch might route the document to the primary shard with an ID of 123 % 5 = 3.


`routing_factor = num_routing_shards / num_primary_shards shard_num = (hash(_routing) % num_routing_shards) / routing_factor`


M**apping types**

Certainly, let's walk through an example to help you understand mapping types in Elasticsearch before they were deprecated in version 7.0. In this example, we'll use an index for storing information about books, and we'll define multiple mapping types within that index. 

However, it's essential to note that, as of Elasticsearch 7.0, the above approach is no longer supported. In recent versions, you'd define your data structures within a single mapping type within an index. Mapping types have been deprecated in favor of a simpler data model.

`Dynamic` →allows you to experiment with and explore data when you’re just getting started. Elasticsearch adds new fields automatically, just by indexing a document. it add mapping by the first document you inserted

`Type coresion` → on first document if we passed age as float then we pass string it will be converted to float but if we send that can’t be converted it will throw error


## **Analyzer**

It is responsible for breaking down, or tokenizing, a text into individual terms or tokens, applying various transformations to these tokens, and preparing them for efficient storage and retrieval. Analyzers are essential for full-text search engines like Elasticsearch and Lucene, and they greatly influence the accuracy and relevance of search results.

Here are the key components and functions of an analyzer:

1. **Tokenization:** The analyzer tokenizes the input text, which means it splits the text into individual words, phrases, or terms. For example, the sentence "The quick brown fox" might be tokenized into individual terms like "The," "quick," "brown," and "fox."
2. **Lowercasing:** In many analyzers, text is converted to lowercase to ensure case-insensitive searching. This means that "Quick" and "quick" would be treated as the same term.
3. **Filtering:** Analyzers often apply filters to remove or modify tokens. Common filters include:
    - **Stopword Removal:** Removing common words (e.g., "a," "an," "the") that do not carry significant meaning.
    - **Stemming:** Reducing words to their root or base form (e.g., "running" becomes "run").
    - **Synonym Expansion:** Expanding tokens to include synonyms (e.g., "car" and "automobile").
    - **Character Filtering:** Removing or replacing specific characters (e.g., punctuation marks).
4. **Token Type Assignments:** Analyzers may assign token types or attributes to each token. This information can be used for advanced searching and filtering.
5. **Normalization:** Some analyzers perform normalization, ensuring that different forms of a word (e.g., singular and plural) are treated as equivalent.
6. **Customization:** Analyzers can be customized to suit specific requirements. In Elasticsearch, you can define your own custom analyzers, combining various tokenizers and filters.
```
    POST /_analyze {text:"hai",analyzer:standard}
```

**ADMIN**

To get full detail about elastic search `GET _cluster/state` will return

```json
{
        "cluster_name" : "elasticsearch",
        "cluster_uuid" : "HazzKHDGSA2hmmsKGVDCeA",
        "version" : 33584,
        "state_uuid" : "LEl8OGf2S9iVh73NovnQVQ",
        "master_node" : "TtayhcCMTmqa020lB-3k4A",
        "blocks" : { },
        "nodes" : {  
            "cp1SAtq2SIKWjJwfm2pqGg" : {
              "name" : "elasticsearch-data",
              "ephemeral_id" : "jhqJvC_PS2a0KvoZV6zSWg",
              "transport_address" : "10.244.2.25:9300",
              "attributes" : {
                "ml.machine_memory" : "6442450944",
                "ml.max_open_jobs" : "512",
                "xpack.installed" : "true",
                "ml.max_jvm_size" : "4294967296",
                "transform.node" : "true"
              },
              "roles" : [
                "data",
                "data_cold",
                "data_content",
                "data_frozen",
                "data_hot",
                "data_warm",
                "ml",
                "remote_cluster_client",
                "transform"
              ]
            },
            "hXF1BAkKT6eaFlcqqE5_6Q" : {
              "name" : "elasticsearch-client",
              "ephemeral_id" : "tQpQ2OldR3G6k6IL1B5a7w",
              "transport_address" : "10.244.2.20:9300",
              "attributes" : {
                "ml.machine_memory" : "4294967296",
                "ml.max_open_jobs" : "512",
                "xpack.installed" : "true",
                "ml.max_jvm_size" : "3221225472",
                "transform.node" : "false"
              },
              "roles" : [
                "ingest",
                "ml",
                "remote_cluster_client"
              ]
            },
            "TtayhcCMTmqa020lB-3k4A" : {
              "name" : "elasticsearch-master",
              "ephemeral_id" : "LDfJRzqtQ8m9VsdD07XGdA",
              "transport_address" : "10.244.2.23:9300",
              "attributes" : {
                "ml.machine_memory" : "4294967296",
                "ml.max_open_jobs" : "512",
                "xpack.installed" : "true",
                "ml.max_jvm_size" : "3221225472",
                "transform.node" : "false"
              },
              "roles" : [
                "master",
                "ml",
                "remote_cluster_client"
              ]
            }
          },
        metadata:{}
        }
```
- nodes → all nodes with details
 - metadata → contain all indices and template and all details

**logstash**

ilm_rollover_alias
    
Certainly! Let's illustrate the concept of `ilm_rollover_alias` using an example scenario:
```
 elasticsearch {
        hosts => "<https://localhost:9243>"
        user => "elastic"
        password => "1234567"
        ilm_rollover_alias => 'klenty-consumers'
        ilm_pattern => "{now/d}-00001"
        template => '/etc/logstash/templates/index_template.json'
        ilm_policy => "klenty-rollover-1-day"
        ilm_enabled => true
    }
    
```
**Scenario**: Imagine you have a system that generates log data, and you want to store this data in Elasticsearch. To efficiently manage the data and make it easier to query, you decide to use Elasticsearch's Index Lifecycle Management (ILM) feature and Logstash to send the data to Elasticsearch.
**Logstash Configuration**:
    
Here's your Logstash configuration with the `ilm_rollover_alias` setting:    

**Explanation with an Example**:
1. **Initialization**:
	- You start Logstash, and it begins sending log data to Elasticsearch.
	- Logstash creates an initial Elasticsearch index with a name based on the `ilm_pattern` and the current date. For example, if the current date is September 17, 2023, it might create an index named "2023.09.17-00001."
	- The alias 'klenty-consumers' is associated with this initial index because of the `ilm_rollover_alias` setting.
2. **Data Ingestion**:
	- Logstash continues to send log data to the active index, which is "2023.09.17-00001" in this example.
	- Elasticsearch stores the data in this index.
3. **Rollover Trigger**:
	- Your ILM policy, "klenty-rollover-1-day," is configured to perform a rollover based on a time condition, such as daily.
	- When the new day begins (e.g., on September 18, 2023), the ILM policy triggers a rollover operation.
4. **Rollover Operation**:
	- The ILM policy orchestrates the rollover. It closes the current active index, which is "2023.09.17-00001," and creates a new index for the new day, let's say "2023.09.18-00001."
5. **Alias Update**:
	- After the rollover operation, the alias 'klenty-consumers' is automatically updated by Elasticsearch to point to the newly created index, which is now "2023.09.18-00001."
6. **Continued Data Ingestion**:
	- Logstash continues to send log data, and Elasticsearch stores it in the newly active index, "2023.09.18-00001."
7. **Querying Data**:
	- Applications can consistently query data using the 'klenty-consumers' alias. They don't need to be aware of the specific index names because the alias always points to the active index.
8. **Lifecycle Management**:
	- The process repeats daily as per the ILM policy, creating new indices for each day, automatically updating the alias, and ensuring that data is efficiently organized and managed.
    
In summary, `ilm_rollover_alias` allows you to create a consistent entry point ('klenty-consumers' alias) for your data, making it easy to query and manage, even as Elasticsearch automatically rolls over to new indices based on your defined criteria, such as time or size. This ensures efficient storage and retrieval of time-series data while abstracting the complexity of index management.
    
- iim_rollover_alias → used when we want to send data with same index name to handle rollover
    
    Certainly, let's break down the concept of aliases in Elasticsearch and how they are used in conjunction with index rollovers with a practical example.
    
    **Alias in Elasticsearch**: An alias is like a virtual pointer or nickname that you give to one or more indices. Instead of referring to a specific index directly, your applications, including Logstash, interact with the alias. This provides a level of abstraction that allows you to change which index the alias points to without needing to modify your application code.
    
    **Index Rollover with Aliases**: When working with time-series data, like logs, you often want to create new indices at regular intervals (e.g., daily or monthly) to efficiently manage and store your data. Index rollover is a technique used to automate this process.
    
    Here's a step-by-step explanation with an example:
    
    **Step 1: Set Up Initial Index and Alias**:
    
    1. You create an initial index, let's call it `logs-2023-09-26`, where the date is part of the index name.
    2. You also create an alias, say `logs-alias`, and initially, you point it to the initial index `logs-2023-09-26`. Logstash is configured to send data to `logs-alias`.
    
    **Step 2: Index Rollover Conditions**:
    
    You define conditions in Elasticsearch, typically using an ILM policy, for when a rollover should occur. These conditions might include criteria like the index reaching a certain size, age, or number of documents.
    
    **Step 3: Elasticsearch Monitoring**:
    
    Elasticsearch constantly monitors the index associated with the alias, which, in this case, is `logs-2023-09-26`. It checks whether the rollover conditions defined in the ILM policy are met.
    
    **Step 4: Rollover Triggered**:
    
    Let's say your ILM policy specifies that a rollover should occur when an index reaches a size of 5GB. When the data ingested into `logs-2023-09-26` exceeds 5GB, Elasticsearch triggers the rollover automatically, without any direct action from Logstash.
    
    **Step 5: New Index Created**:
    
    Elasticsearch creates a new index, such as `logs-2023-09-27`, based on the template and settings you've defined. This new index is empty and ready to receive data.
    
    **Step 6: Alias Update**:
    
    Elasticsearch updates the alias `logs-alias` to point to the new index, `logs-2023-09-27`. Now, any new data sent by Logstash is directed to the newly created index without Logstash needing to be aware of this change. Logstash continues to send data to `logs-alias`.
    
    **Step 7: Repeat**:
    
    The process repeats as data continues to be ingested. When the rollover conditions are met again, another new index is created, and the alias is updated. This cycle continues, and Logstash sends data to the alias without interruption.
    
    **Example**: Let's assume you have an ILM policy with a rollover condition of reaching 5GB in index size. When `logs-2023-09-26` reaches 5GB, Elasticsearch creates `logs-2023-09-27` and updates `logs-alias` to point to it. Logstash, which has been sending data to `logs-alias`, seamlessly starts sending data to the new index.
    
    This process ensures that your data is efficiently managed, and you can query it across multiple indices using the same alias (`logs-alias`), regardless of how many indices have been created through rollovers. It abstracts the complexity of index management from your applications, making it easier to scale and maintain your Elasticsearch setup.
    
- Alias vs index patterns
    
    You're correct that both aliases and index patterns can be used to facilitate querying in Elasticsearch. However, they serve slightly different purposes, and their use cases can overlap. Let's explore the differences between aliases and index patterns:
    
    **1. Aliases:**
    
    - **Purpose**: Aliases are primarily used to abstract the physical index names and provide a consistent, user-friendly name for querying. They act as pointers to one or more indices.
    - **Use Case**: Aliases are useful when you want to ensure that your application or users always interact with a specific index or a group of indices, regardless of the underlying index names. They are often used for managing data lifecycle, providing a stable entry point for querying, and for tasks like index swapping during rollovers.
    - **Dynamic**: Aliases can be updated dynamically by Elasticsearch to point to different indices based on conditions, such as rollover events or data aging.
    - Aliases only accessed through code
    
    **2. Index Patterns:**
    
    - **Purpose**: Index patterns are used to define patterns for matching multiple indices based on their names. They are typically used for querying or applying actions across multiple indices that match a specific naming pattern.
    - **Use Case**: Index patterns are useful when you want to perform actions or queries across a set of indices that share a common naming convention. For example, you might use an index pattern like "logstash-*" to match all indices with names starting with "logstash-" regardless of the date or version in the index name.
    - **Static**: Index patterns are defined in advance and are not updated dynamically like aliases. They rely on consistent naming conventions.
    - Acessed through discover page in kibana
    
    **Key Differences:**
    
    1. **Dynamic vs. Static**: Aliases can be updated dynamically by Elasticsearch, making them suitable for scenarios where the active index changes due to rollovers or other events. Index patterns are defined statically and do not change automatically.
    2. **Stability vs. Flexibility**: Aliases provide stability by always pointing to a specific index or the most recent index in a sequence. Index patterns offer flexibility for querying across multiple indices with similar names.
    3. **Use Cases**: Aliases are commonly used for index lifecycle management, ensuring consistent querying, and providing a stable entry point. Index patterns are more suitable for querying and performing actions across sets of indices with similar names.
    
    **Use Both When Needed:**
    
    In practice, you can use both aliases and index patterns in combination to achieve different objectives. For instance, you can use an alias like 'klenty-consumers' for stable querying and use an index pattern like 'logstash-*' for broader operational tasks across multiple log indices.
    
    In summary, aliases and index patterns are complementary tools in Elasticsearch, and their effectiveness depends on the specific requirements of your use case. Aliases provide stability and dynamic management, while index patterns are helpful for querying and performing actions across groups of indices with similar names.
    

Need to check how

```jsx
 elasticsearch {
      hosts => "<https://klenty-kibana-pipeline.es.us-east4.gcp.elastic-cloud.com:9243>"
    user => "elastic"
    password => "zQb0V7ZssNF7oygnLfz5SetO"
      ilm_rollover_alias => 'klenty-email-validation-logs'
      ilm_pattern => "{now/d}-00001"
      template => '/etc/logstash/templates/index_template.json'
      ilm_policy => "klenty-rollover-1-day"
      ilm_enabled => true
    }
```

this make created as klenty-email-validation-logs-data-0001

- Why
    
    The Logstash configuration you provided is used to create an Elasticsearch index with specific settings and configurations. Let's break down the configuration and explain how the index will be created on a daily basis with examples and in-depth details.
    
    1. **Elasticsearch Output Configuration**:
        
        ```yaml
        elasticsearch {
          hosts => "<https://localhost:9243>"
          user => "elastic"
          password => "123456789"
          ilm_rollover_alias => 'klenty-email-validation-logs'
          ilm_pattern => "{now/d}-00001"
          template => '/etc/logstash/templates/index_template.json'
          ilm_policy => "klenty-rollover-1-day"
          ilm_enabled => true
        }
        
        ```
        
        - `hosts`: This specifies the Elasticsearch cluster's address where Logstash will send data. In your case, it's "[](https://localhost:9243/)[https://localhost:9243](https://localhost:9243)," assuming Elasticsearch is running on localhost on port 9243.
        - `user` and `password`: These are the credentials used for authentication when sending data to Elasticsearch.
        - `ilm_rollover_alias`: This is the alias that will be associated with the index. It's set to 'klenty-email-validation-logs.'
        - `ilm_pattern`: This pattern is used to generate the index name. `{now/d}` represents the current date in the format 'YYYY.MM.dd,' and '-00001' is added to create a unique index name. This will result in an index name like 'klenty-email-validation-logs-2023.09.17-00001' for today's date (assuming today is September 17, 2023).
        - `template`: Specifies the path to an index template. The template contains predefined settings and mappings for the index. You provided a JSON template located at '/etc/logstash/templates/index_template.json.'
        - `ilm_policy`: This is the name of the Index Lifecycle Management (ILM) policy associated with the index. In your case, it's "klenty-rollover-1-day." ILM policies define how indices are managed, including when they should be rolled over and deleted.
        - `ilm_enabled`: This flag is set to true, indicating that ILM is enabled for this index.
        - DOCS
            1. [https://www.elastic.co/guide/en/logstash/current/plugins-outputs-elasticsearch.html](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-elasticsearch.html)
    2. **Index Template** (From '/etc/logstash/templates/index_template.json'):
        
        The index template provided in your configuration contains various settings for the index. Let's break down some key settings:
        
        - `lifecycle`: Specifies the ILM settings for the index, including the ILM policy name ("klenty-rollover-1-day") and the rollover alias ("klenty-email-validation-logs").
        - `routing`: Defines how documents are allocated across nodes in the Elasticsearch cluster. In your configuration, it includes `_tier_preference: "data_content"` for routing.
        - `mapping`: Sets the total fields limit to 10,000 for this index. This limit restricts the number of fields that can be indexed in a single document.
        - `refresh_interval`: Specifies that the index will be refreshed every 5 seconds. This controls how often changes become visible to search.
    3. **Index Creation on a Daily Basis**:
        
        With this configuration, an index will be created in Elasticsearch daily. Here's how it works with an example:
        
        - Today is September 17, 2023.
        - Logstash processes and sends data to Elasticsearch.
        - Elasticsearch uses the ILM pattern "{now/d}-00001" to create the index name: "klenty-email-validation-logs-2023.09.17-00001."
        - The index inherits settings and mappings from the provided template '/etc/logstash/templates/index_template.json.'
        - The ILM policy "klenty-rollover-1-day" is associated with the index, which defines when the index should roll over (in this case, likely after one day) and how data retention is managed.
        
        The next day (September 18, 2023), a new index will be created with the name "klenty-email-validation-logs-2023.09.18-00001," and this process repeats daily.
        
    
    This setup ensures that your Elasticsearch indices are created with the specified settings, follow a daily naming pattern, and are managed according to the ILM policy you've defined, allowing for efficient data retention and maintenance.

## **Query**

1. Elasticsearch sorts matching search results by **relevance score**,
    
2. The relevance score is a positive floating point number, returned in the `_score` metadata field of the [search](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-search.html) API. The higher the `_score`, the more relevant the document. While each query type can calculate relevance scores differently, score calculation also depends on whether the query clause is run in a **query** or **filter** context.
    
3. **Query context → a**nswers the question “_How well does this document match this query clause?_” Besides deciding whether or not the document matches, the query clause also calculates a relevance score in the `_score` metadata field.
    
    1. Query context is in effect whenever a query clause is passed to a `query` parameter
4. **Filter context →**answers the question “_Does this document match this query clause?_” The answer is a simple Yes or No — no scores are calculated
    
    1. Frequently used filters will be cached automatically by Elasticsearch, to speed up performance
    2. Filter context is in effect whenever a query clause is passed to a `filter` parameter
5. **Relevance Score**
    
    The relevance score, also known as the "_score" in Elasticsearch, represents how well a document matches a given query. It's a numerical value assigned to each document returned by a search, indicating the degree of relevance to the search criteria. The higher the score, the more relevant the document is considered.
    
    **How Relevance Score is Calculated:**
    
    - Elasticsearch uses a scoring algorithm, typically the TF-IDF (Term Frequency-Inverse Document Frequency) algorithm or its variations, to calculate the relevance score.
    - The score takes into account factors such as the frequency of query terms in the document, the length of the document, and the overall frequency of terms in the entire index.
    
    **Example:** Let's consider a simple example. Assume you have an index of articles and you perform a search for the query "machine learning."
    
    - Document 1: "Introduction to Machine Learning"
        - Contains the exact phrase "machine learning" multiple times.
        - Medium-length document.
    - Document 2: "The History of Artificial Intelligence"
        - Mentions "machine learning" once.
        - Long document.
    - Document 3: "Cooking Recipes for Beginners"
        - Doesn't contain the phrase "machine learning."
        - Short document.
    
    In this scenario, Elasticsearch would calculate a relevance score for each document based on factors like term frequency and document length. Let's say the scores are as follows:
    
    - Document 1: Relevance Score = 8.2
    - Document 2: Relevance Score = 4.5
    - Document 3: Relevance Score = 1.1
    
    When presenting the search results, Elasticsearch would order them by relevance score in descending order, so Document 1 would be listed first, followed by Document 2, and then Document 3.
    
    This scoring mechanism allows Elasticsearch to provide more relevant search results to users by considering not only the presence of query terms but also their frequency and importance in the documents.
    

- Example [basic cannot use complex]
    
    ```jsx
    GET _search
    {
      "query": {
        "match": {
          "FIELD": "TEXT"
        }
      }
    }
    ```
    
- Example with aggregation
    
    ```jsx
    GET /_search
    {
      "query": {
        "bool": {
          "must": [
            {
              "match": {
                "title": "smith"
              }
            }
          ],
          "must_not": [
            {
              "match_phrase": {
                "title": "granny smith"
              }
            }
          ],
          "filter": [
            {
              "exists": {
                "field": "title"
              }
            }
          ]
        }
      },
      "aggs": {
        "my_agg": {
          "terms": {
            "field": "user",
            "size": 10
          }
        }
      },
      "highlight": {
        "pre_tags": [
          "<em>"
        ],
        "post_tags": [
          "</em>"
        ],
        "fields": {
          "body": {
            "number_of_fragments": 1,
            "fragment_size": 20
          },
          "title": {}
        }
      },
      "size": 20,
      "from": 100,
      "_source": [
        "title",
        "id"
      ],
      "sort": [
        {
          "_id": {
            "order": "desc"
          }
        }
      ]
    }
    ```
    
- Query Result with explanation
    
    - Output
        
        ```jsx
        {
          "took" : 5,
          "timed_out" : false,
          "_shards" : {
            "total" : 1,
            "successful" : 1,
            "skipped" : 0,
            "failed" : 0
          },
          "hits" : {
            "total" : {
              "value" : 2,
              "relation" : "eq"
            },
            "max_score" : 29.626675,
            "hits" : [
              {
                "_index" : "news",
                "_id" : "RzrouIsBC1dvdsZHf2cP",
                "_score" : 29.626675,
                "_source" : {
                  "link" : "<https://www.huffpost.com/entry/guard-cat-hailed-as-hero_n_62e9a515e4b00f4cf2352a6f>",
                  "headline" : "Bandit The 'Guard Cat' Hailed As Hero After Thwarting Would-Be Robbery",
                  "short_description" : "When at least two people tried to break into a Tupelo, Mississippi, home last week, the cat did everything she could to alert its owner.",
                  "category" : "WEIRD NEWS",
                  "authors" : "",
                  "country" : "US",
                  "timestamp" : 1693070640
                }
              },
              .....
            ]
          }
        }
        ```
        
    - The `_shards` field tells how many shards were involved in this search operation, how many of them returned successfully, how many failed, and how many skipped.
        
    - The `hits` field contains the documents returned from the search. Each document is given a score based on how relevant it is to our search. The hits field also contains a field `total` mentioning the total number of documents returned, and the max score of the documents.
        
    - Finally, in the nested field, `hits` we get all the relevant documents, along with their `_id`, and their `score`. The documents are sorted by their scores.
        

Qurries

1. **leaf queries →Leaf queries look for specific values in certain field/fields. These queries can be used independently. Some of these queries include match, term, range queries.**
    1. ex full text,term query,span query,joining queries,
2. **compound queries →Compound queries uses the combination of leaf/compound queries. Basically, they combine multiple queries together to achieve their target results.**
    1. ex → bool,boosting,function score,constant socre

Boolean query

- Basic syntax
    
    ```jsx
    {
    	query:{
    		bool:{
    			"must/should/filter":{
    				"term":{conditon}	
    				"range":{"age":{"gte":10,"lte":10}}
    			}
    		}
    	}
    }
    ```
    
- must → The clause (query) must appear in matching documents and will contribute to the score.
    
    ```jsx
    POST /indexName/_search
    {
      "query": {
        "bool" : {
          "must" : {
            "term" : { "user.id" : "kimchy" }
          },
    		}
    }
    ```
    
- should →should appear in the matching document.where documents meeting these conditions get a boost in their `relevance scores`. but same as filter
    
- filter → ingore for the score calculation,clauses are considered for caching.
    
    ```jsx
    POST _search
    {
      "query": {
        "bool" : {
          "must" : {
            "term" : { "user.id" : "kimchy" }
          },
          "filter": {
            "term" : { "tags" : "production" }
          },
    }
    }
    ```
    
- must_not →ingore the score calculation,clauses are considered for caching.
    
    ```jsx
    {
    	"must_not" : {
            "range" : {
              "age" : { "gte" : 10, "lte" : 20 }
            }
          },
    }
    ```
    

Params

- **`minimum_should_match` →** to specify the number or percentage of `should` clauses returned documents _must_ match.
    
    ```jsx
    POST _search
    {
      "query": {
        "bool" : {
          "must" : {
            "term" : { "user.id" : "kimchy" }
          },
          "filter": {
            "term" : { "tags" : "production" }
          },
          "must_not" : {
            "range" : {
              "age" : { "gte" : 10, "lte" : 20 }
            }
          },
          "should" : [
            { "term" : { "tags" : "env1" } },
            { "term" : { "tags" : "deployed" } }
          ],
          "minimum_should_match" : 1,
          "boost" : 1.0
        }
      }
    }
    ```
    

Math query

- sample →this would return documents matching either “confidence” **OR** “buildings” . The default behavior of the **match** query is of **OR**
    
    ```jsx
    POST fb-post/_search
    {
      "query": {
        "match": {
          "description": {
            "query":"confidence buildings"
          }
        }
      }
    }
    
    Or
    
    POST fb-post/_search
    {
      "query": {
        "match": {
          "description": "confidence buildings"
        }
      }
    }
    ```
    
- And
    
    ```jsx
    POST fb-post/_search
    {
      "query": {
        "match": {
          "description": {
            "query":"confidence buildings",
            "operator":"AND"
          }
        }
      }
    }
    ```
    
- **multi-match**
    
    ```jsx
    POST fb-post/_search
    {
      "query": {
        "multi_match" : {
          "query":    "Giffords family", 
          "fields": [ "name", "description" ] 
        }
      }
    }
    ```
    

We can also give custom scoring against the specific fields. In the below query, a boost of 5 is given to all the documents which match the keywords against the field “name”

- **multi-match**
    
    ```jsx
    POST fb-post/_search
    {
      "query": {
        "multi_match" : {
          "query":    "Giffords family", 
          "fields": [ "name^5", "description" ] 
        }
      }
    }
    ```
    
- **query_string**
    
    ```jsx
    POST fb-post/_search
    {
        "query": {
            "query_string" : {
                "query" : "(step down) OR (official act)",
                "fields" : ["description","name"]
            }
        }
    }
    
    "query":"status:active" -> staus is active
    "query":"book.\\*:(quick OR brown)" -> book.any work has qucik or brown
    _exists_:title -> has exists true
    count:[1 TO 5] -> 1 to 4
    date:{* TO 2012-01-01} 
    
    ```
    
- **match_phrase →Match_phrase query is a particularly useful piece of query which looks for matching the phrase rather than individual words.**
    
    ```jsx
    POST fb-post/_search
    {
        "query": {
            "match_phrase" : {
                "description" : "deeply concerned"
    							"slop": 2 [optional]
            }
        }
    }
    
    The slop value defaults to 0 and ranges to a maximum of 50. 
    In the above example, the slop value 2 indicates, 
    how far the terms can be allowed to consider as a match.
    
    ```
    

**Term query**

Returns documents that contain an **exact** term in a provided field. Dont user for text field because analyzer will remove the puncation and other special character so if search with specail char it wont match but you can use match query that will work.

- Query
    
    ```jsx
    {
      "query": {
        "term": {
          "full_text": "Quick Brown Foxes!",
    			case_insensitive:true / from 7.0 
        }
      }
    }
    
    eventhough if you have `Quick Brown Foxes!` in index it wont work beacause it
    stored as [quick, brown, fox] during analysis.
    ```
    

**Fuzzy query**

Returns documents that contain terms similar to the search term,

- query
    
    ```jsx
    GET /_search
    {
      "query": {
        "fuzzy": {
          "user.id": {
            "value": "ki",
    				"fuzziness": "AUTO",
            "max_expansions": 50,
            "prefix_length": 0,
            "transpositions": true,
            "rewrite": "constant_score_blended"
          }
        }
      }
    }
    ```
    

**IDs quey**

- query

    ```jsx
    GET /_search
    {
      "query": {
        "ids" : {
          "values" : ["1", "4", "100"]
        }
      }
    }
    ```
    

**Regexp query**

- Query
    
    ```jsx
    GET /_search
    {
      "query": {
        "regexp": {
          "user.id": {
            "value": "k.*y",
            "flags": "ALL",
            "case_insensitive": true,
            "max_determinized_states": 10000,
            "rewrite": "constant_score_blended"
          }
        }
      }
    }
    ```
    

Aggregation

The structure of an aggregation query is as follows:

```jsx
GET _search
{
  "size": 0,
  "aggs": {
    "NAME": {
      "AGG_TYPE": {}
    }
  }
}
```

## **Types of aggregations**

There are three main types of aggregations:

- Metric aggregations - Calculate metrics such as `sum`, `min`, `max`, and `avg` on numeric fields.
- Bucket aggregations - Sort query results into groups based on some criteria.
- Pipeline aggregations - Pipe the output of one aggregation as an input to another.

Query resources

1. [https://medium.com/elasticsearch/introduction-to-elasticsearch-queries-b5ea254bf455](https://medium.com/elasticsearch/introduction-to-elasticsearch-queries-b5ea254bf455)
2. [https://medium.com/elasticsearch/elasticsearch-queries-part-02-full-text-queries-e5ffe8fb3110](https://medium.com/elasticsearch/elasticsearch-queries-part-02-full-text-queries-e5ffe8fb3110)

Advanced

1. How many request can be handled by ELK
    
    1. The queue size (how many requests ES can accept before starting to reject
    
    them) defaults to 1000.
    
    b. This queue size is per thread in the threadpool.
    
    1. **Calculating Request Impact on Threadpool:**
        
        - The number of requests that can be handled simultaneously depends on the number of processors (cores) and shards hit by each search.
        - For instance, if a search runs on 3 indices, each having 2 shards, there would be 6 requests in the threadpool per search.
        - Example: Let's say you have two servers, each with 8 cores. That totals to 16 cores in total, resulting in 16 threads available for processing.
        - Considering the earlier scenario of 6 requests per search, the cluster can handle 8 requests at once with the available threads.
    2. **Queueing and Rejection:**
        
        - The cluster can still queue additional searches. It can queue roughly **`nodes * queue size / requests per search`** searches before starting to reject them.
        - In this case: **`2 nodes * 1000 queue size / 6 requests per search = 333 searches`** can be queued before rejecting them.
        
        a single node for single cluster
        
        quorm system
        
        how to scale write → increase shard for index
        
        to scale the read → increase the replica
        
        when write come it will write to buffer then it will taken and written to segements (immutable) written for every 1s
        
        ```jsx
        put indexname 
        {
          settings:{
        		refresh_interval:"30s"
        	}
        }
        ```
        
        and when write op come it put on trans log the trans log will we wipe out when the data written to segement and disk
        
        from 7.0 if we not doing any search they won’t create the segements if we do search they explictly create the segements.
        
        every update the old segement mark for deletion and it create new
        
        Storage compression
        
        - LZ4 , Deflate
        
        BKD tress → to store the numbers
        
        half and scaled floats
        
        ```yaml
        POST /indexname/_doc/0/_expalin
        {
        	query
        }
        
        will explain why the first doc match for query
        ```
        
        look for
        
        1. rollup
        2. fuzziness
        
        Full text search
        
        1. Tokenizer → remove white space , lowercase the word, remove stop word (the,a,etc) because we not going search using this word
        
        ```yaml
        GET /analyzer
        {
        	analyzer:"english",
        	text:"hai hello how are u"
        }
        ```
        
        1. Steming the word
        2. Create inverted index the tokenzied word are stored in alphabetical order with ID (it uses linked list)
            1. it contain how many time it occur and position
    
    [https://engineering.zalando.com/posts/2023/11/migrating-from-elasticsearch-7-to-8-learnings.html](https://engineering.zalando.com/posts/2023/11/migrating-from-elasticsearch-7-to-8-learnings.html)
    
    [https://www.elastic.co/guide/en/elasticsearch/reference/current/size-your-shards.html#size-your-shards](https://www.elastic.co/guide/en/elasticsearch/reference/current/size-your-shards.html#size-your-shards)
    
    1. **[How Does Full-Text Search Work under the Hood by Philipp Krenn](https://www.youtube.com/watch?v=b7tCjZSvOno)**
        1. Talks abount lucene (how sentence get stored using tokenizer)
        2. How revelence score calculated
    
    Searching 52 indices with 1 shard each is exactly equivalent to searching one index with 52 shards. The search happens at shard level
    
    [https://www.elastic.co/blog/why-am-i-seeing-bulk-rejections-in-my-elasticsearch-cluster](https://www.elastic.co/blog/why-am-i-seeing-bulk-rejections-in-my-elasticsearch-cluster)
    
    [https://www.elastic.co/blog/how-many-shards-should-i-have-in-my-elasticsearch-cluster](https://www.elastic.co/blog/how-many-shards-should-i-have-in-my-elasticsearch-cluster)
    
    ## Tool
    
    1. [[https://github.com/elastic/rally](https://github.com/elastic/rally)](Macrobenchmarking framework for Elasticsearch