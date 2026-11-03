# [OpenCode](https://opencode.ai)

## opencode installation [gitub opencode](https://github.com/anomalyco/opencode)
```sh
curl -fsSL https://opencode.ai/install | bash
```

## usages
```sh
opencode run --quiet "Your prompt here"
opencode run --format json "Your prompt here"
```

## connect opencode to ollama locally  
copy file to your workscpace and run `opencode`
```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "ollama": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Ollama",
      "options": {
        "baseURL": "http://localhost:11434/v1"
      },
      "models": {
        "qwen2.5-coder:14b": {
          "name": "qwen2.5-coder:14b"
        }
      }
    }
  },
  "model": "ollama/qwen2.5-coder:14b"
}
```
