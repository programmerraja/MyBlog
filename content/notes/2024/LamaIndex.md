---
title: LamaIndex
date: 2024-10-23T06:46:16.1616+05:30
draft: true
tags:
  - genrative_ai
  - AI
---

## Data connector

convert unstructured file to Documents (class )

Types
- SimpleDirectoryReader
- DatabaseReader

```python
from llama_index.core import SimpleDirectoryReader
from llama_index.core import Document 

documents = SimpleDirectoryReader("./data").load_data()

doc = Document(text="text")

```
Other data connectors are offered through [LlamaHub](https://llamahub.ai/) 🦙. LlamaHub contains a registry of open-source data connectors that we can easily plug into any LlamaIndex application

### Data Transformations

 Transformations include chunking, extracting metadata, and embedding each chunk.

First Convert the data in to `Document` and then convert to `Node` and do indexing
 
Transformation input/outputs are `Node` objects (a `Document` is a subclass of a `Node`). Transformations can also be stacked and reordered.

```python
from llama_index.core import VectorStoreIndex

vector_index = VectorStoreIndex.from_documents(documents)
vector_index.as_query_engine()

```

Under the hood, this splits your Document into Node objects, which are similar to Documents (they contain text and metadata) but have a relationship to their parent Document.

To customize core components, like the text splitter, through this abstraction we can pass in a custom `transformations` list or apply to the global `Settings`:

```python

from llama_index.core.node_parser import SentenceSplitter

text_splitter = SentenceSplitter(chunk_size=512, chunk_overlap=10)

# global
from llama_index.core import Settings

Settings.text_splitter = text_splitter

# per-index
index = VectorStoreIndex.from_documents(
    documents, transformations=[text_splitter]
)
```

Above are high level API to do but we can do manually as below

```python

from llama_index.core import SimpleDirectoryReader
from llama_index.core.ingestion import IngestionPipeline
from llama_index.core.node_parser import TokenTextSplitter

documents = SimpleDirectoryReader("./data").load_data()

pipeline = IngestionPipeline(transformations=[TokenTextSplitter(), ...])

nodes = pipeline.run(documents=documents)

```

#### Node Parsser

```python
from llama_index.core import Document
from llama_index.core.node_parser import SentenceSplitter

node_parser = SentenceSplitter(chunk_size=1024, chunk_overlap=20)

nodes = node_parser.get_nodes_from_documents(
    [Document(text="long text")], show_progress=False
)
```



### Indexing
- Summary Index
- Vector Store Index
- Tree index
- keyword table index
-  Property Graph Index

### Vector store Index

Vector stores accept a list of [`Node` objects](https://docs.llamaindex.ai/en/stable/module_guides/loading/documents_and_nodes/) and build an index from them

```python

from llama_index.core import VectorStoreIndex, SimpleDirectoryReader

# Load documents and build index
documents = SimpleDirectoryReader(
    "../../examples/data/paul_graham"
).load_data()

index = VectorStoreIndex.from_documents(documents,show_progress=true)
#or

from llama_index.core.schema import TextNode

node1 = TextNode(text="<text_chunk>", id_="<node_id>")
node2 = TextNode(text="<text_chunk>", id_="<node_id>")
nodes = [node1, node2]
index = VectorStoreIndex(nodes)

```

- By default, VectorStoreIndex stores everything in memory.

ingestion pipeline to create nodes

```python
from llama_index.core import Document
from llama_index.embeddings.openai import OpenAIEmbedding
from llama_index.core.node_parser import SentenceSplitter
from llama_index.core.extractors import TitleExtractor
from llama_index.core.ingestion import IngestionPipeline, IngestionCache

# create the pipeline with transformations
pipeline = IngestionPipeline(
    transformations=[
        SentenceSplitter(chunk_size=25, chunk_overlap=0),
        TitleExtractor(),
        OpenAIEmbedding(),
    ]
)

# run the pipeline
nodes = pipeline.run(documents=[Document.example()])
```

Storing index in DB pass `storage_context=storage_context`

```python 
import pinecone
from llama_index.core import (
    VectorStoreIndex,
    SimpleDirectoryReader,
    StorageContext,
)
from llama_index.vector_stores.pinecone import PineconeVectorStore

# init pinecone
pinecone.init(api_key="<api_key>", environment="<environment>")
pinecone.create_index(
    "quickstart", dimension=1536, metric="euclidean", pod_type="p1"
)

# construct vector store and customize storage context
storage_context = StorageContext.from_defaults(
    vector_store=PineconeVectorStore(pinecone.Index("quickstart"))
)

# Load documents and build index
documents = SimpleDirectoryReader(
    "../../examples/data/paul_graham"
).load_data()

index = VectorStoreIndex.from_documents(
    documents, storage_context=storage_context
)
```


Node Postprocessor

Node postprocessors are a set of modules that take a set of nodes, and apply some kind of transformation or filtering before returning them. 

- **LLMRerank** (retervie the relvant context and send to LLM rerank it based on revelvance score)
- **SentenceEmbeddingOptimizer** (Optimize a node text given the query by shortening the node text.)

Has many more

```python

from llama_index.core.optimization.optimizer import Optimizer

optimizer = SentenceEmbeddingOptimizer(percentile_cutoff=0.5,
#this means that the top 50% of sentences will be used.
#Alternatively, you can set the cutoff using a threshold on the similarity score. In this case only sentences with a similarity score higher than the threshold will be used.
threshold_cutoff=0.7,
)

query_engine = index.as_query_engine(optimizer=optimizer)
response = query_engine.query("<query_str>")
```


`Response Synthesizer` is what generates a response from an LLM, using a user query and a given set of text chunks. The output of a response synthesizer is a `Response` object.

Types
- BaseSynthesizer
- SynthesizerComponent
- Refine
- SimpleSummarize
- TreeSummarize
- Generation
- CompactAndRefine


## Settings

Settings will be used by all module instead of passing LLM to all module we can set it globally

```python
from llama_index.core import Settings

settings.llm = 
```





Schema have document and node etc

Node are class with below properties
- `id_`
- `embedding`
- `metadata`
- `excluded_embed_metadata_keys`
- `excluded_llm_metadata_keys`
- `relationships`
- `metadata_template`
- `metadata_separator`



Vector store
- interface between actula db and has retervieral
- storage_context represent the obj of db that is storing data
- 

StorageContext is a utility container in LlamaIndex for managing storage components like nodes, indices, and vectors. It includes components such as `docstore`, `index_store`, `vector_store`, `graph_store`, and optionally `property_graph_store`

Will be intermediate between store and index


All store are under storage 
 Storeage have
 - doc store 
 - index store
 - grpah store

All are stored in memory


A interface for storage is StorageContext which is used in vector store

Every index has store and retervial



A query engine processes queries to return structured responses, often using a combination of retrieval and synthesis techniques. A retriever, on the other hand, focuses on retrieving relevant nodes or documents based on the query, typically using methods like vector similarity or keyword matching.



Knowelege graph

Extractor

- `SimpleLLMPathExtractor` : Extract short statements using an LLM to prompt and parse single-hop paths in the format (`entity1`, `relation`, `entity2`)

-  `ImplicitPathExtractor`: Extract paths using the `node.relationships` attribute on each llama-index node object.This extractor does not need an LLM or embedding model to run, since it's merely parsing properties that already exist on llama-index node objects.

-  `SchemaLLMPathExtractor`: Extract paths following a strict schema of allowed entities, relationships, and which entities can be connected to which relationships.


Reteriver

All retrievers currently include: - 
- `LLMSynonymRetriever` - retrieve based on LLM generated keywords/synonyms 

- `VectorContextRetriever` - retrieve based on embedded graph nodes  

- `TextToCypherRetriever` - ask the LLM to generate cypher based on the schema of the property graph 

- `CypherTemplateRetriever` - use a cypher template with params inferred by
	the LLM 

- `CustomPGRetriever` - easy to subclass and implement custom retrieval logic


Here's an explanation of Advanced Retrieval Techniques as discussed in the sources:

### Advanced Retrieval 

- **SubQuestion Query Engine**: This technique tackles complex questions by breaking them down into simpler sub-questions. It then uses metadata descriptions of different data sources to route each sub-question to the most appropriate source. This enables answering multifaceted queries that require drawing information from various sources.

```python
# setup base query engine as tool
query_engine_tools = [
    QueryEngineTool(
        query_engine=simple_query_engine,
        metadata=ToolMetadata(
            name="pg_essay",
            description="Paul Graham essay on What I Worked On",
        ),
    ),
]

query_engine = SubQuestionQueryEngine.from_defaults(
    query_engine_tools=query_engine_tools
)
```

- **Small-to-Big Retrieval**: This method addresses the need for both precision and context. It begins by retrieving small, highly specific chunks of text (like sentences) for maximum precision. Then, to provide the LLM with sufficient context, it expands the retrieval to include surrounding sentences before sending it for answer generation.

```python
query_engine = index.as_query_engine(
    similarity_top_k=2,
    node_postprocessors=[
        MetadataReplacementPostProcessor(target_metadata_key="window")
    ],
)
response = query_engine.query(
    "What happened on August 3rd?"
)
print(response)
```

- **Metadata Filtering**: This technique leverages pre-existing metadata attached to data to filter and refine the retrieval process before employing vector search. For instance, when dealing with financial documents, one could filter by year or company name to increase the precision of retrieved context. LlamaIndex allows using the LLM itself to automate this metadata filtering process based on the query.
- 
```python
query_engine = index.as_query_engine(
    filters=MetadataFilters(
        filters=[ExactMatchFilter(key="year", value="2021")]
    )
)
response = query_engine.query(
    "What was the annual profit in 2021?"
)
print(response)
```
- **Hybrid Search**: Recognizing the strengths of both traditional search and vector search, this approach combines the two. Traditional search engines like BM25 excel at keyword-based retrieval, while vector search captures semantic meaning. Hybrid search leverages both, with a parameter to control the weighting between the two methods.

- **Time Series Filtering**: This technique is especially potent for time-dependent data. TimeScale, a vector database with time series capabilities, allows filtering by specific time ranges or intervals before performing vector search. This is illustrated with the example of querying git commit logs within specific time windows.

- **Complex Document Handling**: For documents containing elements like tables, LlamaIndex employs a recursive retrieval strategy. It can create specialized indexes for tables (using the Pandas query engine) and use a recursive retriever to navigate to the appropriate sub-query engine based on the query. This allows for precise retrieval from complex documents.

- **Direct SQL Querying**: This technique leverages the LLM's capability to generate SQL queries to directly query relational databases. For simpler cases, providing the table schema allows the LLM to formulate the query. For complex databases, LlamaIndex supports creating indexes for tables and using the LLM to select the relevant table based on table and column names or user-provided descriptions.

- **Agent-Based Retrieval**: Agents in LlamaIndex can select the best retrieval strategy or tool from a predefined set based on the query. They can employ any combination of the techniques mentioned above, creating a flexible and dynamic retrieval system. The SEC Insights demo application exemplifies this, showcasing an agent capable of analyzing financial filings using multiple techniques.

