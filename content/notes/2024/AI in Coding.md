+++
title = 'AI in Coding'
date = 2024-09-19T06:32:53.5353+05:30
draft = true
tags =[]
+++ 


Discover vulnerabilities across a codebase with CodeQL, our industry-leading semantic code analysis engine. CodeQL lets you query code as though it were data. Write a query to find all variants of a vulnerability, eradicating it forever. Then share your query to help others do the same. 
https://codeql.github.com/




## Agents in Software Engineering

LLM-based agents comprising three key components: perception, memory, action.
- Perception: . It can process inputs of different modalities such as textual, visual, and auditory input, and convert them into an embedding format that LLM-based agents can understand and process, laying the foundation for reasoning and decision-making actions of LLM-based agents.

- Memory: The memory modules include semantic, episodic, and procedural memory, which can provide additional useful information to help LLM make reasoning decisions

	- Semantic Memory: Semantic memory stores acknowledged world knowledge of LLM-based agents, usually in the form of external knowledge retrieval bases which include documents, libraries, APIs,
	
	- Episodic Memory: Episodic memory records content related to the current case and experience information from previous decision-making processes. Content related to the current case (such as relevant information found in the search database, samples provided by In-context learning (ICL) technology, etc.)
	
	- Procedural Memory: The procedural memory of Agents in software engineering contains the implicit knowledge stored in the LLM weights and the explicit knowledge written in the agent’s code.

- Action : The action module includes two types: internal and external actions. The external actions interact with the external environment to obtain feedback information, including Dialogue with humans/agents and interaction with the digital environment, while the internal actions reason and make decisions based on the input of the LLM and refine the decision based on the obtained feedback, including reasoning, retrieval, and learning actions.

	- Internal Action: can be classified as
		- Reasoning Action (COT,AutoCOT,Brainstorm framework,tree-shaped CoT), 
		- Retrieval Action 
			- Text-code (APIRetriever,De-Hallucinator)
			- Text-Text (DocPrompting,CodeAgent)
			- Code-Code (RepoCoder,RepoFusion)
			- Hybrid-Code (ToolCoder,Query Expansion)
			- Code-Hybrid (CEDAR,Multi-intent)
			- Text-Hybrid (LAIL,AceCoder)
		
		- Learning Action: Learning actions are continuously learning and updating knowledge by learning and updating semantic and procedural memories, thereby improving the quality and efficiency of reasoning and decision-making (CodGen)

- External Action: Agents can interact with humans or other agents, and get rich information in the interaction process as feedback, expanding the knowledge of the agent and refining the answers of LLM more corrector (REFINER,PPOCoder,RLTF)

