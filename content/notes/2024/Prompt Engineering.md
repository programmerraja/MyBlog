+++
title = 'Prompt Engineering'
date = 2024-01-22T09:51:27.2727+05:30
draft = false
tags =[]
+++ 



![[Pasted image 20240720193516.png]]


## Plan Like a Graph
Plan Like a Graph (PLaG) that converts naturalistic questions to equivalent graph problems, which significantly improves the performance of LLMs in asynchronous planning tasks.

```
Let's say we have an asynchronous planning task where we need to bake a cake, frost it, and then decorate it, and we have the following time durations and constraints:

- Mixing the cake batter takes 10 minutes
- Baking the cake takes 30 minutes
- Frosting the cake takes 5 minutes
- Decorating the cake takes 15 minutes
- Baking the cake must be done after mixing the batter
- Frosting the cake must be done after baking the cake
- Decorating the cake must be done after frosting it

To use the PLaG technique, we would first convert this task into a graph representation, where the nodes represent the steps in the task and the edges represent the constraints between them. The resulting graph would look like this:


`1Mix batter (10 min) ----> Bake cake (30 min) ----> Frost cake (5 min) ----> Decorate cake (15 min)`

Next, we would prompt the LLM with the task description and the graph representation, instructing it to reason based on the graph. For example, the prompt might look like this:

"Consider the following task: Bake a cake, frost it, and then decorate it. The steps and time durations are as follows:

- Mixing the cake batter takes 10 minutes
- Baking the cake takes 30 minutes
- Frosting the cake takes 5 minutes
- Decorating the cake takes 15 minutes

The constraints between the steps are as follows:

- Baking the cake must be done after mixing the batter
- Frosting the cake must be done after baking the cake
- Decorating the cake must be done after frosting it

Use the following graph to reason about the task and determine the shortest possible time needed to complete it:

`1Mix batter (10 min) ----> Bake cake (30 min) ----> Frost cake (5 min) ----> Decorate cake (15 min)`

By providing the LLM with a graph representation of the task, we can help it reason more effectively about the constraints and time durations involved, leading to more accurate predictions about the shortest possible time needed to complete the task.
```


| Pattern Category      | Prompt Pattern                                                                           |
| --------------------- | ---------------------------------------------------------------------------------------- |
| Input Semantics       | Meta Language Creation                                                                   |
| Output  Customization | Output Automater  <br>Persona Visualization <br>Generator Recipe Template                |
| Error Identification  | Fact Check List<br>Reflection                                                            |
| Prompt  Improvement   | Question Refinement <br>Alternative Approaches<br>Cognitive Verifier <br>Refusal Breaker |
| Interaction           | Flipped Interaction<br> Game Play <br>Infinite Generation                                |
| Context Control       | Context Manager                                                                          |

## Claude Meta Prompt

Claude have written a prompt that will help to get perfect prompt in XML
check [here](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/prompt-generator)


## Dspy

Dspy(Declarative Self-improving Language Programs) make it easy to follow the data science process when building LM apps

Workflow
- define your task
- collect some data and LM/RM connection
- Define your metrics
- setup a pipeline
- compile/optimize the program
- Save your experiment and iterate

Components of Dspy
- Signatures: Define the input-output structure for model interactions, ensuring clarity and consistency across different modules. (question -> answer , doc-> summary)
- Modules: Encapsulate specific tasks or operations as reusable components. This modular design enhances the flexibility and scalability of applications built with DSPy.
- Teleprompters: Manage the execution flow of modules, allowing for sophisticated sequencing and optimization of interactions with language models.

- Hand-written prompts and fine-tuning are abstracted and replaced by **signatures**
- Prompting techniques, such as **Chain of Thought** or **ReAct**, are abstracted and replaced by **modules**
- Manual prompt engineering is automated with optimizers **teleprompters** and a DSPy **Compiler**


### Singature
 signature is a short function that specifies **what** a transformation does rather than **how** to prompt the LM to do it (e.g., "consume questions and context and return answers").
```txt
"context, question"    -> "answer"
Input seprated by comma | output

"question -> answer" 

"long-document -> summary" 

"context, question -> answer"
```

```python
class GenerateAnswer(dspy.Signature): 
	"""Answer questions with short factoid answers.""" 
	context = dspy.InputField(desc="may contain relevant facts") 
	question = dspy.InputField() 
	answer = dspy.OutputField(desc="often between 1 and 5 words")

predict = dspy.predict(GenerateAnswer)
prediction = predict(question="how many hydrogent present in water",context="")

print(predict.answer)

turbo.inspect_history(n=10) #"Prints the last n prompts and their completions
```

