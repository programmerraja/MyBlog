---
title: Langchain
date: 2024-05-25T09:42:14.1414+05:30
draft: false
tags:
  - genrative_ai
---



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
from langchain_core.prompts import ChatPromptTemplate


prompt_template = PromptTemplate.from_template("Tell me a joke about {topic}")

prompt = prompt_template.invoke({"topic": "cats"}) # return as human message format

messages = [
			("system","you are .."),
			("human","tell me ${msg}")
]
prompt = ChatPromptTemplate.from_messages(messages)
prompt.invoke({msg:""})
```

### ChatPromptTemplates

These prompt templates are used to format a list of messages.


## Chains 

chains has support `invoke`, `ainvoke`, `stream`, `astream`, `batch`, `abatch`, `astream_log` calls
### Sequential Chain

A sequential chain is a chain that combines various individual chains, where the output of one chain serves as the input for the next in a continuous sequence. It operates by running a series of chains consecutively.

```python
from dotenv import load_dotenv
from langchain.prompts import ChatPromptTemplate
from langchain.schema.runnable import RunnableLambda, RunnableSequence
from langchain_openai import ChatOpenAI

model = ChatOpenAI(model="gpt-4")

prompt_template = ChatPromptTemplate.from_messages([
("system", "You are a comedian who tells jokes about {topic}."),
("human", "Tell me {joke_count} jokes.")])

# Create individual runnables (steps in the chain)

format_prompt = RunnableLambda(lambda x: prompt_template.format_prompt(**x))

invoke_model = RunnableLambda(lambda x: model.invoke(x.to_messages()))

parse_output = RunnableLambda(lambda x: x.content)

# Create the RunnableSequence (equivalent to the LCEL chain)
chain = RunnableSequence(first=format_prompt, middle=[invoke_model], last=parse_output)

# the above line is equlient to 
chain = prompt_template | model | StrOutputParser()

# Run the chain
response = chain.invoke({"topic": "lawyers", "joke_count": 3})



# Output

print(response)
```

### Parallel Chain

```python
from dotenv import load_dotenv
from langchain.prompts import ChatPromptTemplate
from langchain.schema.output_parser import StrOutputParser
from langchain.schema.runnable import RunnableParallel, RunnableLambda
from langchain_openai import ChatOpenAI

# Create a ChatOpenAI model
model = ChatOpenAI(model="gpt-4o")

# Define prompt template
prompt_template = ChatPromptTemplate.from_messages(
    [
        ("system", "You are an expert product reviewer."),
        ("human", "List the main features of the product {product_name}."),
    ]
)


# Define pros analysis step
def analyze_pros(features):
    pros_template = ChatPromptTemplate.from_messages(
        [
            ("system", "You are an expert product reviewer."),
            ("human","Given these features: {features}, list the pros of these features."),
        ]
    )
    return pros_template.format_prompt(features=features)


# Define cons analysis step
def analyze_cons(features):
    cons_template = ChatPromptTemplate.from_messages(
        [
            ("system", "You are an expert product reviewer."),
            ("human","Given these features: {features}, list the cons of these features.",
            ),
        ]
    )
    return cons_template.format_prompt(features=features)


# Combine pros and cons into a final review
def combine_pros_cons(pros, cons):
    return f"Pros:\n{pros}\n\nCons:\n{cons}"


# Simplify branches with LCEL
pros_branch_chain = (
    RunnableLambda(lambda x: analyze_pros(x)) | model | StrOutputParser()
)

cons_branch_chain = (
    RunnableLambda(lambda x: analyze_cons(x)) | model | StrOutputParser()
)

# Create the combined chain using LangChain Expression Language (LCEL)
chain = (
    prompt_template
    | model
    | StrOutputParser()
    | RunnableParallel(branches={"pros": pros_branch_chain, "cons": cons_branch_chain})
    | RunnableLambda(
        lambda x: combine_pros_cons(x["branches"]["pros"], x["branches"]["cons"])
    )
)

# Run the chain
result = chain.invoke({"product_name": "MacBook Pro"})

# Output
print(result)

```

After parallel chain it will return as dict of branches that we provided in to get the the output of that use `x["branches"]["pros"]` 

### Chain branching

based on the condition we can choose which branch we can choose for further operation

```python

from dotenv import load_dotenv
from langchain.prompts import ChatPromptTemplate
from langchain.schema.output_parser import StrOutputParser
from langchain.schema.runnable import RunnableBranch
from langchain_openai import ChatOpenAI

