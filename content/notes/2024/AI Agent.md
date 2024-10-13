+++
title = 'AI Agent'
date = 2024-07-30T06:27:13.1313+05:30
draft = false
tags =[]
+++ 

## Multi-Agent Orchestrator
Flexible and powerful framework for managing multiple AI agents and handling complex conversations



## RAG

### Retrevial types

	Rank GPT
- instead of just querying in vector and sending to LLM after querying ask LLM can you rank the doc that fetched from vecotr db based relvant to the query and again send to LLM with re ranked doc

Multi query retrieval 
- Send the user query to LLM and ask can you suggest revelant query to this query get that and use that query to get from db

Contextual compression
- Ask the LLM can you give relavant part that required for the doc by asking this we reducing the context then again send to LLM

Hypothetical document embedding
- ask LLM to suggest Hypothetical document for query and use that to fetch from DB

### RAG Fusion

How it works
1. **Multi-Query Generation:** RAG-Fusion generates multiple versions of the user's original query. As we've discussed above, this is different to single query generation, which traditional RAG does. This allows the system to explore different interpretations and perspectives, which significantly broadens the search's scope and improvs the relevance of the retrieved information. 
	- Use AI to generate the multiple version of the user query 
	
2. **Reciprocal Rank Fusion (RRF):** In this technique, we combine and re-rank search results based on relevance. By merging scores from various retrieval strategies, RAG-Fusion ensures that documents consistently appearing in top positions are prioritized, which makes the response more accurate.

3. **Improved Contextual Relevance:** Because we consider multiple interpretations of the user's query and re-ranking results, RAG-Fusion generates responses that are more closely aligned with user intent, which makes the answers more accurate and contextually relevant.

