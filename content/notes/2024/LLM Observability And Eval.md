+++
title = 'LLM Observability And Eval'
date = 2024-08-12T08:35:40.4040+05:30
draft = true
tags =[]
+++ 

## Eval

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

## G-Eval
 
 Using GPT-4 and chain-of-thoughts (CoT) approach to generate detailed evaluation steps for NLG outputs. 

**How G-EVAL Works**

1. **Text Embeddings**: G-EVAL uses GPT-4 to generate text embeddings for both the generated text and human-written reference texts.
2. **Similarity Computation**: The similarity between the generated text and human-written text is computed using a similarity metric, such as cosine similarity or dot product.
3. **Score Computation**: The similarity scores are aggregated to compute a final score, which reflects the overall quality of the generated text.

```txt
Coherence (1-5) - the collective quality of all sentences. We align this dimension with the DUC quality question of structure and coherence whereby "the summary should be well-structured and well-organized. The summary should not just be a heap of related information, but should build from sentence to a coherent body of information about a topic."

Evaluation Steps:

1. Read the news article carefully and identify the main topic and key points.
2. Read the summary and compare it to the news article. Check if the summary covers the main topic and key points of the news article, and if it presents them in a clear and logical order.
3. Assign a score for coherence on a scale of 1 to 5, where 1 is the lowest and 5 is the highest based on the Evaluation Criteria.


Example:


Source Text:

{{Document}}

Summary:

{{Summary}}


Evaluation Form (scores ONLY):

- Coherence:
```

