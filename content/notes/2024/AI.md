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

LLM evalution metrics
- Recall-Oriented Understudy for Gisting Evaluation(ROUGE)
- Bilingual Evaluation Understudy (BLEU)
- Perplexity





Natural language model

LoRA (Low-Rank Adaptation) and PEFT (Parameter-Efficient Fine-Tuning) are techniques used in the field of natural language processing and machine learning to fine-tune large pre-trained models in a more computationally and memory-efficient manner.



LLM 

Model architecture

Transformers -> Attention mechnaism

Training at scale
- Mixed precision traning uses both 32bit and 16bit float point 
- 3d parallelism 
- Zero redundancy optimizer



## Langchain













ollama

uses docker to run all model
Model stored in  /user/share/ollama/.ollama/models

which has blobs and manifests/registry.ollama.ai

blobs contain actula model code 

https://github.com/vercel/ai








Rank GPT
- instead of just querying in vector and sending to LLM after querying ask LLM can you rank the doc that fetched from vecotr db based relvant to the query and again send to LLM with re ranked doc


Multi query retrieval 
- Send the user query to LLM and ask can you suggest revelant query to this query get that and use that query to get from db

Contextual compression
- Ask the LLM can you give relavant part that required for the doc by asking this we reducing the context then again send to LLM

Hypothetical document embedding
- ask LLM to suggest Hypothetical document for query and use that to fetch from DB

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











## Resources
- LLORA  for finetuning ->https://lightning.ai/lightning-ai/studios/code-lora-from-scratch
- https://www.philschmid.de/fine-tune-llms-in-2024-with-trl






Neural network 
- https://nnfs.io/ Neural Networks From Scratch





Resources
- https://arxiv.org/abs/2010.06467 # Pretrained Transformers for Text Ranking: BERT and Beyond
- principal component analysis 
- https://www.deeplearning.ai/short-courses/
- https://www.datacamp.com/tutorial/chromadb-tutorial-step-by-step-guide
- https://medium.com/@sarmadafzalj/visualize-vector-embeddings-in-a-rag-system-89d0c44a3be4
- https://www.coursera.org/specializations/natural-language-processing#courses [# Natural Language Processing Specialization]
- https://arxiv.org/abs/1405.4053  Distributed Representations of Sentences and Documents
- https://medium.com/wisio/a-gentle-introduction-to-doc2vec-db3e8c0cce5e 
- https://code.google.com/archive/p/word2vec/
- https://developer.nvidia.com/blog/mastering-llm-techniques-inference-optimization/
- https://docs.likejazz.com/llama3.np/
- https://radimrehurek.com/gensim/auto_examples/core/run_core_concepts.html#sphx-glr-auto-examples-core-run-core-concepts-py
- https://www.tensorflow.org/tensorboard
- https://weaviate.io/blog/rag-evaluation
- https://www.sbert.net/docs/quickstart.html ## Sentence Transformer hugging face
- https://weaviate.io/blog/local-rag-with-ollama-and-weaviate
- https://videolectures.net/eswc2016_fortuna_machine_learning/

To avoid halucination
- https://arxiv.org/pdf/2303.08896 SelfCheckGPT: Zero-Resource Black-Box Hallucination Detection for Generative Large Language Models

emdeding
- https://arxiv.org/pdf/1411.2738  need ot read
- https://ronxin.github.io/wevi/# word embedding visual inspector




books
- https://shepherd.com/best-books/machine-learning-and-deep-neural-networks


Langchain alternative
- https://haystack.deepset.ai/tutorials




## AutoML 
Frameworks represent a noteworthy leap in the evolution of machine learning. By streamlining the complete model development cycle, including tasks such as data cleaning, feature selection, model training, and hyperparameter tuning, AutoML frameworks significantly economize on the time and effort customarily expended by data scientists.






### LLM Evaluation Metrics

#### Types

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
- using GPT-4 and chain-of-thoughts (CoT) approach to generate detailed evaluation steps for NLG outputs. 

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


https://applied-llms.org/


**DeepEval** is an open-source evaluation framework for LLMs. DeepEval makes it extremely easy to build and iterate on LLM (applications) and was built with the following principles in mind:

## 1 Bit LLM 
 BitNet b1.58 where every weight in a Transformer can be represented as a {-1, 0, 1} instead of a floating point number. 