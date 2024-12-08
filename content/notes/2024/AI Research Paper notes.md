---
title: AI Research Paper notes
date: 2024-09-19T06:01:47.4747+05:30
draft: false
tags:
  - genrative_ai
  - AI
---
## De-Hallucinator: Mitigating LLM Hallucinations in Code Generation Tasks via Iterative Grounding

### Steps of the De-Hallucinator:

1. **Initial Prompt**:

    - This is a standard query to a language model, such as asking it to generate code without any external context.
    - **Example**: You ask the model to write code for connecting to a database, but the model doesn't have enough context to provide the exact API or method to use.

2. **RAG (Retrieval-Augmented Generation)**:

    - To improve accuracy, De-Hallucinator performs a _retrieval_ step. It fetches relevant information or context (e.g., documentation or specific API references) related to the initial prompt to help guide the model.
    - **Example**: After the initial prompt, De-Hallucinator searches for information about database connection APIs that might be relevant, like APIs from the same programming environment.

3. **Iterative Prompt**:

    - Even after the retrieval step, the model might still produce code that's partially incorrect or "hallucinated" (wrong). So, De-Hallucinator uses the generated code itself as a clue and refines the next prompt. This iterative process continues, narrowing down on the correct API or function.
    - **Example**: If the model produces code using an API that looks similar but isn't quite right, De-Hallucinator refines the query using that generated code to search for project-specific APIs. In the next round, the model will generate code that is much more accurate.


## Promptriever: Instruction-Trained Retrievers Can Be Prompted Like Language Models

### Problem Statement

Modern information retrieval (IR) models generally match queries to passages based on a single semantic similarity score. As a result, the user experience of search can be opaque, with users needing to find particular keywords/phrasings, apply various filters in advanced search settings, and iterate based on previous searches to find the “just right" query that returns the desired passages.

They introduce Promptriever: a retrieval model that can instead be controlled via natural language prompts

### Solution 

- Promptriever is a **bi-encoder retriever**, meaning it encodes both the query and the document separately using a pre-trained large language model (LLaMA-2 7B, for example).
- Unlike traditional retrieval models that rely on fixed semantic similarity between queries and documents, **Promptriever is trained to follow instructions**.
- During training, each query is paired with **instance-level instructions** (e.g., "Find documents that discuss movies directed by James Cameron before 2022") that modify what is considered relevant for that specific query.
- These instructions are incorporated to adjust how the model understands "relevance" on a per-query basis.
- The model is trained on a **curated dataset of ~500K query-passage pairs**, where each pair is augmented with an instruction that defines relevance in detail.

Prompt used for instruction generation
```
## Input Data
I have the following query and REL_DOCS_NUM_FILL_ME documents which have been marked as relevant and NON_REL_DOCS_NUM_FILL_ME which are non-relevant.

**Query:** QUERY_FILL_ME  
**Positive Document:** POS_DOC_FILL_ME  
**Negative Document:** NEG_DOC_FILL_ME  

## Your task
I need you to come up with an instruction that can be appended onto the end of this query that will make only one relevant document and make all other documents (including previously relevant docs) non-relevant. You can choose which document will stay relevant to the new instruction by writing an instruction that applies to only one of the relevant documents (you choose). This additional instruction should provide a test for strong frontier language models to determine if they can follow instructions. Triple-check your work to be certain that the chosen document is still relevant and that the others are non-relevant – if you mess up, you will be fired. Do not give away the answer in the instruction!

For this example, please generate the instruction to be LENGTH_FORMAT_FILL_ME. In the instructions, provide detailed specifics for what makes a document relevant. Remember that this criteria should make the one document relevant and all others irrelevant. Also, be sure that the instruction is generic and does not contain the answer to the query.

**Output the response in JSON form only with no other text**, with the keys: 
- “instruction” (str) 
- “relevant_docs” (one document id that is the first doc, e.g. “[2]”) 
- “non-relevant_docs” (all other document ids, e.g. “[1,3,...]”). 

## Your output (JSON only):
```

DataSet:  `MS MARCO`


## AceCoder: Utilizing Existing Code to Enhance Code Generation

### Intro
A new method called AceCoder for generating code. This method aims to tackle two main challenges: understanding requirements and implementing code. AceCoder has two new features to help with these issues:

1. **Guided code generation**: This feature makes large language models (LLMs) first analyze the requirements and create a preliminary output, like test cases. This helps clarify what is needed and guides the LLMs on what to write.

2. **Example retrieval**: This feature finds similar programs to use as examples in prompts. These examples provide useful content, such as algorithms and APIs, and show the LLMs how to write the code.
### Solution 