# Load environment variables from .env
load_dotenv()

# Create a ChatOpenAI model
model = ChatOpenAI(model="gpt-4o")

# Define prompt templates for different feedback types
positive_feedback_template = ChatPromptTemplate.from_messages(
    [
        ("system", "You are a helpful assistant."),
        ("human", "Generate a thank you note for this positive feedback: {feedback}."),
    ]
)

negative_feedback_template = ChatPromptTemplate.from_messages(
    [
        ("system", "You are a helpful assistant."),
        ("human", "Generate a response addressing this negative feedback: {feedback}."),
    ]
)



# Define the feedback classification template
classification_template = ChatPromptTemplate.from_messages(
    [
        ("system", "You are a helpful assistant."),
        (
            "human",
            "Classify the sentiment of this feedback as positive, negative : {feedback}.",
        ),
    ]
)

# Define the runnable branches for handling feedback
branches = RunnableBranch(
    (
        lambda x: "positive" in x,
        positive_feedback_template
        | model
        | StrOutputParser(),  # Positive feedback chain
    ),
    (
        lambda x: "negative" in x,
        negative_feedback_template
        | model
        | StrOutputParser(),  # Negative feedback chain
    )
)

# Create the classification chain
classification_chain = classification_template | model | StrOutputParser()

# Combine classification and response generation into one chain
chain = classification_chain | branches


review = (
    "The product is terrible. It broke after just one use and the quality is very poor."
)
result = chain.invoke({"feedback": review})

# Output the result
print(result)

```

### internal  of chains

```python
from abc import ABC, abstractmethod


class CRunnable(ABC):
    def __init__(self):
        self.next = None

    @abstractmethod
    def process(self, data):
        """
        This method must be implemented by subclasses to define
        data processing behavior.
        """
        pass

    def invoke(self, data):
        processed_data = self.process(data)
        if self.next is not None:
            return self.next.invoke(processed_data)
        return processed_data

    def __or__(self, other):
        return CRunnableSequence(self, other)


class CRunnableSequence(CRunnable):
    def __init__(self, first, second):
        super().__init__()
        self.first = first
        self.second = second

    def process(self, data):
        return data

    def invoke(self, data):
        first_result = self.first.invoke(data)
        return self.second.invoke(first_result)


class AddTen(CRunnable):
    def process(self, data):
        print("AddTen: ", data)
        return data + 10


class MultiplyByTwo(CRunnable):
    def process(self, data):
        print("Multiply by 2: ", data)
        return data * 2


class ConvertToString(CRunnable):
    def process(self, data):
        print("Convert to string: ", data)
        return f"Result: {data}"


a = AddTen()
b = MultiplyByTwo()
c = ConvertToString()

chain = a | b | c

result = chain.invoke(10)
print(result)

```


## Agent

Type of agent
- ChatAgent
- ConversationalAgent
- ConversationalChatAgent
- OpenAIAssistantFinish
- OpenAIFunctionsAgent
- create_react_agent
- LLMAgent
- RetrievalAgent
- HybridAgent
- ChainAgent

```python

from dotenv import load_dotenv
from langchain import hub
from langchain.agents import AgentExecutor, create_react_agent
from langchain_core.tools import Tool
from langchain_openai import AzureChatOpenAI


def get_current_time(*args, **kwargs):
    """Returns the current time in H:MM AM/PM format."""
    import datetime

    now = datetime.datetime.now()
    return now.strftime("%I:%M %p")


tools = [
    Tool(
        name="Time",
        func=get_current_time,
        description="Useful for when you need to know the current time",
    ),
]

# Pull the prompt template from the hub
# ReAct = Reason and Action
# https://smith.langchain.com/hub/hwchase17/react

# return the prompt template 
prompt = hub.pull("hwchase17/react")

llm = AzureChatOpenAI(deployment_name="")

agent = create_react_agent(
    llm=llm,
    tools=tools,
    prompt=prompt,
    stop_sequence=True,
)

agent_executor = AgentExecutor.from_agent_and_tools(
    agent=agent,
    tools=tools,
    verbose=True,
)

response = agent_executor.invoke({"input": "What time is it?"})

print("response:", response)