below data will send to LLM and context,question and answer to get by using `pydantic` 

```txt
Answer questions with short factoid answers. 

---

Follow the following format.

Context: may contain relevant facts
Question: ${question}
Answer: often between 1 and 5 words

---

Context: 
Question: how many hydrogent present in water
Answer:Context: 
Question: how many hydrogent present in water
Answer: Two  #answer from chat gpt

```

###  Modules: Abstracting prompting techniques

Modules in DSPy are templated and parameterized to abstract these prompting techniques. This means that they are used to adapt DSPy signatures to a task by applying prompting, fine-tuning, augmentation, and reasoning techniques.

```python
# Option 1: Pass minimal signature to ChainOfThought module 
generate_answer = dspy.ChainOfThought("context, question -> answer") 

# Option 2: Or pass full notation signature to ChainOfThought module 
generate_answer = dspy.ChainOfThought(GenerateAnswer) 

# Call the module on a particular input. 
pred = generate_answer(context = "Which meant learning Lisp, since in those days Lisp was regarded as the language of AI.", question = "What programming language did the author learn in college?")

print(pred.answer)
```

Below prompt will send to LLM for above code
```txt
Given the fields `context`, `question`, produce the fields `answer`.

---

Follow the following format.

Context: ${context}

Question: ${question}

Reasoning: Let's think step by step in order to ${produce the answer}. We ...

Answer: ${answer}

---

Context: Which meant learning Lisp, since in those days Lisp was regarded as the language of AI.

Question: What programming language did the author learn in college?

Reasoning: Let's think step by step in order to

Context: Which meant learning Lisp, since in those days Lisp was regarded as the language of AI.

Question: What programming language did the author learn in college?

Reasoning: Let's think step by step in order to find the answer to the question. The context states that the author learned Lisp in college. 

Answer: Lisp 
```

- `dspy.Predict`: Processes the input and output fields, generates instructions, and creates a template for the specified `signature`.

- `dspy.ChainOfThought`: Inherits from the `Predict` module and adds functionality for "Chain of Thought" processing.

- `dspy.ChainOfThoughtWithHint`: Inherits from the `Predict` module and enhances the `ChainOfThought` module with the option to provide hints for reasoning.

- `dspy.MultiChainComparison`: Inherits from the `Predict` module and adds functionality for multiple chain comparisons.

- `dspy.Retrieve`: Retrieves passages from a retriever module.

- `dspy.ReAct`: Designed to compose the interleaved steps of Thought, Action, and Observation.

You can chain these modules together in classes that are inherited from `dspy.Module` and take two methods. You might already notice a syntactic similarity to PyTorch

- `__init__()`: Declares the used submodules.
- `forward()`: Describes the control flow among the defined sub-modules.

```python
class RAG(dspy.Module): 
 def __init__(self, num_passages=3): 
	super().__init__() 
	self.retrieve = dspy.Retrieve(k=num_passages) 
	self.generate_answer = dspy.ChainOfThought(GenerateAnswer)

def forward(self, question): 
	  context = self.retrieve(question).passages 
	  prediction = self.generate_answer(context=context, question=question) 
	  return dspy.Prediction(context=context, answer=prediction.answer)
```

### Optimizer

A **DSPy optimizer** is an algorithm that can tune the parameters of a DSPy program (i.e., the prompts and/or the LM weights) to maximize the metrics you specify, like accuracy.

DSPy programs consist of multiple calls to LMs, stacked together as [DSPy modules]. Each DSPy module has internal parameters of three kinds: (1) the LM weights, (2) the instructions, and (3) demonstrations of the input/output behavior.

Given a metric, DSPy can optimize all of these three with multi-stage optimization algorithms. These can combine gradient descent (for LM weights) and discrete LM-driven optimization, i.e. for crafting/updating instructions and for creating/validating demonstrations. DSPy Demonstrations are like few-shot examples, but they're far more powerful. They can be created from scratch, given your program, and their creation and selection can be optimized in many effective ways.

Automatic Few-Shot Learning
- **`LabeledFewShot`**
- **`BootstrapFewShot`**
- **`BootstrapFewShotWithRandomSearch`**
- **`BootstrapFewShotWithOptuna`**
- **`KNNFewShot`**

