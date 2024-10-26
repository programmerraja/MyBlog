+++
title = 'AI'
date = 2024-05-18T18:29:19.1919+05:30
draft = true
tags =[]
+++ 


LLM contain 
- Parameters
- code that run parameters

Parameters

Neural network 
- predict the next word

Pretraining,finetunning



GPU are good at matrix multiplication so only they need for AI beacause all AI are matrix multiplication


Deep learning models
- Discriminative  -> classify or predict trainned on labeled data
- Generative -> predict next word in sequenece,understand distribution of data

Computational linguistics: strudy of language grammer,syntax and phonetics

## Embedding

https://huggingface.co/blog/how-to-generate
Word embedding algo
1. Wrord2Vec (CBOW and Skip grammer)
2. Glove (Global vector for word representation)
3. ELMOC
4. BERT
5. Transformer based models (generative pretrained transformer and text to text transformer)

#### Word2vec

There are two main models in Word2Vec: Continuous Bag of Words (CBOW) and Skip-gram

### Continuous Bag of Words (CBOW)

The CBOW model predicts the current word based on the context (surrounding words). For instance, in the sentence:

_"The quick brown fox jumps over the lazy dog"_

If we want to predict the word "fox" (target word), the context words might be "The", "quick", "brown", "jumps", "over".

#### Example Process for CBOW:

1. **Input Context Words**: Let's consider a simplified version where we use a window of size 2 (two words on each side of the target word). 
     - Context words for "fox" would be ["quick", "brown", "jumps", "over"].
3. **One-Hot Encoding**: Each context word is converted into a one-hot vector. If our vocabulary consists of the words ["The", "quick", "brown", "fox", "jumps", "over", "the", "lazy", "dog"], the one-hot vector for "quick" might look like: "quick" → [0, 1, 0, 0, 0, 0, 0, 0, 0]
4. **Hidden Layer**: These one-hot vectors are fed into a neural network with a single hidden layer. The hidden layer typically has fewer neurons than the vocabulary size, capturing the dense representations.
5. **Output Layer**: The network then combines the hidden layer's outputs to predict the target word. The output is a probability distribution over the vocabulary.   
6. **Training**: The network is trained using backpropagation to minimize the prediction error, adjusting weights to improve accuracy.

### Skip-gram

The Skip-gram model works oppositely to CBOW. It predicts context words from the target word. This model is particularly effective for larger datasets.

#### Example Process for Skip-gram:

Using the same sentence, "The quick brown fox jumps over the lazy dog":

1. **Target Word**: Let's choose "fox" as our target word.
2. **Context Words**: For "fox", the context words could be ["quick", "brown", "jumps", "over"] using a window size of 2.
3. **One-Hot Encoding**: Convert the target word "fox" into a one-hot vector.
    - "fox" → [0, 0, 0, 1, 0, 0, 0, 0, 0]
4. **Hidden Layer**: This one-hot vector is fed into the hidden layer, producing a dense vector representation of the target word.
5. **Output Layer**: The dense vector is then used to predict the context words. The network outputs a probability distribution over the vocabulary for each context word.
6. **Training**: The network is trained by adjusting the weights to maximize the probability of the correct context words given the target word.

**Cosine similarity** is a metric used to measure how similar two vectors

**Euclidean Distance:** It measures the straight-line distance between two vectors in the multidimensional space

**Manhattan Distance (L1 Distance):** This calculates the sum of the absolute differences between corresponding elements of two vectors.

**One-hot encoding** is a technique used to represent categorical data as binary vectors. Each category is converted into a vector where one element is "hot" (1) and all other elements are "cold" (0).
**Example:** Let say we take fruits and consider it only having 5 varites such as apple,orange,mango,grapes and banana the one hot encoding will be
- "apple" → [1, 0, 0, 0, 0]
- "orange" → [0, 1, 0, 0, 0]
- "mango" → [0, 0, 1, 0, 0]
- "grapes" → [0, 0, 0, 1, 0]
- "banana" → [0, 0, 0, 0, 1]

The main disadvantage is If the number of unique categories is very large, the resulting one-hot encoded data can become very high-dimensional and sparse.

