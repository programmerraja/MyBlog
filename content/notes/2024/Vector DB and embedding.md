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

- https://r2r-docs.sciphi.ai/documentation/deep-dive/providers/database
- **Integrated Vector Storage:** Store your vectors seamlessly alongside your other data in PostgreSQL, leveraging the database's robustness, ACID compliance, and recovery features.
- **Versatile Search:** pgvector supports both exact and approximate nearest neighbor searches, catering to various accuracy and speed requirements.
- **Distance Functions:** Offers a range of distance metrics, including L2 distance, inner product, cosine distance, L1 distance, Hamming distance, and Jaccard distance. This flexibility allows you to select the most appropriate measure for your data and task.
- **Diverse Vector Types:** Handles various vector formats, including single-precision, half-precision, binary, and sparse vectors, accommodating different use cases.
- **Multilingual Accessibility:** pgvector can be used with any programming language that has a PostgreSQL client, enabling you to work with vectors in your preferred environment.

- **Enabling the Extension:** Use the SQL command `CREATE EXTENSION vector;` to enable pgvector in your desired database.
- **Creating a Vector Column:** Define a vector column in your table using the `vector(n)` data type, where 'n' represents the number of dimensions.
- **Inserting Vectors:** Insert vectors as array-like strings, e.g., ''.
- **Querying Nearest Neighbors:** Employ distance operators like `<->` for L2 distance, `<#>` for inner product, and `<= >` for cosine distance to find nearest neighbors, ordering results and limiting output as needed.

### Indexing and Performance Optimization

pgvector provides indexing options to accelerate searches, particularly when dealing with large datasets:

- **HNSW (Hierarchical Navigable Small World):** This graph-based index offers excellent query performance but requires more memory and build time. It's suitable for scenarios with frequent queries.
- **IVFFlat (Inverted File Flat):** This index is faster to build and uses less memory but may provide slightly lower query performance. It's a good choice for datasets that are updated less frequently.

pgvector also offers various options to fine-tune indexing and querying performance:

- **Index Parameters:** Customize parameters like `m` (connections per layer) and `ef_construction` (candidate list size) for HNSW, and `lists` (number of inverted lists) for IVFFlat, to balance recall and build time.
- **Query Options:** Control search parameters like `hnsw.ef_search` (dynamic candidate list size for HNSW) and `ivfflat.probes` (number of lists to probe for IVFFlat) to adjust the speed-recall trade-off during query execution.
- **Filtering Techniques:** Employ techniques like creating indexes on filter columns, using multicolumn indexes, utilizing approximate indexes for selective filters, and considering partial indexing or partitioning for specific filtering scenarios to optimize performance.
- **Iterative Index Scans:** Introduced in pgvector 0.8.0, iterative scans improve recall for queries with filtering by progressively scanning more of the index.

### Advanced Features

pgvector extends its capabilities beyond basic vector operations, supporting:

- **Half-Precision Vectors:** The `halfvec` type efficiently stores vectors using half-precision floating-point numbers, reducing storage requirements without significant loss of accuracy.
- **Binary Vectors:** pgvector can handle binary vectors using the `bit` data type, enabling efficient storage and querying for data represented in this format.
- **Sparse Vectors:** The `sparsevec` type effectively stores vectors with a large number of zero values, reducing storage and processing overhead.
- **Hybrid Search:** Combine pgvector's vector similarity search with PostgreSQL's full-text search to create powerful hybrid search systems for retrieving relevant data based on both semantic meaning and vector representations.
- **Subvector Indexing:** Index and query specific portions of vectors using expression indexing, facilitating specialized searches and analysis.

https://severalnines.com/blog/vector-similarity-search-with-postgresqls-pgvector-a-deep-dive/



Supported distance functions are:

<-> - L2 distance
<#> - (negative) inner product
<=> - cosine distance
<+> - L1 distance (added in 0.7.0)
<~> - Hamming distance (binary vectors, added in 0.7.0)
< %> - Jaccard distance (binary vectors, added in 0.7.0)


https://milvus.io/  
https://qdrant.tech/
https://weaviate.io/

Here’s a breakdown of each of these distance metrics and how they’re commonly used:

### 1. **Jaccard Distance**
   - **Definition**: Jaccard Distance measures the dissimilarity between two sets. It’s defined as:
     \[
     \text{Jaccard Distance} = 1 - \frac{|A \cap B|}{|A \cup B|}
     \]
     where \( A \cap B \) is the intersection of sets \( A \) and \( B \) (elements common to both), and \( A \cup B \) is the union of \( A \) and \( B \) (all unique elements in both sets).
   - **Range**: The Jaccard Distance ranges from 0 to 1, where 0 means the sets are identical (maximum similarity), and 1 means they are completely disjoint.
   - **Usage**: It's commonly used for binary or categorical data, such as in text similarity (e.g., comparing the overlap of keywords in documents) or recommendation systems.
   
   **Example**: For sets \( A = \{1, 2, 3\} \) and \( B = \{2, 3, 4\} \):
     \[
     \text{Jaccard Distance} = 1 - \frac{|\{2, 3\}|}{|\{1, 2, 3, 4\}|} = 1 - \frac{2}{4} = 0.5
     \]

### 2. **Hamming Distance**
   - **Definition**: Hamming Distance counts the number of positions at which two strings of equal length differ. For binary data, it’s just the count of differing bits.
   - **Range**: It’s an integer value, where a distance of 0 indicates identical strings, and higher values indicate more differences.
   - **Usage**: This metric is widely used in error detection and correction, particularly in coding theory. It’s also used for comparing binary or categorical data.

   **Example**: For two binary strings \( A = 10101 \) and \( B = 11100 \), the Hamming Distance is 3 because they differ in the first, fourth, and fifth positions.

### 3. **L1 Distance (Manhattan Distance)**
   - **Definition**: Also known as Manhattan Distance or Taxicab Distance, L1 Distance measures the absolute difference between coordinates in a space. For two vectors \( \mathbf{x} = (x_1, x_2, \dots, x_n) \) and \( \mathbf{y} = (y_1, y_2, \dots, y_n) \):
     \[
     \text{L1 Distance} = \sum_{i=1}^n |x_i - y_i|
     \]
   - **Range**: L1 Distance is non-negative and can theoretically go up to infinity, depending on how far apart the points are.
   - **Usage**: It’s used in machine learning for evaluating model errors (e.g., regression models) and is often preferred in high-dimensional spaces with sparse data.

   **Example**: For points \( A = (1, 2) \) and \( B = (4, 6) \):
     \[
     \text{L1 Distance} = |1 - 4| + |2 - 6| = 3 + 4 = 7
     \]

### 4. **Cosine Similarity (and Cosine Distance)**
   - **Definition**: Cosine Similarity measures the cosine of the angle between two vectors. Cosine Distance is simply 1 minus the Cosine Similarity. Given vectors \( \mathbf{x} \) and \( \mathbf{y} \):
     \[
     \text{Cosine Similarity} = \frac{\mathbf{x} \cdot \mathbf{y}}{||\mathbf{x}|| \, ||\mathbf{y}||}
     \]
     where \( \mathbf{x} \cdot \mathbf{y} \) is the dot product, and \( ||\mathbf{x}|| \) and \( ||\mathbf{y}|| \) are the magnitudes (norms) of the vectors.
   - **Range**: Cosine Similarity ranges from -1 to 1, where 1 means vectors are identical in direction, 0 means they are orthogonal (completely dissimilar), and -1 means they are exactly opposite. Cosine Distance (1 - Cosine Similarity) ranges from 0 to 2.
   - **Usage**: Cosine Similarity is especially useful in text analysis and document similarity, where the direction (rather than magnitude) of the vectors is more relevant.

   **Example**: For vectors \( \mathbf{x} = (1, 0, 1) \) and \( \mathbf{y} = (1, 1, 0) \):
     \[
     \text{Cosine Similarity} = \frac{(1 \cdot 1 + 0 \cdot 1 + 1 \cdot 0)}{\sqrt{1^2 + 0^2 + 1^2} \times \sqrt{1^2 + 1^2 + 0^2}} = \frac{1}{\sqrt{2} \times \sqrt{2}} = 0.5
     \]

These distances are commonly used in machine learning, natural language processing, and clustering tasks, where they help determine similarity and grouping of data points.



How to reduce the optimize the vector embedding
- **Principal component analysis** : reduce the vector dimension and store but not efficient
- **Matryoshka Representation Learning** : 
- **Binary Quantization** : quantization in models where you reduce the precision of weights, quantization for embeddings refers to a post-processing step for the embeddings themselves. In particular, binary quantization refers to the conversion of the `float32` values in an embedding to 1-bit values, resulting in a 32x reduction in memory and storage usage.

for more check [here](https://huggingface.co/blog/embedding-quantization)
