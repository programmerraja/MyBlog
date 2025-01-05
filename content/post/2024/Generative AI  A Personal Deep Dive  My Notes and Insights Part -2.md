---
title: "Generative AI: A Personal Deep Dive  My Notes and Insights Part -2"
date: 2024-12-18T06:27:31.3131+05:30
draft: false
tags:
  - AI
  - genrative_ai
---

Welcome back to part 2 of my series, _Personal Deep Dive: My Notes and Insights_. If you found the first post useful, you’re in for more today. If you missed it, don’t worry — you can catch up here. Now, let’s get into the fascinating world of Artificial Intelligence.
## Structured Output 

**Why Structured Output Matters for AI**

When working with LLMs, one of the key considerations is how the model generates and outputs information. For any AI-driven system to be effective, especially when integrated into automation workflows, _structured output_ becomes an absolute necessity. You might wonder, "Why is structure so important?" Well, let's break it down.

In simple terms, structured output refers to the way AI organizes and formats its responses. Whether you’re using an LLM for data analysis, automation, or function calls, having outputs that follow a clear, predictable format ensures the information can be properly parsed and used by other systems. Without this structure, AI might generate responses that are too vague, incomplete, or incompatible with your application.

For instance, if you're building a chatbot to handle customer service inquiries, the AI needs to return responses in a structured format — like JSON — so the system can easily integrate with the backend and trigger the appropriate actions (e.g., escalating a query or updating a customer record).

But how do we get this structured output? There are several ways, and I’ll walk you through the most popular and effective approaches.

### **Leveraging Function Tools for Structured Output**

One of the most powerful capabilities of modern LLMs is _function calling_. This feature allows the model to recognize when specific functions should be invoked based on the user's prompt. Function calling isn’t just about answering questions; it's about crafting outputs that match the specific input requirements of an external system or workflow. For example, many LLMs, including GPT-3.5-turbo, GPT-4, Gemini-1.0, and Claude-3, offer function calling support, which allows them to generate responses as structured JSON.

### **Using JSON Mode for Precise Output**

If you’re using OpenAI’s API, there's a handy feature called _JSON mode_ that guarantees structured output. This feature ensures that your LLM response adheres to a predefined format (typically JSON), making it much easier to integrate the results with other systems. you can check more about it [here](https://platform.openai.com/docs/guides/structured-outputs)

### **The Role of Pydantic in Structuring LLM Outputs**

Now, let’s dive into some more technical territory. If you’re a developer, you might want to go a step further by validating the structure of the AI-generated output. This is where _Pydantic_, a Python library, comes in handy.