**Resources**
- [ Not RAG, but RAG Fusion?](https://pub.towardsai.net/not-rag-but-rag-fusion-understanding-next-gen-info-retrieval-477788da02e2)
- https://github.com/Raudaschl/rag-fusion 

###  CRAG

Corrective Retrieval Augmented Generation.The strategy we followed for this let’s say for each topic, we consult the book and identify relevant sections. Before forming an opinion, categorize the gathered information into three groups: `**Correct**`, `**Incorrect**`, and `**Ambiguous**`. Process each type of information separately. Then, based on this processed information, compile and summarize it mentally

**How it works**
- **Retrieval Evaluator:** A lightweight model assesses the relevance of retrieved documents to the input query, assigning a confidence score to each document. This evaluator is fine-tuned on datasets with relevance signals, allowing it to distinguish relevant documents from irrelevant ones, even if they share surface-level similarities with the query.
- **Action Trigger:** Based on the confidence scores, CRAG triggers one of three actions:
    - **Correct:** If at least one document has a high confidence score, CRAG assumes the retrieval is correct and refines the retrieved documents to extract the most relevant knowledge strips.
        - **Example:** If the query is "What is Henry Feilden's occupation?" and a retrieved document mentions Henry Feilden's political affiliation, CRAG would identify this as relevant and refine the document to focus on the information about his occupation.
    - **Incorrect:** If all documents have low confidence scores, CRAG assumes the retrieval is incorrect and resorts to web search for additional knowledge sources.
        - **Example:** If the query is "Who was the screenwriter for Death of a Batman?" and the retrieved documents do not contain the correct information, CRAG would initiate a web search using keywords like "Death of a Batman, screenwriter, Wikipedia" to find more reliable sources.
    - **Ambiguous:** If the confidence scores are neither high nor low, CRAG combines both refined retrieved knowledge and web search results.
- **Knowledge Refinement:** For relevant documents, CRAG employs a decompose-then-recompose approach:
    - It breaks documents into smaller knowledge strips, filters out irrelevant strips based on their relevance scores, and then recomposes the remaining relevant strips into a concise knowledge representation.
- **Web Search:** When the initial retrieval fails, CRAG utilizes web search to find complementary information.
    - It rewrites the input query into keyword-based search queries and prioritizes results from authoritative sources like Wikipedia to mitigate the risk of biases and unreliable information from the open web.
    - The retrieved web content is then refined using the same knowledge refinement method.

Check out implementation from langchain [here](https://langchain-ai.github.io/langgraph/tutorials/rag/langgraph_crag/#use-the-graph)
### Contextual Retrieval

Contextual Retrieval is a technique that enhances the accuracy of retrieving relevant information from a knowledge base, especially when used in Retrieval-Augmented Generation (RAG) systems. It addresses the limitations of traditional RAG, which often disregards context, by adding relevant context to the individual chunks of information before they are embedded and indexed. This process significantly improves the system's ability to locate the most pertinent information.

- This technique applies the BM25 ranking function, which relies on lexical matching for finding specific terms or phrases, to the contextualized chunks.
- Contextual Retrieval leverages AI models like Claude to generate the contextual text for each chunk
- It required more window context model and space


## Retrieval Interleaved Generation (RIG)

A technique devloped by google that enhances the factual accuracy of large language models (LLMs) by integrating structured external data into their responses.Unlike standard LLMs that rely solely on their internal knowledge, RIG dynamically retrieves data from a trusted repository like Data Commons.This allows [RIG](https://research.google/blog/grounding-ai-in-reality-with-a-little-help-from-data-commons/) to provide more accurate and reliable information by leveraging real-time data.

**How it works**

1. **Identifying the Need for External Data**: The LLM recognizes when a query requires external information beyond its internal knowledge, distinguishing between general knowledge and specific data-driven facts.
  
2. **Generating a Natural Language Query**: Once external data is needed, the model generates a natural language query, designed to interact with a data repository like Data Commons, for example, "What was the unemployment rate in California in 2020?"

3. **Fetching and Integrating Data**: The model sends the query to Data Commons through an API, retrieves relevant data from diverse datasets, and seamlessly incorporates it into its response.

4. **Providing a Verified Answer**: The LLM uses the retrieved data to give a reliable, data-backed answer, reducing the risk of hallucinations and improving the trustworthiness of its output.

### Late Chunking
- Let say we have doc as `Berin is captial of germany it is more then 3M population` if we chunk as `Berin is captial of germany` and `it is more then 3M population` in second chunk we loose the context to avoid that 
- We first applies the transformer layer of the embedding model to _the entire text_ or as much of it as possible. This generates a sequence of vector representations for each token that encompasses textual information from the entire text. Subsequently, mean pooling is applied to each chunk of this sequence of token vectors, yielding embeddings for each chunk that consider the entire text's contex
- It required large context window model 

`The process of converting a sequence of embeddings into a sentence embedding is called pooling`

Mean pooling in Natural Language Processing (NLP) is a technique used to create a fixed-size vector representation from a variable-length input sequence, such as a sentence or document. It works by averaging the vectors of all tokens (words or subwords) in the sequence

1. **Tokenization**: The input text is split into tokens (words, subwords, or characters).
2. **Embedding**: Each token is mapped to a corresponding vector using an embedding layer (e.g., pre-trained embeddings like Word2Vec, GloVe, or BERT embeddings).
3. **Mean Pooling**: The vectors of all the tokens in the sequence are averaged to produce a single vector. This can be done by summing the vectors and then dividing by the total number of tokens.
### Resources

**Frameworks and Toolkits for RAG (Retrieval-Augmented Generation)**:
1. **[Pinecone Canopy](https://github.dev/pinecone-io/canopy)**: A framework built by Pinecone for developing vector-based applications.
2. **[FlashRAG](https://github.com/ruc-nlpir/flashrag)**: A Python toolkit for efficient RAG research, designed to support retrieval-augmented generation use cases.
3. **[RAGFlow](https://ragflow.io/)**: An open-source RAG engine focusing on deep document understanding for more advanced RAG applications.

**Evaluation and Experimentation for LLMs**:
1. **[Trulens](https://github.com/truera/trulens)**: A tool for evaluating and tracking LLM experiments. Useful for monitoring and analyzing the performance of your RAG or LLM models.

**Courses and Learning Resources**:
1. **[Introduction to RAG by Ben](https://parlance-labs.com/education/rag/ben.html)**: Educational material to help understand the fundamentals and implementation of Retrieval-Augmented Generation (RAG).
2. **[LLM Twin Course](https://github.com/decodingml/llm-twin-course)**: A course focused on using twin architectures for RAG-based research and projects.

 **Research Repositories and Articles**:
1. **[RAG Techniques](https://github.com/NirDiamant/RAG_Techniques)**: A GitHub repository that compiles techniques, methods, and best practices for working with RAG systems.
2. **[Beyond the Basics of RAG](https://github.com/ruc-nlpir/flashrag)**: Advanced topics and concepts for pushing the limits of RAG technology.
3.  [Microsoft AutoGen](https://microsoft.github.io/autogen/)**: A toolkit provided by Microsoft to automate the process of generating language models and leveraging RAG workflows.

---

This structure groups similar resources together and will help streamline your workflow when referencing frameworks, tools, or educational content for RAG-based development.
## Micro Agent

The idea of a micro agent is to
1. Create a definitive test case that can give clear feedback if the code works as intended or not, and
2. Iterate on code until all test cases pass

https://github.com/BuilderIO/micro-agent 


## Crew AI

Alternative to auto gen



- Shorterm memory
- long term memory
- enitiy memory

Tools
-  Agent Level: The Agent can use the Tool(s) on any Task it performs.
- Task Level: The Agent will only use the Tool(s) when performing that specific Task.

**Note**: Task Tools override the Agent Tools.

## Nerve
 Red team with AI
**[Nerve](https://github.com/evilsocket/nerve) is a tool that creates stateful agents with any LLM — without writing a single line of code.** Agents created with Nerve are capable of both planning and enacting step-by-step whatever actions are required to complete a user-defined task.



## Agent with no code

- [dify](https://github.com/langgenius/dify) 
- https://mindpal.space/



Agent flow
- graph based 
- event based