---
title: Hugging face
date: 2024-06-23T04:57:31.3131+05:30
draft: true
tags:
  - AI
  - genrative_ai
  - machine_learning
---

## Dataset
Datasets to download the data from the Hugging Face Hub. We can use the **list_datasets()** function to see what datasets are available on the Hub:

```python

from datasets import list_datasets
all_datasets = list_datasets()


from datasets import load_dataset
emotions = load_dataset("emotion")

# emotions will have data set for train,validation and test
DatasetDict({
	train: Dataset({
	features: ['text', 'label'],
	num_rows: 16000
	})
	validation: Dataset({
	features: ['text', 'label'],
	num_rows: 2000
	})
	test: Dataset({
	features: ['text', 'label'],
	num_rows: 2000
	})
})

train_ds = emotions["train"]

# convert to pands

import pandas as pd

emotions.set_format(type="pandas")
df = emotions["train"][:]
df.head()

# to load data set from local
load_dataset("csv", data_files="my_file.csv")
```

Datasets are memory-mapped using Apache Arrow and cached locally.This means that only the necessary data will be loaded into memory, allowing the possibility to work with a dataset that is larger than the system memory
## Transformer

 Python library for working with pre-trained natural language processing (NLP) models.
 
Is wrapper which contain tokenizer ,model and the things need for after conversion of model output


The AutoTokenizer class belongs to a larger set of “auto” classes whose job is to automatically retrieve the model’s configuration, pretrained weights, or vocabulary from the name of the checkpoint.


```python

from transformers import AutoTokenizer
model_ckpt = "distilbert-base-uncased"
tokenizer = AutoTokenizer.from_pretrained(model_ckpt)

encoded_text = tokenizer(text)
print(encoded_text)

#both do same

from transformers import DistilBertTokenizer
distilbert_tokenizer = DistilBertTokenizer.from_pretrained(model_ckpt)
```

