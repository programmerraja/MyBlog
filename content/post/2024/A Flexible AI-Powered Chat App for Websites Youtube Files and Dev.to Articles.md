---
title: A Flexible AI-Powered Chat App for Websites,Youtube, Files, and Dev.to Articles
date: 2024-11-09T12:14:24.2424+05:30
draft: false
tags:
  - hackthon
---

*This is a submission for the [Open Source AI Challenge with pgai and Ollama](https://dev.to/challenges/pgai)*

## What I Built

For this project, I created an AI-powered chat application designed to provide users with an engaging and interactive experience. The app allows users to interact with content from a variety of sources, including websites, YouTube videos, files, and Dev.to articles. Additionally, it offers a flexible environment for experimenting with Retrieval-Augmented Generation (RAG) configurations, allowing users to fine-tune aspects like chunking methods, LLM providers, and models based on their specific use cases.

### Key Features

The app provides three main modes of interaction:

1. **Chat with Website**: Users can input any website URL or YouTube link. The app processes the content in the background by chunking it and storing it in a PostgreSQL vector database (pgVector). Once indexed, users can start chatting with the content.
   
2. **Chat with File**: Users can upload a file and engage in a conversation with its content.

3. **Chat with Dev.to Articles**: Users can input a Dev.to username, and the app fetches all articles written by that user. These articles are then indexed, and users can start interacting with them.
 
#### Configurable RAG Options

The app offers several customizable options to fine-tune the RAG process, including:

- **LLM Provider**: Choose between **OpenAI** or **Ollama**.

- **Embedding Model**:
    - For OpenAI: "text-embedding-3-small" or "text-embedding-3-large"
    - For Ollama: "nomic-embed-text" or "jina-embeddings-v2-base-en"

- **Use pgai for LLM interaction and chunking**: Decide whether to use pgai’s tools for handling chunking and text processing.

- **Chunking Method**: Choose from various chunking methods: **SENTENCE_SPLITTER**, **MARKDOWN_SPLITTER**, **TOKEN_SPLITTER**, **SEMANTIC_SPLITTER**.

- **Chunk Size & Chunk Overlap**: Control the size of each chunk and the overlap between them for better embedding accuracy.

- **HNSW Indexing**: Option to use **HNSW** (Hierarchical Navigable Small World) for faster retrieval during indexing.

- **Distance Algorithm**: Select the distance metric for retrieval, including **Cosine Similarity**, **L1**, or **L2**.

- **Retrieval Limit**: Control how many documents are retrieved when providing context to the LLM.

## Demo

You can check out the app in action [here](https://devtoragchallenge.streamlit.app/). Below are some screenshots demonstrating the functionality of the app:

[![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fx0tcv6qlnehlebjiq7uf.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fx0tcv6qlnehlebjiq7uf.png)

[![chat with file](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F7mb8igzcr9u568sdwvfo.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F7mb8igzcr9u568sdwvfo.png)

[![chat with dev to articel](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxepy6lexxb65rn6v7xjj.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxepy6lexxb65rn6v7xjj.png)

you can check the source code [here](https://github.com/programmerraja/RAG)

## Tools Used

- **Timescale Cloud**: Database hosting
- **pgVector**: For efficient data storage and retrieval of embeddings
- **pgai Vectorizer**: For enhanced text processing and chunking within the database
- **Ollama**: For generating embeddings and handling response generation
- **Streamlit**: Used for building the user interface

## Final Thoughts

Building this application has been an insightful journey, especially when it comes to understanding how vector databases work and experimenting with various indexing strategies. One of the key concepts I explored was **HNSW** (Hierarchical Navigable Small World), a graph-based algorithm that significantly improves search retrieval performance. Although I didn't implement HNSW in this initial version due to the relatively small dataset, it’s something I plan to explore further in the future.

I also dived deep into **pgai**, a suite of tools designed to simplify the development of RAG (Retrieval-Augmented Generation) systems and semantic search applications with PostgreSQL. pgai has made it easier to manage and query embeddings directly within the database, which simplified many aspects of this project.

Additionally, I go through the source code for **pgai** and gained a better understanding How **pgai** use sql extension feature to interact with LLM.

A funny thing is that I haven’t been able to fully test OpenAI Mode yet, because I don’t have a paid OpenAI account. So, if you encounter any issues or bugs, feel free to reach out to me—I’d be happy to assist!

## Future Improvements

While time constraints limited what I could implement for this project, I have plans to add the following features in the future:

- **Pre-Retrieval Optimization using Query Transformation techniques**
- **Implementing Late Chunking**
- **Evaluating Chunking Strategies using Recall, with LLMs as judges**
- **Retrieval Optimization using Ensemble/Fusion Retrievers**



