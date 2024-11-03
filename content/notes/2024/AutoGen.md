---
title : AutoGen
date : 2024-11-02T14:52:59.5959+05:30
draft : true
tags : 
---

Autogen is an open-source, multi-agent framework that enables multiple AI agents to interact and collaborate to solve tasks

It use chat like approach and build a framework where every agents will intract in way of chat to solve the problem

## Agents

In AutoGen, an agent is an entity that can send and receive messages to and from other agents in its environment. An agent can be powered by models (such as a large language model like GPT-4), code executors (such as an IPython kernel), human, or a combination of these and other pluggable and customizable components.

**Builtin Agents**
- ConversableAgent
- UserProxyAgent (this will act as user)
- AssistantAgent  (act as an AI assistant)

**Types**

- **Conversable**: Agents in AutoGen are conversable, which means that any agent can send and receive messages from other agents to initiate or continue a conversation
- **Customizable**: Agents in AutoGen can be customized to integrate LLMs, humans, tools, or a combination of them.


Simple agent

```python
from autogen import ConversableAgent

config_list = [{"model": "gpt-4", "api_key": os.environ["OPENAI_API_KEY"]}]

agent = ConversableAgent(
    "chatbot",
    llm_config={
		"cache_seed": 41, # seed for caching and reproducibility  
		"config_list": config_list, # a list of OpenAI API configurations  
		"temperature": 0,
    },
    code_execution_config=False,  # Turn off code execution, by default it is off.
    function_map=None,  # No registered functions, by default it is None.
    human_input_mode="NEVER",  # Never ask for human input.
    
)

reply = agent.generate_reply(messages=[{"content": "Tell me a joke.", "role": "user"}])  
print(reply)
```
- **cache_seed** will create a cache folder with name we passed and store the data and use from there when we run use a same prompt again
- 
**Multi Agent**

```python

import os
from autogen import AssistantAgent, UserProxyAgent
from autogen.coding import DockerCommandLineCodeExecutor

config_list = [{"model": "gpt-4", "api_key": os.environ["OPENAI_API_KEY"]}]

# create an AssistantAgent instance named "assistant" with the LLM configuration.
assistant = AssistantAgent(name="assistant", llm_config={"config_list": config_list})

# create a UserProxyAgent instance named "user_proxy" with code execution on docker.
code_executor = DockerCommandLineCodeExecutor()

user_proxy = UserProxyAgent(name="user_proxy", code_execution_config={"executor": code_executor})

# the assistant receives a message from the user, which contains the task description
chat_res = user_proxy.initiate_chat(
    assistant,
    message="""What date is today? Which big tech stock has the largest year-to-date gain this year? How much is the gain?""",
)


```

1. The assistant receives a message from the `user_proxy`, which contains the task description.
2. The assistant then tries to write Python code to solve the task and sends the response to the user_proxy.
3. Once the `user_proxy` receives a response from the assistant, it tries to reply by either soliciting human input or preparing an automatically generated reply. If no human input is provided, the `user_proxy` executes the code and uses the result as the auto-reply.
4. The assistant then generates a further response for the user_proxy. The user_proxy can then decide whether to terminate the conversation. If not, steps 3 and 4 are repeated.
5. chat_res will contain following things
	- `chat_history`: a list of chat history.
	- `summary`: a string of chat summary. A summary is only available if a summary_method is provided when initiating the chat.
	- `cost`: a tuple of (total_cost, total_actual_cost), where total_cost is a dictionary of cost information, and total_actual_cost is a dictionary of information on the actual incurred cost with cache.
	- `human_input`: a list of strings of human inputs solicited during the chat. (Note that since we are setting `human_input_mode` to `NEVER` in this notebook, this list is always empty.)

Proxy Agent params
### Agent Configuration Notes

- **Name**: `str` - Name of the agent.
- **Termination Message Check**: 
  - **Function**: `is_termination_msg(msg: dict) -> bool` - Checks if a message is a termination message.
- **Auto Reply Settings**: 
  - **Max Consecutive Auto Replies**: `int` - Default is `None` (uses `MAX_CONSECUTIVE_AUTO_REPLY`).
- **Human Input Mode**: 
  - **Options**: 
    - `"ALWAYS"`: Prompts for human input every time. Stops on "exit" or if termination message received.
    - `"TERMINATE"`: Prompts only on termination messages or max auto replies reached.
    - `"NEVER"`: No prompts. Stops on max auto replies or termination message.
- **Function Mapping**: `dict[str, callable]` - Maps function names to callable functions. 
- **Code Execution Config**: 
  - `dict` or `False` - Config for executing code, including `work_dir`, `use_docker`, `timeout`, `last_n_messages`.
- **Default Auto Reply**: `str` or `dict` or `None` - Default message if no reply is generated.
- **LLM Config**: `dict` or `False` or `None` - Configuration for LLM inference; defaults to `False`.
- **System Message**: `str` or `List` - System message for ChatCompletion inference.
- **Description**: `str` - Short description of the agent for other agents to decide on calls.
## Conversation Patterns

- Two Agent chat
- Sequential chats
- Group chat
- Nested chat


## Code Exector

- Docker
- Local Command Line
- PodCommandLineCodeExecutor

```python

from autogen.coding import LocalCommandLineCodeExecutor
from autogen.coding import DockerCommandLineCodeExecutor

code_executor = DockerCommandLineCodeExecutor()


user_proxy = UserProxyAgent(name="user_proxy", code_execution_config={"executor": code_executor})
```