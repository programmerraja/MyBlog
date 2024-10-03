+++
title = 'Hugging face'
date = 2024-06-23T04:57:31.3131+05:30
draft = true
tags =[]
+++ 

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


## Transformer

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



## Resources
- [Huggingface 🤗 is all you need for NLP and beyond ](https://jarvislabs.ai/blogs/hf-getting-started )
- 