```

### AgentExecutor 
is a component that executes an agent's logic to generate a response to a given input. It's responsible for managing the agent's lifecycle, handling input and output, and providing a way to customize the agent's behavior.

An `AgentExecutor` is typically used to wrap an `Agent` instance and provide additional functionality, such as:

1. **Input processing**: The `AgentExecutor` can preprocess the input data before passing it to the agent.
2. **Output processing**: The `AgentExecutor` can postprocess the agent's output before returning it to the caller.
3. **Error handling**: The `AgentExecutor` can catch and handle errors raised by the agent during execution.
4. **Context management**: The `AgentExecutor` can manage the agent's context, such as maintaining a conversation history or storing intermediate results.
5. **Customization**: The `AgentExecutor` can provide hooks for customizing the agent's behavior, such as injecting additional data or modifying the agent's parameters
 AgentExecutor has `_call` function which is called by the `chain` and the call function will be responsible for communicating with agent
 
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


## Agents

the agent with the LLM, the prompt, and the tools. The agent is responsible for taking in input and deciding what actions to take. Crucially, the Agent does not execute those actions - that is done by the AgentExecutor

```python

from langchain.agents import create_tool_calling_agent
from langchain.tools.retriever import create_retriever_tool
from langchain_openai import ChatOpenAI
from langchain import hub

from langchain_community.document_loaders import WebBaseLoader  
from langchain_community.vectorstores import FAISS  
from langchain_openai import OpenAIEmbeddings  
from langchain_text_splitters import RecursiveCharacterTextSplitter

from langchain.agents import AgentExecutor

llm = ChatOpenAI(model="gpt-3.5-turbo-0125", temperature=0)

  
loader = WebBaseLoader("https://docs.smith.langchain.com/overview")  

docs = loader.load()  

documents = RecursiveCharacterTextSplitter(  
chunk_size=1000, chunk_overlap=200  
).split_documents(docs)  

vector = FAISS.from_documents(documents, OpenAIEmbeddings())

retriever = vector.as_retriever()

retriever_tool = create_retriever_tool(  
retriever,  
"langsmith_search",  
"Search for information about LangSmith. For any questions about LangSmith, you must use this tool!",  
)

tools = [retriever_tool]

prompt = hub.pull("hwchase17/openai-functions-agent")

agent = create_tool_calling_agent(llm, tools, prompt)

agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)

agent_executor.invoke({"input": "hi!"})

```

#### ZepMemory

[Zep](https://github.com/getzep/zep) persists and recalls chat histories, and automatically generates summaries and other artifacts from these chat histories. It also embeds messages and summaries, enabling you to search Zep for relevant context from past conversations. Zep does all of this asyncronously, ensuring these operations don't impact your user's chat experience. Data is persisted to database, allowing you to scale out when growth demands.


### Output Parser

**PydanticOutputParser**


### Langgraph

State and graph is core concepts in langgraph

State is dict the data used by agent will be write or read.

In graph each node is agent or tools and the edges connect nodes determine sequence of ops


```python

```


#### Memory

- **Persistence:** LangGraph's system for saving the graph's state so it can be accessed and used later. Think of it like saving your progress in a video game.
- **Checkpointers:** Special components that handle the process of saving the graph's state at various points. They're like the "save" button in a video game.
- **Checkpoints:** The actual saved snapshots of the graph's state. Imagine them as the individual save files in your game.
- **Threads:** A way to organize sets of checkpoints. Imagine playing the same game with multiple different characters each character would have its own set of save files, or threads.
- **State Snapshot (`StateSnapshot`):** This is a Python object that holds all the important information about a particular checkpoint. It's like opening a save file and seeing all the details of your game at that point.


- **`langgraph-checkpoint`:** The core library that defines how checkpointers work. It comes included with LangGraph and includes a simple checkpointer called `MemorySaver`.
- **`langgraph-checkpoint-sqlite` and `langgraph-checkpoint-postgres`:** More powerful checkpointers that use SQLite and Postgres databases, respectively. These are better for handling larger amounts of data or for use in production environments.



- https://github.dev/emarco177/langgaph-course
- https://github.com/pinecone-io/examples/blob/master/learn/generation/langchain/langgraph/00-langgraph-intro.ipynb
- https://colab.research.google.com/drive/1WemHvycYcoNTDr33w7p2HL3FF72Nj88i?usp=sharing [check this]
- https://langchain-ai.github.io/langgraph/concepts/agentic_concepts/#subgraphs








### Resources
- https://nanonets.com/blog/langchain/ 
- https://github.dev/bhancockio/langchain-crash-course 
- https://github.dev/Codium-ai/cover-agent