- it uses ONNX [[Hugging face#ONNX]] runtime to run on all device

## Inference

Inference is the process of using a trained model to make predictions on new data. As this process can be compute-intensive, running on a dedicated server can be an interesting option. The `huggingface_hub` library provides an easy way to call a service that runs inference for hosted models. There are several services you can connect to:

- [Inference API](https://huggingface.co/docs/api-inference/index): a service that allows you to run accelerated inference on Hugging Face’s infrastructure for free. This service is a fast way to get started, test different models, and prototype AI products.
- [Inference Endpoints](https://huggingface.co/docs/inference-endpoints/index): a product to easily deploy models to production. Inference is run by Hugging Face in a dedicated, fully managed infrastructure on a cloud provider of your choice.

https://huggingface.co/docs/hub/en/models-widgets 

### Pipelines

It connects a model with its necessary preprocessing and postprocessing steps, allowing us to directly input any text and get an intelligible answer:

```python
from transformers import pipeline

classifier = pipeline("sentiment-analysis")
classifier("I've been waiting for a HuggingFace course my whole life.")
```

Some of the currently [available pipelines](https://huggingface.co/transformers/main_classes/pipelines) are:

- `feature-extraction` (get the vector representation of a text)
- `fill-mask`
- `ner` (named entity recognition)
- `question-answering`
- `sentiment-analysis`
- `summarization`
- `text-generation`
- `translation`
- `zero-shot-classification`

#### Using any model from the Hub in a pipeline

```python
from transformers import pipeline

generator = pipeline("text-generation", model="distilgpt2")
generator(
    "In this course, we will teach you how to",
    max_length=30,
    num_return_sequences=2,
)
```


- **Checkpoints**: These are the weights that will be loaded in a given architecture.

#### Pipelines under the hood

first step of our pipeline is to convert the text inputs into numbers that the model can make sense of. To do this we use a _tokenizer_, which will be responsible for:

- Splitting the input into words, subwords, or symbols (like punctuation) that are called _tokens_
- Mapping each token to an integer
- Adding additional inputs that may be useful to the model

All this preprocessing needs to be done in exactly the same way as when the model was pretrained, so we first need to download that information from the [Model Hub](https://huggingface.co/models). To do this, we use the `AutoTokenizer` class and its `from_pretrained()` method. Using the checkpoint name of our model

```python
from transformers import AutoTokenizer

checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)
```

Note: Transformer models only accept _tensors_ as input.

```python
raw_inputs = [
    "I've been waiting for a HuggingFace course my whole life.",
    "I hate this so much!",
]
inputs = tokenizer(raw_inputs, padding=True, truncation=True, return_tensors="pt")
print(inputs)

{
    'input_ids': tensor([
        [  101,  1045,  1005,  2310,  2042, ...],
        [  101,  1045,  5223,  ....]
    ]), 
    'attention_mask': tensor([
        [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0]
    ])
}
```

The output itself is a dictionary containing two keys, `input_ids` and `attention_mask`. `input_ids`

we can use  Transformers without having to worry about which ML framework is used as a backend; it might be PyTorch or TensorFlow, or Flax for some models. but transformer will take care of it.

To specify the type of tensors we want to get back (PyTorch, TensorFlow, or plain NumPy), we use the `return_tensors` argument:

Next step pass the token to transformer

```python 
from transformers import AutoModel

checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
model = AutoModel.from_pretrained(checkpoint)

outputs = model(**inputs)

print(outputs.logits.shape)
```

 A Transformer model processes text and produces a high-dimensional vector of hidden states, which represent the model's contextual understanding of the input

These hidden states are usually fed into another part of the model called a **head**.
The head transforms these high-dimensional vectors into a format suitable for a specific task.

Example: 
 
 **Classification Head**
- **Purpose**: Used for tasks where the goal is to assign an input to one of several predefined categories (e.g., sentiment analysis, image classification).
- **How It Works**:
    - The output from the transformer layers (hidden states) is fed into a linear layer, which projects the high-dimensional vectors down to the number of classes.
    - A softmax function is then applied to convert these scores into probabilities for each class.

**Sequence Generation Head**

- **Purpose**: Used for tasks where the goal is to generate a sequence of outputs, such as text generation or machine translation.
- **How It Works**:
    - Typically, this involves a decoder structure that predicts the next token in the sequence based on the previous tokens and the context provided by the encoder.
    - It often uses techniques like beam search or greedy decoding to generate coherent sequences.

Different head architectures are designed for specific tasks. Some examples are:
- ForCausalLM
- ForMaskedLM
- ForMultipleChoice
- ForQuestionAnswering
- ForSequenceClassification
- ForTokenClassification

The output of the head often requires further processing to make sense of it
For instance, the raw output of the model (logits) may need to be converted into probabilities using a SoftMax layer

**Postprocessing the output**

Output will contain probablity to convert we need softmax layer(Transformers models output the logits, as the loss function for training will generally fuse the last activation function, such as SoftMax, with the actual loss function, such as cross entropy)

```python
from transformers import AutoTokenizer, AutoModelForSequenceClassification
import torch

# Define the model checkpoint
checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"

# Load the tokenizer and model
tokenizer = AutoTokenizer.from_pretrained(checkpoint)
model = AutoModelForSequenceClassification.from_pretrained(checkpoint)

# Input sentences
raw_inputs = [
    "I've been waiting for a HuggingFace course my whole life.",
    "I hate this so much!",
]

# Tokenize the input sentences
inputs = tokenizer(raw_inputs, padding=True, truncation=True, return_tensors="pt")

# Pass the inputs through the model
outputs = model(**inputs)

# Convert logits to probabilities
predictions = torch.nn.functional.softmax(outputs.logits, dim=-1)

# Print the predictions and labels
print(predictions)
print(model.config.id2label)
```


## Models

 The `AutoModel` class in Hugging Face acts as a wrapper for different model architectures within the library. This class can intelligently determine the appropriate model architecture for a given checkpoint and instantiate a model with that architecture. For instance, if we load a BERT checkpoint, `AutoModel` will automatically instantiate a BERT model.
 
```python
from transformers import BertConfig, BertModel

# Building the config
config = BertConfig()

# Building the model from the config
model = BertModel(config)
```

**Direct Instantiation:** If we know the specific model type you want to use, we can directly use the class corresponding to its architecture. For example, we can use `BertModel` directly to create a BERT model.
   
**Model Configuration:** Models are built based on a configuration object, like `BertConfig` for BERT models. This configuration contains attributes that define the model's architecture, such as the hidden state size (`hidden_size`) and the number of Transformer layers (`num_hidden_layers`).
   
**Model Initialization:** Creating a model from the default configuration initializes it with random values. Such a model needs to be trained before it can be used effectively.
   
 **Loading Pre-trained Models:** Pre-trained models can be loaded using the `from_pretrained()` method. This method takes a model identifier (e.g., "bert-base-cased") and downloads and caches the model weights. Using pre-trained models is crucial to save time, resources, and minimize environmental impact.
 
```python
from transformers import BertModel

model = BertModel.from_pretrained("bert-base-cased")
```
   
**Checkpoint Agnostic Code:** The `AutoModel` class allows you to write checkpoint-agnostic code, which means your code can work with different checkpoints, even if the architecture is different, as long as the checkpoints are trained for similar tasks.
  
 **Saving Models:** The `save_pretrained()` method saves the model to your disk in two files: `config.json` and `pytorch_model.bin`.
    - `config.json`: This file contains the model's architecture and metadata.
    - `pytorch_model.bin`: This file, known as the state dictionary, contains the model's weights.

 **Model Inference:** Once loaded, models can be used for inference. This involves tokenizing the input text and converting it into tensors that the model can understand.
    - The `model()` function is then called with these tensors to generate predictions.
    - For specific tasks like sentiment analysis, we might use specialized models like `AutoModelForSequenceClassification`.
    - The output of a model often requires further processing, such as converting logits into probabilities using a SoftMax layer, before interpretation.


```python
from transformers import AutoConfig, AutoModel, AutoTokenizer

# Model identifier
model_id = "bert-base-cased"

# Loading the configuration, tokenizer, and model
config = AutoConfig.from_pretrained(model_id)
tokenizer = AutoTokenizer.from_pretrained(model_id)
model = AutoModel.from_pretrained(model_id)

# Example input sequences
sequences = ["Hello!", "Cool.", "Nice!"]

# Tokenization
encoded_sequences = tokenizer(sequences, padding=True, truncation=True, return_tensors="pt")
model_inputs = torch.tensor(encoded_sequences["input_ids"])

# Model inference
output = model(model_inputs)

# Saving the model
model.save_pretrained("my_model_directory")

# Accessing configuration attributes
print(config.hidden_size)
print(config.num_hidden_layers)
```

**Models expect a batch of inputs**

Batching involves sending multiple sentences to the model at once. 

```python
sequence = "I've been waiting for a HuggingFace course my whole life."

tokens = tokenizer.tokenize(sequence)
ids = tokenizer.convert_tokens_to_ids(tokens)
input_ids = torch.tensor(ids)
# This line will fail.
model(input_ids) # it is single dim array 
```

So to work with model we need to pass it as batch as 

```python
input_ids = torch.tensor([ids]) # we converting in to 2d by  [] enclosing 
print("Input IDs:", input_ids)

output = model(input_ids)
print("Logits:", output.logits)
```

Another method of overriding this issue is padding

 **Padding the inputs:** Sentences in a batch often have different lengths. To address this, padding is used. Padding involves adding a special token called the "padding token" to shorter sentences, making all sentences in the batch the same length. This is essential because tensors require a rectangular shape.

The padding token ID can be found in `tokenizer.pad_token_id`

But when we have huge data let say 1TB when we doing Padding it will take max token length and do padding for all which is not efficient so to avoid we can do `batch wise padding` and `DataCollatorWithPadding`

```python
def tokenize_and_pad(batch):
    return tokenizer(batch['text'], padding=True, truncation=True, return_tensors="pt")

# Apply the map function
tokenized_dataset = dataset.map(tokenize_and_pad, batched=True)

```

 **Attention masks:** When using padding, it's crucial to use attention masks. Attention masks are tensors that guide the model to focus on the actual tokens and ignore the padding tokens. This ensures accurate results, as attention layers in Transformers models contextualize each token. Without attention masks, padding tokens would be incorrectly considered in the attention mechanism.

**Longer sequences:** Transformer models have limitations on the sequence length they can process. Most models handle up to 512 or 1024 tokens. To handle longer sequences:
- Utilize models specifically designed for long sequences, such as Longformer or LED.
- Truncate the sequences to the maximum supported length using the `max_sequence_length` parameter.


## Tokenzier

- https://github.com/openai/tiktoken
The process of converting text to numbers is called encoding.

Encoding involves two steps:
- **Tokenization**: splitting text into smaller units (tokens) such as words, characters, or subwords.
- **Conversion to Input IDs**: mapping each token to a unique numerical identifier from the tokenizer's vocabulary.

**AutoTokenizer:**    
 - This class is a more generic tokenizer class that acts as a wrapper, enabling you to load tokenizers for different model architectures without explicitly specifying the tokenizer class.
- It can intelligently determine the correct tokenizer class based on the model checkpoint name we provide.
- For instance, if we use `AutoTokenizer.from_pretrained("bert-base-cased")`, it will automatically load the `BertTokenizer` class because the checkpoint name indicates a BERT model.

Methods 
- **`tokenizer()`:** This method performs the complete encoding process, converting raw text into input IDs.
- **`tokenize()`:** This method handles only the tokenization step, splitting the text into tokens.
- **`convert_tokens_to_ids()`:** This method converts a list of tokens into their corresponding numerical IDs.
- **`decode()`:** This method performs the reverse operation of encoding, converting a list of input IDs back into a text string.

```python
from transformers import AutoTokenizer

# Load the pre-trained tokenizer
tokenizer = AutoTokenizer.from_pretrained("bert-base-cased")

# Input text
text = "This is a sample text for tokenization and decoding."

# Tokenize the input text
tokens = tokenizer.tokenize(text)
print("Tokens:", tokens)
["this","i",...]

# Convert tokens to input IDs
input_ids = tokenizer.convert_tokens_to_ids(tokens)
print("Input IDs:", input_ids)

# Decode the input IDs back to text
decoded_text = tokenizer.decode(input_ids)
print("Decoded Text:", decoded_text)

print(tokenizer.vocab_size) # print the no of vocab the model have

print(tokenizer.model_max_length) #max len for the model
```

- it uses Rust under the hood

**Preprocessing the data**

Padding and Truncation: Sentences often have varying lengths. To create uniform input tensors, the tokenizer uses padding (adding a special padding token to shorter sequences) and truncation (shortening sequences that exceed the model's maximum length).

```python
batch_sentences = [
    "But what about second breakfast?",
    "Don't think he knows about second breakfast, Pip.",
    "What about elevensies?",
]
encoded_input = tokenizer(batch_sentences, padding=True, truncation=True, return_tensors="pt")
print(encoded_input)
```


## Trainer

Transformers provides a [Trainer](https://huggingface.co/docs/transformers/v4.46.0/en/main_classes/trainer#transformers.Trainer) class optimized for training  Transformers models, making it easier to start training without manually writing your own training loop. The [Trainer](https://huggingface.co/docs/transformers/v4.46.0/en/main_classes/trainer#transformers.Trainer) API supports a wide range of training options and features such as logging, gradient accumulation, and mixed precision.

The Trainer is built on PyTorch, so it's not suitable for projects that use Keras or TensorFlow

The Trainer may not be the best choice for highly specialized training logic. In such cases, the Accelerate library offers more fine-grained control.


## Hugging Face X Langchain

`langchain-huggingface` 

```python 
from langchain_huggingface import HuggingFacePipeline

llm = HuggingFacePipeline.from_model_id(
    model_id="microsoft/Phi-3-mini-4k-instruct",
    task="text-generation",
    pipeline_kwargs={
        "max_new_tokens": 100,
        "top_k": 50,
        "temperature": 0.1,
    },
)
llm.invoke("Hugging Face is")

```

Accessing the inference on serverless model

```python 
from langchain_huggingface import HuggingFaceEndpoint

llm = HuggingFaceEndpoint(
    repo_id="meta-llama/Meta-Llama-3-8B-Instruct",
    task="text-generation",
    max_new_tokens=100,
    do_sample=False,
)
llm.invoke("Hugging Face is")

```

https://huggingface.co/blog/langchain



## ONNX
  
ONNX is an open format built to represent machine learning models. ONNX defines a common set of operators - the building blocks of machine learning and deep learning models - and a common file format to enable AI developers to use models with a variety of frameworks, tools, runtimes, and compilers

ONNX enables exporting trained models from one framework and importing them into another, facilitating seamless transitions between different tools.

An ONNX model file contains a graph representing the model's structure. This graph is a collection of computation nodes, analogous to the layers in a neural network

Each computation node represents a specific operation, defined by an operator. These operators map to deep learning conventions and encompass functions like activation functions (ReLU, sigmoid, tanh), convolutional operations, and more.

ONNX supports standard data types for tensors (int8, int16, bool, float16, etc.) as well as non-tensor types like sequences and maps for traditional machine learning

**ONNX Runtime** is an open-source inference engine that implements the ONNX standard. Its primary goal is to provide high-performance inference across a wide range of platforms and hardware.ONNX Runtime achieves this through a pluggable architecture that allows for the integration of different execution providers, each optimized for specific hardware


## Transformers.js v3

We can use transformer js and run models in brower using webGPU (WebGPU is a new web standard for accelerated graphics and compute).


```js
import { pipeline } from "@huggingface/transformers";

// Create a feature-extraction pipeline
const extractor = await pipeline(
  "feature-extraction",
  "mixedbread-ai/mxbai-embed-xsmall-v1",
  { device: "webgpu" },
);

// Compute embeddings
const texts = ["Hello world!", "This is an example sentence."];
const embeddings = await extractor(texts, { pooling: "mean", normalize: true });
console.log(embeddings.tolist());
// [
//   [-0.016986183822155, 0.03228696808218956, -0.0013630966423079371, ... ],
//   [0.09050482511520386, 0.07207386940717697, 0.05762749910354614, ... ],
// ]

```



## Resources
- [Huggingface 🤗 is all you need for NLP and beyond ](https://jarvislabs.ai/blogs/hf-getting-started )
- https://paperswithcode.com/sota
- https://playground.tensorflow.org/
- https://github.com/ajinkyakolhe112/Huggingface-NLP-COURSE-NOTES/tree/main/1.%20Introduction%20to%20Huggingface/3.%20Fine-tuning%20a%20Pretrained%20model

Visualizer for neural network, deep learning and machine learning models https://netron.app/

Deep Learning Visualization Toolkit https://github.com/PaddlePaddle/VisualDL


Depending on the [model](https://beta.openai.com/docs/engines/gpt-3) used, requests can use up to 128,000 tokens shared between prompt and completion. Some models, like GPT-4 Turbo, have different limits on input and output tokens.

There are often creative ways to solve problems within the limit, e.g. condensing your prompt, breaking the text into smaller pieces, etc.



## Models
- https://huggingface.co/myshell-ai/MeloTTS-English
- [text-to-audio](https://huggingface.co/suno/bark )
- Bloom
- openai-community/gpt2
- https://huggingface.co/1bitLLM 
- SmolLM models are designed for local deployment and have low memory footprints, making them suitable for devices like smartphones
- Human-Like-Llama-3-8B-Instruct 
- Human-Like-Qwen-2.5-7B-Instruct

Here are some interesting and lightweight LLMs available on Hugging Face that you can use for various projects:

1. **DistilBERT**: A smaller, faster, and cheaper version of BERT, retaining 97% of its language understanding while being 60% faster. Great for text classification and sentiment analysis.

2. **TinyBERT**: An even smaller version of BERT, optimized for mobile and edge devices. It's useful for applications requiring low latency.

3. **ALBERT**: A lightweight model that reduces the parameters of BERT while maintaining performance. It’s great for tasks like text classification and question answering.

4. **MiniLM**: A compact model that balances speed and performance, making it suitable for a range of NLP tasks, including summarization and dialogue systems.

5. **ELECTRA**: This model is more sample-efficient than traditional masked language models, making it great for text generation and understanding tasks with fewer resources.

6. **T5 (Text-to-Text Transfer Transformer)**: Though larger, you can find smaller variants. T5 is versatile, allowing you to tackle various tasks by framing them as text generation problems.

7. **GPT-Neo**: An open-source alternative to GPT-3, with smaller versions available. Good for creative writing, chatbots, and text generation projects.

8. **BART (with smaller configurations)**: BART is great for text generation and summarization tasks. Smaller configurations can be effective for various applications without being too heavy.

9. **Flan-T5**: A variant of T5 that's fine-tuned on a diverse set of tasks. It's useful for applications needing generalization across multiple NLP tasks.

10. **CodeGen**: A model designed for code generation tasks. If you're interested in building tools related to programming or code assistance, this could be a fun choice.
11. # LLaVA 



You can easily find these models on the Hugging Face Model Hub. Depending on your project, consider the trade-offs between model size, performance, and the specific task you want to tackle!



