+++
title = 'Transformer'
date = 2024-06-22T10:48:42.4242+05:30
draft = true
tags =[]
+++ 

## RNN

```plaintext

    xt: Input at time step t
    ht: Hidden state at time step t
    yt: Output at time step t
    Wh: Weights from input to hidden layer
    Uh: Weights from hidden to hidden layer
    Wy: Weights from hidden to output layer
    bh: Bias for the hidden layer
    by: Bias for the output layer

                   Forward Pass
    ------------------------------------------------
    
    Input Sequence           Hidden States              Outputs
    
    xt       xt+1      xt+2      ...       xt+n
     |         |         |                   |
     v         v         v                   v
   +---+     +---+     +---+     ...       +---+
   |   |     |   |     |   |               |   |
   |Wh |     |Wh |     |Wh |               |Wh |
   |   |     |   |     |   |               |   |
   +---+     +---+     +---+     ...       +---+
     |         |         |                   |
     v         v         v                   v
   +---+     +---+     +---+     ...       +---+
   | h0| --> | h1| --> | h2| --> ... -->   | ht| --> ... 
   +---+     +---+     +---+     ...       +---+
     |         |         |                   |
     v         v         v                   v
   +---+     +---+     +---+     ...       +---+
   |Uh |     |Uh |     |Uh |               |Uh |
   +---+     +---+     +---+     ...       +---+
     |         |         |                   |
     v         v         v                   v
   +---+     +---+     +---+     ...       +---+
   |bh |     |bh |     |bh |               |bh |
   +---+     +---+     +---+     ...       +---+
     |         |         |                   |
     v         v         v                   v
   +---+     +---+     +---+     ...       +---+
   |   |     |   |     |   |               |   |
   |Wy |     |Wy |     |Wy |               |Wy |
   |   |     |   |     |   |               |   |
   +---+     +---+     +---+     ...       +---+
     |         |         |                   |
     v         v         v                   v
   +---+     +---+     +---+     ...       +---+
   | y0|     | y1|     | y2|     ...       | yt| --> ...
   +---+     +---+     +---+     ...       +---+
     |         |         |                   |
     v         v         v                   v
   +---+     +---+     +---+     ...       +---+
   |by |     |by |     |by |               |by |
   +---+     +---+     +---+     ...       +---+

```

- https://www.kaggle.com/code/fareselmenshawii/rnn-from-scratch

####  LSTM

LSTMs have been the most effective architecture to process long sequences of data, until our world was taken over by the Transformers.

LSTMs belong to the broader family of recurrent neural network (RNNs) that process data sequentially in a recurrent manner.

Transformers, on the other hand, abandon recurrence and use self-attention instead to process data concurrently in parallel.

Recently, there is renewed interest in recurrence as people realized self-attention doesn’t scale to extremely long sequences, like hundreds of thousands of tokens. Mamba is a good example to bring back recurrence.

How do LSTMs work?

1.Given
↳ 🟨 Input sequence X1, X2, X3 (d = 3)
↳ 🟩 Hidden state h (d = 2)
↳ 🟦 Memory C (d = 2)
↳ Weight matrices Wf, Wc, Wi, Wo

Process t = 1

2.Initialize
↳ Randomly set the previous hidden state h0 to [1, 1] and memory cells C0 to [0.3, -0.5]

3.Linear Transform
↳ Multiply the four weight matrices with the concatenation of current input (X1) and the previous hidden state (h0).
↳ The results are feature values, each is a linear combination of the current input and hidden state.

4.Non-linear Transform
↳ Apply sigmoid σ to obtain gate values (between 0 and 1).
• Forget gate (f1): [-4, -6] → [0, 0]
• Input gate (i1): [6, 4] → [1, 1]
• Output gate (o1): [4, -5] → [1, 0]
↳ Apply tanh to obtain candidate memory values (between -1 and 1)
• Candidate memory (C’1): [1, -6] → [0.8, -1]

5.Update Memory
↳ Forget (C0 .* f1): Element-wise multiply the current memory with forget gate values.
↳ Input (C’1 .* o1): Element-wise multiply the “candidate” memory with input gate values.
↳ Update the memory to C1 by adding the two terms above: C0 .* f1 + C’1 .* o1 = C1

6.Candiate Output
↳ Apply tanh to the new memory C1 to obtain candidate output o’1.
[0.8, -1] → [0.7, -0.8]

7.Update Hidden State
↳ Output (o’1 .* o1 → h1): Element-wise multiply the candidate output with the output gate.
↳ The result is updated hidden state h1
↳ Also, it is the first output.

Process t = 2

8.Initialize
↳ Copy previous hidden state h1 and memory C1

9.Linear Transform
↳ Repeat [3]

10.Update Memory (C2)
↳ Repeat [4] and [5]

