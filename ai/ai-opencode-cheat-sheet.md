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

## opencode config
```sh
/debug
```
```json:~/.config/opencode/opencode.jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "model": "openrouter/z-ai/glm-5.3-flashx-legal",
  "providers": {
    "openrouter": {
      "models": {
        "z-ai/glm-5.3-flashx-legal": {
          "limit": {
            "context": 128000,
            "output": 16000
          }
        }
      }
    }
  }
}
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

## providers
* [OpenRouter](https://openrouter.ai/keys)
* [Google Gemini (AI Studio)](https://aistudio.google.com/app/apikey)
* [Groq](https://console.groq.com/keys)
* [Cerebras,Free 1 Million tokens/day](https://cloud.cerebras.ai)
* [Together AI](https://api.together.xyz/settings/api-keys)
* [OpenAI](https://platform.openai.com/api-keys)  
  need credits
* [DeepSeek](https://platform.deepseek.com/api_keys)  
  need credits
* [Anthropic (Claude)](https://console.anthropic.com/settings/keys)
* [Mistral](https://console.mistral.ai/api-keys/)
* [Together](https://api.together.xyz/settings/api-keys)
* [Perplexity](https://www.perplexity.ai/settings/api)
* [OpenCode Go / Zen](https://opencode.ai/auth)