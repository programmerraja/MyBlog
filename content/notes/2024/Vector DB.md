---
title: Vector DB
date: 2024-10-19T16:23:07.077+05:30
draft: true
tags:
  - database
  - genrative_ai
---


## How it works

A dataset of three sentences, each has 3 words (or tokens)
- In practice, a dataset may contain millions or billions of sentences. 
- The max number of tokens may be tens of thousands (e.g., 32,768 mistral-7b).

Process "how are you"

**Word Embeddings**
- For each word, look up corresponding word embedding vector from a table of 22 vectors, where 22 is the vocabulary size.
- In practice, the vocabulary size can be tens of thousands. The word embedding dimensions are in the thousands (e.g., 1024, 4096)

**Encoding**
- Feed the sequence of word embeddings to an encoder to obtain a sequence of feature vectors, one per word.
- Here, the encoder is a simple one layer perceptron (linear layer + ReLU)
- In practice, the encoder is a transformer or one of its many variants.

**Mean Pooling**
- Merge the sequence of feature vectors into a single vector using "mean pooling" which is to average across the columns.
- The result is a single vector. We often call it "text embeddings" or "sentence embeddings." 
- Other pooling techniques are possible, such as CLS. But mean pooling is the most common.

**Indexing**
- Reduce the dimensions of the text embedding vector by a projection matrix. The reduction rate is 50% (4->2). 
- In practice, the values in this projection matrix is much more random.  The purpose is similar to that of hashing, which is to obtain a short representation to allow faster comparison and retrieval. 
- The resulting dimension-reduced index vector is saved in the vector storage.

**Process "who are you"**
- Repeat Word Embeddings to indexing

**Query: "am I you"**
- Repeat Word Embeddings to indexing
- The result is a 2-d query vector.

**Dot Products**
- Take dot product between the query vector and database vectors. They are all 2-d. 
- The purpose is to use dot product to estimate similarity. 
- By transposing the query vector, this step becomes a matrix multiplication.

**Nearest Neighbor**
-  Find the largest dot product by linear scan. 
- The sentence with the highest dot product is "who am I" 
- In practice, because scanning billions of vectors is slow, we use an Approximate Nearest Neighbor (ANN) algorithm like the Hierarchical Navigable Small Worlds (HNSW).

![[Screenshot from 2024-05-28 06-45-42.png]]

algorithms commonly used for similarity search indexing
- Product quantization (PQ)
- Locality sensitive hashing
- Hierarchical navigable small world (HNSW



**vector DB**
- Pinecone
- Chroma
- Milvus
- Qdrant
- weaviate

## Postgress Vector DB




https://milvus.io/  
https://qdrant.tech/
https://weaviate.io/