11.Update Hidden State (h2)
↳ Repeat [6] and [7]

Process t = 3

12.Initialize
↳ Copy previous hidden state h2 and memory C2

13.Linear Transform
↳ Repeat [3]

14.Update Memory (C3)
↳ Repeat [4] and [5]

15.Update Hidden State (h3)
↳ Repeat [6] and [7]

- http://colah.github.io/posts/2015-08-Understanding-LSTMs/
- 

#### BiLSTM

A Bidirectional Long Short-Term Memory (BiLSTM) is a type of recurrent neural network (RNN) architecture that is used to process sequential data. It extends the standard Long Short-Term Memory (LSTM) model by introducing the concept of bidirectionality, allowing the model to have both forward and backward information about the sequence.

The architecture of a BiLSTM is as follows:

1. **Input Layer**: Takes the input sequence.
2. **Embedding Layer**: Converts the input tokens to dense vectors (embeddings).
3. **Forward LSTM Layer**: Processes the input sequence from start to end.
4. **Backward LSTM Layer**: Processes the input sequence from end to start.
5. **Concatenation Layer**: Combines the outputs from both the forward and backward LSTM layers.
6. **Dense Layer**: Optional layer(s) for further processing.
7. **Output Layer**: Produces the final predictions.

## Types

#### Encoder-only
These models convert an input sequence of text into a rich numerical representa‐
tion that is well suited for tasks like text classification or named entity recogni‐
tion. BERT and its variants, like RoBERTa and DistilBERT, belong to this class of
architectures.

#### Decoder-only
Given a prompt of text like “Thanks for lunch, I had a…” these models will auto-
complete the sequence by iteratively predicting the most probable next word.
The family of GPT models belong to this class. 

#### Encoder-decoder
These are used for modeling complex mappings from one sequence of text to
another; they’re suitable for machine translation and summarization tasks. In
addition to the Transformer architecture, which as we’ve seen combines an
encoder and a decoder, the BART and T5 models belong to this class.


- **query** — asking for information
- **key** — saying that it has some information
- **value** — giving the information


## Attention

The self-attention mechanism involves three main components:

1. **Query (Q)**: The query is the token that is being processed.
2. **Key (K)**: The key is the token to which we are checking compatibility with the query.
3. **Value (V)**: The value is the actual representation vector of the token.

 For each word embedding we create a Query vector, a Key vector, and a Value vector. These new vectors are smaller in dimension than the embedding vector. Their dimensionality is 64, while the embedding and encoder input/output vectors have dimensionality of 512.

**Step 1: Calculate Query, Key, and Value Vectors**

The first step is to multiply each of the input vectors with three weights matrices (W(Q), W(K), W(V)) that are trained during the training process. This matrix multiplication will give us three vectors for each of the input vectors: the query vector, the key vector, and the value vector.

```python
import numpy as np

# Input vectors
input_vectors = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])

# Weights matrices
W_Q = np.array([[0.1, 0.2, 0.3], [0.4, 0.5, 0.6], [0.7, 0.8, 0.9]])
W_K = np.array([[0.9, 0.8, 0.7], [0.6, 0.5, 0.4], [0.3, 0.2, 0.1]])
W_V = np.array([[0.5, 0.6, 0.7], [0.8, 0.9, 0.1], [0.2, 0.3, 0.4]])

# Calculate query, key, and value vectors
query_vectors = np.dot(input_vectors, W_Q)
key_vectors = np.dot(input_vectors, W_K)
value_vectors = np.dot(input_vectors, W_V)

# Calculate attention scores
attention_scores = np.dot(query_vectors, key_vectors.T)

#The third step is to divide the attention scores by the square root of the dimensions of the key vector

scaled_attention_scores = attention_scores / np.sqrt(key_vectors.shape[1])
```

Example
```python
# Input vectors for the sentence "I am going to play"
input_vectors = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9], [10, 11, 12], [13, 14, 15]])

# Calculate query, key, and value vectors
query_vectors = np.dot(input_vectors, W_Q)
key_vectors = np.dot(input_vectors, W_K)
value_vectors = np.dot(input_vectors, W_V)

# Calculate attention scores for the word "going" 
#key_vectors.T -> transpose matrics
attention_scores = np.dot(query_vectors[2], key_vectors.T)

# Scale attention scores
scaled_attention_scores = attention_scores / np.sqrt(key_vectors.shape[1])

# Apply softmax
attention_weights = np.exp(scaled_attention_scores) / np.sum(np.exp(scaled_attention_scores), axis=-1, keepdims=True) 17

# Calculate final representation of the word "going"
final_representation = np.dot(attention_weights, value_vectors)
```


Single formula for repersentating above

`Attention(Q, K, V) = softmax(Q * K^T / sqrt(d)) * V`

where:

