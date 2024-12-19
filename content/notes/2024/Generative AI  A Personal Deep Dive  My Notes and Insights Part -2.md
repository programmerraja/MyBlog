---
title: "Generative AI: A Personal Deep Dive  My Notes and Insights Part -2"
date: 2024-12-18T06:27:31.3131+05:30
draft: true
tags:
---


### **Structured Output with LLM**

When we working with LLM structured output is very important because structured output is needed for api here some libaray helps

function tools

Function calling is a capability provided by the LLM. From the user prompt, it determines whether a predefined function needs to be called and generates an output that matches the function’s input requirements as a JSON response. Many popular LLMs like gpt-3.5-turbo, gpt-4 gemini-1.0, claude-3, and llama-3 support a form of function calling.
#### Pydantic

Pydantic, a popular Python library with over 70 million downloads per month, offers a robust and developer-friendly way to structure your prompts and validate LLM output. Here's how it works:

- **Define data models with type hints:** Pydantic allows you to create classes that represent the structure of your desired output using Python type hints.

- **Automatic validation and parsing:** When you pass data to a Pydantic model, it automatically validates the data against the defined types and converts it to the appropriate Python objects.

- **Seamless integration with OpenAI function calling:** Pydantic models can be easily converted to JSON schema, which can be used with OpenAI function calling to ensure that the LLM output conforms to your expectations.

## JSON Mode:
#### Outlines

Generate structured JSON using regular expressions (regex) and finite state machines (FSMs). This technique is used in a library called Outlines to make Large Language Model (LLM) inference faster.

https://python.useinstructor.com/

https://docs.boundaryml.com/home

**BAML is a domain-specific language to generate structured outputs from LLMs — with the best developer experience.**
#### xgrammar