Pydantic helps you define clear data models with type hints. These models ensure that the data returned from the LLM is consistent and conforms to the expected structure. you can check [here](https://python.langchain.com/v0.1/docs/modules/model_io/output_parsers/types/pydantic/) 
### **Instructor: A User-Friendly Solution for Structured Data**

For those looking for an even easier way to manage structured outputs, the [_Instructor_](https://python.useinstructor.com/) library is a fantastic tool. Instructor is built on top of Pydantic and aims to simplify the process of getting structured data like JSON from LLMs.

With Instructor, you don’t have to manually manage data validation or retry logic. It automatically handles retries (using the Tenacity package) and can stream responses in real time. Plus, it integrates with many LLMs, including GPT-4, GPT-3.5, and open-source models like Mistral and Llama.

### **Advanced Techniques: Outlines and XGrammar**

For those diving deeper into structured output, there are more advanced tools worth exploring.

One such technique is the use of [_Outlines_](https://dottxt-ai.github.io/outlines/latest/) , [_XGrammar_](https://github.com/mlc-ai/xgrammar) and [BAML](https://docs.boundaryml.com/home) for efficient structured generation. Outlines uses regular expressions (regex) and finite state machines (FSMs) to generate structured JSON, making it ideal for high-performance LLM inference. XGrammar, an open-source library, takes this a step further by providing a portable backend that integrates seamlessly with LLMs, optimizing structured generation and reducing inference time.

Below are the some resources to learn more about structured output 

- [Every Way To Get Structured Output From LLMs](https://www.boundaryml.com/blog/structured-output-from-llms) 
- [LLMs are bad at returning code in JSON](https://aider.chat/2024/08/14/code-in-json.html) 
- [Structured text generation and robust prompting for language models](https://dottxt-ai.github.io/outlines/latest/)
- [Structured Outputs cohere]( https://docs.cohere.com/v2/docs/structured-outputs)
- [LLM Function Calling - AI Tools Deep Dive](https://www.youtube.com/watch?v=gMeTK6zzaO4)
- [Function Calling with LLMs](https://www.promptingguide.ai/applications/function_calling) 
- [Generating Structured Output with LLMs ](https://ankur-singh.github.io/blog/structured-output)
- [OpenAI DevDay 2024 | Structured outputs for reliable applications](https://www.youtube.com/watch?v=kE4BkATIl9c) 
- [The Best Way to Generate Structured Output from LLMs](https://www.instill.tech/blog/llm-structured-outputs)
- [Choosing the Best Structured Output Parser Approach | 3 Ways To Generate Structured Output](https://blog.gopenai.com/choosing-the-best-structured-output-parser-approach-3-ways-to-generate-structured-output-d9686482729c)

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

## LLM Wrapper

LLMs are incredibly powerful, the process of interacting with them  from making requests to handling their responses  can sometimes be cumbersome, especially if you’re not deeply familiar with the underlying technicalities.

This is where **LLM wrappers** come in. An LLM wrapper is essentially a library or framework that abstracts away the repetitive, lower-level details involved in interacting with an LLM. Think of it as a bridge that makes it easier and more efficient to communicate with these models by providing pre-built functionality and common abstractions. With a wrapper, you can focus on the big picture, while the wrapper handles the technical nuances.

There are several great libraries available, each with its own strengths and use cases. Based on my experience, here are a few of the most popular ones:

| **Name**                                      | **Description**                                                                                                                     | **Key Features**                                                                                                                                                     |
| --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [LlamaIndex](https://llamaindex.ai/)          | An open-source data framework optimized for building Retrieval Augmented Generation (RAG) applications.                             | Data Ingestion and Structuring, Efficient Indexing, Data Connectors for Diverse Sources                                                                              |
| [LangChain](https://langchain.com/)           | A framework for building applications powered by large language models (LLMs), such as chatbots, knowledge-based systems, and more. | Chainable Components for Advanced Workflows, Seamless Integration with Various LLMs, Support for Retrieval-Augmented Generation (RAG), Memory Management for Context |
| [AutoChain](https://www.autochain.ai/)        | A lightweight framework for building generative AI agents, known for its simplicity and ease of use.                                | Simplified Concepts Similar to LangChain, Support for OpenAI Function Calling, Basic Memory Tracking for Conversation History and Tool Outputs                       |
| [Vellum AI](https://vellum.ai/)               | A platform designed for product and engineering teams to build, evaluate, and deploy AI systems.                                    | Experimentation and Evaluation Tools, Deployment and Monitoring Capabilities, Collaboration Features                                                                 |
| [Flowise](https://flowiseai.com/)             | An open-source platform for creating LLM applications without coding using a drag-and-drop interface.                               | Code-Free LLM Development, Drag-and-Drop Interface, Integration with LangSmith and LangFuse                                                                          |
| [Galileo](https://www.rungalileo.io/)         | A toolkit for improving and fine-tuning LLM applications, with a focus on debugging and observability.                              | Prompt Management Tools, Guardrail Metrics, Integration with Various LLM Providers                                                                                   |
| [Klu AI](https://klu.ai/)                     | An LLM platform providing a unified API for accessing LLMs and integrating data sources.                                            | Unified API, Default Prompt Templates, Workflow Creation through Connecting Actions                                                                                  |
| [Braintrust](https://www.braintrustdata.com/) | A platform for evaluating, improving, and deploying LLMs with tools for prompt engineering and data management.                     | Prompt Playground, Logging and Debugging, Eval() Function for Output Scoring                                                                                         |
| [HumanLoop](https://humanloop.com/)           | Helps product teams develop reliable and scalable LLM-based applications by combining best practices with LLM-specific tools.       | Collaborative Prompt Workspace, Automated Evaluations, User Feedback Collection                                                                                      |
| [HoneyHive AI](https://www.honeyhive.ai/)     | Focused on evaluating, debugging, and monitoring production LLM applications with an emphasis on observability.                     | Tracing Execution Flows, Customizable Event Feedback, Dataset Creation for Fine-Tuning                                                                               |
| [Parea AI](https://www.parea.ai/)             | A platform for debugging, testing, and monitoring LLM applications, providing tools for the entire LLM workflow.                    | Prompt Experimentation, Performance Evaluation, Log and Trace Observability                                                                                          |

## AI Agents

After the advent of **RAG (Retrieval-Augmented Generation)**, the spotlight is now turning toward **AI Agents**. If you've been keeping an eye on AI trends, you may have noticed the growing buzz around agents — and for good reason. These intelligent systems are capable of autonomous decision-making and executing tasks based on given goals, making them a crucial advancement in AI.

An **AI agent** is essentially an autonomous system that can perform tasks and make decisions based on a set of goals and inputs, typically using machine learning models and algorithms. Unlike traditional AI systems that might only answer questions or perform a single, predefined task, AI agents are designed to execute sequences of tasks, interact with other systems, learn from their environment, and adapt to new information or situations.

Here some resources that help you learn about AI agents 

1. **[CS 194/294-196 (LLM Agents) - Lecture 1, Denny Zhou](https://www.youtube.com/live/QL-FS_Zcmyo)**  
2. **[AI Agents' Secret Sauce](https://www.youtube.com/watch?v=MRYqhbtLTmM)**  
3. **[16 Months of Building AI Agents in 60 Minutes](https://www.youtube.com/watch?v=AWQ6DaCy46U)**  
4. **[18 Months of Building Autonomous AI Agents in 42 Minutes](https://www.youtube.com/watch?v=8N2_iXC16uo)**  
5. **[Using Agents to Build an Agent Company: Joao Moura](https://www.youtube.com/watch?v=Dc99-zTMyMg)**  
6.  **[Agentic Design Patterns Part 1](https://www.deeplearning.ai/the-batch/how-agents-can-improve-llm-performance/)**  
7. **[Multi AI Agent Systems with crewAI](https://learn.deeplearning.ai/courses/multi-ai-agent-systems-with-crewai/lesson/1/introduction)**  
8. **[Agents in Llamaindex](https://docs.llamaindex.ai/en/stable/understanding/putting_it_all_together/agents/)**  
9. **[Memory in Agent Systems](https://www.newsletter.swirlai.com/p/memory-in-agent-systems)**  

**Advanced**
-  **[AI Agent Mastery: Agent Architectures](https://www.youtube.com/watch?v=kRZE7gRCPQA&list=PL86ARIu_ElO7HOm7cVgzfEs_NwSdfhHFA)**  
    Series on mastering AI agents, covering:
    - Agent architectures
    - Agent frameworks
    - Evaluating agents
    - Handling agents stuck in loops

- **[LLM Agents Learning Platform](https://llmagents-learning.org/f24)**  
    Comprehensive resource for learning about LLM agents with various topics, such as:
    - Foundation of LLMs
    - Reasoning, planning, and tool use
    - LLM agent infrastructure
    - Retrieval-augmented generation
    - Code generation and data science
    - Multimodal agents and robotics
    - Evaluation and benchmarking
    - Privacy, safety, and ethics
    - Human-agent interaction and personalization
    - Multi-agent collaboration


## Evaluating AI

Evaluating AI models is an essential step in the AI development lifecycle. Without a proper evaluation process, it's difficult to determine whether the AI is performing as expected, or if there are issues that need to be addressed. This is particularly crucial for **Large Language Models (LLMs)**, where the complexity and scale of tasks can lead to unexpected results or suboptimal performance. Evaluating the outputs, behaviors, and predictions of these models helps ensure that they are doing the job correctly and allows you to spot areas where improvements are necessary.

Below are the some concepts that I learned during studying about Evaluating AI
### **Types of Metrics Used for Evaluating LLMs**

To evaluate large language models effectively, several types of metrics are used. These metrics can be broadly classified into **intrinsic**, **extrinsic**, and **hybrid** categories, each serving a specific purpose.

#### **1. Intrinsic Metrics**

Intrinsic metrics evaluate the internal workings of the model, focusing on its quality and natural language generation capabilities. These metrics help assess how well the model is functioning from a linguistic or statistical perspective.

- **Perplexity**: Measures how well the model predicts the next word in a sequence. A lower perplexity score means the model is more confident and accurate in its predictions.
    
- **Fluency**: Measures the coherence and naturalness of the generated text. It’s essentially an assessment of how human-like the AI's output sounds.
    
- **BLEU (Bilingual Evaluation Understudy) Score**: Commonly used for translation tasks, this metric evaluates the similarity between the generated text and a reference text. It’s a way of quantifying how well the model's output matches a "correct" answer.

#### **2. Extrinsic Metrics**

Extrinsic metrics focus on evaluating how well the model performs specific tasks, often related to business goals or real-world applications.

- **Accuracy**: Measures the proportion of correct predictions or answers made by the model, typically used for classification tasks.

- **F1 Score**: A combination of precision and recall, this score is valuable for tasks with imbalanced data. It strikes a balance between avoiding false positives and minimizing false negatives.

- **ROUGE (Recall-Oriented Understudy for Gisting Evaluation) Score**: Used for summarization tasks, ROUGE measures the overlap between the generated summary and the reference summary in terms of recall, precision, and F1 score.

#### **3. Hybrid Metrics**

Hybrid metrics combine both intrinsic and extrinsic measures to provide a more comprehensive evaluation of the model’s performance.

- **METEOR**: Used primarily for machine translation tasks, METEOR considers not just the exact matches between the generated and reference text, but also synonyms and word order. This makes it more flexible than BLEU in evaluating quality.

- **GEVAL**: Another hybrid metric that evaluates the overall quality of the text generation while considering both syntactical and semantic aspects.

### **Libraries and Platforms for LLM Evaluation**

There are several powerful libraries and platforms that can help you evaluate your LLMs effectively. These tools assist in different aspects of evaluation, from basic performance metrics to more advanced tasks like **hallucination detection**, **bias analysis**, and **prompt evaluation**.

Here’s a list of useful tools and libraries for evaluating LLMs:

|**Tool/Library**|**Description**|**Use Case**|
|---|---|---|
|**[DeepEval](https://docs.confident-ai.com/)**|An open-source LLM evaluation framework that simplifies the process of evaluating model performance.|General LLM evaluation framework for various tasks.|
|**[SelfcheckGPT](https://github.com/potsawee/selfcheckgpt)**|A zero-resource tool for detecting hallucinations in generative LLMs.|Hallucination detection in LLM-generated text.|
|**[LLM-as-Judge](https://arxiv.org/pdf/2306.05685)**|Uses an LLM as the evaluator to assess another LLM's outputs.|Evaluating one model using another model (meta-evaluation).|
|**[Agent-as-a-Judge](https://arxiv.org/pdf/2410.10934)**|Evaluates agents with agents, focusing on multi-agent system performance.|Multi-agent evaluation for complex systems.|
|**[Chainpoll](https://docs.galileo.ai/galileo-ai-research/chainpoll)**|A flexible technique for LLM-based evaluation, powered by Galileo platform.|LLM-based evaluation across different domains and tasks.|
|**[Prometheus](https://huggingface.co/papers/2405.01535)**|A family of language models specialized in evaluating other LLMs.|Cross-evaluation of multiple LLMs using specialized models.|
|**[EvalLM](https://evallm.kixlab.org/)**|A tool that helps prompt designers evaluate and compare generated outputs on user-defined criteria.|Prompt optimization and evaluation of LLM outputs.|
|**[ChainForge](https://www.chainforge.ai/)**|An open-source visual programming environment for evaluating LLM robustness and model performance.|Testing prompt robustness and model stability.|
|**[SPADE](https://blog.langchain.dev/spade-automatically-digging-up-evals-based-on-prompt-refinements/)**|System for prompt analysis and delta-based evaluation.|Analyzing prompt effectiveness and adjusting for optimal results.|
|**[Giskard](https://www.giskard.ai/)**|A platform that helps assess bias, performance, and security issues in AI models.|Bias and security analysis for AI models in production.|
|**[TrueLens](https://www.trulens.org/)**|A tool that allows users to evaluate, iterate faster, and select the best LLM application.|Iterative evaluation of LLMs in production environments.|
|**[Quotient AI](https://www.quotientai.co/)**|An AI development and evaluation platform combining research and real-world applications.|Prototyping and fast evaluation of AI products in real-world settings.|
|**[Inspect AI](https://inspect.ai-safety-institute.org.uk/)**|A framework for large language model evaluations developed by the UK AI Safety Institute.|Ethical AI evaluation and performance monitoring.|

## Observablity

After Evaluation, **observability** plays a crucial role in ensuring AI systems are working as intended. Without observability, you may miss key issues such as performance drops, biased outputs, or unexpected behaviors.

Below are the some tool that i have explored that might helps you to get started

| **Tool**                                                                 | **Features**                                                                                                                                                 | **LLM Integration**                                                                                                                                         | **Pros & Cons**                                                                                                                                                                                                                                                             |
| ------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Helicone](https://us.helicone.ai/dashboard)                             | Playground, Datasets, Rate Limiting, Webhooks, Alerts, Cache                                                                                                 | OpenAI, Azure, Anthropic                                                                                                                                    | Comprehensive set of features, ideal for various use cases                                                                                                                                                                                                                  |
| [LangFuse](https://langfuse.com/)                                        | Prompt Management, Trace Logging, User Tracking, Evaluation                                                                                                  | Langfuse is model agnostic for tracing/observability, and supports openai, azure openai, anthropic, aws bedrock, and google vertex for playground and evals | Langfuse is very simple to self-host at production scale on any cloud vendor (only OSS dependencies) compared to many of the other solutions, might be something interesting to highlight as I think the other solutions are less scalable or more complex to host yourself |
| [Phoenix](https://app.phoenix.arize.com/)                                | Model Tracing, Model Evaluation                                                                                                                              | -                                                                                                                                                           | Suitable for deep model analysis but lacks transparency in hosting and integration details                                                                                                                                                                                  |
| [OPENLIT](https://docs.openlit.io/latest/connections/intro)              | Prompt Management, Model Tracing, Cost Tracking                                                                                                              | OpenAI, Azure, Anthropic                                                                                                                                    | Helm chart available for easy deployment; flexible integrations                                                                                                                                                                                                             |
| [Langtrace](https://github.com/Scale3-Labs/langtrace?tab=readme-ov-file) | Prompt Management, Model Tracing, Cost Tracking                                                                                                              | OpenAI, Azure, Anthropic                                                                                                                                    | Open-source with strong tracking features, but limited integration options                                                                                                                                                                                                  |
| [Lunary](https://lunary.ai/)                                             | Prompt Management, Model Tracing, Cost Tracking, Multi-User Support                                                                                          | OpenAI                                                                                                                                                      | Offers multi-user support but limited integration with other LLMs                                                                                                                                                                                                           |
| [Arize](https://app.arize.com/)                                          | Model Tracing, Evaluation, Retrieval (RAG) Analysis, Datasets, Fine-Tuning Export, Annotations, Human Feedback, Experiments, Embedding Analysis, Data Export | OpenAI, Azure, Google Cloud                                                                                                                                 | Excellent for Retrieval-Augmented Generation (RAG), including playground and evaluation features, but requires enterprise subscription                                                                                                                                      |

Resources 
- [A Survey on Hallucination in Large Language Models](https://github.com/LuckyyySTA/Awesome-LLM-hallucination?tab=readme-ov-file)
- [Evaluating the Effectiveness of LLM-Evaluators](https://eugeneyan.com/writing/llm-evaluators/)
- [A framework for few-shot evaluation of language models. ](https://github.com/EleutherAI/lm-evaluation-harness) 
- [LLM Evaluation Skills Are Easy to Pick Up](https://freedium.cfd/https://towardsdatascience.com/llm-evaluation-techniques-and-costs-3147840afc53) 
- [How to Cook Good AI Products with What You Already Have in your Data Warehouse](https://www.youtube.com/watch?v=G87yQv5CMq8)
- [Optimizing RAG Through an Evaluation-Based Methodology](https://qdrant.tech/articles/rapid-rag-optimization-with-qdrant-and-quotient/)
- [A comprehensive guide to LLM evaluation methods](https://github.com/alopatenko/LLMEvaluation?tab=readme-ov-file)

## My Hackthon expereince


**After diving into the world of Large Language Models (LLMs) and getting comfortable with their capabilities, I decided to take the next step and work on some projects.** I started looking for interesting project ideas, and that’s when I discovered an exciting opportunity: a hackathon organized by [TimeScale](https://www.timescale.com/), featuring an Open Source AI Challenge in collaboration with **[pgai](https://github.com/timescale/pgai)** and **[Ollama](https://ollama.com/)** on dev.to

For this challenge, I built an **AI-Powered Chat App for Websites, YouTube, Files, and Dev.to Articles**. You can read more about it in this [article](https://dev.to/programmerraja/a-flexible-ai-powered-chat-app-for-websitesyoutube-files-and-devto-articles-4ihh). Unfortunately, I didn’t win the hackathon, but the experience was incredibly valuable.

**Next, I participated in another hackathon, this time organized by [AssemblyAI](https://assembly.ai/dev2).** In this one, I developed a tool called _“Boost Your Sales Calls with Free AI-Powered Analysis & Practice.”_ While I didn’t win this one either, you can check out more details about the project [here](https://dev.to/programmerraja/boost-your-sales-calls-with-free-ai-powered-analysis-practice-40jn).

**Though I didn’t win either hackathon, I gained practical experience and learned a lot from both projects.** The process of building and refining these applications has significantly deepened my understanding of AI and how it can be applied in real-world scenarios.

**Looking ahead, I’m excited to continue building more cool AI-driven projects.** If you have any interesting project ideas, feel free to leave a comment below—I’m always open to new suggestions.

Finally, I’ve compiled all the AI-related code I’ve worked on in a GitHub repository. While I’ve done my best to clean it up, be aware that it might still have a few bugs here and there! You can check it out [here](https://github.com/programmerraja/AI-learning-code).

**That’s all for today!** My next goal is to dive into advanced topics like fine-tuning models, retaining context, and much more. Once I’ve learned those concepts, I’ll be sure to share my progress in a future blog post. Until then, happy learning!




After learning and getting comfortable with LLM I have planed to do some projects so i am started looking for the project ideas and i find there i hackthon conducted by a [TimeScale](https://www.timescale.com/) was  the Open Source AI Challenge with **[pgai](https://github.com/timescale/pgai)** and **[Ollama](https://ollama.com/)** where i have  build the **AI-Powered Chat App for Websites,Youtube, Files, and Dev.to Articles** you can check out more in this [articel](https://dev.to/programmerraja/a-flexible-ai-powered-chat-app-for-websitesyoutube-files-and-devto-articles-4ihh) But unlucly  i have not won the hackthon 

Next i have particpated the hackthon conducted by  [AssemblyAI](https://assembly.ai/dev2) where i have build the  Boost Your Sales Calls with Free AI-Powered Analysis & Practice unluclky i have not won this hackthon also you can check [here](https://dev.to/programmerraja/boost-your-sales-calls-with-free-ai-powered-analysis-practice-40jn) for more 

But my participating two hackthon i have gained some practial knowelege Next i am planing to build some other cool projects using AI if you any ideas feel free to comment it 

FInally here the repo where i have put all my AI realated code that i have mentioned above you can check it out but it may have bugs :sweat simle 

That all today So next i am plaing to learn finetuning , reataing context and many more once i learned i will share that in next blog unitl then bye happy learning










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
