+++
title = 'Prompt Engineering'
date = 2024-01-22T09:51:27.2727+05:30
draft = false
tags =[]
+++ 



## Resources
- https://eugeneyan.com/writing/prompting/

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