**Stemming** is the process of reducing words to their root or base form by removing suffixes, such as converting "running" to "run" and "happily" to "happi".

**Lemmatization** is the process of reducing words to their base or dictionary form (lemma) by considering the context and morphological analysis, such as converting "running" to "run" and "better" to "good".

lib that used for above nltk, spacy in python


https://www.tensorflow.org/text/tutorials/word2vec

### GloVe
Global Vectors for Word Representation.Unlike other word embedding methods like Word2Vec, which rely on local context information (e.g., predicting a word based on its neighbors), GloVe leverages global word co-occurrence statistics from a corpus to capture the meanings of words.

GloVe constructs a word-word co-occurrence matrix using the entire corpus. Each element of this matrix represents how often a pair of words appears together in a context window like below let say we have a "**The cat is fluffy. The dog is fluffy. The cat and the dog are friends.**"

| Word    | the | cat | is  | fluffy | dog | and | are | friends |
| ------- | --- | --- | --- | ------ | --- | --- | --- | ------- |
| the     | 0   | 2   | 2   | 2      | 2   | 1   | 1   | 1       |
| cat     | 2   | 0   | 1   | 1      | 1   | 1   | 0   | 0       |
| is      | 2   | 1   | 0   | 2      | 1   | 0   | 0   | 0       |
| fluffy  | 2   | 1   | 2   | 0      | 1   | 0   | 0   | 0       |
| dog     | 2   | 1   | 1   | 1      | 0   | 1   | 0   | 0       |
| and     | 1   | 1   | 0   | 0      | 1   | 0   | 1   | 1       |
| are     | 1   | 0   | 0   | 0      | 0   | 1   | 0   | 1       |
| friends | 1   | 0   | 0   | 0      | 0   | 1   | 1   | 0       |

We create vectors (magic numbers) for each word. Here’s an example of what the vectors might look like after training

| Word    | Vector     |
| ------- | ---------- |
| the     | [0.8, 0.6] |
| cat     | [0.3, 0.7] |
| is      | [0.5, 0.5] |
| fluffy  | [0.3, 0.8] |
| dog     | [0.4, 0.7] |
| and     | [0.6, 0.4] |
| are     | [0.6, 0.3] |
| friends | [0.7, 0.2] |
#### Dimension
- The dimensionality of word embedding refers to the number of dimensions in which the vector representation of a word is defined.
- This is typically a fixed value determined while creating the word embedding. 
- The dimensionality of the word embedding represents the total number of features that are encoded in the vector representation.
- Larger datasets can support higher-dimensional embeddings as they provide more training data to inform the model. As a rule of thumb, a dataset with less than 100,000 sentences may benefit from a lower-dimensional embedding

Two paths for custom data LLM
1. Fine tune it with custom data to answer
2. Retrieve text related to question at runtime (vector search) and feed relevant text with question to LLM to answer. example (chroma and langchanin)


Modern LLM stack
- Data bricks -> data pipelines preprocessing and summary analytics
- hugging face -> datasets ,models,tokenizers and inference tools
- mosaic ML -> model traning ,LLM config and manged GPU


### Keras embedding layer

Keras offers an embedding layer that can be used for neural networks, such as RNN’s (recurrent neural networks) for text data. This layer is defined as the first layer of a more complex architecture. The embedding layer needs at least three input values:

- **input_dim:** Integer. Size of the vocabulary, i.e. maximum integer index+1.
- **output_dim:** Integer. Dimension of the dense embedding.
- **input_length:** Length of input sequences, when it is constant. This argument is required if you are going to connect Flatten then Dense layers upstream (without it, the shape of the dense outputs cannot be computed).


