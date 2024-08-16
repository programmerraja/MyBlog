+++
title = 'LLM Observability And Eval'
date = 2024-08-12T08:35:40.4040+05:30
draft = true
tags =[]
+++ 


## Tools
- Port key
- 


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


[G-Eval](https://github.com/nlpyang/geval)
 
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

[SelfcheckGPT](https://github.com/potsawee/selfcheckgpt)
1. **BERTScore**: Compares the generated text with reference samples using BERT embeddings.
2. **Question-Answering (QA)**: Generates questions from the text and checks consistency in answers.
3. **N-gram Analysis**: Uses statistical properties of n-grams for consistency checks.
4. **Natural Language Inference (NLI)**: Uses entailment and contradiction probabilities.
5. **LLM Prompting**: Queries LLMs directly to check consistency.

[DeepEval, an open-source LLM evaluation framework](https://github.com/confident-ai/deepeval)
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
- etc.

### LLM-as-Judge

- Use pairwise comparisons: Instead of asking the LLM to score a single output on a [Likert](https://en.wikipedia.org/wiki/Likert_scale) scale, present it with two options and ask it to select the better one. This tends to lead to more stable results.
- Control for position bias: The order of options presented can bias the LLM’s decision. To mitigate this, do each pairwise comparison twice, swapping the order of pairs each time. Just be sure to attribute wins to the right option after swapping!
- Allow for ties: In some cases, both options may be equally good. Thus, allow the LLM to declare a tie so it doesn’t have to arbitrarily pick a winner.
- Use Chain-of-Thought: Asking the LLM to explain its decision before giving a final answer can increase eval reliability. As a bonus, this lets you to use a weaker but faster LLM and still achieve similar results. Because this part of the pipeline is typically run in batch, the extra latency from CoT isn’t a problem.
- Control for response length: LLMs tend to bias toward longer responses. To mitigate this, ensure response pairs are similar in length.


use YAML because it is less verbose, and hence consumes fewer tokens than JSON. when getting output from LLM

 ### Tools
 
 - [truelens](https://www.trulens.org/) 
 - https://github.com/UKGovernmentBEIS/inspect_ai


Metrics that use N-Gram matching
- BLEU | ROUGE-N compare to one or more reference cmpletions A score between zero and one indicating similarity to the reference once indicating a prefect match


### Chain poll

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

https://github.com/explodinggradients/ragas