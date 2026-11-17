---
name: llm-models
description: "Access Claude, Gemini, Kimi and GLM via inference.sh CLI. Models: Claude Opus 5.5, Claude Sonnet 5.5, Claude Haiku 5.5, Gemini 4 Argon, Kimi K3, GLM-5.3. One CLI and one input format for every model. Use for: AI assistants, code generation, reasoning, agents, chat, content generation. Triggers: claude api, llm api, claude sonnet, claude opus, gemini api, kimi, glm, language model, gpt alternative, anthropic api, ai model api, llm access, chat api, claude alternative, openai alternative"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# LLM Models

Access leading language models via [inference.sh](https://inference.sh) CLI.

![LLM Models](https://cloud.inference.sh/app/files/u/4mg21r6ta37mpaz6ktzwtt8krr/01kgvftjwhby36trvaj66bwzcf.jpeg)

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

# Call Claude Sonnet
belt app run anthropic/claude-sonnet-5-5 --input '{"text": "Explain quantum computing"}'
```


## Available Models

| Model | App ID | Best For |
|-------|--------|----------|
| Claude Fable 5.1 | `anthropic/claude-fable-5-1` | Anthropic's most capable |
| Claude Opus 5.5 | `anthropic/claude-opus-5-5` | Complex reasoning, coding |
| Claude Sonnet 5.5 | `anthropic/claude-sonnet-5-5` | Balanced performance |
| Claude Haiku 5.5 | `anthropic/claude-haiku-5-5` | Fast, economical |
| Gemini 4 Argon | `google/gemini-4-argon` | Google's flagship |
| Kimi K3 | `melious/kimi-k3` | Multi-step reasoning |
| GLM-5.3 | `melious/glm-5-3` | Open-weight, coding |

Every model takes the same input: `text`, plus optional `system_prompt`, `temperature`, `max_tokens`, `images`, `tools`.

## Search LLM Apps

```bash
belt app search "claude"
belt app search "gemini"
```

## Examples

### Claude Opus (Best Quality)

```bash
belt app run anthropic/claude-opus-5-5 --input '{
  "text": "Write a Python function to detect palindromes with comprehensive tests"
}'
```

### Claude Sonnet (Balanced)

```bash
belt app run anthropic/claude-sonnet-5-5 --input '{
  "text": "Summarize the key concepts of machine learning"
}'
```

### Claude Haiku (Fast & Cheap)

```bash
belt app run anthropic/claude-haiku-5-5 --input '{
  "text": "Translate this to French: Hello, how are you?"
}'
```

### Kimi K3 (Reasoning)

```bash
belt app run melious/kimi-k3 --input '{
  "text": "Plan a step-by-step approach to build a web scraper"
}'
```

### With System Prompt

```bash
belt app sample anthropic/claude-sonnet-5-5 --save input.json

# Edit input.json:
# {
#   "system_prompt": "You are a helpful coding assistant",
#   "text": "How do I read a file in Python?"
# }

belt app run anthropic/claude-sonnet-5-5 --input input.json
```

## Use Cases

- **Coding**: Generate, review, debug code
- **Writing**: Content, summaries, translations
- **Analysis**: Data interpretation, research
- **Agents**: Build AI-powered workflows
- **Chat**: Conversational interfaces

## Related Skills

```bash
# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# Web search (combine with LLMs for RAG)
npx skills add inference-sh/skills@web-search

# Image generation
npx skills add inference-sh/skills@ai-image-generation

# Video generation
npx skills add inference-sh/skills@ai-video-generation
```

Browse all apps: `belt app list`

## Documentation

- [Agents Overview](https://inference.sh/docs/concepts/agents) - Building AI agents
- [Agent SDK](https://inference.sh/docs/api/agent/overview) - Programmatic agent control
- [Building a Research Agent](https://inference.sh/blog/guides/research-agent) - LLM + search integration guide

