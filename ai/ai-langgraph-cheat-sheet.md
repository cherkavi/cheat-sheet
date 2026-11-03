# LangGraph

## Links 
* [Building RAG with LangChain, Cohere, and FAISS](https://zilliz.com/tutorials/rag/langchain-and-faiss-and-cohere-command-r-and-cohere-embed-multilingual-light-v3.0)
* [langgraph](https://www.langchain.com/langgraph)

## create multi agent app
```sh
## create project 
uv init multi-agent
cd multi-agent

## add packages
# langgraph-openai
uv add langgraph langgraph-cli langgraph-api  cudf-cu13 python-dotenv

## obtain https://build.nvidia.com -> API Keys -> 
echo 'export NVIDIA_API_KEY="xxxxxxxx"' > .env 

## download source code
curl -Lo https://raw.githubusercontent.com/will-hill/Data-Science-Agents-Simplified/refs/heads/master/002_agent.py
mv 002_agent.py agent.py

curl -Lo https://raw.githubusercontent.com/will-hill/Data-Science-Agents-Simplified/refs/heads/master/003_multi_agent.py
mv 003_multi_agent.py multi_agent.py

curl -L 'https://raw.githubusercontent.com/will-hill/Data-Science-Agents-Simplified/refs/heads/master/004_langgraph.json' > langgraph.json

## start LangGraph UI 
uv run langgraph dev
```