When we directly ask A LLM to generate a code for snake game it may generate but not efficient but if we use give the proper requirement and determine specific details, e.g., input-output formats, and possible exceptions it can do better so to sovle that 

They Design a special prompt consisting of triple examples (i.e., ). A preliminary is a specific software artifact (e.g., test cases, APIs) for clarifying the requirement
Given a new requirement, based on the prompt, LLMs first output a preliminary and then generate code based on the preliminary

After understanding the requirement, how to implement the source code using a programming language is challenging

They propose using example retrieval, which is based on how human developers reuse code. In real-life situations, when faced with a new requirement, developers often look for similar programs to learn from or reuse parts of them.

Specifically, we use a retriever to find programs with similar requirements, selecting the top 20 results. Since large language models (LLMs) have a limit on input length (like 1024 tokens), we can only include a few examples in a prompt—typically three. To manage this, we designed a selector that filters out unnecessary programs and chooses the most helpful examples. These selected examples are then added to prompts to guide the LLMs on how to write the code.


Example:
- let say we want to write a unit test for the given program first we can send a program to LLM and ask for the what all the unit test case we can write for this funciton get a response
- use the response again send that to LLM and ask to write a test case 


## Code Chain

CodeChain introduces modularity in code generation by using chain-of-thought prompting to help LLMs break down solutions into modular segments, each representing a high-level task. To enhance this process, we implement a series of self-revisions based on sampled sub-modules:

1. We extract sub-modules from generated programs and group them into clusters. We then select centroid sub-modules as representative, reusable code parts.
2. We enhance the original prompts with these selected sub-modules and guide LLMs to create new modular solutions.

This method allows LLMs to leverage insights from past modular components, improving their future code generation like an experienced developer would.

COT prompt
```
Develop a well-structured Python solution for the provided problem that obeys the constraints and passes the example test cases. Ensure modularity while considering potential edge cases and failures. Start by outlining the required code modules, including function headers and signatures. Subsequently, proceed to implement each module to create the final code.

(modules) with clear function names and input/output specifications. Once the structure is ready, write the actual code for each module to complete the solution.
```


## Think Outside the Code: Brainstorming Boosts Large Language Models in Code Generation

### BRAINSTORM Framework

The BRAINSTORM framework decomposes the generation task into three steps:

1. **Brainstorming**: Multiple prompts are constructed and fed to a large language model (LLM) to generate diverse thoughts, which are high-level descriptions of potential solutions for the given problem.

2. **Thoughts Selection**: A neural model is utilized to rank and select the thought from the above step with the highest probability of solving the given problem. used (XLNet Model for this)

3. **Writing Code**: A code generation model implements code based on the problem description and the selected thought.

Prompt used to generate high level description for the problem

```
### Prompt 1

Your task is to read a problem description from Codeforces and provide a detailed explanation of your approach to solving the problem without including any code. Please ensure that your explanation is clear, concise, and easy to understand for someone who may not be familiar with the specific programming language or algorithm used.

In your response, please include:

1. **Problem Statement Overview**: 
   - Summarize the key points of the problem statement, including the main objective.

2. **Key Constraints and Requirements**: 
   - Highlight any important constraints or requirements that must be considered.

3. **Approach to the Problem**:
   - Explain how you approached the problem, detailing any relevant data structures, algorithms, or techniques used.

Please note that your response should be flexible enough to allow for various relevant and creative approaches to solving the problem.


PROMPT 2

Read a problem description on Codeforces and use your knowledge of algorithms, data structures, and mathematics to provide ideas for solving it. When giving an idea for solving a problem, please analyze the range of input values in detail to determine the appropriate time complexity to avoid timeout errors. 

Please note that your answers should not contain any form of code or programming language.

To make your problem-solving ideas more creative and unique, be sure to fully explain the algorithms, data structures, and mathematical concepts involved. At the same time, when discussing time complexity, please explain in as much detail as possible.


```


## Principled Instructions Are All You Need for Questioning LLaMA-1/2, GPT-3.5/4

This paper introduces 26 guiding principles designed to streamline the process of querying and prompting large language models.

