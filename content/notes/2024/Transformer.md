+++
title = 'Transformer'
date = 2024-06-22T10:48:42.4242+05:30
draft = true
tags =[]
+++ 

## RNN

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

## BERT Model

has 768 feature vector for embedding and BERT has a fixed vocabulary of 30,000 words (or tokens).

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








Resource
- https://machinelearningmastery.com/a-gentle-introduction-to-positional-encoding-in-transformer-models-part-1/ [need to cehck]
-  https://jalammar.github.io/illustrated-transformer/
- https://daleonai.com/transformers-explained
-  https://www.philschmid.de/getting-started-pytorch-2-0-transformers
- https://www.borealisai.com/research-blogs/tutorial-14-transformers-i-introduction/