- `Q` is the query vector
- `K` is the key vector
- `V` is the value vector
- `d` is the dimensionality of the key vector
- `^T` denotes the transpose operation
- `softmax` is the softmax function
- `*` denotes the matrix multiplication operation

1. Each word in the input sequence is embedded into a vector space using a learned embedding matrix.
2. For each word, three vectors are computed: `Q`, `K`, and `V`. These vectors are computed by applying three separate linear transformations to the word's embedding vector.
3. The `Q` vector is used as a query to compute attention weights with respect to all other words in the sequence. This is done by taking the dot product of the `Q` vector with the `K` vectors of all other words, and applying a softmax function to obtain a set of attention weights.
4. The attention weights are then used to compute a weighted sum of the `V` vectors of all other words, which produces the output of the self-attention mechanism.
## Optimizer and loss function

**Optimizer:** The optimizer in a Transformer model refers to the algorithm used to update the model parameters during training in order to minimize the loss function. Some common optimizers used in Transformer models include:

1. **Adam (Adaptive Moment Estimation)**: This is a popular optimizer that computes adaptive learning rates for each parameter. It combines the advantages of two other extensions of stochastic gradient descent, namely AdaGrad and RMSProp.
    
2. **AdamW**: This is a variant of Adam that incorporates weight decay regularization directly into the optimizer, which can stabilize training and improve generalization.
    
3. **SGD (Stochastic Gradient Descent)**: Though less commonly used in Transformers compared to Adam variants, SGD updates model parameters based on the gradient of the loss function with respect to the parameters.
    
4. **AdaGrad and RMSProp**: These are older optimizers that adjust the learning rates of model parameters based on the frequency of their updates during training.

### Casual Attention mechanism

Causal attention is a variant of attention mechanism, commonly used in natural language processing, especially in autoregressive models like GPT and Transformer-based models for tasks like language generation. The term "causal" refers to the property that the attention mechanism ensures that the model only attends to past tokens when predicting the next token. This is essential for autoregressive models, which generate tokens sequentially, one at a time, and should not "look ahead" into future tokens.

While causal attention has benefits in sequence generation, it comes at the cost of reduced parallelism during training, especially when compared to bidirectional attention models like BERT, where all tokens attend to each other.

#### SGD

**Batch Gradient Descent**: Computes the gradient of the loss function with respect to the parameters for the entire training dataset and updates the parameters once per iteration. This can be computationally expensive for large datasets.

**Stochastic Gradient Descent**: Computes the gradient of the loss function for each training example individually and updates the parameters for each training example. This makes SGD much faster and more efficient for large datasets.

**Loss Function:** The loss function in a Transformer model defines the objective that the optimizer seeks to minimize during training. For tasks like machine translation or language modeling, common loss functions include:

1. **Cross-Entropy Loss**: This is widely used in classification tasks and measures the performance of a classification model whose output is a probability value between 0 and 1.
    
2. **Sequence-to-Sequence Loss**: Specifically designed for tasks where the model outputs a sequence (such as machine translation), this loss function considers the entire predicted sequence and compares it with the target sequence.
    
3. **Masked Language Modeling Loss**: Used in models like BERT (which uses Transformer architecture), this loss function helps the model learn to predict masked words within a sentence.
    
4. **Perplexity**: In language modeling tasks, perplexity is often used as an evaluation metric that correlates with the cross-entropy loss. Lower perplexity indicates a better-performing model.

## Logits

**Logits** are the raw, unnormalized scores or outputs from a machine learning model, often used in classification tasks. In simpler terms, logits represent the values produced by a neural network before applying an activation function like softmax or sigmoid, which converts these raw values into probabilities.

How it used in GPT

- After tokenization, the tokens are passed through the model (like GPT). The model is a neural network that predicts the next token in the sequence based on the previous ones.
- The model outputs **logits** for each possible token in its vocabulary. If the model has a vocabulary of 50,000 tokens, it will produce 50,000 logits as raw scores for each token after processing the current input sequence.
-  The logits are then passed through a **softmax function** to convert them into a probability distribution over the vocabulary. The softmax function transforms the logits into probabilities that sum to 1, which indicates how likely each token is to follow the current sequence.


## BERT Model

has 768 feature vector for embedding and BERT has a fixed vocabulary of 30,000 words (or tokens).

GPT-3 has 50,257 words of voc and 12288 dimension 
query,key  have  `128 * 12,288` dimension
vaule  = `12288 * 12288`
96 head attention

Has 12 layer of encoder only 

**Handling Unknown Words**
- **Subword Tokens**: When BERT encounters a word that is not in its vocabulary, it uses the WordPiece tokenization to break it into subword units that are in the vocabulary. This means that even if a word is not directly known, its subcomponents likely are, allowing the model to still process and understand it.
- **[UNK] Token**: In cases where a word or subword cannot be tokenized into known vocabulary units, it is represented by a special token [UNK] (unknown). However, this is rare due to the effectiveness of the WordPiece tokenization

