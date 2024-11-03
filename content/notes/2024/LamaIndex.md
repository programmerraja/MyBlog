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