```python
import numpy as np
from keras.preprocessing.text import Tokenizer
from keras.preprocessing.sequence import pad_sequences
from keras.models import Sequential
from keras.layers import Embedding, Flatten, Dense

# Sample training data
texts = [
    "I love this movie, it is fantastic!",
    "The movie was okay, not great.",
    "I did not like the movie, it was boring.",
    "Fantastic film, I enjoyed every moment!",
    "Terrible movie, I won't watch it again."
]
labels = [1, 0, 0, 1, 0]  # 1 for positive, 0 for negative

# Tokenize the text
tokenizer = Tokenizer(num_words=10000)
tokenizer.fit_on_texts(texts)
sequences = tokenizer.texts_to_sequences(texts)

# Pad sequences to ensure uniform length
max_length = 10
data = pad_sequences(sequences, maxlen=max_length)

# Convert labels to a numpy array
labels = np.array(labels)

# Define vocabulary size and embedding dimensions
vocab_size = 10000
embedding_dim = 50

# Create the model
model = Sequential()
model.add(Embedding(input_dim=vocab_size, output_dim=embedding_dim, input_length=max_length))
model.add(Flatten())
model.add(Dense(1, activation='sigmoid'))  # For binary classification

# Compile the model
model.compile(optimizer='adam', loss='binary_crossentropy', metrics=['accuracy'])

# Print the model summary
model.summary()

# Train the model
model.fit(data, labels, epochs=10, batch_size=2, validation_split=0.2)

# Sample test data
test_texts = [
    "I really enjoyed this film, it was excellent!",
    "The film was not good, I did not enjoy it.",
]
test_labels = [1, 0]  # 1 for positive, 0 for negative

# Tokenize the test text
test_sequences = tokenizer.texts_to_sequences(test_texts)

# Pad sequences to ensure uniform length
test_data = pad_sequences(test_sequences, maxlen=max_length)

# Convert test labels to a numpy array
test_labels = np.array(test_labels)

# Evaluate the model on the test data
loss, accuracy = model.evaluate(test_data, test_labels, batch_size=2)
print(f"Test Loss: {loss}")
print(f"Test Accuracy: {accuracy}")

# Make predictions on the test data
predictions = model.predict(test_data)

# Print the predictions and corresponding test labels
for i, prediction in enumerate(predictions):
    print(f"Text: {test_texts[i]}")
    print(f"Predicted: {'Positive' if prediction > 0.5 else 'Negative'}")
    print(f"Actual: {'Positive' if test_labels[i] == 1 else 'Negative'}\n")

```


## Open Source Embedding models 

 Two key resources for open-source embeddings:

- **Sentence Transformers (expert.net):** This Python framework simplifies loading and using various embedding models, including the popular "all-mpnet-base-v2" and "all-MiniLM-L6-v2".
- **Hugging Face (huggingface.co):** This platform hosts a vast collection of machine learning models and datasets, including the "Massive Text Embedding Benchmark" (MTEB) project, which ranks and evaluates embedding models.

**Choosing the Right Embedding Model**

- **Task:** Different models specialize in different tasks, like semantic search, clustering, or bitext mining.
- **Performance:** The MTEB leaderboard offers a valuable resource for comparing model performance across various tasks.
- **Dimension Size:** Smaller dimensions generally result in faster computation and lower memory requirements, especially for similarity searches.
- **Sequence Length:** Models have limitations on the input length (measured in tokens), impacting how you process longer documents.

**Optimizing Embedding Generation**

- **Mean Pooling:** This aggregation method combines multiple embeddings into a single representative embedding, essential for sentence-level comparisons.

	Example:
	Text embedding model will return text the probablity of passed text with the no of vocabulary it has let say we have a text embedding model with dimension of 468 then it have 468 voc so it will return the probablity of passed text with all 468 word but if we want a single probablity we need to use meanpooling 

- **Normalization:** Normalizing embeddings (creating unit vectors) enables accurate comparisons using methods like dot product.
- **Quantization:** This technique reduces the precision of model weights, shrinking the model size and potentially improving inference speed.
- **Caching:** Transformers.js automatically caches models in the browser, significantly speeding up subsequent inference operations.
### Byte Pair Encoding (BPE): A Tokenization Technique

BPE is a data compression algorithm that has been adapted for tokenization. It's a bottom-up approach that starts by treating each character in the training data as a separate token. Then, it iteratively merges the most frequently occurring pairs of tokens into a single new token. This process continues until a predefined vocabulary size or a merging criteria is met.

Let's illustrate with a simplified example. Suppose our training data contains the words "lower," "lowest," "newer," and "newest."

