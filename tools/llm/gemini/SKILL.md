---
name: gemini
description: "Google Gemini chat models via inference.sh CLI. Models: Gemini 4 Argon (flagship), Gemini 3.8 Flash (most capable Flash), Gemini 3.5 Flash-Lite (fastest, lowest cost). 1M token context, thinking levels, vision, file input, tool use, structured output. Runs on Vertex AI. Use for: coding, autonomous agents, long documents, finance, law, science, high-throughput pipelines, image understanding, chat. Triggers: gemini, google gemini, gemini api, gemini 4, gemini 4 argon, argon, gemini flash, gemini 3.8 flash, gemini flash lite, gemini 3.5 flash-lite, vertex ai, google llm, google ai model, gemini chat"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# Gemini

Run Google's Gemini chat models via [inference.sh](https://inference.sh) CLI.

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

belt app run google/gemini-3-8-flash --input '{"text": "Explain how HTTPS certificate validation works"}'
```

## Models

| Model | App ID | Best For |
|-------|--------|----------|
| Gemini 4 Argon | `google/gemini-4-argon` | Flagship: deep multi-step work in coding, finance, law, security, science |
| Gemini 3.8 Flash | `google/gemini-3-8-flash` | Most capable Flash: long-horizon software engineering, autonomous agents, enterprise workflows |
| Gemini 3.5 Flash-Lite | `google/gemini-3-5-flash-lite` | Fastest, most cost-effective 3.5-class model (350 output tokens/s), high-throughput agentic workflows |

All run via Vertex AI with vision and tool use. Argon and 3.8 Flash have a 1M token context window; Argon supports thinking levels low to high.

Other Gemini apps (older chat versions, image models, and the Gemini Omni video models): `belt app search gemini`. `google/gemini-omni-1-2-flash` is a video generation model, not a chat model.

## Inputs

| Field | Description |
|-------|-------------|
| `text` | The user message |
| `system_prompt` | System instructions |
| `images` | Image files or URLs (vision) |
| `files` | Document files |
| `context` | Prior messages: `[{"role": "user" \| "assistant", "text": "..."}]` |
| `reasoning_effort` | `none` (default), `low`, `medium`, `high` |
| `temperature` | Default 1.0 |
| `top_k` | Default 40, -1 to disable |
| `max_tokens` | Default 64000 |
| `tools` | Function tool definitions |
| `tool_choice` | `{"mode": "auto" \| "none" \| "required" \| "function", "name": ...}` |
| `response_format` | `{"type": "text" \| "json_object" \| "json_schema", ...}` |

Output: `response`, `reasoning`, `tool_calls`, `usage`.

Each app also has an `openai` function that takes Chat Completions `messages`: `belt app sample google/gemini-4-argon -f openai`.

## Examples

### Deep Analysis with Argon

```bash
belt app run google/gemini-4-argon --input '{
  "system_prompt": "You are a securities lawyer. Cite the clause for each finding.",
  "text": "Review this term sheet for investor-unfriendly provisions",
  "files": ["https://example.com/term-sheet.pdf"],
  "reasoning_effort": "high"
}'
```

### Agentic Coding with 3.8 Flash

```bash
belt app run google/gemini-3-8-flash --input '{
  "system_prompt": "You are a Python engineer. Return a unified diff.",
  "text": "Add retry with exponential backoff to this HTTP client: <code>",
  "reasoning_effort": "medium"
}'
```

### High Throughput with Flash-Lite

```bash
belt app run google/gemini-3-5-flash-lite --input '{
  "text": "Translate to Spanish: The meeting is moved to Thursday.",
  "reasoning_effort": "none",
  "max_tokens": 100
}'
```

Run many inputs at once:

```bash
# one JSON input per line
belt app run google/gemini-3-5-flash-lite --batch inputs.jsonl
```

### Vision

```bash
belt app run google/gemini-3-8-flash --input '{
  "text": "Describe this chart and list the three largest values",
  "images": ["https://example.com/chart.png"]
}'
```

### Multi-turn Conversation

```bash
belt app run google/gemini-4-argon --input '{
  "context": [
    {"role": "user", "text": "I am planning a 5-day trip to Kyoto."},
    {"role": "assistant", "text": "Great choice. What are your interests?"}
  ],
  "text": "Temples and food. Draft an itinerary."
}'
```

### Structured Output

```bash
belt app run google/gemini-3-5-flash-lite --input '{
  "text": "Extract product and price: The Pixel case costs $29.",
  "response_format": {
    "type": "json_schema",
    "name": "product",
    "json_schema": {
      "type": "object",
      "properties": {"product": {"type": "string"}, "price": {"type": "number"}},
      "required": ["product", "price"]
    }
  }
}'
```

### Tool Use

```bash
belt app run google/gemini-3-8-flash --input '{
  "text": "Find open issues labeled bug",
  "tools": [{
    "type": "function",
    "function": {
      "name": "search_issues",
      "description": "Search the issue tracker",
      "parameters": {"type": "object", "properties": {"label": {"type": "string"}, "state": {"type": "string"}}, "required": ["label"]}
    }
  }],
  "tool_choice": {"mode": "auto"}
}'
```

## Pricing

| Model | Input | Output |
|-------|-------|--------|
| Gemini 4 Argon | $2.00/M | $10.00/M |
| Gemini 3.8 Flash | $1.50/M | $7.50/M |
| Gemini 3.5 Flash-Lite | $0.30/M | $2.50/M |

Estimate a run: `belt app estimate <app-id> --input input.json`.

## Related Skills

```bash
# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# All LLMs in one place
npx skills add inference-sh/skills@llm-models

# Other providers
npx skills add inference-sh/skills@claude
npx skills add inference-sh/skills@gpt

# Agents and tool use
npx skills add inference-sh/skills@agent-tools

# RAG pipelines
npx skills add inference-sh/skills@ai-rag-pipeline
```

Browse all apps: `belt app list`

## Documentation

- [Agents Overview](https://inference.sh/docs/concepts/agents) - Building AI agents
- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
