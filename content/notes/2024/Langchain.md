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
