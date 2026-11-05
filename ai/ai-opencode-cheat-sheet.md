# [OpenCode](https://opencode.ai)

## opencode installation [gitub opencode](https://github.com/anomalyco/opencode)
```sh
curl -fsSL https://opencode.ai/install | bash
```

## usages
```sh
opencode run --quiet "Your prompt here"
opencode run --format json "Your prompt here"

opencode --print-logs --log-level DEBUG --quiet "are you online ?"
```

## opencode logs
```sh
tail -f ~/.local/share/opencode/log/opencode.log
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


## connect opencode to ollama locally/localhost
copy file to your workscpace and run `opencode`
```json:~/.config/opencode/opencode.jsonc
{
  "$schema": "https://opencode.ai/config.json",

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
        },

        "ollama": {
            "name": "Ollama Local",
            "package": "@opencode/ai/providers/openai-compatible",
            "settings": {
                "baseURL": "http://localhost:11434/v1"
            },
            "models": {
                "mistral:latest": {
                    "name": "Mistral Local",
                    "modelID": "mistral:latest",
                    "limit": {
                        "context": 32768,
                        "output": 8192
                    }
                }
            }
        }
    }
}
```

## providers
* [OpenRouter](https://openrouter.ai/keys)  
  [openrouter dashboard](https://openrouter.ai/workspaces/default/activity)
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
  no free plan
* [Together](https://api.together.xyz/settings/api-keys)  
  no free plan
* [Perplexity](https://www.perplexity.ai/settings/api)  
  no free plan
* [OpenCode Go / Zen](https://opencode.ai/auth)  
  [opencode web console](https://opencode.ai/console)