- Initial Tokens: Start with individual characters as tokens: 'l', 'o', 'w', 'e', 'r', 's', 't', 'n', 'ew'.

- Merging: The pair 'e' and 'r' might occur most frequently, so they are merged into a new token 'er'.

### WordPiece

WordPiece tokenization is a subword tokenization algorithm developed by Google,
### Massive Text Embedding Benchmark 

MTEB, is a project developed by Hugging Face to address the challenge of evaluating and comparing the performance of different text embedding models

MTEB goes beyond a single metric and instead evaluates models across a wide spectrum of tasks1. This ensures a more holistic understanding of a model's capabilities. Some of the tasks covered by MTEB include2:

Bitext Mining: Identifying pairs of sentences in different languages that convey the same meaning.

Classification: Assigning predefined categories to text snippets.

Clustering: Grouping text snippets based on their semantic similarity.

Pair Classification: Determining the relationship between two text snippets (e.g., paraphrase, contradiction).

Re-ranking: Ordering search results based on their relevance to a given query.

Retrieval: Finding the most relevant documents for a specific query (often used in semantic search).

Semantic Textual Similarity (STS): Measuring the degree of semantic overlap between two text snippets.

Summarization: Evaluating how well a short text summarizes a longer document.
### Resources
- https://medium.com/@RobinVetsch/nlp-from-word-embedding-to-transformers-76ae124e6281
- [Text Embeddings: Comprehensive Guide](https://towardsdatascience.com/text-embeddings-comprehensive-guide-afd97fce8fb5)

## Transformer

Types
1. Encode only model -> translate tokens in to semantically meaningful rep (text classfication)
2. Decode only model -> predict next text (Text genereation)  GPT family and LLAMA
3. Both encode and decode model -> BART (translation)

**Encoder:** Encodes input with context val understanding and produces one vector per token

**Decoder:** Accept input token and generate new token

**Max new tokens:** No of tokens that will generate


Natural language model

LoRA (Low-Rank Adaptation) and PEFT (Parameter-Efficient Fine-Tuning) are techniques used in the field of natural language processing and machine learning to fine-tune large pre-trained models in a more computationally and memory-efficient manner.


## ollama Notes

- uses docker to run all model
- Model stored in  /user/share/ollama/.ollama/models
- which has blobs and manifests/registry.ollama.ai
- blobs contain actula model code 



## Vector database

A dataset of three sentences, each has 3 words (or tokens)
- In practice, a dataset may contain millions or billions of sentences. 
- The max number of tokens may be tens of thousands (e.g., 32,768 mistral-7b).

Process "how are you"

**Word Embeddings**
- For each word, look up corresponding word embedding vector from a table of 22 vectors, where 22 is the vocabulary size.
- In practice, the vocabulary size can be tens of thousands. The word embedding dimensions are in the thousands (e.g., 1024, 4096)

**Encoding**
- Feed the sequence of word embeddings to an encoder to obtain a sequence of feature vectors, one per word.
- Here, the encoder is a simple one layer perceptron (linear layer + ReLU)
- In practice, the encoder is a transformer or one of its many variants.

**Mean Pooling**
- Merge the sequence of feature vectors into a single vector using "mean pooling" which is to average across the columns.
- The result is a single vector. We often call it "text embeddings" or "sentence embeddings." 
- Other pooling techniques are possible, such as CLS. But mean pooling is the most common.

**Indexing**
- Reduce the dimensions of the text embedding vector by a projection matrix. The reduction rate is 50% (4->2). 
- In practice, the values in this projection matrix is much more random.  The purpose is similar to that of hashing, which is to obtain a short representation to allow faster comparison and retrieval. 
- The resulting dimension-reduced index vector is saved in the vector storage.

**Process "who are you"**
- Repeat Word Embeddings to indexing

**Query: "am I you"**
- Repeat Word Embeddings to indexing
- The result is a 2-d query vector.

**Dot Products**
- Take dot product between the query vector and database vectors. They are all 2-d. 
- The purpose is to use dot product to estimate similarity. 
- By transposing the query vector, this step becomes a matrix multiplication.

**Nearest Neighbor**
-  Find the largest dot product by linear scan. 
- The sentence with the highest dot product is "who am I" 
- In practice, because scanning billions of vectors is slow, we use an Approximate Nearest Neighbor (ANN) algorithm like the Hierarchical Navigable Small Worlds (HNSW).

![[Screenshot from 2024-05-28 06-45-42.png]]

algorithms commonly used for similarity search indexing
- Product quantization (PQ)
- Locality sensitive hashing
- Hierarchical navigable small world (HNSW


https://aibyhand.substack.com/ -> AI by hand



Neural network 
- https://nnfs.io/ Neural Networks From Scratch


Resources
- **[Pretrained Transformers for Text Ranking: BERT and Beyond](https://arxiv.org/abs/2010.06467)**
- **[Principal Component Analysis](https://en.wikipedia.org/wiki/Principal_component_analysis)**
- **[Deep Learning AI Short Courses](https://www.deeplearning.ai/short-courses/)**
- **[ChromaDB Tutorial on DataCamp](https://www.datacamp.com/tutorial/chromadb-tutorial-step-by-step-guide)**
- **[Visualize Vector Embeddings in a RAG System](https://medium.com/@sarmadafzalj/visualize-vector-embeddings-in-a-rag-system-89d0c44a3be4)**
- **[Natural Language Processing Specialization on Coursera](https://www.coursera.org/specializations/natural-language-processing#courses)**
- **[Distributed Representations of Sentences and Documents](https://arxiv.org/abs/1405.4053)**
- **[A Gentle Introduction to Doc2Vec](https://medium.com/wisio/a-gentle-introduction-to-doc2vec-db3e8c0cce5e)**
- **[Word2Vec Archive](https://code.google.com/archive/p/word2vec/)**
- **[Mastering LLM Techniques: Inference and Optimization](https://developer.nvidia.com/blog/mastering-llm-techniques-inference-optimization/)**
- **[LLAMA3 Documentation](https://docs.likejazz.com/llama3.np/)**
- **[Gensim Documentation and Examples](https://radimrehurek.com/gensim/auto_examples/core/run_core_concepts.html#sphx-glr-auto-examples-core-run-core-concepts-py)**
- **[TensorBoard Documentation](https://www.tensorflow.org/tensorboard)**
- **[Evaluation of RAG Systems](https://weaviate.io/blog/rag-evaluation)**
- **[Sentence Transformers on Hugging Face](https://www.sbert.net/docs/quickstart.html)**
- **[Local RAG with Ollama and Weaviate](https://weaviate.io/blog/local-rag-with-ollama-and-weaviate)**
- **[Video Lectures from ESWC 2016 on Machine Learning](https://videolectures.net/eswc2016_fortuna_machine_learning/)**
- https://huyenchip.com/2023/04/11/llm-engineering.html
- https://github.com/rasbt/LLMs-from-scratch 
- [ reasonable and good explanations of how stuff works. No hype and no vendor content ](https://gist.github.com/veekaybee/be375ab33085102f9027853128dc5f0e)
	


emdeding
- https://arxiv.org/pdf/1411.2738  need ot read
- https://ronxin.github.io/wevi/# word embedding visual inspector


books
- https://shepherd.com/best-books/machine-learning-and-deep-neural-networks







## AutoML 
Frameworks represent a noteworthy leap in the evolution of machine learning. By streamlining the complete model development cycle, including tasks such as data cleaning, feature selection, model training, and hyperparameter tuning, AutoML frameworks significantly economize on the time and effort customarily expended by data scientists.






###  Finetune

**LoRA**:
- Introduce two low-rank matrices, A and B, to work alongside the weight matrix W.
- Adjust these matrices instead of the behemoth W, making updates manageable.

**LoRA-FA (Frozen-A):**
- Takes LoRA a step further by freezing matrix A.
- Only matrix B is tweaked, reducing the activation memory needed.

**VeRA:**
- All about efficiency: matrices A and B are fixed and shared across all layers.
- Focuses on tiny, trainable scaling vectors in each layer, making it super memory-friendly.

**Delta-LoRA:**
- A twist on LoRA: adds the difference (delta) between products of matrices A and B across training steps to the main weight matrix W.
- Offers a dynamic yet controlled approach to parameter updates.

**LoRA+:**
- An optimized variant of LoRA where matrix B gets a higher learning rate.
This tweak leads to faster and more effective learning.

#### Resources
 - [LLORA  for finetuning](https://lightning.ai/lightning-ai/studios/code-lora-from-scratch)
- [Fine tune LLMs 2024](https://www.philschmid.de/fine-tune-llms-in-2024-with-trl)

## Model Merging

Model merging is an efficient alternative to fine-tuning that leverages the work of the open-source community. It involves combining the weights of different fine-tuned models to create a new model with enhanced capabilities. This technique has proven highly effective, as demonstrated by the dominance of merged models in performance benchmarks.

#### Merging Techniques

- **SLURP (Spherical Linear Interpolation):** Interpolates the weights of two models using spherical linear interpolation. Different interpolation factors can be applied to various layers, allowing for fine-grained control.
- **Decomposed Redundancy Addition (DeRA):** Reduces redundancy in model parameters through pruning and rescaling of weights. This technique allows merging multiple models simultaneously.
- **Pass-Through:** Concatenates layers from different LLMs, including the possibility of concatenating layers from the same model (self-merging).
- **Mixture of Experts (MoE):** Combines feed-forward network layers from different fine-tuned models, using a router to select the appropriate layer for each token and layer. This technique can be implemented without fine-tuning by initializing the router using embeddings calculated from positive prompts.

**Advantages of Model Merging:**
- No GPU requirement, making it highly efficient.
- Ability to leverage existing fine-tuned models from the open-source community.
- Proven effectiveness in producing high-quality models.

## 1 Bit LLM 
 BitNet b1.58 where every weight in a Transformer can be represented as a {-1, 0, 1} instead of a floating point number. 




## LAMA Notes

LAMA 2 All are instruction tunned model
 - lama2 7B instruction
 - lama2 13B instruction
 - lama2 70B instruction
 -  lama2 7B Chat
 - lama2 13B Chat
 - lama2 70B Chat
 
 Code lama
 - 7B/13B/34B has both base model and instruct model

Purpel lama
- Generative AI safety model wil take care of check does the genreated code is safe 
- CybersecEval dataset to test
- lama Gaurd -> check input and output 


```
[INST] -> addd by lama to identify the inst tag base model dont undertstand
</s> -> ending tag
```
- Lama we need to manully add INST and end tag with 

- **Llama Stack** was introduced to address the challenges of integrating Llama models into existing workflows.10 It provides a stable API and CLI, simplifying tasks like downloading models, inspecting their properties, and deploying them.11 Llama Stack also incorporates features like memory, RAG, and safety orchestration using multiple models. https://github.com/meta-llama/llama-stack
## GEMINI Notes
- ultra 
- pro -> performance and speed
- flash -> fastest and low cost
- nano ->



Large multimodal model -> can handel both image and other input type



## fine tune

### LLM traning methods

**Causal Language Modeling (CLM)**

CLM is an autoregressive method where the model is trained to predict the next token in a sequence given the previous tokens. CLM is used in models like GPT-2 and GPT-3 and is well-suited for tasks such as text generation and summarization. However, CLM models have unidirectional context, meaning they only consider the past and not the future context when generating predictions.

**Masked Language Modeling (MLM)**

MLM is a training method used in models like BERT, where some tokens in the input sequence are masked, and the model learns to predict the masked tokens based on the surrounding context. MLM has the advantage of bidirectional context, allowing the model to consider both past and future tokens when making predictions. This approach is especially useful for tasks like text classification, sentiment analysis, and named entity recognition.

**Sequence-to-Sequence (Seq2Seq)**

Seq2Seq models consist of an encoder-decoder architecture, where the encoder processes the input sequence and the decoder generates the output sequence. This approach is commonly used in tasks like machine translation, summarization, and question-answering. Seq2Seq models can handle more complex tasks that involve input-output transformations, making them versatile for a wide range of NLP tasks.