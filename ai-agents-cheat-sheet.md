# AI Agents

## AI agents with a built-in learning loop

###  [Hermes Agent](https://github.com/NousResearch/hermes-agent)
* [github](https://github.com/NousResearch/hermes-agent)
* [documentation](https://hermes-agent.nousresearch.com/docs?categoryid=2826672)
  * [running hermes on personal or work machine](https://hermes-agent.nousresearch.com/docs/guides/secure-hermes-on-a-work-machine)
* [usage use cases](https://hermes-agent.nousresearch.com/docs/user-stories)
* [using cli interface](https://hermes-agent.nousresearch.com/docs/user-guide/cli)

#### hermes installation
```sh
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash

hermes --version
```

#### hermes configuration 
```sh
cat  ~/.hermes/config.yaml

hermes config 
hermes config edit
hermes config set   # Set individual config values
hermes config get   # Print individual config values

hermes profile import
hermes profile export
```

#### hermes setup 
```sh
hermes setup 
hermes setup tools
```

#### other commands 
```sh
hermes              # Interactive CLI — start a conversation
hermes model        # Choose your LLM provider and model
hermes tools        # Configure which tools are enabled
hermes gateway      # Start the messaging gateway (Telegram, Discord, etc.)
hermes update       # Update to the latest version
hermes doctor       # Diagnose any issues

hermes chat -q "are you online ?" 
hermes chat --query-file prompt
```

### Letta
* [GitHub](https://github.com/letta-ai/letta)
* [Documentation](https://docs.letta.com/)

### Agent Zero
* [GitHub](https://github.com/agent0ai/agent-zero)
* [Website](https://www.agent-zero.ai/)

### OpenHands
* [GitHub](https://github.com/All-Hands-AI/OpenHands)
* [Documentation](https://docs.openhands.dev/)

### Khoj
* [GitHub](https://github.com/khoj-ai/khoj)
* [Documentation](https://docs.khoj.dev/)

## learning-loop frameworks

| Project        | Learning mechanism                                                | Type                        |
| -------------- | ----------------------------------------------------------------- | --------------------------- |
| Memento-Skills | Read → Execute → Reflect → Write; repairs and rewrites skills     | Self-evolving framework     |
| MUSE           | Converts execution trajectories into reusable hierarchical memory | Research framework          |
| ACE            | Improves agent instructions using execution feedback              | Context evolution framework |
| LangMem        | Extracts, updates and retrieves long-term memories                | Memory framework            |
| Hindsight      | Retains, recalls and reflects on experience                       | Memory framework            |

- [Memento-Skills](https://www.memento.run/research/skills/)
- [MUSE research](https://arxiv.org/abs/2510.08002)
- [Agent self-improvement comparison](https://rywalker.com/research/agent-self-improvement)