# [Fine tuning](./ai-tools-fine-tuning.md)

## fine tuning Use Case:
* Specialized tasks (e.g., legal, medical, proprietary dialogue styles)
* Performance tuning on consistent tasks (classification, summarization)

## fine tuning Tools:
* MLX
* unsloth
* axolotl

## find tuning Steps:
### 1.create dataset 
> need to provide question:answer in understand format for model 
#### find out template
```sh
ollama list # select one existing model 
MODEL_NAME=$(ollama list | tail -n -1 | awk '{print $1}')
ollama show --modelfile $MODEL_NAME
ollama show --template $MODEL_NAME
```
let's image, just for example:
```docker
FROM /usr/share/ollama/.ollama/models/blobs/sha256-bd9d5f911cb214dba65cc678b115d022b042da77507429bd24c5082262209a20
TEMPLATE {{ .Prompt }}
PARAMETER stop <|endoftext|>
```

#### create JSONLines file
> each line - separate json document 
```json:data.jsonl
echo '
{"prompt": "What is the capital of France?", "response": "Paris."}
{"prompt": "What is 2 + 2?", "response": "4."}
' > data.jsonl
```

#### create Modelfile
```sh
echo '
FROM llama3
TEMPLATE {{ .Prompt }}
PARAMETER stop <|endoftext|>
' > Modelfile
```

#### extend model with new data
```sh
MODEL_UPDATED_NAME=ollama.updated
ollama create ${MODEL_UPDATED_NAME} -f Modelfile --finetune data.jsonl
```

#### run model 
```sh
ollama run ${MODEL_UPDATED_NAME}
```

### 2. run fine tuning process
### 3. use the new adapter with a model
