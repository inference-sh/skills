---
name: llm-models
description: "Access Claude, Gemini, Kimi, GLM and 100+ LLMs via inference.sh CLI using OpenRouter. Models: Claude Opus 4.6, Claude Sonnet 4.5, Claude Haiku 4.5, Gemini 3 Pro, Kimi K2, GLM-4.6, Intellect 3. One API for all models with automatic fallback and cost optimization. Use for: AI assistants, code generation, reasoning, agents, chat, content generation. Triggers: claude api, openrouter, llm api, claude sonnet, claude opus, gemini api, kimi, language model, gpt alternative, anthropic api, ai model api, llm access, chat api, claude alternative, openai alternative"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# LLM Models via OpenRouter

Access 100+ language models via [inference.sh](https://inference.sh) CLI.

![LLM Models via OpenRouter](https://cloud.inference.sh/app/files/u/4mg21r6ta37mpaz6ktzwtt8krr/01kgvftjwhby36trvaj66bwzcf.jpeg)

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

# Call Claude Sonnet
belt app run openrouter/claude-sonnet-45 --input '{"prompt": "Explain quantum computing"}'
```


## Available Models

| Model | App ID | Best For |
|-------|--------|----------|
| Claude Opus 4.6 | `anthropic/claude-opus-4-6` | Complex reasoning, coding |
| Claude Sonnet 4.5 | `openrouter/claude-sonnet-45` | Balanced performance |
| Claude Haiku 4.5 | `openrouter/claude-haiku-45` | Fast, economical |
| Gemini 3 Pro | `openrouter/gemini-3-pro-preview` | Google's latest |
| Kimi K2 Thinking | `openrouter/kimi-k2-thinking` | Multi-step reasoning |
| GLM-4.6 | `openrouter/glm-46` | Open-source, coding |
| Intellect 3 | `openrouter/intellect-3` | General purpose |
| Any Model | `openrouter/any-model` | Auto-selects best option |

## Search LLM Apps

```bash
belt app search "openrouter"
belt app search "claude"
```

## Examples

### Claude Opus (Best Quality)

```bash
belt app run anthropic/claude-opus-4-6 --input '{
  "prompt": "Write a Python function to detect palindromes with comprehensive tests"
}'
```

### Claude Sonnet (Balanced)

```bash
belt app run openrouter/claude-sonnet-45 --input '{
  "prompt": "Summarize the key concepts of machine learning"
}'
```

### Claude Haiku (Fast & Cheap)

```bash
belt app run openrouter/claude-haiku-45 --input '{
  "prompt": "Translate this to French: Hello, how are you?"
}'
```

### Kimi K2 (Thinking Agent)

```bash
belt app run openrouter/kimi-k2-thinking --input '{
  "prompt": "Plan a step-by-step approach to build a web scraper"
}'
```

### Any Model (Auto-Select)

```bash
# Automatically picks the most cost-effective model
belt app run openrouter/any-model --input '{
  "prompt": "What is the capital of France?"
}'
```

### With System Prompt

```bash
belt app sample openrouter/claude-sonnet-45 --save input.json

# Edit input.json:
# {
#   "system": "You are a helpful coding assistant",
#   "prompt": "How do I read a file in Python?"
# }

belt app run openrouter/claude-sonnet-45 --input input.json
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

