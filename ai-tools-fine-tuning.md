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
    > "do not use a truck for a pizza delivery"
  * license

## fine tuning types/approaches:
* Full funetuning
  > rewrite model everything
* LORA - LOw Ranked Adaptation
  > add additional information granurarly 
  * QLORA - Quantized LOw Ranked Adaptation

## fine tuning parameters:
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
##### select one model, for instance:
   > https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.3
##### run command like
[mlx_lm.lora doc](https://github.com/ml-explore/mlx-lm/blob/main/mlx_lm/LORA.md#Data)  

check ability to train your model
```sh
# python3
import mlx
mlx.core.metal.is_available()
```

```sh
# huggingface-cli whoami  

### model 
## remote
# --model mistralai/Mistral-7B-Instruct-v0.3 \
## local folder
# --model $HOME/.cache/huggingface/hub/models--mistralai--Mistral-7B-Instruct-v0.3/snapshots/e0bc86c23ce5aae1db576c8cca6f06f1f73af2db \

mlx_lm.lora --train \
--model mistralai/Mistral-7B-Instruct-v0.2 \
--data path/to/folder-with-files \
--batch-size 2  # how much examples in each iteration 
```
where `path/to/folder-with-files` contains:
* train.jsonl
  > The model sees these examples and adjusts its internal parameters to minimize error on this data.
* valid.jsonl 
  > optional
  > The model does not learn from this data, but you check its performance here during training to decide when to stop, which settings are best, etc.
* test.jsonl 
  > optional
  > The model never sees this data during training or validation. It is only used once, after all training and tuning are done, to measure true, unbiased accuracy.

##### jsonl data checker
```py
# export train_file=train_file.json
# python3
import json
import os
path = os.environ.get("train_file")

with open(path, "r") as fid:
    data=[]
    for l in fid:
        print(l)
        data.append(json.loads(l))
    # print(data)
```

##### **Issues with running:**
* if you see: `You must have access to it and be authenticated to access it. Please log in.`  
  > [install cli and login to huggingface](https://github.com/cherkavi/cheat-sheet/blob/master/ai-tools.md#huggingface)
* if you see: `Access to model mistralai/Mistral-7B-Instruct-v0.2 is restricted and you are not in the authorized list. Visit https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.2 to ask for access.`
  > https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.3, login and click button "agree"
* in case of: `Connection broken: IncompleteRead`
  > execute command one more time
* in case of: `ValueError: Training set not found or empty. Must provide training set for fine-tuning.`
  > check path to folder! and file train.jsonl inside

* resolve via huggingface
```sh
huggingface-cli download mistralai/Mistral-7B-Instruct-v0.3
huggingface-cli scan-cache -vvv
# huggingface-cli delete-cache 
```

* manual run of the dataset loading
  * `None of PyTorch, TensorFlow >= 2.0, or Flax have been found.`
  * `Dataset 'mistralai/Mistral-7B-Instruct-v0.3' doesn't exist on the Hub or cannot be accessed.`
```sh
pip3 install -U --break-system-packages "datasets"
pip3 install -U --break-system-packages "tensorflow" 
# pip3 install -U --break-system-packages "tensorflow-gpu"
```
```py
# python3
from transformers import Trainer, TrainingArguments, AutoModelForSequenceClassification, AutoTokenizer
from datasets import load_dataset
import os
home_directory = os.path.expanduser("~")
dataset = load_dataset(home_directory+"/.cache/huggingface/hub/models--mistralai--Mistral-7B-Instruct-v0.3")
print(dataset)
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
#### jsonl "mistral" model example:
template
```jsonl
{"messages":[{"role": "user", "content": "question1"},{"role": "assistant", "content": "answer1"}]}
{"messages":[{"role": "user", "content": "question2"},{"role": "assistant", "content": "answer2"}]}
{"messages":[{"role": "user", "content": "question3"},{"role": "assistant", "content": "answer3"}]}
```

#### jsonl possible extension
your "question" can be more complex like (with description, steps, question parts):
```json
{"messages": [
  {"role": "user", "content": "{\"description\": \"This function ....\", \"steps\": [\"Initialize ...\", \"Call ...\", \"Return ...\"], \"question\": \"How can I ... ?\"}"},
  {"role": "assistant", "content": "You can ..."}
]}
```
or even simpler
```json
{"messages": [
  {"role": "user", "content": "description: This function ....\n\n steps:\n Initialize ...\n Call ...\n Return ...\n\n question: How can I ... ?}"},
  {"role": "assistant", "content": "You can ..."}
]}
```

#### Ideally you should create three files:
| train.jsonl | Practice problems | learn from these               |  
| valid.jsonl | Quizzes           | check progress while studying  |  
| test.jsonl  | Final exam        | used to measure final ability  |  

### 3. train
#### [train with mlx](#mlx-train)
