---
title : Running Model locally
date : 2024-11-10T14:35:27.2727+05:30
draft : true
tags : 
---
## Ollama

- uses docker to run all model
- Model stored in  /user/share/ollama/.ollama/models
- which has blobs and manifests/registry.ollama.ai
- blobs contain actula model code 

## Llamafile

An [open-source](https://github.com/Mozilla-Ocho/llamafile) project by Mozilla aiming to democratize access to AI by enabling users to run large language models locally on their machines, including CPUs

- **Single File Executable**: Llamafile distributes large language models as single file executables, simplifying the process of running these models across various operating systems, CPU architectures, and GPU architectures. This portability eliminates the need for complex installations and ensures accessibility across a wide range of devices.

- **Focus on CPU Inference**: Recognizing the limitations of relying solely on expensive and power-hungry GPUs, Llamafile emphasizes the potential of CPUs for running large language models. This focus democratizes access to AI by leveraging readily available and affordable hardware found in computers worldwide

-  Llamafile utilizes Cosmopolitan, a tool that enables the creation of single file executables that can run on multiple operating systems, achieving this through a clever hack involving a Unix shell script embedded in the MS-DOS stub of a portable executable

-  Outer Loop Unrolling for Prompt Processing:A key optimization technique involves unrolling the outer loop in matrix multiplication operations, which constitute a significant portion of LLM computations


**what is loop unrolling?**

```python

for i in range(5): 
	print(i)

#will be change in to 

print(0)
print(1)
print(2)
print(3)
print(4)
```


How to run 
- Download the lamafile of the model and make it executable and run the file as `./model-name` 

```sh

wget https://huggingface.co/Mozilla/Meta-Llama-3.1-8B-Instruct-llamafile/resolve/main/Meta-Llama-3.1-8B-Instruct.Q6_K.llamafile

chmod +x Meta-Llama-3.1-8B-Instruct.Q6_K.llamafile

./Meta-Llama-3.1-8B-Instruct.Q6_K.llamafile

```

### Using llamafile with external weights

```sh

curl -L -o llamafile.exe https://github.com/Mozilla-Ocho/llamafile/releases/download/0.8.11/llamafile-0.8.11

curl -L -o mistral.gguf https://huggingface.co/TheBloke/Mistral-7B-Instruct-v0.1-GGUF/resolve/main/mistral-7b-instruct-v0.1.Q4_K_M.gguf

./llamafile.exe -m mistral.gguf
```

### Running ollama models 

When we download a new model with [ollama](https://ollama.com/), all its metadata will be stored in a manifest file under `~/.ollama/models/manifests/registry.ollama.ai/library/`. The directory and manifest file name are the model name as returned by `ollama list`. For instance, for `llama3:latest` the manifest file will be named `.ollama/models/manifests/registry.ollama.ai/library/llama3/latest`.

The manifest maps each file related to the model (e.g. GGUF weights, license, prompt template, etc) to a sha256 digest. The digest corresponding to the element whose `mediaType` is `application/vnd.ollama.image.model` is the one referring to the model's GGUF file.

Each sha256 digest is also used as a filename in the `~/.ollama/models/blobs` directory (if you look into that directory you'll see _only_ those sha256-* filenames). This means you can directly run llamafile by passing the sha256 digest as the model filename. So if e.g. the `llama3:latest` GGUF file digest is `sha256-00e1317cbf74d901080d7100f57580ba8dd8de57203072dc6f668324ba545f29`, you can run llamafile as follows:

```
cd /usr/share/ollama/.ollama/models/blobs
llamafile -m sha256-00e1317cbf74d901080d7100f57580ba8dd8de57203072dc6f668324ba545f29

```

TO get model sha value it will under `/usr/share/ollama/.ollama/models/manifests/registry.ollama.ai/library/modelname/latest` 

or check logs of ollama

Note: If we run a ollama as service only it will store the model in above both if we running ollama as `ollama server` and pulling the model store the model in home dir

By default when we pulling model Ollama may default to using a highly compressed model variant (e.g. Q4). (4 bit  [Quantization](Transformer.md#Quantization) ) which may have less accuracy so if we want more accurate model pull with `dolphin2.2-mistral:7b-q6_K.` (Q6)




## PowerInfer

[High-speed ](https://github.com/SJTU-IPADS/PowerInfer)Large Language Model Serving on PCs with Consumer-grade GPUs