[XGrammar](https://github.com/mlc-ai/xgrammar) is an open-source library for efficient, flexible, and portable structured generation. It supports general context-free grammar to enable a broad range of structures while bringing careful system optimizations to enable fast executions. XGrammar features a minimal and portable C++ backend that can be easily integrated into multiple environments and frameworks, and is co-designed with the LLM inference engine and enables zero-overhead structured generation in LLM inference.


https://blog.gopenai.com/choosing-the-best-structured-output-parser-approach-3-ways-to-generate-structured-output-d9686482729c

| Library                                                                                                         | Method                       | Description                                                |
| --------------------------------------------------------------------------------------------------------------- | ---------------------------- | ---------------------------------------------------------- |
| [langchain](https://python.langchain.com/docs/modules/model_io/output_parsers/types/pydantic/)                  | Prompting & function calling | Pydantic output parser as part of langchain                |
| [llama_index](https://docs.llamaindex.ai/en/stable/module_guides/querying/structured_outputs/pydantic_program/) | Prompting & function calling | Pydantic program as part of llama_index                    |
| [guidance](https://github.com/guidance-ai/guidance)                                                             | Constrained token sampling   | Programming paradigm for constrained generation            |
| [outlines](https://github.com/outlines-dev/outlines)                                                            | Constrained token sampling   | Constrained token sampling using CFGs²                     |
| [instructor](https://github.com/jxnl/instructor)                                                                | Function calling             | Specify Pydantic models to define structure of LLM outputs |
| [marvin](https://github.com/prefecthq/marvin)                                                                   | Function calling             | Toolbox of task-specific OpenAI API wrappers               |
| [spacy-llm](https://github.com/explosion/spacy-llm)                                                             | Prompting                    | spaCy plugin to add LLM responses to a pipeline            |
| [fructose](https://github.com/bananaml/fructose)                                                                | Function calling             | LLM calls as strongly-typed functions                      |
| [mirascope](https://github.com/Mirascope/mirascope)                                                             | Function calling             | Prompting, chaining and structured information extraction  |
| [texttunnel](https://github.com/qagentur/texttunnel)                                                            | Function calling             | Efficient async OpenAI API function calling                |

Resources
- [Every Way To Get Structured Output From LLMs](https://www.boundaryml.com/blog/structured-output-from-llms) 
- [LLMs are bad at returning code in JSON](https://aider.chat/2024/08/14/code-in-json.html) 
- [Structured text generation and robust prompting for language models](https://dottxt-ai.github.io/outlines/latest/)
- [Structured Outputs cohere]( https://docs.cohere.com/v2/docs/structured-outputs)
- [Generating Structured Output with LLMs ](https://ankur-singh.github.io/blog/structured-output)
- [The Best Way to Generate Structured Output from LLMs](https://www.instill.tech/blog/llm-structured-outputs)


## Library

i have explored with langchain and llamaindex  where i  my personal favoruite is llamaindex the 


## Agents

After Rag Agent are getting popular in so jump in to learning about AI 

Here the some resources to help you to get started in AI
- [CS 194/294-196 (LLM Agents) - Lecture 1, Denny Zhou](https://www.youtube.com/live/QL-FS_Zcmyo)
- [AI Agents' Secret Sauce](https://www.youtube.com/watch?v=MRYqhbtLTmM)
- [16 Months of Building AI Agents in 60 Minutes](https://www.youtube.com/watch?v=AWQ6DaCy46U)
- [18 Months of Building Autonomous AI Agents in 42 Minutes](https://www.youtube.com/watch?v=8N2_iXC16uo)
- [Using agents to build an agent company: Joao Moura](https://www.youtube.com/watch?v=Dc99-zTMyMg)
- [AI Agent Mastery: Agent Architectures](https://www.youtube.com/watch?v=kRZE7gRCPQA&list=PL86ARIu_ElO7HOm7cVgzfEs_NwSdfhHFA)
	- Agent architecures
	- Agent frameworks
	- Evalating Agents
	- Agent stuck in loop

- https://llmagents-learning.org/f24
-
 - Foundation of LLMs
- Reasoning
- Planning, tool use
- LLM agent infrastructure
- Retrieval-augmented generation
- Code generation, data science
- Multimodal agents, robotics
- Evaluation and benchmarking on agent applications
- Privacy, safety and ethics
- Human-agent interaction, personalization, alignment
- Multi-agent collaboration



- [Agentic Design Patterns Part 1](https://www.deeplearning.ai/the-batch/how-agents-can-improve-llm-performance/)
- [Multi AI Agent Systems with crewAI](https://learn.deeplearning.ai/courses/multi-ai-agent-systems-with-crewai/lesson/1/introduction)
- [Agents in Llamaindex](https://docs.llamaindex.ai/en/stable/understanding/putting_it_all_together/agents/)
- [Memory in Agent Systems](https://www.newsletter.swirlai.com/p/memory-in-agent-systems)
- 


Multi Agent


## Observablity

Observalibity in AI curcial because without observablity we not able to find how fell AI is performing and easy and we have lot of option but based on our need we can choose the tool i have did some reasearch i have shared below if you know any other good tool free to commient it 

Obserblaitu also important for monitoring the amount we spend for AI 

| **Tool**                                                                 | **Features**                                                                                                                                                 | **LLM Integration**         | **Pros & Cons**                                                                                                                        |
| ------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| [Helicone](https://us.helicone.ai/dashboard)                             | Playground, Datasets, Rate Limiting, Webhooks, Alerts, Cache                                                                                                 | OpenAI, Azure, Anthropic    | Comprehensive set of features, ideal for various use cases                                                                             |
| [LangFuse](https://langfuse.com/)                                        | Prompt Management, Trace Logging, User Tracking, Evaluation                                                                                                  | -                           | Lacks prompt playground and Role-Based Access Control (RBAC) for self-hosted setups                                                    |
| [Phoenix](https://app.phoenix.arize.com/)                                | Model Tracing, Model Evaluation                                                                                                                              | -                           | Suitable for deep model analysis but lacks transparency in hosting and integration details                                             |
| [OPENLIT](https://docs.openlit.io/latest/connections/intro)              | Prompt Management, Model Tracing, Cost Tracking                                                                                                              | OpenAI, Azure, Anthropic    | Helm chart available for easy deployment; flexible integrations                                                                        |
| [Langtrace](https://github.com/Scale3-Labs/langtrace?tab=readme-ov-file) | Prompt Management, Model Tracing, Cost Tracking                                                                                                              | OpenAI, Azure, Anthropic    | Open-source with strong tracking features, but limited integration options                                                             |
| [Lunary](https://lunary.ai/)                                             | Prompt Management, Model Tracing, Cost Tracking, Multi-User Support                                                                                          | OpenAI                      | Offers multi-user support but limited integration with other LLMs                                                                      |
| [Arize](https://app.arize.com/)                                          | Model Tracing, Evaluation, Retrieval (RAG) Analysis, Datasets, Fine-Tuning Export, Annotations, Human Feedback, Experiments, Embedding Analysis, Data Export | OpenAI, Azure, Google Cloud | Excellent for Retrieval-Augmented Generation (RAG), including playground and evaluation features, but requires enterprise subscription |


Evalution

Evaluting the AI will help us make sure that AI is doing the job correctly or we can find out does we doing any thing wrong here some lib that help you to get started

LLM metrics
### LLM Evaluation Metrics Types

**Intrinsic metrics**: evaluate the model's internal workings, such as perplexity and fluency.
- **Perplexity**: measures how well the model predicts a test dataset. Lower perplexity indicates better performance.
- **Fluency**: measures the coherence and naturalness of the generated text.
- **BLEU (Bilingual Evaluation Understudy) Score**: measures the similarity between the generated text and a reference text.

**Extrinsic metrics**: evaluate the model's performance on specific tasks, such as question-answering and text classification.
- **Accuracy**: measures the proportion of correct predictions or answers.
- **F1 Score**: measures the balance between precision and recall.
- **ROUGE (Recall-Oriented Understudy for Gisting Evaluation) Score**: measures the quality of generated summaries.

 **Hybrid metrics**: combine intrinsic and extrinsic metrics to provide a more comprehensive evaluation.
 - **METEOR (Metric for Evaluation of Translation with Explicit ORdering) Score**: measures the similarity between generated and reference translations, taking into account the order of the words.
- GEVAL  

Some Library and Platform that help you to evalute the LLM 

- [DeepEval](https://docs.confident-ai.com/)   The open-source LLM evaluation framework
- [SelfcheckGPT ](https://github.com/potsawee/selfcheckgpt)Zero-Resource Black-Box Hallucination Detection for Generative Large Language Models
- [LLM-as-Judge](https://arxiv.org/pdf/2306.05685)
- [Agent-as-a-Judge: Evaluate Agents with Agents](https://arxiv.org/pdf/2410.10934)
- [Chainpoll](https://docs.galileo.ai/galileo-ai-research/chainpoll)  is a powerful, flexible technique for LLM-based evaluation that is unique to Galileo. It is used to power multiple metrics across the Galileo platform.
- [Prometheus](https://huggingface.co/papers/2405.01535) is a family of open-source language models specialized in evaluating other language models
- [EvalLM](https://evallm.kixlab.org/) is an interactive system that aids prompt designers in iterating on **prompts** by evaluating and comparing generated outputs on user-defined **criteria**.
- [ChainForge](https://www.chainforge.ai/)  is an open-source visual programming environment for prompt engineering. With ChainForge, you can **evaluate the robustness of prompts and text generation models** in a way that goes beyond anecdotal evidence.
- [SPADE](https://blog.langchain.dev/spade-automatically-digging-up-evals-based-on-prompt-refinements/) (System for Prompt Analysis and Delta-based Evaluation) will suggest binary eval functions for your prompt that you can run on all future LLM responses on
- [Giskard](https://www.giskard.ai/) Protect your company against biases, performance & security issues in AI models
- [True lens](https://www.trulens.org/) Evaluate, iterate faster, and select your best LLM app with TruLens.
- [Quotient AI ](https://www.quotientai.co/)  is an advanced AI development and evaluation platform. We combine our experience building evals for top AI products with state-of-the-art research to make it simple and fast to prototype, evaluate, and ship AI products without the unnecessary technical overhead.
- [Inspect AI](https://inspect.ai-safety-institute.org.uk/)a framework for large language model evaluations created by the [UK AI Safety Institute](https://aisi.gov.uk/). 



- [A Survey on Hallucination in Large Language Models](https://github.com/LuckyyySTA/Awesome-LLM-hallucination?tab=readme-ov-file)
- [Evaluating the Effectiveness of LLM-Evaluators](https://eugeneyan.com/writing/llm-evaluators/)
- [A framework for few-shot evaluation of language models. ](https://github.com/EleutherAI/lm-evaluation-harness) 
- [LLM Evaluation Skills Are Easy to Pick Up](https://freedium.cfd/https://towardsdatascience.com/llm-evaluation-techniques-and-costs-3147840afc53) 
- [How to Cook Good AI Products with What You Already Have in your Data Warehouse](https://www.youtube.com/watch?v=G87yQv5CMq8)
- [Optimizing RAG Through an Evaluation-Based Methodology](https://qdrant.tech/articles/rapid-rag-optimization-with-qdrant-and-quotient/)



https://www.deeplearning.ai/the-batch/how-agents-can-improve-llm-performance/

https://github.com/alopatenko/LLMEvaluation?tab=readme-ov-file

My repo have code that might help full for you to get started may be not organized

Next i am exploring Memory in LLM and Finetuning so once i get enogh knoweledeg i will share that next blog untill happy learning
