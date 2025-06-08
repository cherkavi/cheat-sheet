# [Fine tuning](./ai-tools-fine-tuning.md)

## fine tuning Use Case:
* Domain adaptation
* Specialized tasks (e.g., legal, medical, proprietary dialogue styles)
* Performance tuning on consistent tasks (classification, summarization)
* ❗for totally new knowledge - RAG
* ❗small set of training data - find out more, but not too much 
* quality vs quantity - clear, consistence, focused
* consistence in format and style 
* free of errors and contradictions
* each example should correspond to your use case
* choose proper base model
  * model size ( consider to use: Lama 3.2:3b, 3.1.8b )
    > "do not use a truck for pizze delivery"
  * license

## fine tuning parameters:
* LOw Ranked Adaptation
  * Quantized LOw Ranked Adaptation
* Learning Rate
  > size each step during the learing to reach out the goal ( distance )
* Batch Size
  > how many expamples to take at once, before knowledge update ( more examples at once - more memory )
* Number of Epochs
  > number of complete passes through the training dataset.
* Optimizer Selection
  > different teaching methods

## fine tuning Tools:

### 🧠 [MLX](https://github.com/ml-explore/mlx-lm)
> MLX is a machine learning framework built by Apple, optimized for Apple Silicon (M1, M2, M3). `mlx-lm` is the LLM component allowing inference and training with minimal memory.
- **Apple MLX Framework Info**: [https://github.com/ml-explore/mlx](https://github.com/ml-explore/mlx)

#### mlx install 
```sh
# pip install mlx-lm
```

#### mlx train
> https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.3
```sh
mlx_lm.lora --train \
--model mistralai/Mistral-7B-Instruct-v0.2 \
--data path/to/jsonl-file \
--batch-size 2  # how much examples in each iteration 
```

#### mlx train result
```sh
cd adapters
ls *.safetensors
```

#### mlx create Modelfile
```sh
echo 'FROM mistral
ADAPTER ./adapters' > Modelfile
```

#### mlx create model
```sh
ollama create mistral-update -f Modelfile
ollama run mistral-update
```

### 🦥 [Unsloth](https://www.unsloth.ai)
> fast, works in limit resources
> Unsloth is a library that enables fast and memory-efficient fine-tuning of large language models, such as LLaMA 3, Qwen2, Mistral, and more — especially with QLoRA.
- **GitHub**: [https://github.com/unslothai/unsloth](https://github.com/unslothai/unsloth)

### 🦎 [Axolotl](https://axolotl.ai)
> a lot of parameters - you should know what you are doing 
> Axolotl is a flexible and scalable framework for fine-tuning LLMs using YAML config files. Supports multi-GPU setups, LoRA, QLoRA, and various backends.
- [axolotl GitHub](https://github.com/axolotl-ai-cloud/axolotl)
- [axolotl doc](https://docs.axolotl.ai/docs/getting-started.html)
- **Steps:** azolotl ( finetuning )  --> llama-cpp (export) --> ollam ( run model )

## fine tuning Steps:

### 1.create dataset 
> need to provide question:answer in understand format for model 

#### find out template
```sh
ollama list # select one existing model 
MODEL_NAME=$(ollama list | tail -n -1 | awk '{print $1}')

ollama show --modelfile $MODEL_NAME
# or 
ollama show --template $MODEL_NAME
# or 
ollama run $MODEL_NAME # /show template
```

#### let's image, just for example:
```docker
FROM /usr/share/ollama/.ollama/models/blobs/sha256-bd9d5f911cb214dba65cc678b115d022b042da77507429bd24c5082262209a20
TEMPLATE {{ .Prompt }}
PARAMETER stop <|endoftext|>
```

#### Magic words of templates
|        |                                      |  
|--------|--------------------------------------|  
| INST   | Instruction Start                    |  
| /INST  | Instruction End (sometimes implicit) |  


### 2. create JSONLines file
> each line - separate json document 
```json:data.jsonl
echo '
{"prompt": "What is the capital of France?", "response": "Paris."}
{"prompt": "What is 2 + 2?", "response": "4."}
' > data.jsonl
```
**Ideally you should create three files:**  
| train.jsonl | Practice problems | learn from these               |  
| valid.jsonl | Quizzes           | check progress while studying  |  
| test.jsonl  | Final exam        | used to measure final ability  |  

### go to:
* [mlx train](#mlx-train)