checkout [here](https://github.com/nlpyang/geval) 

## SelfcheckGPT

1. **BERTScore**: Compares the generated text with reference samples using BERT embeddings.
2. **Question-Answering (QA)**: Generates questions from the text and checks consistency in answers.
3. **N-gram Analysis**: Uses statistical properties of n-grams for consistency checks.
4. **Natural Language Inference (NLI)**: Uses entailment and contradiction probabilities.
5. **LLM Prompting**: Queries LLMs directly to check consistency.

[SelfcheckGPT](https://github.com/potsawee/selfcheckgpt)

## DeepEval
An open-source LLM evaluation framework that includes:

- G-Eval
- Summarization
- Answer Relevancy
- Faithfulness
- Contextual Recall
- Contextual Precision
- RAGAS
- Hallucination
- Toxicity
- Bias
- and more. [GitHub](https://github.com/confident-ai/deepeval)
## LLM-as-Judge

- Use pairwise comparisons: Instead of asking the LLM to score a single output on a [Likert](https://en.wikipedia.org/wiki/Likert_scale) scale, present it with two options and ask it to select the better one. This tends to lead to more stable results.
- Control for position bias: The order of options presented can bias the LLM’s decision. To mitigate this, do each pairwise comparison twice, swapping the order of pairs each time. Just be sure to attribute wins to the right option after swapping!
- Allow for ties: In some cases, both options may be equally good. Thus, allow the LLM to declare a tie so it doesn’t have to arbitrarily pick a winner.
- Use Chain-of-Thought: Asking the LLM to explain its decision before giving a final answer can increase eval reliability. As a bonus, this lets you to use a weaker but faster LLM and still achieve similar results. Because this part of the pipeline is typically run in batch, the extra latency from CoT isn’t a problem.
- Control for response length: LLMs tend to bias toward longer responses. To mitigate this, ensure response pairs are similar in length.


use YAML because it is less verbose, and hence consumes fewer tokens than JSON. when getting output from LLM

**Metrics for N-Gram Matching**
- **BLEU**: Compares the generated text with reference completions, scoring between 0 (no match) and 1 (perfect match).
- **ROUGE-N**: Measures n-gram overlap between generated text and references.


## Chain poll

 A HIGH EFFICACY METHOD FOR LLM HALLUCINATION DETECTION

The Correctness and Context Adherence metrics in the Galileo console are powered by ChainPoll-Correctness and ChainPoll-Adherence,

 ChainPoll-based metric for each of these cases.
 
 - ChainPoll-Correctness uses ChainPoll to detect open-domain hallucination.
 - ChainPoll-Adherence uses ChainPoll to detect open-domain hallucination.

Steps 
1. Ask gpt-3.5-turbo whether the completion contained hallucination(s), using a detailed and carefully engineered prompt. 
2. Run step 1 multiple times, typically 5. (We use batch inference here for its speed and cost advantages.) 
3. Divide the number of "yes" answers from step 2 by the total number of answers to produce a score between 0 and 1

```
I need you to verify the following statements for correctness using the ChainPoll method:

1. Break down the response into individual facts.
2. Verify each fact using reliable sources.
3. Identify any inconsistencies or errors.
4. Provide the correct information if any fact is incorrect.
```

## Prometheus

**Prometheus** is a family of open-source language models specialized in evaluating other language models. By effectively simulating human judgments and proprietary LM-based evaluations, we aim to resolve the following issues:

- _Fairness_: Not relying on closed-source models for evaluations!
    
- _Controllability_: You don’t have to worry about GPT version updates or sending your private data to OpenAI by constructing internal evaluation pipelines
    
- _Affordability_: If you already have GPUs, it is free to use!

```txt

You are a fair judge assistant tasked with providing clear, objective feedback 
based on specific criteria, ensuring each assessment reflects the absolute 
standards set for performance.

###Task Description:
An instruction (might include an Input inside it), a response to evaluate, a 
reference answer that gets a score of 5, and a score rubric representing a 
evaluation criteria are given.
1. Write a detailed feedback that assess the quality of the response strictly 
based on the given score rubric, not evaluating in general.
2. After writing a feedback, write a score that is an integer between 1 and 5. 
You should refer to the score rubric.
3. The output format should look as follows: \\"Feedback: (write a feedback for 
criteria) [RESULT] (an integer number between 1 and 5)\\"
4. Please do not generate any other opening, closing, and explanations.

###The instruction to evaluate:
{instruction}

###Response to evaluate:
{response}

###Reference Answer (Score 5):
{reference_answer}

###Score Rubrics:
{score_rubric}

###Feedback:
```

## Ragas

Ragas is a framework that helps you evaluate your Retrieval Augmented Generation (RAG) pipelines. RAG denotes a class of LLM applications that use external data to augment the LLM’s context.

**Ragas Framework**
- [GitHub Repository](https://github.com/explodinggradients/ragas)
- [Comet Blog](https://www.comet.com/site/blog/rag-evaluation-framework-ragas/)

### Resources
- [DSPy Multi-Hop Chain-of-Thought RAG](https://github.com/sachink1729/DSPy-Multi-Hop-Chain-of-Thought-RAG)

## EvalLM

Interactive Evaluation of Large Language Model Prompts on User-Defined Criteria it a website where we can enter prompt and do eval using perdefind criteria and user defined criteria

## ChainForge

ChainForge is an open-source visual programming environment for prompt engineering, LLM evaluation and experimentation

## SPADE

System for Prompt Analysis and Delta-Based Evaluation ([SPADE](https://github.com/shreyashankar/spade-experiments)) A method for automatically synthesizing data quality assertions that identify bad LLM outputs

**How it works**
- **Prompt Tracking**
    - SPADE monitors and logs changes made to prompts over time. Each modification in the prompt is recorded, capturing the original prompt and all subsequent variations.
- **Prompt Changes Evaluation**
    - After a prompt change is detected, SPADE sends the revised prompt to the LLM. The system then generates the LLM’s response based on the updated prompt.
- **Automated Unit Test Generation**
    - SPADE automatically synthesizes a unit test (UT) for each modified prompt. This unit test assesses the correctness and relevance of the LLM’s output in response to the prompt change.
- **Delta-Based Analysis**
    - The system performs a delta-based evaluation, comparing the outputs generated by the LLM before and after the prompt change. This comparison helps in identifying any discrepancies or issues introduced by the modification.
- **Quality Assertion Creation**
    - Based on the delta-based analysis, SPADE formulates quality assertions. These assertions specify what constitutes acceptable and unacceptable output for given prompts, enabling automated detection of bad outputs.




## Resources
- A Survey on Hallucination in Large Language Models: Principles, Taxonomy, Challenges, and Open Questions [here](https://github.com/LuckyyySTA/Awesome-LLM-hallucination?tab=readme-ov-file)
- [Evaluating the Effectiveness of LLM-Evaluators ](https://eugeneyan.com/writing/llm-evaluators/)


## Giskard

[Giskard](https://github.com/Giskard-AI/giskard) is an open-source Python library that **automatically detects performance, bias & security issues in AI applications**. The library covers LLM-based applications such as RAG agents, all the way to traditional ML models for tabular data

 The Giskard LLM scan comprises two main types of detectors:
- **Traditional detectors**: which exploit known techniques or heuristics to detect vulnerabilities Example : LLMCharsInjectionDetector
- **LLM assisted detectors**: which use another LLM model to probe the model under analysis. Example:LLMBasicSycophancyDetector

## Snorkel

Create custom trained model with custom data and use it for eval
Steps
- Create golden dataset
- Encode acceptance criteria into custom quality model
- Slice your prompts to evaluate what matters
- Review fine grainded benchmarks










## Evaluation

### LLM Evaluation Metrics

**Intrinsic Metrics**: Evaluate the model's internal workings.
- **Perplexity**: Measures how well the model predicts a test dataset. Lower perplexity indicates better performance.
- **Fluency**: Measures the coherence and naturalness of the generated text.
- **BLEU Score**: Measures the similarity between the generated text and a reference text.

**Extrinsic Metrics**: Evaluate the model's performance on specific tasks.
- **Accuracy**: Measures the proportion of correct predictions or answers.
- **F1 Score**: Measures the balance between precision and recall.
- **ROUGE Score**: Measures the quality of generated summaries.

**Hybrid Metrics**: Combine intrinsic and extrinsic metrics for comprehensive evaluation.
- **METEOR Score**: Measures the similarity between generated and reference translations, considering word order.
- **GEVAL**: [G-Eval GitHub](https://github.com/nlpyang/geval)

### G-Eval

**How G-EVAL Works**
1. **Text Embeddings**: GPT-4 generates embeddings for both the generated and reference texts.
2. **Similarity Computation**: Uses metrics like cosine similarity to compute text similarity.
3. **Score Computation**: Aggregates similarity scores to compute a final score reflecting overall text quality.

**Coherence Evaluation Example**
```txt
Coherence (1-5): Measures the overall quality of all sentences. The summary should be well-structured, presenting information in a clear and logical order.

Evaluation Steps:
1. Read the news article carefully to identify the main topic and key points.
2. Read the summary and compare it to the article, checking if it covers the main points and presents them logically.
3. Assign a coherence score from 1 (lowest) to 5 (highest) based on these criteria.

Example:
Source Text: {{Document}}
Summary: {{Summary}}

Evaluation Form (scores ONLY):
- Coherence:
```

### SelfcheckGPT

**Features**
1. **BERTScore**: Compares generated text with reference samples using BERT embeddings.
2. **Question-Answering (QA)**: Generates questions from text and checks consistency in answers.
3. **N-gram Analysis**: Uses statistical properties of n-grams for consistency checks.
4. **Natural Language Inference (NLI)**: Uses entailment and contradiction probabilities.
5. **LLM Prompting**: Queries LLMs directly to check consistency.

### DeepEval

An open-source LLM evaluation framework that includes:
- G-Eval
- Summarization
- Answer Relevancy
- Faithfulness
- Contextual Recall
- Contextual Precision
- RAGAS
- Hallucination
- Toxicity
- Bias
- and more. [GitHub](https://github.com/confident-ai/deepeval)

### LLM-as-Judge

**Best Practices**
- **Pairwise Comparisons**: Present two options and ask the LLM to select the better one to get more stable results.
- **Control for Position Bias**: Swap the order of options to mitigate bias. Attribute wins correctly after swapping.
- **Allow for Ties**: Allow the LLM to declare a tie when both options are equally good.
- **Chain-of-Thought (CoT)**: Use CoT to explain decisions, which can improve evaluation reliability. Ensure responses are of similar length to avoid bias towards longer answers.

### Metrics for N-Gram Matching

- **BLEU**: Compares the generated text with reference completions, scoring between 0 (no match) and 1 (perfect match).
- **ROUGE-N**: Measures n-gram overlap between generated text and references.

### ChainPoll

A method for detecting LLM hallucinations using:
- **ChainPoll-Correctness**: Detects open-domain hallucinations.
- **ChainPoll-Adherence**: Checks adherence to factual correctness.

**Steps**
1. Ask GPT-3.5-turbo whether the completion contains hallucinations using a detailed prompt.
2. Repeat the query multiple times (typically 5) for batch inference.
3. Calculate the score based on the number of affirmative responses.

**Prompt Example**
```txt
I need you to verify the following statements for correctness using the ChainPoll method:

1. Break down the response into individual facts.
2. Verify each fact using reliable sources.
3. Identify any inconsistencies or errors.
4. Provide the correct information if any fact is incorrect.
```

### Prometheus

**Prometheus** is an open-source family of language models specialized in evaluating other language models. It addresses:
- **Fairness**: Avoids reliance on closed-source models.
- **Controllability**: No dependency on GPT updates or private data sharing.
- **Affordability**: Free to use if you have GPUs.

**Task Description Example**
```txt
You are a fair judge assistant tasked with providing clear, objective feedback based on specific criteria. 
1. Write detailed feedback assessing the quality of the response based on the given rubric.
2. Provide a score between 1 and 5.
3. Output format: "Feedback: (write feedback) [RESULT] (integer between 1 and 5)"

Instruction: {instruction}
Response: {response}
Reference Answer (Score 5): {reference_answer}
Score Rubrics: {score_rubric}
Feedback:
```

### RAGAS

**RAGAS**: Framework for evaluating Retrieval Augmented Generation (RAG) pipelines. [GitHub](https://github.com/explodinggradients/ragas) [Comet Blog](https://www.comet.com/site/blog/rag-evaluation-framework-ragas/)

### EvalLM

Interactive evaluation of LLM prompts based on predefined and user-defined criteria.

### ChainForge

**ChainForge**: An open-source visual programming environment for prompt engineering, LLM evaluation, and experimentation.

### SPADE

**SPADE**: System for Prompt Analysis and Delta-Based Evaluation. [GitHub](https://github.com/shreyashankar/spade-experiments)

**How it Works**
- **Prompt Tracking**: Logs prompt changes over time.
- **Prompt Changes Evaluation**: Generates responses based on updated prompts.
- **Automated Unit Test Generation**: Creates unit tests for each prompt variation.
- **Delta-Based Analysis**: Compares outputs before and after prompt changes.
- **Quality Assertion Creation**: Forms assertions to detect bad outputs.


## Tools

- **Port Key**
- **TrueLens**: [Website](https://www.trulens.org/)
- **Inspect AI**: [GitHub](https://github.com/UKGovernmentBEIS/inspect_ai)
- **Giskard**: [GitHub](https://github.com/Giskard-AI/giskard)%%  %%

## Resources
- [A Survey on Hallucination in Large Language Models](https://github.com/LuckyyySTA/Awesome-LLM-hallucination?tab=readme-ov-file)
- [Evaluating the Effectiveness of LLM-Evaluators](https://eugeneyan.com/writing/llm-evaluators/)

---