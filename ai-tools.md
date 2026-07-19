# Collection of AI tools
* [copilot community](https://github.com/github/awesome-copilot)
  * [copilot official education] (https://github.com/github/copilot-cli-for-beginners)
  * [copilot alternative education](https://github.com/DanWahlin/github-copilot-get-started)
* [my own AI cheat sheet](https://github.com/cherkavi/ai)
* [my own VectorDB cheat sheet](./vectordb-cheat-sheet.md)
* :TODO: https://github.com/explodinggradients/ragas  

## General info
* [how AI works, what context window is, how token count](https://swnger.github.io/innerloop/#agent-loop)
* each time during the asking question in one prompt you are sending all the previous questions/answers to the server
* [AI model comparison](https://docs.github.com/en/copilot/reference/ai-models/model-comparison)

## vendor agnostic CLI tools
### [opencode](https://opencode.ai)
```sh
curl -fsSL https://opencode.ai/install | bash
```

### [aider](https://aider.chat/docs/install.html)
```sh
python -m pip install aider-install
aider-install
```

### crush

### llxprt-code

## vendors CLI tools
[tool for saving tokens](https://github.com/rtk-ai/rtk)

### [copilot cli](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/about-copilot-cli)
```sh
curl -fsSL https://gh.io/copilot-install | bash
copilot
```
* [features](https://github.com/features/copilot/cli)  
* [awesome copilot](https://github.com/github/awesome-copilot)

### [antigraviti cli](https://antigravity.google/download)
```sh
curl -fsSL https://antigravity.google/cli/install.sh | bash
```

### [gemini cli](https://geminicli.com/docs/get-started/installation/)
```sh
# via NPM package 
sudo npm install -g @google/gemini-cli
# npm install -g @google/gemini-cli@0.45.0
sudo npm remove -g @google/gemini-cli

# via SNAP
sudo snap install gemini-cli

gemini
```
```
/chat share session-name.md
/chat save checkpoint-1
/resume
!ls ~/.gemini/tmp/<project_hash>/chats/
```

### claude cli 
```sh
curl -fsSL https://claude.ai/install.sh | bash
claude 
```
- [cli commands](https://code.claude.com/docs/en/cli-reference)

### [chatgpt / openai ](https://developers.openai.com/codex/cli)
```sh
npm i -g @openai/codex
codex
# non official, alternative 
brew tap kardolus/chatgpt-cli && brew install chatgpt-cli
```

### deep seek  ( non official )
```sh
npm install -g run-deepseek-cli
# alternative
pip install deepseek-cli

deepseek 
```

### groq
```sh
# non official
npm install -g groq-ai-cli
```
### cohere
```sh
# non official
curl -sL https://raw.githubusercontent.com/plyght/cohere-cli/main/install.sh | bash
```

### perplexity
```sh
# non official
pipx install git+https://github.com/zahidoverflow/perplexity-cli.git
```

## Online AI Tools, ready to use prompts
### chat bots 
| Tool                                         | Use Case                                                                                  |
| :------------------------------------------- | :---------------------------------------------------------------------------------------- |
| [ChatGPT](https://chatgpt.com)               | General-purpose assistant for writing, brainstorming, coding, and analysis.               |
| [Gemini](https://gemini.google.com)          | Research and reasoning, integrating with Google services.                                 |
| [Claude](https://claude.ai)                  | Long-form writing, complex reasoning, and safe, nuanced conversations.                    |
| [Grok](https://grok.com)                     | Real-time information access and "witty" responses with minimal safety filters, integrated with X. |
| [Qwen](https://chat.qwen.ai)                 | Business applications, analysis of long documents, and multilingual tasks.                |
| [DeepSeek](https://chat.deepseek.com)        | Complex reasoning and coding assistance with a massive 1M token context window.           |
| [Mistral](https://chat.mistral.ai)           | Developer-focused tasks and coding assistance.                                            |
| [Kimi](https://kimi.com)                     | Processing and analyzing extremely long documents (e.g., 200,000+ characters).            |
| [GLM](https://chat.z.ai)                     | General-purpose tasks with a free tier; useful for coding, writing, and translation.      |
| [Perplexity](https://perplexity.ai)          | Research and fact-finding with citations and real-time web search.                        |
| [Llama](https://www.meta.ai)                 | Private, offline experimentation and development of custom applications.                  |
| [Gemma](https://ai.google.dev/gemma)         | On-device AI applications, mobile development, and fine-tuning.                           |

### chat bots
* https://github.com/copilot  
  * [copilot SDK](https://github.com/github/copilot-sdk)
  * [copilot debug activation, copilot prompt log explanation](https://code.visualstudio.com/docs/agents/agent-troubleshooting/chat-debug-view#_agent-debug-log-panel)
* https://www.perplexity.ai/
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

## AI platforms
* [union](https://www.union.ai/)

## customization/flexible prompt
* [deploy custom models and run them ](https://replicate.com/)

## general purposes tools
* [llm](#llm---cli-tool-for-interacting-with-llm)
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
- whisper - AWS ( amazon services )

## Solutions for local text wiki/knowledgebase
| Approach            | Core Mechanism                               | Primary Use Case                                     | Example Tools                                                                                                   |
| ---                 | ---                                          | ---                                                  | ---                                                                                                             |
| **Wiki LLM**        | Automated synthesis & markdown generation    | Compounding personal research and note-taking        | `llm_wiki`, [cli](https://github.com/Pratiyush/llm-wiki), [nashsu/llm_wiki](https://github.com/nashsu/llm_wiki) |
| **Vector DB + MCP** | Standardized API connecting DB to LLM client | Connecting custom local databases to Claude/Cursor   | SQLite [MCP](https://modelcontextprotocol.io), Chroma MCP                                                       |
| **Knowledge Graph** | Explicit relationship mapping (Nodes/Edges)  | Large codebases, complex architectures, debugging    | [Graphify](https://github.com/safishamsi/graphify), [GraphRAG](https://microsoft.github.io/graphrag)            |
| **GUI RAG Apps**    | Bundled UI, Vector DB, and LLM runner        | Easy, zero-code document querying for standard users | [AnythingLLM](https://anythingllm.com), [Chatd](https://chatd.ai), [lmstudio.ai](https://lmstudio.ai/)          |
| **CLI RAG Tools**   | Python scripts, LangChain/LlamaIndex         | Terminal-based privacy-first document analysis       | [LocalGPT](https://github.com/PromtEngineer/localGPT), [PrivateGPT](https://github.com/zylon-ai/private-gpt)    |
| **IDE Indexing**    | Workspace chunking and embedding             | Software development and code documentation          | [Cursor](https://cursor.com/), [Continue.dev](https://www.continue.dev/)                                        |
| **PKM Plugins**     | Vault embedding within a note app            | Zettelkasten, personal diaries, connected thought    | [Khoj](https://khoj.dev/), [Smart Connections](https://github.com/brianpetro/obsidian-smart-connections)        |




## Solutions for text recognition (OCR)
- [Amazon Textract](https://aws.amazon.com/textract/) + ( Claude Opus 4.5 | Claude Sonnet 4.5 )  
  Amazon Textract is an AWS service that uses machine learning to automatically extract text, handwriting, and data from scanned documents.
  - Merges cells with faint borders.
  - Double borders in tables are often misunderstood.
- [Amazon Bedrock Data Automation](https://aws.amazon.com/bedrock/data-automation/)  
  Amazon Bedrock is a fully managed service that offers a choice of high-performing foundation models (FMs) from leading AI companies via an API.
  - Limited accuracy with German language medical documents.
- [Tesseract OCR](https://github.com/tesseract-ocr/tesseract)
  - No semantic understanding of table structure.
  - Poor German medical terminology accuracy.
- [Docling](https://github.com/docling-project/docling) - IBM's tool for parsing PDF/Docx into Markdown/JSON.

## Large Language Model ( LLM ) hubs
* https://huggingface.co/models
* https://ollama.com/library

## Large Language Model ( LLM ) local clients ( run locally )

### opencode - ai coding agent
* https://opencode.ai/

### openclaw - ai assistant for general purposes 
* https://openclaw.ai/

### [llm - cli tool for interacting with LLM](https://github.com/simonw/llm)
```sh
pip3 install llm 

alias llm-list-of-plugins='x-www-browser https://llm.datasette.io/en/stable/plugins/directory.html'
alias llm-doc='x-www-browser https://llm.datasette.io/en/stable/index.html#'

## llm-youtube
llm install llm-youtube
llm -f yt:zv72WMmVkPw 'Please summarize this video'
```

### [anythingLLM](https://docs.anythingllm.com/installation-desktop/linux#install-using-the-installer-script)
> get local LLMs, RAG and Agents with little to zero configuration and full privacy
```sh
# Download the installer script to wherever you want to run it from
curl -fsSL https://cdn.anythingllm.com/latest/installer.sh -o installer.sh

# Make the script executable
chmod +x installer.sh

# Run the script
./installer.sh

$HOME/AnythingLLMDesktop.AppImage
# Upload files to Workspace
# use Query Mode: The AI is forced to only answer based on your documents.
```

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

#### Cohere & FAISS DB
These are often used together to build **Retrieval-Augmented Generation (RAG)** systems. Cohere provides the "brain" (embeddings/LLM), while FAISS provides the "memory" (vector storage).
* **Cohere (AI Models):**
* **Official Website:** [Cohere.com](https://cohere.com/)
* **Developer Docs:** [Cohere Documentation](https://docs.cohere.com/)
* **FAISS (Vector Database):**
* **GitHub Repository:** [facebookresearch/faiss](https://github.com/facebookresearch/faiss)
* **Official Wiki/Docs:** [FAISS Documentation](https://faiss.ai/)
* **Integration Example:**

#### Langchain
* [Building RAG with LangChain, Cohere, and FAISS](https://zilliz.com/tutorials/rag/langchain-and-faiss-and-cohere-command-r-and-cohere-embed-multilingual-light-v3.0)
* [langgraph](https://www.langchain.com/langgraph)

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


## LLM Processing
how a user's raw text request transitions into mathematical data, passes through a Transformer architecture, and ultimately predicts the next word.

| Stage               | Input Type          | Processing Mechanism                 | Output Type              |
| ---                 | ---                 | ---                                  | ---                      |
| **1. Tokenization** | Raw Text Prompt     | String Splitting / Vocabulary Lookup | Token IDs (Integers)     |
| **2. Embedding**    | Token IDs           | Coordinate Mapping                   | Dense Vectors (Floats)   |
| **3. Transforming** | Input/Output Matrix | Multi-Head Attention Layers          | Contextual Output Matrix |
| **5. Prediction**   | Training            | Probability Distribution (Softmax)   | The Next Word            |

### 1. Tokenization
* **What it is:** The gateway from human language to machine logic.
* **The Process:** When a **request** (input text) enters the system, it is chopped up into smaller chunks called **tokens** (which can be whole words, syllables, or characters).
* **Why it matters:** LLMs don't read words; they read token IDs (integers assigned to each unique piece of text in a massive vocabulary dictionary).

### 2. Embedding & Vector DB
* **What it is:** The translation of tokens into multi-dimensional geometry.
* **The Process:** Every token ID is mapped to an **embedding vector**—a long string of numbers acting as coordinates in a high-dimensional space.
* **The Logic ("Relations by Coordinates"):** As your green notes hint, words with similar meanings or grammatical roles are placed close to each other in this coordinate space (e.g., "king" and "queen" will have similar vector values).
* > **A Quick Nuance:** In a standard LLM, an *Embedding Layer* handles this live conversion. A *Vector DB* (Vector Database) is typically an external tool used to store and quickly search millions of these embeddings for long-term memory or document retrieval (RAG).


### 3. Transforming & Multi-Head Attention
* **What it is:** The "engine room" of the LLM based on the Transformer architecture.
* **Multi-Head Attention:** This is the secret sauce. Instead of looking at a sentence one word at a time, the model uses multiple "attention heads" simultaneously.
* **The Purpose:** It calculates how much every word in a sentence relates to *every other word*, capturing context, sarcasm, pronouns, and complex sentence structures perfectly.

### 4. Input / Output Matrix
* **Input Matrix:** A structured grid of data feeding into the Transformer blocks. It combines the token embeddings with "positional encodings" (which tell the model the exact order of the words).
* **Output Matrix:** The final matrix generated by the Transformer layers. It contains updated numerical values representing the model's "understanding" of the entire text sequence.
* **The Transition:** This output matrix is then passed through a final mathematical layer (Softmax) to turn those raw numbers into a clean list of percentages/probabilities for every word in its vocabulary.

### 5. Training Model & Next-Word Prediction

* **The Training Model:** The green box represents the massive, pre-trained neural network weights. During training, the model looks at billions of sentences to learn exactly how those vector coordinates *should* interact.
* **Next Word Prediction:** At its core, an LLM is a highly advanced autocomplete engine. Using the output matrix, it picks the highest-probability token to follow the input sequence, appends it to the prompt, and repeats the entire loop to generate a full response.


## LLM: Lifecycle & Evaluation

### 1. From Transforming to Prediction (The Inference Bridge)

* **The Loop:** The Transformer architecture (`A Trans`) processes the tokens to perform **Next-Word Prediction**.
* **The Output:** The model outputs a probability distribution for the next token. But how do we know if these predictions are actually good? That brings us to the next layer of your sketch: **Evaluation**.

### 2. Evaluation & Perplexity Metrics

* **What it is:** The mathematical audit of an LLM's language skills.
* **Perplexity (PPL):** The gold-standard metric for language models. Intuitively, perplexity measures how "surprised" or confused a model is by a piece of text.
* **The Math:** It is calculated as the exponentiated cross-entropy loss of the model:

$$\text{Perplexity} = 2^{H(p)}$$

* **How to read it:** * **Low Perplexity = Highly Confident Model.** The model is certain about its next-word choices and mimics natural human language well.
* **High Perplexity = Confused Model.** The model is guessing wildly, leading to nonsensical outputs or "hallucinations."


### 3. The Customization Lifecycle

An out-of-the-box model rarely fits every specific business need. Your green and blue sketches layout the classic downstream development pipeline:

* **Foundation Model:** The raw, massive brain (e.g., base GPT-4, Llama 3, Mistral). It has read the internet and understands general language grammar, but lacks your specific domain knowledge.
* **Dataset:** A curated, high-quality collection of specific text (e.g., medical records, legal contracts, customer support history).
* **Fine-Tuning:** The process of taking the **Foundation Model**, feeding it the specialized **Dataset**, and running a lightweight training phase to adjust its neural weights.
* **Final Data Model:** The end result—a highly specialized, fine-tuned model optimized for a specific domain or task.

### LLM End-to-End

### How Everything Ties Together
1. **The Foundation:** You start with a **Foundation Model** and a target **Dataset**.
2. **The Adaptation:** You run **Fine-Tuning** to create a **Specialized Data Model**.
3. **The Execution:** When a user submits a **Request**, 
   this specialized model uses its weights inside the **Multi-Head Attention** system 
   to turn **Tokenized Embeddings** into an optimized **Output Matrix**.
4. **The Validation:** The engine delivers the **Next-Word Prediction**, 
   which is audited by **Evaluation Metrics** like **Perplexity** 
   to guarantee high-quality, accurate AI responses.

```mermaid
flowchart TD

    %% Class/Style Definitions
    classDef core fill:#e1f5fe,stroke:#01579b,stroke-width:2px;
    classDef process fill:#fff3e0,stroke:#ff6f00,stroke-width:2px;
    classDef lifecycle fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;
    classDef eval fill:#fce4ec,stroke:#c2185b,stroke-width:2px;

    %% Phase 1: The Lifecycle (Sketch 2)
    subgraph Lifecycle [1. Model Creation & Customization]
        A([Dataset]) -->|Feeds into| C[Fine-Tuning Process]
        B([Foundation Model]) -->|Base Architecture| C
        C -->|Outputs| D[(Specialized Data Model)]
    end

    %% Phase 2: The Core Inference Pipeline (Sketch 1)
    subgraph Inference [2. Real-Time Processing Engine]
        E[User Request] -->|Raw Text| F(Tokenization)
        F -->|Token IDs| G(Embedding Layer / Vector DB)
        
        subgraph Transformer [A Transforming Block]
            H[Input Matrix] <--> I[Multi-Head Attention]
            I <--> J[Output Matrix]
            J -->|Recursive Layers Loop| H
        end
        
        G -->|Coordinate Vectors| H
        D -.->|Provides trained weights to| I
    end

    %% Phase 3: Output & Evaluation (Sketch 1 & 2)
    subgraph Analysis [3. Output & Quality Control]
        J --> K[Next-Word Prediction]
        K -->|Generated Output Sequence| L{Evaluation Layer}
        L -->|Calculate Surprisal| M([Perplexity Metrics])
    end

    %% Apply Styles
    class B,D,A lifecycle;
    class F,G,H,I,J process;
    class E,K core;
    class L,M eval;

```

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


## Prompt Engineering
```mermaid
graph TD

    %% Nodes
    InternetData[Internet Data]
    LLM[LLM]
    ChatPrompt[Chat Prompt]
    Retriever[Retriever <br> Vector DB]
    DomainKnowledge[Domain Specific Knowledge]
    Generator[Generator]
    
    %% Relationships
    InternetData -->|via training| LLM
    LLM -.->|used by| Generator
    LLM -.->|used by| ChatPrompt
    
    ChatPrompt -->|triggers| LLM
    
    %% RAG Sequence Steps
    ChatPrompt -->|1: triggers| Retriever
    Retriever -->|2: triggers| DomainKnowledge
    DomainKnowledge -->|3: returns to| Retriever
    Retriever -->|4: goes to| Generator
    Retriever -->|5: goes to| ChatPrompt

    %% Styling for clarity
    style InternetData fill:#f9f9f9,stroke:#333,stroke-width:1px
    style LLM fill:#d1e7dd,stroke:#0f5132,stroke-width:2px
    style ChatPrompt fill:#fff3cd,stroke:#664d03,stroke-width:1px
    style Retriever fill:#cff4fc,stroke:#087990,stroke-width:1px
    style DomainKnowledge fill:#f8d7da,stroke:#842029,stroke-width:1px
    style Generator fill:#e2e3e5,stroke:#41464b,stroke-width:1px

```

Based on the diagram, the system outlines two paths—the traditional **Prompt Engineering** path (which is vulnerable to false results and hallucinations) and the enhanced **Retrieval-Augmented Generation (RAG)** path:

1. **The Core LLM Foundation:** * **Internet Data** serves as the initial raw foundation, ingested directly into the **LLM** via **training**.
* Once trained, the **LLM** is utilized as the underlying intelligence driving both the **Chat Prompt** interface and the final **Generator**.


2. **Standard Prompt Engineering Interaction:** * A user interacts directly through a **Chat Prompt**, which **triggers the LLM** to respond based purely on its static, pre-trained internet knowledge. As highlighted on the whiteboard, relying purely on this path frequently introduces issues like *false results* and *hallucinations*.
3. **The Optimized RAG Workflow (Steps 1–4):**
To fix these hallucinations, the diagram illustrates a 4-step factual grounding pipeline:
   * **Step 1:** The user's input via the **Chat Prompt triggers the Retriever (Vector DB)** instead of just relying on the LLM's memory.
   * **Step 2:** The **Retriever triggers a search** within the curated **Domain Specific Knowledge** repository to find accurate, up-to-date documentation.
   * **Step 3:** The factual documents matching the query are **returned to the Retriever**.
   * **Step 4:** The Retriever bundles the user's original query along with this verified context and **goes to the Generator**, allowing the LLM to write an accurate, hallucination-free response grounded in proprietary knowledge.
   * **Step 5:** Back to Prompt

## Prompt Engineering techniques

### Chain of Thought (COT) 

```mermaid
graph TD
    %% Styling
    classDef title text-align:center,font-size:16pt,font-weight:bold,fill:#eee,stroke:#333;
    classDef mainNode fill:#ffe6cc,stroke:#d79b00,color:black,stroke-width:1px;
    classDef textNode fill:none,stroke:none,text-align:left,color:black;

    %% Top Section (Chain of Thought - COT)
    ChatPrompt[chat Prompt]:::mainNode --> Q[Q: Question]:::mainNode
    
    Q -.-> SQ1(subQ 1)
    Q -.-> SQ2(subQ 2)
    Q -.-> SQ3(subQ 3)
    
    SQ1 -.-> SA1(sA 1)
    SQ2 -.-> SA2(sA 2)
    SQ3 -.-> SA3(sA 3)
    
    SA1 --> FinalAnsw(Answ: Final Answer):::mainNode
    SA2 --> FinalAnsw
    SA3 --> FinalAnsw

    %% COT Annotation
    subgraph COTDesc ["Chain of Thought (COT) Process"]
    Q --> FinalAnsw
    end
```

#### The Top Section (Chain of Thought - COT)
* **The Flow:** It shows a step-by-step process. A **"chat Prompt"** goes into a box with a **"Q"** (for "Question").
* **Branching:** This single question then branches out into three parallel sub-paths, each labeled **"subQ"** (for "sub-question").
* **Solving:** Each subQ points to a corresponding **"sA"** (for "sub-answer").
* **Synthesis:** All three sub-answers merge into a final box labeled **"Answ"** (Answer).
* **Text Label:** To the right, there is a text block that says **"COT Chain Of Thought provide examples"**. This label is linked to the overall process above it, indicating this entire branching flow is an example of COT.

#### Chain of Thought (COT): The "Break it Down" Method
Instead of expecting them to know the answer instantly, 
you teach them to take small steps. This whiteboard diagram shows how we do that for an AI.
1. Takes the big **question (Q)**.
2. Breaks it into three smaller, easier-to-solve **sub-questions (subQ)**.
3. Finds a mini-**answer (sA)** for each small question.
4. Comes up with the **final answer (Answ)** by combining those three mini-answers.
*Think of it as breaking down a messy room into smaller tasks like 'make the bed', 'pick up toys', and 'dust the desk' to make the big job manageable.*
This method works best when you **provide examples** to the AI of how to solve similar problems.

###  ReAct: The "Detective" Method
Instead of solving a problem in a straight line like the COT diagram above, 
ReAct is an interactive loop:
1. The system (either the main **LLM** or a helper program called 'dsp') generates a small **sub-question (subQ)** about what knowledge it needs.
2. It uses RAG to "act" and find an outside source to create a **sub-answer (subA)**.
3. It then uses COT to look at that new answer and decide what to do next, repeating the loop.
*Think of ReAct like a detective: they look at a clue (the subA), reason about what it means (COT), and then use that reasoning to decide what clue to look for next (the dynamic loop).*

```mermaid
graph TD
    %% Styling
    classDef title text-align:center,font-size:16pt,font-weight:bold,fill:#eee,stroke:#333;
    classDef mainNode fill:#ffe6cc,stroke:#d79b00,color:black,stroke-width:1px;
    classDef textNode fill:none,stroke:none,text-align:left,color:black;
    
    %% Bottom Section (ReAct and Loop)
    subgraph ReActLoop ["ReAct Loop (iterative) "]

    direction BT
    LLMdsp[LLM or dsp]:::mainNode
    sub_Q(subQ)
    sub_A(subA)
    
    LLMdsp -.-> sub_Q
    LLMdsp -.-> sub_A
    sub_A -.-> LLMdsp
    sub_Q --- sub_A
    end

    %% ReAct Description
    ReActText[ReAct<br>mix RAG & COT]:::textNode --- LLMdsp
    
    %% Connection few-shot to ReAct
    FewShot -..-> ReActText
```