**Embedding Representation**

The maximum sentence length is 512 tokens.

**Word Embeddings**: Each token in the vocabulary is represented by a 768-dimensional vector (in the case of BERT-base). This means that after tokenization, each subword token is converted into its corresponding embedding.

 **Positional Embeddings**: BERT also uses positional embeddings to encode the position of each token in the sequence, ensuring that the model can understand the order of words.

**Segment Embeddings**: If the input consists of multiple segments (e.g., a pair of sentences), segment embeddings are used to differentiate between them.

Transformer models, including BERT, require input sequences to be of the same length. Since text sequences can have varying lengths, shorter sequences are padded with a special token (usually `[PAD]`) to match the length of the longest sequence in the batch.

**Vector masks** help distinguish between actual data and padding. This ensures that the model does not treat padding tokens as meaningful input.

```python
# Input: [101, 2054, 2003, 1996, 512, 0, 0, 0]
# Attention Mask: [1, 1, 1, 1, 1, 0, 0, 0]
```
Here, `101, 2054, 2003, 1996, 512` are real tokens, and the remaining are padding tokens.

BERT requires special tokens `[CLS]` at the beginning and `[SEP]` at the end of each input sequence. For sentence pairs, `[SEP]` is also used to separate the two sentences.

```python
from transformers import BertTokenizer

# Initialize the tokenizer
tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')

# Example sentences
sentence1 = "What is the capital of France?"
sentence2 = "Paris is the capital of France."

# Tokenize with padding and truncation
encoding = tokenizer(
    sentence1, sentence2,
    add_special_tokens=True,  # Add [CLS] and [SEP]
    max_length=20,  # Pad & truncate all sentences to 20 tokens
    padding='max_length',  # Pad to max_length
    truncation=True,  # Truncate to max_length
    return_tensors='pt'  # Return PyTorch tensors
)

# Print the encoded inputs
print("Input IDs:")
print(encoding['input_ids'])
print("\nAttention Mask:")
print(encoding['attention_mask'])
print("\nToken Type IDs:")
print(encoding['token_type_ids'])

for sent in sentences:
    # `encode` will:
    #   (1) Tokenize the sentence.
    #   (2) Prepend the `[CLS]` token to the start.
    #   (3) Append the `[SEP]` token to the end.
    #   (4) Map tokens to their IDs.
    encoded_sent = tokenizer.encode(
        sent,  # Sentence to encode.
        add_special_tokens=True,  # Add '[CLS]' and '[SEP]'
        # This function also supports truncation and conversion
        # to pytorch tensors, but we need to do padding, so we
        # can't use these features :( .
        # max_length = 128,          # Truncate all sentences.
        # return_tensors = 'pt',     # Return pytorch tensors.
    )
    print(encoded_sent)
```




## Resources

- [Deconstructing BERT Part 2: Visualizing the Inner Workings of Attention](https://towardsdatascience.com/deconstructing-bert-part-2-visualizing-the-inner-workings-of-attention-60a16d86b5c1)
- [Transformers: The Architecture](https://peterbloem.nl/blog/transformers)
- [BERTViz GitHub Repository](https://github.com/jessevig/bertviz)
- [A Gentle Introduction to Positional Encoding in Transformer Models](https://machinelearningmastery.com/a-gentle-introduction-to-positional-encoding-in-transformer-models-part-1/) _(Check)_
- [Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/)
- [Transformers Explained](https://daleonai.com/transformers-explained)
- [Getting Started with PyTorch 2.0 Transformers](https://www.philschmid.de/getting-started-pytorch-2-0-transformers)
- [Tutorial 14: Transformers I - Introduction](https://www.borealisai.com/research-blogs/tutorial-14-transformers-i-introduction/)
- [BERT Notes](https://notesonai.com/bert)
- [The Effectiveness of Recurrent Neural Networks](https://karpathy.github.io/2015/05/21/rnn-effectiveness/)
- [Llama3 from Scratch](https://github.com/naklecha/llama3-from-scratch)
- [Finetuned Spam Classifier](https://colab.research.google.com/gist/virattt/ddb43fc3d6c0c66abe29a158fe79aa85/finetuned-spam-classifier.ipynb)
- [Ecco GitHub Repository](https://github.com/jalammar/ecco?tab=readme-ov-file)
- [The Transformers Architecture in Detail: What's the Magic Behind LLMs](https://aigents.co/data-science-blog/publication/the-transformers-architecture-in-detail-whats-the-magic-behind-llms)

#### Visualizer
- [Inspectus GitHub Repository](https://github.com/labmlai/inspectus)

