+++
title = 'AI Agent'
date = 2024-07-30T06:27:13.1313+05:30
draft = true
tags =[]
+++ 

## Multi-Agent Orchestrator
Flexible and powerful framework for managing multiple AI agents and handling complex conversations



### RAG


https://github.com/zahaby/intro-llm-rag

https://github.dev/pinecone-io/canopy -> Framework build by pinecone 

https://github.com/truera/trulens Evaluation and Tracking for LLM Experiments

https://microsoft.github.io/autogen/
https://github.com/decodingml/llm-twin-course 
A Python Toolkit for Efficient RAG Research https://github.com/ruc-nlpir/flashrag 

[RAGFlow](https://ragflow.io/) is an open-source RAG (Retrieval-Augmented Generation) engine based on deep document understanding.
 Beyond the Basics of RAG
 
https://parlance-labs.com/education/rag/ben.html

https://github.com/NirDiamant/RAG_Techniques

https://github.com/ruc-nlpir/flashrag

## Retrevial types

Rank GPT
- instead of just querying in vector and sending to LLM after querying ask LLM can you rank the doc that fetched from vecotr db based relvant to the query and again send to LLM with re ranked doc


Multi query retrieval 
- Send the user query to LLM and ask can you suggest revelant query to this query get that and use that query to get from db

Contextual compression
- Ask the LLM can you give relavant part that required for the doc by asking this we reducing the context then again send to LLM

Hypothetical document embedding
- ask LLM to suggest Hypothetical document for query and use that to fetch from DB

## RAG Fusion

How it works
1. **Multi-Query Generation:** RAG-Fusion generates multiple versions of the user's original query. As we've discussed above, this is different to single query generation, which traditional RAG does. This allows the system to explore different interpretations and perspectives, which significantly broadens the search's scope and improvs the relevance of the retrieved information. 
	- Use AI to generate the multiple version of the user query 
	
2. **Reciprocal Rank Fusion (RRF):** In this technique, we combine and re-rank search results based on relevance. By merging scores from various retrieval strategies, RAG-Fusion ensures that documents consistently appearing in top positions are prioritized, which makes the response more accurate.

3. **Improved Contextual Relevance:** Because we consider multiple interpretations of the user's query and re-ranking results, RAG-Fusion generates responses that are more closely aligned with user intent, which makes the answers more accurate and contextually relevant.

**Resources**
- [ Not RAG, but RAG Fusion?](https://pub.towardsai.net/not-rag-but-rag-fusion-understanding-next-gen-info-retrieval-477788da02e2)
- https://github.com/Raudaschl/rag-fusion 
##  CRAG

Corrective Retrieval Augmented Generation.The strategy we followed for this let’s say for each topic, we consult the book and identify relevant sections. Before forming an opinion, categorize the gathered information into three groups: `**Correct**`, `**Incorrect**`, and `**Ambiguous**`. Process each type of information separately. Then, based on this processed information, compile and summarize it mentally


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

