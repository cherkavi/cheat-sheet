# Github Copilot 
* [copilot cli](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/about-copilot-cli)
* [features](https://github.com/features/copilot/cli)  
* [awesome copilot](https://github.com/github/awesome-copilot)

## copilot installation 
```sh
# https://docs.github.com/en/copilot
curl -fsSL https://gh.io/copilot-install | bash
copilot
```

## copilot cli commands
```sh
copilot -p "are you online ? answer only yes/no" --silent 
```

## copilot folder/files structure
```
.github/
└── skills/
    └── temp-skill/
        ├── SKILL.md
        ├── TEMPLATE.md
        ├── scripts/
        └── metadata
```
| Component      | Purpose                                      |
| -------------- | -------------------------------------------- |
| `SKILL.md`     | Defines the skill and its instructions       |
| `scripts/`     | Contains scripts used by the skill           |
| `TEMPLATE.md`  | Reusable template for generating content     |
| Skill metadata | Name, description, and discovery information |
| Settings       | Configure the chat and debugging experience  |

```
.github/skills/temp-skill/
          │
          ▼
       SKILL.md
          │
          ▼
   Skill metadata
    ┌─────┴─────┐
    ▼           ▼
 Discovery    Description
    │
    ▼
  Copilot
    │
    ▼
 Execute skill instructions
    │
    ▼
 Scripts / templates
```

[`SKILL.md` — basic structure](https://github.com/anthropics/skills)
```md
---
name: temp-skill
description: >
  Describe what the skill does and when Copilot
  should use it.
---

# Skill title

## Instructions
- Explain the task.
- Define expected behavior.
- Specify constraints and requirements.

## Workflow
1. Analyze the request.
2. Execute the required steps.
3. Validate the result.

## Resources
- `scripts/`
- `TEMPLATE.md`
```

