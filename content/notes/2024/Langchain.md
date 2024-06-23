+++
title = 'Langchain'
date = 2024-05-25T09:42:14.1414+05:30
draft = true
tags =[]
+++ 



The concepts in langchain

- Prompt template
- chains
- Models
- Retrivers
- Agent 
- Tools
- Output
- Memory


## Prompt templates

Prompt templates help to translate user input and parameters into instructions for a language model. This can be used to guide a model's response, helping it understand the context and generate relevant and coherent language-based output.

 types of prompt templates
 
 ### **String PromptTemplates**
 
 ```python
from langchain_core.prompts import PromptTemplate

prompt_template = PromptTemplate.from_template("Tell me a joke about {topic}")

prompt_template.invoke({"topic": "cats"})
```

### ChatPromptTemplates

These prompt templates are used to format a list of messages.







## Chains 

### Sequential Chain

A sequential chain is a chain that combines various individual chains, where the output of one chain serves as the input for the next in a continuous sequence. It operates by running a series of chains consecutively.







## Langgraph

https://github.dev/emarco177/langgaph-course







lamaindex







https://github.dev/Codium-ai/cover-agent


## Agent

```python

from langchain.llms.openai import OpenAI
from langchain.agents import load_tools, initialize_agent, AgentType

llm = OpenAI()

tools = load_tools(["python_repl"])
agent = initialize_agent(
			tools, 
			llm, 
			agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION, 
			verbose=True
		)

result = agent("What are the prime numbers until 20?")
print(result)
```



## Retrievers 

- **BM25 retriever**: This retriever uses the BM25 algorithm to rank documents based on their relevance to a given query
- **TF-IDF retriever**: This retriever uses the TF-IDF (Term Frequency-Inverse Document Frequency) algorithm to rank documents based on the importance of terms in the document collection
- **Dense retriever**: This retriever uses dense embeddings to retrieve documents. It encodes documents and queries into dense vectors, and calculates the similarity between them using cosine similarity or other distance metrics.
- **kNN retriever:** This utilizes the well-known k-nearest neighbors algorithm to retrieve relevant documents based on their similarity to a given query.

```python

from langchain.retrievers import KNNRetriever
from langchain.embeddings import OpenAIEmbeddings

words = ["cat", "dog", "computer", "animal"]
retriever = KNNRetriever.from_texts(words, OpenAIEmbeddings())
result = retriever.get_relevant_documents("dog")
print(result)
```


## Memory

#### Conversation buffers
```python

from langchain.memory import ConversationBufferMemory
from langchain.chains import ConversationChain
# Creating a conversation chain with memory
memory = ConversationBufferMemory()
llm = ChatOpenAI(
model_name="gpt-3.5-turbo", temperature=0, streaming=True
)
chain = ConversationChain(llm=llm,verbose=True, memory=memory)
# User inputs a message
user_input = "Hi, how are you?"
# Processing the user input in the conversation chain
response = chain.predict(input=user_input)
# Printing the response
print(response)
# User inputs another message
user_input = "What's the weather like today?"
# Processing the user input in the conversation chain
response = chain.predict(input=user_input)
# Printing the response
print(response)
# Printing the conversation history stored in memory
print(memory.chat_memory.messages)

```
- Unlike **ConversationBufferMemory** , which retains all previous interactions, **ConversationBufferWindowMemory** only keeps the last k interactions, where k is the window size specified

#### ZepMemory

[Zep](https://github.com/getzep/zep) persists and recalls chat histories, and automatically generates summaries and other artifacts from these chat histories. It also embeds messages and summaries, enabling you to search Zep for relevant context from past conversations. Zep does all of this asyncronously, ensuring these operations don't impact your user's chat experience. Data is persisted to database, allowing you to scale out when growth demands.