### Internal of DSPY
Dspy uses pydantic [[Python#Pydantic]]  for `dspy.InputField` and other things 


## Zenbase

Developer tools and cloud infrastructure for perfectionists using LLMs. [Zenbase](https://zenbase.ai) takes care of the hassle of prompt engineering and model selection.


## EvalLM
Interactive Evaluation of Large Language Model Prompts on User-Defined Criteria


## Reflection Fine-Tuning

Reflection is the new fine-tuning technique where the fine-tuning prompt is changed a bit to incorporate self reflection while training the LLM, improving the results by a big margin.
Prompt

```
You are a world-class AI system, capable of complex reasoning and reflection.  
Reason through the query inside <thinking> tags, and  
then provide your final response inside <output> tags.  
If you detect that you made a mistake in your reasoning at any point,  
correct yourself inside <reflection> tags.
```

- The model begins by generating its reasoning within `<thinking>` tags. This section contains the model's internal thought process as it analyzes the input query.
- Within the `<thinking>` section, the model may include `<reflection>` tags if it identifies any mistakes in its reasoning. This indicates that the model is capable of recognizing errors and will attempt to correct them before finalizing its answer.


## Chain-of-thought (CoT)

 By leveraging in-context learning abilities, CoT prompting encourages a language model to more effectively solve complex problems by outputting along with its solution a corresponding “chain of thought” (i.e., a step-by-step explanation for how the problem was solved). The model can be prompted to generate a chain of thought via a few-shot learning approach that provides several chain of thought exemplars; see above. The CoT technique is most effective when the map from input to output is highly non-trivial; e.g., math or multi-step reasoning problems.

Note: use COT for mathematical and reasoning where the perform good check out [here](https://arxiv.org/pdf/2409.12183) for more

## Auto COT

Auto-CoT paradigm to automatically construct demonstrations with questions and reasoning chains. Specifically, Auto-CoT leverages LLMs with the “Let’s think step by step” prompt to generate reasoning chains for demonstrations one by one, and sending that COT with question to LLM as few shot prompting

## Self-consistency with (CoT)

## ReAct

ReAct is a general paradigm that combines reasoning and acting with LLMs. ReAct prompts LLMs to generate verbal reasoning traces and actions for a task.

 it to be better than chain-of-thought prompting
```
- Thought 1: I need to search for “Colorado orogeny,” identify the area covered by the eastern sector, and then determine the elevation range of that area.
- 
- Action 1: Search [Colorado orogeny]
- Observation 1: The Colorado orogeny refers to a phase of mountain formation in Colorado and nearby regions.
- 
- Thought 2: The eastern sector isn’t mentioned here. I should look up “eastern sector.”
- Action 2: Lookup [eastern sector]
- Observation 2: The eastern sector extends into the High Plains and is known as the Central Plains orogeny.

- Thought 3: The eastern sector of the Colorado orogeny reaches into the High Plains. I should search for the High Plains and find its elevation range.
- Action 3: Search [High Plains]
- Observation 3: High Plains refers to one of two distinct land regions.

- Thought 4: I should search specifically for “High Plains (United States).”

- Action 4: Search [High Plains (United States)]
- Observation 4: The High Plains are a part of the Great Plains, with elevations rising from around 1,800 to 7,000 ft (550 to 2,130 m).

- Thought 5: The elevation range of the High Plains is 1,800 to 7,000 ft, so that’s the answer.
- Action 5: Finish [1,800 to 7,000 ft]
```
## Tree of Thoughts

Tree of Thoughts is an innovative approach to AI reasoning that expands upon the Chain of Thought (CoT) methodology. While CoT prompts an AI to explain its thinking in a linear fashion, ToT takes this a step further by encouraging the AI to explore multiple paths of reasoning simultaneously, much like a tree branching out in various directions.

check out implementation [here](https://github.com/kyegomez/tree-of-thoughts) 
Prompt

```

Imagine three different experts are answering this question. All experts will write down 1 step of their thinking, then share it with the group. Then all experts will go on to the next step, etc. If any expert realises they're wrong at any point then they leave. The question is...



################ 2nd ################

Simulate three brilliant, logical experts collaboratively answering a question. Each one verbosely explains their thought process in real-time, considering the prior explanations of others and openly acknowledging mistakes. At each step, whenever possible, each expert refines and builds upon the thoughts of others, acknowledging their contributions. They continue until there is a definitive answer to the question. For clarity, your entire response should be in a markdown table. The question is...


################ ################

Imagine three highly intelligent experts working together to answer a question. They will follow a tree of thoughts approach, where each expert shares their thought process step by step. They will consider the input from others, refine their thoughts, and build upon the group's collective knowledge. If an expert realizes their thought is incorrect, they will acknowledge it and withdraw from the discussion. Continue this process until a definitive answer is reached. Present the entire response in a markdown table. The question is...


################ 2nd ################

Three experts with exceptional logical thinking skills are collaboratively answering a question using a tree of thoughts method. Each expert will share their thought process in detail, taking into account the previous thoughts of others and admitting any errors. They will iteratively refine and expand upon each other's ideas, giving credit where it's due. The process continues until a conclusive answer is found. Organize the entire response in a markdown table format. The question is...
################ 2nd ################


Envision a group of three experts working in unison to tackle a question by employing a tree of thoughts strategy. Each expert will thoroughly explain their line of thinking at every step, while also considering the insights provided by their peers. They will openly recognize any mistakes and build upon the group's shared understanding. This iterative process will continue until a definitive solution is reached. Structure the entire response as a markdown table. The question is...


################ 2nd ################

"Three experts with exceptional logical thinking skills are collaboratively answering a question using the tree of thoughts method. Each expert will share their thought process in detail, taking into account the previous thoughts of others and admitting any errors. They will iteratively refine and expand upon each other's ideas, giving credit where it's due. The process continues until a conclusive answer is found. Organize the entire response in a markdown table format. The task is:
```


## Re-Reading Improves Reasoning in Large Language Models

The core concept of the paper "Re-Reading Improves Reasoning in Large Language Models" is that repeating the input question can enhance the reasoning capabilities of Large Language Models (LLMs),

Unlike many thought-eliciting prompting methods (e.g., Chain-of-Thought) that focus on structuring the output, RE2 focuses on improving how the LLM processes the input This is analogous to how understanding the question is paramount to solving a problem for humans.

Re-Reading + COT

```
Q: Roger has 5 tennis balls. He buys 2 more cans of tennis balls. Each can has 3 tennis balls. How many tennis balls does he have now? 

Read the question again: Roger has 5 tennis balls. He buys 2 more cans of tennis balls. Each can has 3 tennis balls. How many tennis balls does he have now? A:

Let’s think step by step.
```

- potentially improve the reasoning performance of Large Language Models (LLMs).


## Summarization
- https://towardsdatascience.com/summarize-podcast-transcripts-and-long-texts-better-with-nlp-and-ai-e04c89d3b2cb



## ChatML

ChatML (Chat Markup Language) is a lightweight markup format used by OpenAI to structure conversations between users and models, especially in chatbot-like environments. It is designed to define roles and organize the flow of conversation between different participants, such as system instructions, user inputs, and model responses.

In a typical ChatML format, the message blocks are defined by tags such as:

- `<|system|>`: Instructions or setup given to the model (usually hidden from the user).
- `<|user|>`: Represents what the user says.
- `<|assistant|>`: Represents the assistant’s responses.
- `<im_start|>` : the start of an interactive mode where messages will alternate between participants. It’s generally used to transition into the back-and-forth of a conversation.
- `<im_end|>`




## Claude 

Some takeaways you can use for writing your long-context Q&A prompts:

- Use many examples and the scratchpad for best performance on both context lengths.
- Pulling relevant quotes into the scratchpad is helpful in all head-to-head comparisons. It comes at a small cost to latency, but improves accuracy. In Claude Instant’s case, the latency is already so low that this shouldn’t be a concern.
- Contextual examples help on both 70K and 95K, and more examples is better.
- Generic examples on general/external knowledge do not seem to help performance.


```
I need to write a blog post on the topic of [integrating enterprise data with an LLM] for my AI solutions company, AI Disruptor.

Begin in <scratchpad> tags and write out and brainstorm in a couple paragraphs your plan for how you will create an informative and engaging blog. Also brainstorm how you will create a CTA at the end for our company, AI Disruptor.
```


## Prompt Hacking

### output2prompt

The core idea behind output2prompt is clever in its simplicity. By analyzing patterns in the AI’s responses, another AI can infer the instructions that produced those responses.


## My Thoughts

- When you write a prompt think how it process and response by yourself it will give you a idea how your prompt will work and where to improve
- Provide important thing at start of the prompt
- Think as it just next word predictor not more then that so think in the way when writing prompt
- Visulize attention mechnaism on the prompt
- Tell how to handle negative else it will hallucinate
- If you using too much example the response will be more genric based on the example so keep that in mind
- use stop sequence if you want avoid unwanted text




## FrugalGPT

**FrugalGPT** is a framework proposed by Lingjiao Chen, Matei Zaharia and James Zou from Stanford University in their 2023 [paper](https://portkey.ai/blog/frugalgpt-how-to-use-large-language-models-while-reducing-cost-and-improving-performance-summary/) "_FrugalGPT: How to Use Large Language Models While Reducing Cost and Improving Performance_". The paper outlines strategies for more cost-effective and performant usage of large language model (LLM) APIs.

The core of FrugalGPT revolves around three key techniques for reducing LLM inference costs:

### Prompt Optimization 

**Prompt Adaptation**:  FrugalGPT wants us to either reduce the size of the prompt OR combine similar prompts together. Core idea is to minimise tokens and thus reduce LLM costs
**Example** for email classification task we can only pick the **top k** similar examples. using similarity .
FrugalGPT suggests identifying the best examples to be used instead of all of them.

**Combine similar requests together** : LLMs have been found to retain context for multiple tasks together and FrugalGPT proposes to use this to group multiple requests together thus decreasing the redundant prompt examples in each request.

**Better utilize a smaller model with a more optimized prompt** : 

Example: [from Claude 3.5 Sonnet to GPT-4o-mini -- reducing costs massively while keeping quality high](https://github.dev/mshumer/gpt-prompt-engineer).

**Compress the prompt** : The compression process happens in three main steps:

1. **Token Classification**:
    - The trained model processes each token in the original prompt and assigns a **preserve** or **discard** probability based on the token’s importance for preserving the meaning of the text.
2. **Selection of Tokens**:
    - The target **compression ratio** is used to decide how many tokens to retain. For example, if a 2x compression ratio is desired, 50% of the original tokens will be retained. The model sorts the tokens based on their **preserve probability** and selects the top tokens to keep in the compressed prompt.
3. **Preserve Token Order**:
    - After selecting the tokens to preserve, the original order of the tokens is maintained to ensure that the compressed prompt remains coherent and grammatically correct.

check out [here](https://github.com/microsoft/LLMLingua)


###  LLM Approximation

**Cache LLM Requests** : When the prompt is exactly the same, we can save the inference time and cost by serving the request from cache.

**Fine-tune a smaller model in parallel** : In a production environment, it can be massively beneficial to keep serving requests through a bigger model while continuously logging and fine-tuning a smaller model on those responses. We can then evaluate the results from the fine-tuned model and the larger model to determine when it make sense to switch.


### LLM Cascade

The key idea is to sequentially query different LLMs based on the confidence of the previous LLM's response. If a cheaper LLM can provide a satisfactory answer, there's no need to query the more expensive models, thus saving costs.

In essence, the LLM cascade makes a request to the smallest model first, evaluates the response, and returns it if it's good enough. Otherwise, it requests the next larger model and so on until a satisfactory response is obtained or the largest model is reached.


Here are 5 papers you want to read to understand better how [OpenAI](https://www.linkedin.com/company/openai/) o1 might work. Focusing on Improving LLM reasoning capabilities for complex tasks via training/RLHF, not prompting. 👀  
  
> Quiet-STaR: Language Models Can Teach Themselves to Think Before Speaking ([https://lnkd.in/eCPaa-wc](https://lnkd.in/eCPaa-wc)) from Stanford  
  
> Agent Q: Advanced Reasoning and Learning for Autonomous AI Agents ([https://lnkd.in/eebwEkPi](https://lnkd.in/eebwEkPi)) from MultiOn/Stanford  
  
> Let's Verify Step by Step ([https://lnkd.in/egf6EpMd](https://lnkd.in/egf6EpMd)) from OpenAI  
  
> V-STaR: Training Verifiers for Self-Taught Reasoners ([https://lnkd.in/ebRcEKBn](https://lnkd.in/ebRcEKBn)) from Microsoft, Mila**  
  
> Learn Beyond The Answer: Training Language Models with Reflection for Mathematical Reasoning ([https://lnkd.in/eeeaqm6x](https://lnkd.in/eeeaqm6x)) from Notre Dam, Tencent





## Resources

1. [Eugene Yan's Prompting Guide](https://eugeneyan.com/writing/prompting/)
2. [Leaked Prompts of GPTs on GitHub](https://github.com/linexjlin/GPTs?tab=readme-ov-file)
3. https://substack.com/@cwolferesearch/p-143156742 
4. https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/

## Tools

1. [AI Prompt Optimizer](https://promptperfect.jina.ai/)
2. [Start Generating Prompts with OctiAI](https://www.octiai.com/)