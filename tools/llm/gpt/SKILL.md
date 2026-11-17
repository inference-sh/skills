---
name: gpt
description: "OpenAI GPT models via inference.sh CLI. Models: GPT-6 Astra (frontier), GPT-6.1 Sol (near-Astra at lower cost), GPT-6 Luna (efficient, high-volume). 1M context, 128k output, reasoning effort, vision, file input, tool use, structured output. Daybreak variants for vetted cybersecurity work on your own OpenAI key. Use for: coding, agents, reasoning, document analysis, extraction, chat, high-volume classification. Triggers: gpt, openai, gpt-6, gpt 6, gpt-6 astra, gpt-6.1 sol, gpt-6 luna, openai api, chatgpt api, gpt api, openai model, daybreak, openai daybreak, gpt reasoning"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# GPT

Run OpenAI's GPT-6 models via [inference.sh](https://inference.sh) CLI.

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

belt app run openai/gpt-6-1-sol --input '{"text": "Explain the CAP theorem with an example"}'
```

## Models

| Model | App ID | Best For |
|-------|--------|----------|
| GPT-6 Astra | `openai/gpt-6-astra` | OpenAI's frontier model |
| GPT-6.1 Sol | `openai/gpt-6-1-sol` | Near-Astra quality at a fraction of the cost |
| GPT-6 Luna | `openai/gpt-6-luna` | Most efficient, focused high-volume tasks |

All three: 1M context, 128k output, reasoning, vision, file input and tool use via the OpenAI Responses API. Direct API.

### Daybreak Variants

`openai/gpt-6-astra-daybreak`, `openai/gpt-6-1-sol-daybreak`, `openai/gpt-6-luna-daybreak` are for security teams in OpenAI's Daybreak program (Trusted Access for Cyber). They run on your own Daybreak-enabled OpenAI project key, and OpenAI bills your organization directly. Reduced refusals require OpenAI approval (Red for Astra and Sol; Blue or Red for Luna). Connect the key once per workspace:

```bash
belt credentials connect openai-daybreak --team
```

Older GPT versions: `belt app search gpt`.

## Inputs

| Field | Description |
|-------|-------------|
| `text` | The user message |
| `system_prompt` | System instructions |
| `images` | Image files or URLs (vision) |
| `files` | Document files |
| `context` | Prior messages: `[{"role": "user" \| "assistant", "text": "..."}]` |
| `reasoning_effort` | `none` (default), `low`, `medium`, `high` |
| `temperature` | Default 0.7 |
| `max_tokens` | Default 64000 |
| `tools` | Function tool definitions |
| `tool_choice` | `{"mode": "auto" \| "none" \| "required" \| "function", "name": ...}` |
| `response_format` | `{"type": "text" \| "json_object" \| "json_schema", ...}` |

Output: `response`, `reasoning`, `tool_calls`, `usage`.

Each app also has an `openai` function that takes Chat Completions `messages` directly: `belt app sample openai/gpt-6-astra -f openai`.

## Examples

### Frontier Reasoning with Astra

```bash
belt app run openai/gpt-6-astra --input '{
  "text": "Prove that there are infinitely many primes of the form 4k+3",
  "reasoning_effort": "high"
}'
```

### Everyday Coding with Sol

```bash
belt app run openai/gpt-6-1-sol --input '{
  "system_prompt": "You are a TypeScript expert. Return only code.",
  "text": "Write a debounce hook for React with cancel support",
  "reasoning_effort": "medium"
}'
```

### High Volume with Luna

```bash
belt app run openai/gpt-6-luna --input '{
  "text": "Tag this support ticket as billing, bug, or feature-request: I was charged twice this month.",
  "reasoning_effort": "none",
  "max_tokens": 10
}'
```

### Document Analysis

```bash
belt app run openai/gpt-6-1-sol --input '{
  "text": "List every payment obligation and its due date in this contract",
  "files": ["https://example.com/contract.pdf"]
}'
```

### Vision

```bash
belt app run openai/gpt-6-astra --input '{
  "text": "Transcribe the handwritten notes in this photo",
  "images": ["https://example.com/notes.jpg"]
}'
```

### Structured Output

```bash
belt app run openai/gpt-6-luna --input '{
  "text": "Extract name and email: Contact Jane Doe at jane@example.com",
  "response_format": {
    "type": "json_schema",
    "name": "contact",
    "json_schema": {
      "type": "object",
      "properties": {"name": {"type": "string"}, "email": {"type": "string"}},
      "required": ["name", "email"]
    }
  }
}'
```

### Tool Use

```bash
belt app run openai/gpt-6-1-sol --input '{
  "text": "Book a table for two at 7pm",
  "tools": [{
    "type": "function",
    "function": {
      "name": "book_table",
      "description": "Reserve a restaurant table",
      "parameters": {
        "type": "object",
        "properties": {"party_size": {"type": "integer"}, "time": {"type": "string"}},
        "required": ["party_size", "time"]
      }
    }
  }],
  "tool_choice": {"mode": "auto"}
}'
```

### Batch Inputs

```bash
# one JSON input per line
belt app run openai/gpt-6-luna --batch inputs.jsonl
```

## Pricing

| Model | Input | Cached input | Output |
|-------|-------|--------------|--------|
| GPT-6 Astra | $10.00/M | $1.00/M | $50.00/M |
| GPT-6.1 Sol | $2.00/M | $0.10/M | $10.00/M |
| GPT-6 Luna | $0.10/M | $0.01/M | $0.50/M |

Daybreak variants bill your OpenAI organization. Estimate a run: `belt app estimate <app-id> --input input.json`.

## Related Skills

```bash
# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# All LLMs in one place
npx skills add inference-sh/skills@llm-models

# Other providers
npx skills add inference-sh/skills@claude
npx skills add inference-sh/skills@gemini

# Agents and tool use
npx skills add inference-sh/skills@agent-tools

# Web search for grounding
npx skills add inference-sh/skills@web-search
```

Browse all apps: `belt app list`

## Documentation

- [Agents Overview](https://inference.sh/docs/concepts/agents) - Building AI agents
- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