Check out more research paper about SE Agent [here](https://github.com/DeepSoftwareAnalytics/Awesome-Agent4SE)
### Repo Coder

RepoCoder: Repository-Level Code Completion Through Iterative Retrieval and Generation
### **RepoCoder Framework**

- **Core Idea**: RepoCoder improves upon traditional In-File and Retrieval-Augmented Generation (RAG) methods by iterating between retrieval and code generation. It continuously refines its retrieval process using the generated code from prior iterations to narrow down more relevant context from the repository.

- **Components**:
    - **Retriever**: Searches for relevant code snippets from the repository based on the incomplete code.
    - **Generator**: Uses a pre-trained language model (such as GPT-3.5) to generate the completed code based on the retrieved snippets.
    - **Iterative Process**: By iterating between retrieval and generation, RepoCoder refines the search query and retrieves more accurate code snippets with each iteration.

**Challenges and Limitations**
- **Low Code Duplication**: RepoCoder's performance might not significantly improve in repositories with minimal code duplication
- **Iteration Optimization**: Determining the optimal number of iterations for retrieval-generation is a challenge, as performance gains might not always be consistent with more iterations.
- **Real-Time Deployment**: While effective, RepoCoder's iterative nature can introduce latency, which might not be suitable for real-time applications without optimizations like caching or model acceleration.

Check out code [here](https://github.com/microsoft/CodeT/blob/main/RepoCoder/search_code.py)













To understand how GitHub Copilot, Tabnine, and Amazon CodeWhisperer work, you can explore several research papers and technical reports that discuss the underlying models and techniques used in these tools. Here’s a guide to some relevant areas and papers that can help you learn about their functionality:

### 1. **GitHub Copilot**

GitHub Copilot is based on OpenAI Codex, a language model derived from GPT-3. It is fine-tuned specifically for code completion tasks. Below are relevant papers to understand its underlying architecture:

- **"Language Models are Few-Shot Learners"**  
    By: Tom Brown et al. (2020)  
    [Paper Link](https://arxiv.org/abs/2005.14165)  
    This paper discusses GPT-3, the foundation for OpenAI Codex, the model behind GitHub Copilot. It covers the architecture, training, and performance of GPT-3 in various language tasks, including code generation.
    
- **"Evaluating Large Language Models Trained on Code"**  
    By: Mark Chen et al. (2021)  
    [Paper Link](https://arxiv.org/abs/2107.03374)  
    This paper specifically describes OpenAI Codex, the model that powers GitHub Copilot. It evaluates the performance of Codex in code generation tasks and outlines the challenges of training language models on code.
    

### 2. **Tabnine**

Tabnine is a code completion tool that started as a model based on GPT-2 and later expanded to use more sophisticated models, including large pre-trained models.

- **"TabNine: Using GPT-2 for Code Completion"**  
    This is not an official research paper but an informative blog post or whitepaper by Tabnine on how they use GPT-2 for code completion. Check Tabnine's official blog for technical insights.
    
- **"Code Generation with Pre-trained Language Models"**  
    By: Loubna Ben Allal et al. (2021)  
    [Paper Link](https://arxiv.org/abs/2102.04664)  
    This paper provides insights into how pre-trained language models like GPT-2 (which Tabnine used in earlier versions) can be adapted for code generation tasks. It explores transfer learning for software development.
    

### 3. **Amazon CodeWhisperer**

Amazon CodeWhisperer uses machine learning models to assist developers by suggesting code snippets based on the developer's coding context.

- **"CodeWhisperer: Neural Code Autocompletion in the IDE"**  
    While no specific research paper is available for CodeWhisperer, you can look at related works in neural code completion for IDEs. CodeWhisperer likely leverages models similar to those used in GitHub Copilot and Tabnine.
    
- **"IntelliCode Compose: Code Generation Using Transformer"**  
    By: Alexey Svyatkovskiy et al. (2020)  
    [Paper Link](https://arxiv.org/abs/2005.08025)  
    This paper details how transformer-based models are used for code generation and autocompletion in IDEs. While focused on IntelliCode, the methods are similar to those Amazon CodeWhisperer likely employs.
    

### 4. **General Research on Neural Code Completion**

To gain a broader understanding of how these tools work, you should also explore more general papers on neural code completion and machine learning models for programming tasks:

- **"CoNaLa: Code/Natural Language Challenge"**  
    By: Pengcheng Yin et al. (2018)  
    [Paper Link](https://arxiv.org/abs/1805.09819)  
    This paper presents a dataset and challenge that bridges natural language processing and code generation, providing insights into how code models are trained to generate code from natural language inputs.
    
- **"A Survey of Machine Learning for Big Code and Naturalness"**  
    By: Miltiadis Allamanis et al. (2018)  
    [Paper Link](https://arxiv.org/abs/1709.06182)  
    This survey offers a comprehensive overview of machine learning techniques for programming, covering code completion, generation, and code search.
    
- **"BERT2Code: Can Pretrained Code Models Understand Programs?"**  
    By: Ferdian Thung et al. (2021)  
    [Paper Link](https://arxiv.org/abs/2109.00401)  
    This paper explores whether models like BERT can be applied to code understanding, a precursor to code completion tasks, and can help you understand the use of pre-trained models like BERT in programming tasks.
## Tools

### [TestPilot](https://githubnext.com/projects/testpilot/)  

TestPilot takes the pain out of writing unit tests. It uses GitHub Copilot's AI technology to suggest tests based on your existing code and documentation


https://github.com/Storia-AI/repo2vec



## LLM
- [DeepSeek](https://www.deepseek.com/) Coder comprises a series of code language models trained from scratch on both 87% code and 13% natural language in English and Chinese Open sources and free to use
- https://github.com/huybery/Awesome-Code-LLM 