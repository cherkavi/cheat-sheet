# [Ollama](https://ollama.com)
## ollama installation
[linux installation](https://github.com/ollama/ollama/blob/main/docs/linux.md)
```sh
curl -fsSL https://ollama.com/install.sh | sh
```
check installation
```sh
# ls -la /usr/share/ollama
# ls -la /usr/local/bin/ollama
# /etc/systemd/system/default.target.wants/ollama.service → /etc/systemd/system/ollama.service.
# sudo find /usr/share/ollama/.ollama
```

## ollama first commands 
```sh
x-www-browser http://127.0.0.1:11434/api/version
journalctl -e -u ollama
ollama --version
```

## [ollama models](https://ollama.com/library) 
### ollama list models 
```sh
# list installed models 
ollama list
# your models in local storage
sudo find /usr/share/ollama/.ollama/models

# browse names 
x-www-browser https://ollama.com/search
```

### ollama pull model by name
```sh
ollama pull deepseek-v4.1-flash:cloud
ollama rm deepseek-v4.1-flash:cloud

ollama pull qwen3.8
ollama pull mistral
ollama pull llama4
# example of the direct url:
# pulling 9d507a36062c # https://dd20bb891979d25aebc8bec07b2b3bbc.r2.cloudflarestorage.com/ollama/docker/registry/v2/blobs/sha256/9d/9d507a36062c2845dd3bb3e93364e9abc1607118acd8650727a700f72fb126e5/data?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=66040c77ac1b787c3af820529859349a%2F20250615%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250615T171841Z&X-Amz-Expires=86400&X-Amz-SignedHeaders=host&X-Amz-Signature=d7ec6ee62468113a74f166dd732fd56751d38e5b9576d73a58237f49460e5ab9
```

### ollama models import/convert from: 
1. [download your gguf file](https://huggingface.co/models?library=gguf)
   > your file, for instance, is placed in: /models/mistral.gguf
2. create Modelfile
   ```sh
   echo 'FROM llama2
   PARAMETER model /models/mistral.gguf' > Modelfile
   ```
   ```sh
   echo 'FROM /models/mistral.gguf' > Modelfile
   ```
3. create model
   ```sh
   MODEL_NEW_NAME=mistral-custom
   ollama create $MODEL_NEW_NAME -f Modelfile
   ollama run $MODEL_NEW_NAME
   ```

## Ollama run/prompt via cli
```sh
MODEL_NAME="sqlcoder"
ollama run $MODEL_NAME
# ask question/prompt via cli
ollama run $MODEL_NAME --input "how to ... "

# check runned models 
ollama ps 
```
```sh
# run model 
MODEL_NAME="sqlcoder"
# run model in cloud
MODEL_NAME="deepseek-v4.1-flash:cloud"

ollama run $MODEL_NAME
```

## [Ollama REST API commands](https://github.com/ollama/ollama/blob/main/docs/api.md)
## Ollama run/prompt via REST API commands
```sh
curl -X GET http://localhost:11434

curl -X POST http://localhost:11434/api/generate -d '{
  "model": "sqlcoder",
  "prompt":"insert user_id=1, name='ehlo' into table users "
 }'
```

## [Ollama run/prompt via python](https://pypi.org/project/ollama/)
```python
# pip3 install --break-system-packages  ollama

from ollama import chat
from ollama import ChatResponse

response: ChatResponse = chat(model='sqlcoder', messages=[
  {
    'role': 'user',
    'content': "insert user_id=1, name='ehlo' into table users ",
  },
])
print(response['message']['content'])    # print(response.message.content)
```