| **Principle** | **Details**                                                                                                                                                                                                                                                                                                                                                                                            |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1             | No need to be polite with LLM, so there is no need to add phrases like “please,” “if you don’t mind,” “thank you,” “I would like to,” etc., and get straight to the point.                                                                                                                                                                                                                             |
| 2             | Integrate the intended audience in the prompt, e.g., the audience is an expert in the field.                                                                                                                                                                                                                                                                                                           |
| 3             | Break down complex tasks into a sequence of simpler prompts in an interactive conversation.                                                                                                                                                                                                                                                                                                            |
| 4             | Employ affirmative directives such as ‘do,’ while steering clear of negative language like ‘don’t’.                                                                                                                                                                                                                                                                                                    |
| 5             | When you need clarity or a deeper understanding of a topic, idea, or any piece of information, utilize the following prompts: <br> • Explain [insert specific topic] in simple terms. <br> • Explain to me like I’m 11 years old. <br> • Explain to me as if I’m a beginner in [field]. <br> • Write the [essay/text/paragraph] using simple English like you’re explaining something to a 5-year-old. |
| 6             | Add “I’m going to tip $xxx for a better solution!”                                                                                                                                                                                                                                                                                                                                                     |
| 7             | Implement example-driven prompting (Use few-shot prompting).                                                                                                                                                                                                                                                                                                                                           |
| 8             | When formatting your prompt, start with ‘###Instruction###,’ followed by either ‘###Example###’ or ‘###Question###’ if relevant. Subsequently, present your content. Use one or more line breaks to separate instructions, examples, questions, context, and input data.                                                                                                                               |
| 9             | Incorporate the following phrases: “Your task is” and “You MUST”.                                                                                                                                                                                                                                                                                                                                      |
| 10            | Incorporate the following phrases: “You will be penalized”.                                                                                                                                                                                                                                                                                                                                            |
| 11            | Use the phrase “Answer a question given in a natural, human-like manner” in your prompts.                                                                                                                                                                                                                                                                                                              |
| 12            | Use leading words like writing “think step by step”.                                                                                                                                                                                                                                                                                                                                                   |
| 13            | Add to your prompt the following phrase: “Ensure that your answer is unbiased and does not rely on stereotypes”.                                                                                                                                                                                                                                                                                       |
| 14            | Allow the model to elicit precise details and requirements from you by asking you questions until he has enough information to provide the needed output (for example, “From now on, I would like you to ask me questions to...”).                                                                                                                                                                     |
| 15            | To inquire about a specific topic or idea or any information and you want to test your understanding, you can use the following phrase: “Teach me the [Any theorem/topic/rule name] and include a test at the end, but don’t give me the answers and then tell me if I got the answer right when I respond”.                                                                                           |
| 16            | Assign a role to the large language models.                                                                                                                                                                                                                                                                                                                                                            |
| 17            | Use Delimiters.                                                                                                                                                                                                                                                                                                                                                                                        |
| 18            | Repeat a specific word or phrase multiple times within a prompt.                                                                                                                                                                                                                                                                                                                                       |
| 19            | Combine Chain-of-thought (CoT) with few-Shot prompts.                                                                                                                                                                                                                                                                                                                                                  |
| 20            | Use output primers, which involve concluding your prompt with the beginning of the desired output. Utilize output primers by ending your prompt with the start of the anticipated response.                                                                                                                                                                                                            |
| 21            | To write an essay/text/paragraph/article or any type of text that should be detailed: “Write a detailed [essay/text/paragraph] for me on [topic] in detail by adding all the information necessary”.                                                                                                                                                                                                   |
| 22            | To correct/change specific text without changing its style: “Try to revise every paragraph sent by users. You should only improve the user’s grammar and vocabulary and make sure it sounds natural. You should not change the writing style, such as making a formal paragraph casual”.                                                                                                               |
| 23            | When you have a complex coding prompt that may be in different files: “From now and on whenever you generate code that spans more than one file, generate a [programming language] script that can be run to automatically create the specified files or make changes to existing files to insert the generated code. [your question]”.                                                                |
| 24            | When you want to initiate or continue a text using specific words, phrases, or sentences, utilize the following prompt: <br> • I’m providing you with the beginning [song lyrics/story/paragraph/essay...]: [Insert lyrics/words/sentence]’. Finish it based on the words provided. Keep the flow consistent.                                                                                          |
| 25            | Clearly state the requirements that the model must follow in order to produce content, in the form of the keywords, regulations, hints, or instructions.                                                                                                                                                                                                                                               |
| 26            | To write any text, such as an essay or paragraph, that is intended to be similar to a provided sample, include the following instructions: <br> • Please use the same language based on the provided paragraph/title/text/essay/answer.                                                                                                                                                                |

## Unsupervised Evaluation of Code LLMs with Round-Trip Correctness

To evaluate large language models of code they introduced a new approach 

 **Round-Trip Correctness (RTC)** method for evaluating code LLMs:

1. **Forward Pass**: The LLM performs a coding task, such as converting code to text or applying edits to code based on natural language instructions.
  
2. **Backward Pass**: The LLM is then asked to reverse the task, generating code from the text description or reconstructing the original code before edits.

