# Collection of AI tools
* [my own AI cheat sheet](https://github.com/cherkavi/ai)
* [vector DB comparison](https://superlinked.com/vector-db-comparison)
  * [cosine simularity](https://en.wikipedia.org/wiki/Cosine_similarity)
* :TODO: https://github.com/explodinggradients/ragas  

## AI platforms
* [union](https://www.union.ai/)

## chat bots
* https://www.perplexity.ai/
* https://bard.google.com/chat
* https://openai.com/blog/chatgpt
* [generator of flash cards for learning](https://notegpt.io/ai-flashcard-maker)

### photo/image generators
* https://firefly.adobe.com/generate/image
* https://artlist.io/
* https://pixpal.chat/
* https://chat-photo.com/
* https://www.meta.ai/ai-image-generator/
* https://openart.ai/image/chat
* https://www.fotor.com/ai-assistant/
* https://chatgpt.com/features/images/
* https://miniapps.ai/photo-maker
* https://www.chatphoto.org/en-US
* https://www.usebrushy.com/
* https://deevid.ai/de/ai-image-generator
* https://www.krea.ai 
* https://promptbase.com/
* https://huggingface.co/spaces?category=image-generation
* https://docs.midjourney.com/hc/en-us/articles/33329261836941-Getting-Started-Guide
* https://civitai.com

## customization/flexible prompt
* [deploy custom models and run them ](https://replicate.com/)


## general purposes tools
* [cli tool for interacting with LLM](https://github.com/simonw/llm)
  * llm-youtube
    ```sh
    llm install llm-youtube
    llm -f yt:zv72WMmVkPw 'Please summarize this video'
    ```
* [whisper - translate mp3 to text](https://github.com/openai/whisper)
  > `pip install -U openai-whisper` and then `whisper japanese.wav --language Japanese --task translate`

## Custom Agentic Solutions
* https://github.com/langgenius/dify

## Solutions for developer
- [Kestra – Create and manage event-driven workflows](https://github.com/kestra-io/kestra)
- [Microlink - Extract data from websites](https://github.com/microlinkhq)
- [Keploy - Generate test cases with AI](https://github.com/keploy/keploy)
- [Typesense - Perform searches on large data](https://github.com/typesense/typesense)
- [Milvus - Build apps using a vector database](https://github.com/milvus-io/milvus)
- [Tailscale - Create secure private networks](https://github.com/tailscale/tailscale)
- [PalaCMS - Build static sites visually](https://github.com/palacms/PalaCMS)
- [Vrite - Create technical content in a team](https://github.com/vriteio/vrite)
- [Papermark - Share documents and pitch decks](https://github.com/mfts/papermark)

## Large Language Model ( LLM ) hubs
* https://huggingface.co/models
* https://ollama.com/library

## Large Language Model ( LLM ) local clients

### [Ollama](https://ollama.com)
#### ollama installation
[linux installation](https://github.com/ollama/ollama/blob/main/docs/linux.md)
```sh
curl -fsSL https://ollama.com/install.sh | sh
```
check installation
```sh
# /usr/local/bin/ollama
# /etc/systemd/system/default.target.wants/ollama.service → /etc/systemd/system/ollama.service.
# sudo find /usr/share/ollama/.ollama
# ls /usr/share/ollama
```

#### ollama first commands 
```sh
x-www-browser http://127.0.0.1:11434/api/version
journalctl -e -u ollama
ollama --version
```

#### ollama models 
```sh
x-www-browser https://ollama.com/search
ollama pull mistral
ollama pull llama4
# example of the direct url:
# pulling 9d507a36062c # https://dd20bb891979d25aebc8bec07b2b3bbc.r2.cloudflarestorage.com/ollama/docker/registry/v2/blobs/sha256/9d/9d507a36062c2845dd3bb3e93364e9abc1607118acd8650727a700f72fb126e5/data?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=66040c77ac1b787c3af820529859349a%2F20250615%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250615T171841Z&X-Amz-Expires=86400&X-Amz-SignedHeaders=host&X-Amz-Signature=d7ec6ee62468113a74f166dd732fd56751d38e5b9576d73a58237f49460e5ab9

ollama list

# your models local storage
find /usr/share/ollama/.ollama/models
```

#### ollama models import/convert from: 
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

#### Ollama run/prompt via cli
```sh
MODEL_NAME="sqlcoder"
ollama run $MODEL_NAME
# ask question/prompt via cli
ollama run $MODEL_NAME --input "how to ... "
```

#### [Ollama REST API commands](https://github.com/ollama/ollama/blob/main/docs/api.md)
#### Ollama run/prompt via REST API commands
```sh
curl -X POST http://localhost:11434/api/generate -d '{
  "model": "sqlcoder",
  "prompt":"insert user_id=1, name='ehlo' into table users "
 }'
```

#### [Ollama run/prompt via python](https://pypi.org/project/ollama/)
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

### [Huggingface](https://huggingface.co)
* [Browse compatible GGUF models](https://huggingface.co/models?library=gguf)
#### [huggingface cli installation](https://huggingface.co/docs/huggingface_hub/en/guides/cli)
1. cli installation
   ```sh
   pip3 install -U --break-system-packages "huggingface_hub[cli]"
   huggingface-cli version   
   
   find ~/.cache/huggingface
   huggingface-cli env
   ```
2. [create your token](https://huggingface.co/settings/tokens)
3. set env variable 
   ```sh
   export HF_TOKEN="hf_xxxxxxxxxxxxx"
   huggingface-cli whoami   
   ```
   or make 
   ```sh
   huggingface-cli login
   ```

### [llama cpp](https://github.com/ggerganov/llama.cpp)
> local llama-server
```sh
# Use a local model file
llama-cli -m my_model.gguf

# Or download and run a model directly from Hugging Face
llama-cli -hf ggml-org/gemma-3-1b-it-GGUF

# Launch OpenAI-compatible API server
llama-server -hf ggml-org/gemma-3-1b-it-GGUF
```

### [Jan ](https://jan.ai/)
> client for running your local model

### [Jellybox ](https://jellybox.com/)
> client for running your local model

### [RecurseChat ](https://recurse.chat/)
> client for running your local model
> mac only, $

### [Msty ](https://msty.app/)
> client for running locally AI models

### [Sanctum ](https://sanctum.ai/)
> client for running locally AI models

### [LocalAI ](https://github.com/mudler/LocalAI)
> client for running locally AI models
 
### [vLLM ](https://docs.vllm.ai/)
> client for running locally AI models

### [node llama-cpp](https://node-llama-cpp.withcat.ai/)
> client for running locally AI models


### [MLX LM](https://github.com/ml-explore/mlx-lm)
> cli client for running locally AI models
```sh
# pip install mlx-lm

mlx_lm.chat
mlx_lm.generate --prompt "who you are ?"
```

```sh
ls ~/.local/bin/mlx*
find ~/.cache/huggingface
```

## LLM update
### Retrieval Augemented Generation
> RAG is a technique where an LLM retrieves relevant external data at query time  
> and uses that information to generate responses.   
> It does not modify the model’s parameters.

#### RAG Use Case:
* Frequently changing data (e.g., documentation, support FAQs)
* Domain-specific tasks where knowledge is stored outside the model

#### RAG Types
* Agentic RAG - for creating scalable workflow of tasks
* Enhancement tools: 
  * LangGraph
  * Phoenix Arize
* G-RAG

#### Embeddings
* chunk size
  > 300+ has no sence
  > 100 is more or less optimal
* chunk overlaping 
  > small effect, not more than 5 

### [Fine tuning](./ai-tools-fine-tuning.md)
> updating the weights/parameters of the base LLM  
> using additional labeled data  
> model extending

## [MCP ModelContextProtocol](https://modelcontextprotocol.io/docs/getting-started/intro)
### architecture
![mcp sequence](https://i.ibb.co/s9ywRss4/mcp-workflow.jpg)
```mermaid
graph LR
    U[user]

    subgraph mcp_host
        MH[mcp host]
    end

    subgraph LLM
        L[LLM]
    end

    subgraph mcp_server
        MS[mcp server]
    end

    subgraph External Systems
        A[API]
        D[DB]
        F[File System]
    end

    U -- 1 --> MH
    MH -- 2.get list of tools --> MS
    MH -- 3.request + tools --> L
    L -- 4.selected tool --> MH
    MH -- 5.use tool --> MS
    MH -- 6.msp response --> L
    MH -- 7.response --> U
    MS -- accesses --> A
    MS -- accesses --> D
    MS -- accesses --> F
    
```
- [Model Context Protocol documentation](https://modelcontextprotocol.io)
- [Model Context Protocol specification](https://spec.modelcontextprotocol.io)
- [Python SDK GitHub repository](https://github.com/modelcontextprotocol/python-sdk)
- [Officially supported servers](https://github.com/modelcontextprotocol/servers)
- [MCP Core Architecture](https://modelcontextprotocol.io/docs/concepts/architecture)

### mcp servers
#### collections
- [mcp servers collection](https://github.com/modelcontextprotocol/servers)
- [mcp servers and clients collection](https://www.pulsemcp.com/)
- [mcp servers and clients collection](https://mcp.so/)

#### Documents retrieval, documents recognizing ocr and parsing
* [compari](https://github.com/illuin-tech/colpali)
* [docling](https://github.com/docling-project/docling)

#### SQL
- servers/src/postgres # A powerful MCP server built for PostgreSQL, enabling AI models to interact with relational database systems.
- servers/src/sqlite # Lightweight and efficient, this server brings SQLite functionality to MCP.
- kiliczsh/mcp-mongo-server # A robust MCP server for MongoDB, great for NoSQL database integration.
- ClickHouse/mcp-clickhouse # Tap into the power of ClickHouse with this high-performance MCP server for analytical databases.

#### Search Servers
- nickclyde/duckduckgo-mcp-server # Search privately and efficiently using DuckDuckGo with this MCP server.
- tavily-ai/tavily-mcp # Use Tavily's MCP server for fast and free search results in the JSON format.
- ChanMeng666/server-google-news # Access news articles and headlines with this Google News MCP server.
- servers/src/brave-search # Integrate Brave’s privacy-focused search engine into your AI workflows.

#### Finance Servers
- ferdousbhai/investor-agent # It provides financial insights and analysis to LLMs. 
- QuantGeekDev/coincap-mcp # Fetch cryptocurrency data from the Coincap API.
- anjor/coinmarket-mcp-server # Access CoinMarketCap’s API to retrieve real-time market insights.
- berlinbra/alpha-vantage-mcp # Provides real-time access to financial market data through the free Alpha Vantage API. #  

#### Communication Servers
- servers/src/slack # Interact with Slack workspaces and streamline messaging.
- chaindead/telegram-mcp # Manage Telegram chats, messages, and more with this feature-rich server.
- MarkusPfundstein/mcp-gsuite # Interact with Google GSuite products, such as Gmail, Calendar, and Drive.
- lharries/whatsapp-mcp # This server adds MCP functionality to WhatsApp, enabling message management and conversation tracking.

#### Knowledge Memory Servers
- CheMiguel23/MemoryMesh # A knowledge graph server for structured memory persistence.
- graphlit/graphlit-mcp-server # Use Graphlit’s platform for organizing and retrieving structured knowledge.
- mem0ai/mem0-mcp # Simplify knowledge storage and retrieval with Mem0.
- servers/src/memory # The perfect solution for embedding memory functions into your AI systems.

#### Productivity Servers
- MarkusPfundstein/mcp-obsidian # Connect with Obsidian’s note-taking app using this server.
- danhilse/notion_mcp # Manage your Notion tasks and to-do lists with ease.
- githejie/mcp-server-calculator # A handy server for performing quick calculations.
- inbox-zero/apps/mcp-server # Keep your inbox organized and achieve productivity bliss.

#### Filesystem Servers
- servers/src/filesystem  # Perform essential filesystem operations through MCP.
- servers/src/gdrive # Integrate Google Drive into your workflows with this server.
- exoticknight/mcp-file-merger # Merge multiple files into one with this utility.
- mark3labs/mcp-filesystem-server # A Go-based implementation of filesystem operations using MCP.

#### Coding Servers
- pydantic-ai/mcp-run-python # Execute Python code and manage runtime environments with this server.
- yepcode/mcp-server->js # Enable JavaScript coding workflows with this server.
- wonderwhy-er/DesktopCommanderMCP # A versatile tool to manage desktop commands and workflows.
- oraios/serena # A coding agent toolkit with semantic retrieval and editing capabilities.

