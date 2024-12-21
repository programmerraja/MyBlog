---
title: "Generative AI: A Personal Deep Dive – My Notes and Insights"
date: 2024-12-15T11:45:10.1010+05:30
draft: false
tags:
  - genrative_ai
  - AI
---
Hey Everyone! As the world increasingly moves toward AI-driven solutions, I, as a full-stack developer, became intrigued by the potential of generative AI. Curious to explore its capabilities and challenges, I decided to dive in and learn more. Over the past few months, I've been learning and documenting my journey, collecting insights about generative AI. Today, I'm excited to share the knowledge and experiences I've gained along the way!

I've compiled all my notes on generative AI, embeddings, vector databases, RAG, and more in my personal blog. If you're interested in exploring all the resources and insights I've gathered, feel free to check them out [here](https://programmerraja.github.io/tags/genrative_ai).

### Unpacking the Magic of Transformers

[![Transformers](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fmrstz597enn152g20eun.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fmrstz597enn152g20eun.png)

When I first started diving into LLMs, I was immediately drawn to the "transformer" architecture. As someone who loves understanding how things work under the hood, I knew this was the foundation that held everything together. But let me tell you—it wasn’t easy to grasp at first. At its core, transformers use attention mechanisms to allow the model to weigh the importance of different words in a sequence. This is a big leap from previous models like RNNs or LSTMs, which struggled with long-range dependencies in text.

The reason transformers have been so transformative (no pun intended!) is because they allow models to efficiently process large amounts of text data, while maintaining context across longer passages. However, as much as I learned, I still don’t fully understand every detail of how these models work. For example, while the concept of attention is clear, the deeper mathematical operations behind it can still feel abstract. That said, I’ve put together a list of resources that helped me make sense of these concepts, which might be useful for anyone looking to start their own exploration:

**YouTube Videos:**

- [Transformers (how LLMs work) explained visually](https://www.youtube.com/watch?v=wjZofJX0v4M&pp=ygUQdHJhbnNmb3JtZXJzIGxsbQ%3D%3D)
- [Transformer Neural Networks, ChatGPT's foundation, Clearly Explained!!!](https://www.youtube.com/watch?v=zxQyTK8quyY&t=1550s&pp=ygUQdHJhbnNmb3JtZXJzIGxsbQ%3D%3D)
- [Intro to Large Language Models](https://www.youtube.com/watch?v=zjkBMFhNj_g) (1hr Talk)
- [Transformers Indepth Architecture Understanding- Attention Is All You Need](https://www.youtube.com/watch?v=SMZQrJ_L1vo&pp=ygUbdHJhbnNmb3JtZXJzIGxsbSBrcmlzaCBuYWlr)
- [The math behind Attention: Keys, Queries, and Values matrices](https://www.youtube.com/watch?v=UPtG_38Oq8o&t=1045s)
- [The Most Important Algorithm in Machine Learning](https://www.youtube.com/watch?v=SmZmBKc7Lrs&t=188s)

**Blogs:**

- [Transformers: The Architecture](https://peterbloem.nl/blog/transformers)
- [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/)
- [Tutorial 14: Transformers I - Introduction](https://www.borealisai.com/research-blogs/tutorial-14-transformers-i-introduction/)
- [The Transformers Architecture in Detail: What's the Magic Behind LLMs](https://aigents.co/data-science-blog/publication/the-transformers-architecture-in-detail-whats-the-magic-behind-llms)
- [https://rbcborealis.com/research-blogs/tutorial-14-transformers-i-introduction/](https://rbcborealis.com/research-blogs/tutorial-14-transformers-i-introduction/)
- [Understanding LLMs from Scratch Using Middle School Math](https://towardsdatascience.com/understanding-llms-from-scratch-using-middle-school-math-e602d27ec876)
- [How Do Language Models put Attention Weights over Long Context?](https://yaofu.notion.site/How-Do-Language-Models-put-Attention-Weights-over-Long-Context-10250219d5ce42e8b465087c383a034e)
- [Inspectus GitHub Repository](https://github.com/labmlai/inspectus)
- [How LLM Transformer Models Work with Interactive Visualization](https://poloclub.github.io/transformer-explainer/)

### Prompt Engineering

[![Prompt Engineering](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fh6fa7o0enuortk7y07pt.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fh6fa7o0enuortk7y07pt.png)

Once I felt somewhat comfortable with transformers, I turned my attention to the art and science of prompt engineering. I quickly realized that while transformers can generate impressive text, their output quality often depends on how well the input prompt is crafted. This is where techniques like Chain of Thought (COT), REACT, and Tree of Thoughts come into play. By guiding the model’s reasoning process, we can achieve much more precise and accurate results.

However, prompt engineering isn’t a one-size-fits-all solution. The way you phrase your prompt can significantly impact the model’s performance. For instance, a simple rewording of the same question can lead to vastly different responses. There are many resources out there that helped me refine my approach to prompt design:

**YouTube Videos:**

- [AI prompt engineering: A deep dive by Anthropic](https://www.youtube.com/watch?v=T9aRN5JkmL8&pp=ygUScHJvbXB0IGVuZ2luZWVyaW5n)
- [Building with Anthropic Claude: Prompt Workshop with Zack Witten](https://www.youtube.com/watch?v=hkhDdcM5V94&t=1635s&pp=ygUccHJvbXB0IGVuZ2luZWVyaW5nIEFudGhyb3BpYw%3D%3D)
- [An AI Prompt Engineer Shares Her Secrets](https://www.youtube.com/watch?v=AxfmzLz9xXM&pp=ygUccHJvbXB0IGVuZ2luZWVyaW5nIEFudGhyb3BpYw%3D%3D)
- [Advanced Prompt Engineering: OpenAI Hackathon](https://www.youtube.com/watch?v=kr5X3QvPzvM&pp=ygUccHJvbXB0IGVuZ2luZWVyaW5nIEFudGhyb3BpYw%3D%3D)

**Blogs:**

- [Prompt Engineering Guide](https://www.promptingguide.ai/)
- [Eugene Yan's Prompting Guide](https://eugeneyan.com/writing/prompting/)
- [Leaked Prompts of GPTs on GitHub](https://github.com/linexjlin/GPTs?tab=readme-ov-file)
- [https://substack.com/@cwolferesearch/p-143156742](https://substack.com/@cwolferesearch/p-143156742)
- [https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/](https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/)
- [A collection of prompts, system prompts and LLM instructions](https://github.com/0xeb/TheBigPromptLibrary)
- [Prompt Engineering Guide](https://www.promptingguide.ai/)
- [Prompt Engineering Toolkit build by Uber](https://www.uber.com/en-IN/blog/introducing-the-prompt-engineering-toolkit/)
- [Prompt Optimization](https://arize.com/course/prompt-optimization/)
- [CO-STAR Framework for Prompt Structuring](https://www.linkedin.com/pulse/prompt-engineering-deep-dive-mastering-co-star-framework-mittal-xlqhe)
- [Anthropic's Prompt Engineering Interactive Tutorial](https://docs.google.com/spreadsheets/d/19jzLgRruG9kjUQNKtCg1ZjdD6l6weA6qRXG5zLIAhC8/edit#gid=1733615301)
- [Brex's prompt engineering guide](https://github.com/brexhq/prompt-engineering)
- [Meta's prompt engineering guide](https://llama.meta.com/docs/how-to-guides/prompting/)
- [Google's Gemini prompt engineering guide](https://services.google.com/fh/files/misc/gemini-for-google-workspace-prompting-guide-101.pdf)
- [How I think about LLM prompt engineering](https://fchollet.substack.com/p/how-i-think-about-llm-prompt-engineering) (Francois Chollet, 2023)

I gone through some research paper about prompt engineering that might helpfull for you

- [Graph-enhanced Large Language Models in Asynchronous Plan Reasoning](https://arxiv.org/abs/2402.02805)
- [A Prompt Pattern Catalog to Enhance Prompt Engineering with ChatGPT](https://arxiv.org/pdf/2302.11382)
- [Chain-of-Thought Prompting Elicits Reasoning in Large Language Models](https://arxiv.org/pdf/2201.11903)
- [Automatic Chain of Thought Prompting in Large Language Models](https://arxiv.org/pdf/2210.03493)
- [Self-Consistency Improves Chain of Thought Reasoning in Language Models](https://arxiv.org/pdf/2203.11171)
- [SYNERGIZING REASONING AND ACTING IN LANGUAGE MODELS](https://arxiv.org/pdf/2210.03629)
- [Instance-adaptive Zero-shot Chain-of-Thought Prompting](https://arxiv.org/pdf/2409.20441)
- [Larger language models do in-context learning differently](https://arxiv.org/abs/2303.03846) (Wei et al., 2023)

**Tools to Enhance Your Prompt Engineering Skills**

As with any craft, having the right tools is essential. While prompt engineering can be a time-consuming process, there are tools out there that can help streamline it. For example, platforms like [Zenbase](https://zenbase.ai/) automate model selection and prompt generation, reducing the manual effort involved. Similarly, tools like [Prompt Optimizer](https://github.com/hinthornw/promptimizer) and [EvalLM](https://evallm.kixlab.org/) offer interactive features that help fine-tune prompts by evaluating them against user-defined criteria. These resources made it easier for me to experiment with different strategies and see what worked best.

### RAG

[![RAG](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F7tprkfiu7tpzykqg13zs.png)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F7tprkfiu7tpzykqg13zs.png)

The next exciting step was exploring **Retrieval-Augmented Generation (RAG)**. If you've been following the progression of AI and large language models (LLMs), you're probably aware that these models are extremely powerful but come with a limitation: they can only generate answers based on the data they’ve been trained on. But what happens when you need them to answer questions about topics or domains they haven’t encountered before? This is where RAG comes into play, making LLMs smarter by enabling them to access external knowledge bases in real-time.

**What is RAG?**

At its core, **RAG** is a technique that combines the generative power of LLMs with the retrieval capability of external databases. This combination allows the model to pull in relevant information from a knowledge base or dataset, which it can then use to generate more informed, contextually accurate answers. Think of it as giving an LLM access to a vast, ever-updating pool of information, enabling it to provide answers even about topics it has never been directly trained on.

**Components of a RAG System**

A typical RAG system has three key components:

1. **Knowledge Base / External Corpus**: This is the external dataset or database the retriever will access for relevant information. It could be a static database, a dynamic source like a search engine, or even a set of documents that frequently update.
    
2. **Retriever**: The retriever’s job is to search the knowledge base for relevant documents or snippets of information. It uses various algorithms to fetch the most pertinent pieces of data to answer the query.
    
3. **LLM (Large Language Model)**: After the retriever pulls the necessary information, the LLM uses this data to generate a response. The LLM essentially "augments" its generation process by grounding its answers in the context provided by the retrieved data.
    

Some resources that will help you to get Started in RAG

- [Getting started with RAG](https://medium.com/neuml/getting-started-with-rag-9a0cca75f748)
- [Getting Started with Retrieval-Augmented Generation](https://cohere.com/llmu/rag-start)
- [How to Learn RAG in 2024](https://dev.to/llmware/become-a-rag-professional-in-2024-go-from-beginner-to-expert-41mg)
- [What is Retrieval-Augmented Generation (by Google)?](https://cloud.google.com/use-cases/retrieval-augmented-generation-)
- [RAG from Scratch without any Frameworks](https://www.youtube.com/watch?v=bmduzd1oY7U)

After getting comfortable with the core concepts of **Retrieval-Augmented Generation (RAG)**, I was eager to put my knowledge into practice. The next step in my journey was to explore frameworks that could help me implement RAG effectively. I began experimenting with some popular tools, including **[Langchain](https://www.langchain.com/)**, **[LlamaIndex](https://www.llamaindex.ai/)**, and **[RAGFlow](https://ragflow.io/)**. These frameworks allow you to quickly build RAG-based applications by combining external data retrieval with powerful generative models, and I’m excited to share my experiences in a separate blog post.

I wanted to deepen my understanding of RAG and its more advanced topics. I began reading research papers on the subject, and the more I explored, the more I realized that RAG is a much broader and richer field than I initially thought. Different variations of RAG exist, each catering to specific needs and challenges in information retrieval and generation.

- [RAG-Fusion: a New Take on Retrieval-Augmented Generation](https://arxiv.org/pdf/2402.03367)
- [Corrective Retrieval Augmented Generation](https://arxiv.org/pdf/2401.15884)
- [Retrieval Interleaved Generation (RIG)](https://docs.datacommons.org/papers/DataGemma-FullPaper.pdf)
- [LightRAG](https://github.com/HKUDS/LightRAG)
- [GRAPH RAG](https://microsoft.github.io/graphrag/)
- [HippoRAG: Neurobiologically Inspired Long-Term Memory for Large Language Models](https://arxiv.org/pdf/2405.14831)
- [Astute RAG: Overcoming Imperfect Retrieval Augmentation and Knowledge Conflicts for Large Language Models](https://arxiv.org/pdf/2410.07176)
- [HtmlRAG: HTML is Better Than Plain Text for Modeling Retrieved Knowledge in RAG Systems](https://arxiv.org/pdf/2411.02959)
- [LightRAG: A More Efficient Solution than GraphRAG for RAG Systems?](https://www.youtube.com/watch?v=oageL-1I0GE&t=359s)
- [GraphRAG: LLM-Derived Knowledge Graphs for RAG](https://www.youtube.com/watch?v=r09tJfON6kE)

RAG optimization

- [Strategies for Optimal Performance of RAG](https://medium.com/@bijit211987/strategies-for-optimal-performance-of-rag-6faa1b79cd45)
- [Advance RAG- Improve RAG performance](https://luv-bansal.medium.com/advance-rag-improve-rag-performance-208ffad5bb6a)
- [Advanced RAG: Chunking, Embeddings, and Vector Databases 🚀 | LLMOps](https://www.youtube.com/watch?v=tTW3dOfyCpE&t=1026s&pp=ygUOTExtIGVtYmVkZGluZyA%3D)
- [Building Production-Ready RAG Applications: Jerry Liu](https://www.youtube.com/watch?v=TRjq7t2Ms5I)

#### Advanced RAG concepts

**Chunking Methods**

- **Naive chunking** divides text into fixed-length character chunks, fast but lacks document structure consideration.
- **Sentence splitting** breaks text into sentences using NLP tools like NLTK or SpaCy, offering more precision.
- **Recursive character text splitting** combines character-based and structure-aware chunking, optimizing chunk size while preserving document flow.
- **Structural chunkers** split text based on document schema (e.g., HTML or Markdown), adding metadata for context.
- **Semantic chunking** groups semantically similar sentences using embedding models, creating more coherent chunks at a higher computational cost. check out the nice article to visulize the chunking [here](https://towardsdatascience.com/a-visual-exploration-of-semantic-text-chunking-6bb46f728e30)

**Retrieval algorithms** play a key role in RAG systems, helping to efficiently find relevant data.

- **Cosine Similarity** and **Euclidean Distance** measure similarity between vectors, while **Graph-Based RAG** and **Exact Nearest Neighbor (k-NN)** search for related information.
- **HNSW** and **Product Quantization (PQ)** optimize searches by creating scalable graph structures and reducing storage requirements.
- **Locality-Sensitive Hashing (LSH)** accelerates lookups by hashing similar vectors, and **BM25**, a term-based algorithm, ranks documents based on query term frequency and relevance.

**Retrieval types** used in RAG systems to enhance the quality and relevance of information retrieval:

1. **Rank GPT**: After querying a vector database, the system asks the LLM to rank the retrieved documents based on relevance to the query. The re-ranked documents are then sent back to the LLM for final generation, improving the response quality.
    
2. **Multi-Query Retrieval**: Instead of relying on a single query, this method first sends the user query to the LLM and asks it to suggest additional or related queries. These new queries are then used to fetch more relevant information from the database, enriching the response.
    
3. **Contextual Compression**: The LLM is asked to extract and provide only the most relevant portions of a document, reducing the amount of context that needs to be processed. This helps optimize the input to the LLM, ensuring more focused and efficient responses.
    
4. **Hypothetical Document Embedding**: The LLM is tasked with generating a "hypothetical" document that could best answer the query. This hypothetical document is then used as a prompt to retrieve relevant data from the database, aligning the response more closely with the user’s needs.
    

**RAG Evaluation** relies on a set of key metrics to assess the quality of retrieval-augmented generation outputs. These metrics ensure that the response is not only accurate but also closely tied to the retrieved context.

1. **Context Relevance**: This measures whether the documents retrieved are truly relevant to the user query. If the context is unrelated, the final response will likely be inaccurate or incomplete.
    
2. **Answer Relevance**: This checks if the model's response addresses the query effectively. Even if the context is relevant, the answer must be directly tied to it to be useful.
    
3. **Groundedness**: This ensures that the response is well-supported by the retrieved context. A grounded response refers to one that is clearly backed by the information found in the relevant documents, avoiding hallucinated or fabricated details.
    

Here are some resources to get started with advanced RAG:

- **[RAG Techniques](https://github.com/NirDiamant/RAG_Techniques)**: A GitHub repository that compiles techniques, methods, and best practices for working with RAG systems.
- **[Beyond the Basics of RAG](https://github.com/ruc-nlpir/flashrag)**: Advanced topics and concepts for pushing the limits of RAG technology.

### Vector DB

If you've delved into **RAG (Retrieval Augmented Generation)**, you probably already understand the crucial role that **vector databases** play in optimizing retrieval and generation processes. Vector databases store embeddings—high-dimensional representations of data—that allow for fast similarity searches and efficient retrieval of relevant information.

Unlike traditional databases that rely on keyword matching, vector databases use algorithms to measure the proximity of data points (e.g., cosine similarity or Euclidean distance) in vector space, making them ideal for working with unstructured data like text, images, and audio.

Some popular **vector database solutions** include:

- **Qdrant**
- **Pinecone**
- **Weaviate**
- **Faiss** (by Facebook)
- **Milvus**
- **Chroma**

Before diving into how **vector databases** work, it's important to understand the concept of **embeddings**, as they form the foundation of how data is represented and searched in vector databases. Embeddings are numerical representations of data (like text, images, or audio) that capture their semantic meaning in a high-dimensional space. These embeddings allow algorithms to measure the similarity between different data points, which is essential for tasks like semantic search and recommendation systems.

Here are some great resources to learn about embeddings:

- [Word Embedding and Word2Vec, Clearly Explained!!!](https://www.youtube.com/watch?v=viZrOnJclY0&pp=ygUjTExtIGVtYmVkZGluZyAgd29ya3MgdW5kZXIgdGhlIGhvb2Q%3D)
- [BERT Research - Ep. 2 - WordPiece Embeddings](https://www.youtube.com/watch?v=zJW57aCBCTk)
- [Embeddings: What they are and why they matter](https://simonwillison.net/2023/Oct/23/embeddings/)
- [Word Embeddings](https://www.youtube.com/watch?v=5PL0TmQhItY)
- [$0 Embeddings (OpenAI vs. free & open source)](https://www.youtube.com/watch?v=QdDoFfkVkcw&t=3109s)

Once you're comfortable with **embeddings**, diving into **vector databases** will be much easier, as most of the core concepts are similar across different databases. The main differences usually lie in the syntax and some specific features each database offers. Vector databases are designed to store, index, and retrieve high-dimensional vectors (such as those generated by embeddings), enabling fast similarity searches.

- [Understanding How Vector Databases Work!](https://www.youtube.com/watch?v=035I2WKj5F0)
- [A fun and absurd introduction to Vector Databases by Alexander Chatzizacharias](https://www.youtube.com/watch?v=mQGf9hWTqSw&t=337s)
- [Embeddings & Vector Stores](https://www.kaggle.com/whitepaper-embeddings-and-vector-stores)

Advanced concepts in vector DB

- [Qantization](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-quantization)
- [Indexing and Performance Optimization](https://medium.com/intel-tech/optimize-vector-databases-enhance-rag-driven-generative-ai-90c10416cb9c)
- [How We Made PostgreSQL as Fast as Pinecone for Vector Data](https://www.timescale.com/blog/how-we-made-postgresql-as-fast-as-pinecone-for-vector-data/)
- [PostgreSQL Hybrid Search Using Pgvector and Cohere](https://www.timescale.com/blog/postgresql-hybrid-search-using-pgvector-and-cohere/)
- [Vector Similarity Search with PostgreSQL’s pgvector – A Deep Dive](https://severalnines.com/blog/vector-similarity-search-with-postgresqls-pgvector-a-deep-dive/)
- [Build, scale, and manage user-facing Retrieval-Augmented Generation applications using postgress](https://r2r-docs.sciphi.ai/introduction/what-is-r2r)
- [Vector Search: Under the Hood](https://www.youtube.com/watch?v=FpWQ3qs33QE)

That's all for Today

Now that you have a solid foundation with resources on **Transformer**, **embeddings**, **vector databases**, and **RAG (Retrieval Augmented Generation)**, you're well-equipped to dive deeper into **generative AI**. The concepts we've covered are essential building blocks that will help you understand how AI models retrieve, process, and generate information based on the data they’re trained on.

Whether you're building applications using RAG or exploring the complexities of large-scale vector searches, these resources will guide you step-by-step in mastering the field.

Stay tuned for my next blog where I'll dive into more advanced topics like **Structured Output with LLMs**, **LLM Observability**, **LLM Evaluation**, and **Agents** and **projects using genrative AI**. These are crucial areas that will elevate your understanding and usage of large language models, allowing you to build more sophisticated, efficient, and reliable AI systems. Keep an eye out for more insights coming soon!

Until then, happy exploring! 😊