3. **Semantic Equivalence Check**: The final step involves comparing the original input with the output of the backward pass to ensure they are semantically equivalent. This can be done using:
   - **Discrete metrics** (e.g., exact match).
   - **Continuous metrics** (e.g., CodeBLEU, CodeBERTScore).
   - **Execution-based oracles** (e.g., unit test execution, if available).

## Planning In Natural Language Improves LLM Search For Code Generation

Suggest a  `PLANSEARCH`, a novel search algorithm that explicitly searches for a diverse set of plans, expressed in natural language, before generating code

**How it works**

Imagine a coding problem where the user needs to find the longest increasing subsequence within a given array.

Here's how PLANSEARCH would approach it:

- Prompting for First-Order Observations: PLANSEARCH starts by prompting the LLM with the problem and asking for general observations, not code. This encourages exploration of diverse solution avenues.
	- Example Prompt: "You are an expert Python programmer. Given the task of finding the longest increasing subsequence within an array, what are some useful observations or hints about this problem? Do not provide any code yet."

	- Possible LLM Observations: * "The order of elements in the subsequence matters." * "We need to keep track of previously encountered elements." * "The longest increasing subsequence might not be unique."

- Deriving Second-Order Observations: PLANSEARCH takes subsets of the first-order observations and feeds them back to the LLM, along with the original problem, to derive deeper insights.
	- Example Prompt (using a subset of observations from step 1): "You are an expert Python programmer. Here's a problem: find the longest increasing subsequence in an array. Consider these observations: 1) 'The order of elements in the subsequence matters.' 2) 'We need to keep track of previously encountered elements.' Building upon these, can you provide additional, more specific observations relevant to solving this problem? Remember, no code yet."
	- Possible LLM Observations (O2): * "We could use dynamic programming to store and update the length of the longest increasing subsequence ending at each element." * "Binary search could be used to efficiently find the position to insert a new element while maintaining the increasing order."

- From Observations to Code: PLANSEARCH prompts the LLM to generate a natural language solution, drawing upon the generated observations.
	- Example Prompt (using observations from steps 1 & 2): "Here's the problem: find the longest increasing subsequence within an array. Here are some helpful observations: [... list of observations from O1 and O2... ] Based on these observations, describe in natural language a strategy to solve this problem. Be clear and specific."
	- Possible LLM Solution Sketch: "We can iterate through the array, and for each element, use binary search to find the position in the previously computed subsequence where it can be inserted while maintaining the sorted order. We keep track of the longest subsequence length encountered."

- Implementation: The natural language solution is translated to pseudocode and finally into Python.5 This granular approach minimizes errors that might arise from directly generating code from complex observations.
	- Example: The LLM's solution sketch from step 3 would be translated first into pseudocode and then into functioning Python code.


## LLMS KNOW MORE THAN THEY SHOW: ON THE INTRINSIC REPRESENTATION OF LLM HALLUCINATIONS
 - 

## Does Prompt Formatting Have Any Impact on LLM Performance?

The key takeaway from the research  is that prompt formatting significantly influences LLM performance, and there's no universally optimal format across all models or tasks.

GPT-3.5-turbo's performance varied by up to 40% in a code translation task depending on the prompt template, while larger models like GPT-4 were more robust

Larger models like GPT-4 exhibited higher consistency scores compared to GPT-3.5, suggesting greater reliability in producing similar outputs across different prompt structures.
## Dataset

- HumanEval: Evaluating Large Language Models Trained on Code

- Mostly Basic Python Problems (MBPP): The benchmark consists of around 1,000 crowd-sourced Python programming problems, designed to be solvable by entry level programmers
- CoderEval , ClassEval  and EvoCodeBench




## TOOLSANDBOX

TOOLSANDBOX, a new benchmark for evaluating large language models' (LLMs) ability to use tools. It is designed to be more comprehensive and realistic than previous benchmarks by incorporating stateful tool execution, implicit state dependencies between tools, a built-in user simulator, and a dynamic evaluation strategy.

TOOLSANDBOX is a Python-based testing environment where a User, an Agent, and an Execution Environment interact to complete a task

TOOLSANDBOX uses a novel evaluation strategy based on Milestones and Minefields.
- Milestones are key events that must occur in a trajectory to successfully complete a task. They are defined as a directed acyclic graph (DAG) to capture temporal dependencies. Each milestone is associated with a similarity measure that quantifies how well a given turn in the trajectory matches the milestone.
- Minefields are events that should not occur in a trajectory. They are used to evaluate whether the Agent can recognize when a task is impossible to complete with the given tools and avoid hallucinating tool calls or arguments.18

The overall similarity score for a trajectory combines the scores from matching milestones and ensuring no minefields are violated.













