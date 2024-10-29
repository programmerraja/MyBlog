---
title : Natural Language Processing with Transformers  Notes
date : 2024-06-22T19:11:33.3333+05:30
draft : true
tags : ['books']
---

## Hello Transformers

#### The Encoder-Decoder Framework












 Recurrent Neural Networks
 - auto compelete
 - translation
 - sentiment

we can not use neurons because it is constant input and heavy computation


#### From Text to Tokens

Subword Tokenization
 
#### WordPiece
- **Used by**: BERT, DistilBERT, ALBERT
- **Description**: WordPiece tokenization splits words into subwords or characters based on frequency. It starts with a base vocabulary of individual characters and then iteratively merges the most frequent pairs of tokens into new tokens.

#### Byte-Pair Encoding (BPE)
- **Used by**: GPT-2, RoBERTa
- **Description**: BPE is similar to WordPiece but focuses on merging the most frequent pairs of bytes in the text, creating a hierarchical representation of text where frequent subword units are represented as single tokens.

#### Unigram Language Model
- **Used by**: SentencePiece, XLNet
- **Description**: The Unigram model starts with a large vocabulary of potential subwords and iteratively removes the subwords that are least likely to be beneficial, aiming to maximize the likelihood of the given text under the subword distribution.

#### SentencePiece
- **Used by**: T5, MarianMT
- **Description**: SentencePiece is a subword tokenizer that can use either BPE or Unigram Language Model techniques. It treats the input text as a sequence of Unicode characters and constructs the vocabulary from these, without requiring any pre-tokenization.

#### SentencePieceBPETokenizer
- **Used by**: Hugging Face's `tokenizers` library
- **Description**: This is a hybrid approach provided by the `tokenizers` library from Hugging Face. It leverages SentencePiece algorithms in a framework optimized for speed and efficiency.
#### Byte-Level BPE
- **Used by**: GPT-3, CLIP
- **Description**: Byte-Level BPE operates on byte-level rather than character-level, allowing it to handle any kind of text, including emojis and non-standard characters, without any special preprocessing.

```python
from transformers import BertTokenizer, GPT2Tokenizer, T5Tokenizer

# WordPiece (BERT)
bert_tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')
bert_tokens = bert_tokenizer.tokenize("Hello, how are you?")
print("BERT WordPiece Tokens:", bert_tokens)

# Byte-Pair Encoding (GPT-2)
gpt2_tokenizer = GPT2Tokenizer.from_pretrained('gpt2')
gpt2_tokens = gpt2_tokenizer.tokenize("Hello, how are you?")
print("GPT-2 BPE Tokens:", gpt2_tokens)

# SentencePiece (T5)
t5_tokenizer = T5Tokenizer.from_pretrained('t5-small')
t5_tokens = t5_tokenizer.tokenize("Hello, how are you?")
print("T5 SentencePiece Tokens:", t5_tokens)

```