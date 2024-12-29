---
title : Fine Tunning
date : 2024-12-22T13:40:17.1717+05:30
draft : true
tags : 
---

## Fine tunning

### LORA

when we train a model with more weights it is time and resources consuming because we need to update the weights by loading on the machine which is  hard so to avoid LORA introduce a **low-rank matrix decomposition** 

Instead of updating all the weights, LoRA focuses on tracking the changes induced by fine-tuning and representing these changes in a compact form. It leverages **low-rank matrix decomposition**, which allows a large matrix to be approximated by the product of two smaller matrices.

Example 

Imagine a 5x5 matrix as a storage unit with 25 spaces. LORA breaks it down into two smaller matrices through matrix decomposition with “r” as rank(the dimension): a 5x1 matrix (5 spaces) and a 1x5 matrix (5 spaces). This reduces the total storage requirement from 25 to just 10, making the model more compact.

### QLORA

In this we just quantization matrix value from 32 bit to 8 Bit

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

Resources
- [LLORA  for finetuning](https://lightning.ai/lightning-ai/studios/code-lora-from-scratch)
- [Fine tune LLMs 2024](https://www.philschmid.de/fine-tune-llms-in-2024-with-trl)
- [A collection Fine-Tuning video courses](https://parlance-labs.com/education/fine_tuning/kyle.html)

### Axolotl

Is wrapper above hugging face and provides numerous example configuration files in YAML format, covering a variety of common use cases and model types.

- Supports fullfinetune, lora, qlora, relora, and gptq
- Customize configurations using a simple yaml file or CLI overwrite

```yml
base_model: codellama/CodeLlama-7b-hf
model_type: LlamaForCausalLM
tokenizer_type: CodeLlamaTokenizer

load_in_8bit: true
load_in_4bit: false
strict: false

datasets:
  - path: mhenrichsen/alpaca_2k_test
    type: alpaca
dataset_prepared_path:
val_set_size: 0.05
output_dir: ./outputs/lora-out

sequence_len: 4096
sample_packing: true
pad_to_sequence_len: true

adapter: lora
lora_model_dir:
lora_r: 32
lora_alpha: 16
lora_dropout: 0.05
lora_target_linear: true
lora_fan_in_fan_out:

wandb_project:
wandb_entity:
wandb_watch:
wandb_name:
wandb_log_model:

gradient_accumulation_steps: 4
micro_batch_size: 2
num_epochs: 4
optimizer: adamw_bnb_8bit
lr_scheduler: cosine
learning_rate: 0.0002

train_on_inputs: false
group_by_length: false
bf16: auto
fp16:
tf32: false

gradient_checkpointing: true
early_stopping_patience:
resume_from_checkpoint:
local_rank:
logging_steps: 1
xformers_attention:
flash_attention: true
s2_attention:

warmup_steps: 10
evals_per_epoch: 4
saves_per_epoch: 1
debug:
deepspeed:
weight_decay: 0.0
fsdp:
fsdp_config:
special_tokens:
  bos_token: "<s>"
  eos_token: "</s>"
  unk_token: "<unk>"
```

LLM fine tuning made easy
https://axolotl.ai/   

Methods
- [Mixture of Experts](https://huggingface.co/blog/moe)

### Unsloth
[Unsloth](https://github.com/unslothai/unsloth) is a lightweight library for faster LLM fine-tuning


- https://github.com/mistralai/mistral-finetune

finetune 
https://www.clarifai.com/

## Ray train

https://docs.ray.io/en/latest/train/train.html?_gl=1*fhc3w3*_gcl_au*MTk4OTE5MzY1Ni4xNzM0ODU1MTA0



https://www.lamini.ai/