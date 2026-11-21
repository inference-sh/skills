---
name: claude
description: "Anthropic Claude models via inference.sh CLI. Models: Claude Fable 5.1, Claude Opus 5.5, Claude Sonnet 5.5, Claude Haiku 5.5, plus Claude Code variants that run on your own Claude Pro/Max plan. 1M context, 128k output, thinking with effort, vision, tool use, structured output. Use for: coding, agents, reasoning, long documents, summarization, image understanding, chat. Triggers: claude, anthropic, claude api, claude fable, claude opus, claude sonnet, claude haiku, opus 5.5, sonnet 5.5, haiku 5.5, fable 5.1, claude code, claude pro, claude max, setup-token, anthropic api, claude subscription"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# Claude

Run Anthropic's Claude models via [inference.sh](https://inference.sh) CLI.

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

belt app run anthropic/claude-sonnet-5-5 --input '{"text": "Explain how a B-tree works"}'
```

## Models

| Model | App ID | Best For |
|-------|--------|----------|
| Claude Fable 5.1 | `anthropic/claude-fable-5-1` | Most capable: demanding reasoning, long-horizon agents |
| Claude Opus 5.5 | `anthropic/claude-opus-5-5` | Long-running agentic coding and knowledge work |
| Claude Sonnet 5.5 | `anthropic/claude-sonnet-5-5` | Fast frontier model for coding, agents, everyday work |
| Claude Haiku 5.5 | `anthropic/claude-haiku-5-5` | Fastest, lowest cost, high-volume and latency-sensitive work |

All four: 1M context, 128k output, vision, tool use, direct Anthropic API. Fable 5.1 has always-on thinking with effort; Opus and Sonnet have adaptive thinking with effort; Haiku has adaptive thinking.

### Claude Code (your own plan)

| Model | App ID |
|-------|--------|
| Claude Fable 5.1 (Claude Code) | `anthropic/claude-code-fable-5-1` |
| Claude Opus 5.5 (Claude Code) | `anthropic/claude-code-opus-5-5` |
| Claude Sonnet 5.5 (Claude Code) | `anthropic/claude-code-sonnet-5-5` |
| Claude Haiku 5.5 (Claude Code) | `anthropic/claude-code-haiku-5-5` |

These run the same models through Claude Code on your own Claude Pro or Max plan. Usage counts against your Claude plan, not inference.sh credits. Same inputs as the direct API apps.

Older versions (Opus 5, Opus 4.x, Sonnet 5, Haiku 4.5 and their Claude Code variants) are still available: `belt app search claude`.

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

Each app also has an `openai` function (OpenAI Chat Completions compatible, takes `messages`): `belt app sample anthropic/claude-sonnet-5-5 -f openai`.

## Examples

### Coding with Opus

```bash
belt app run anthropic/claude-opus-5-5 --input '{
  "system_prompt": "You are a senior Go engineer. Return only code.",
  "text": "Write a rate limiter using a token bucket with tests",
  "reasoning_effort": "high"
}'
```

### Hard Reasoning with Fable

```bash
belt app run anthropic/claude-fable-5-1 --input '{
  "text": "Design a migration plan to split a monolith Postgres schema into three services with zero downtime",
  "reasoning_effort": "high"
}'
```

### High Volume with Haiku

```bash
belt app run anthropic/claude-haiku-5-5 --input '{
  "text": "Classify the sentiment of this review as positive, negative or neutral: The battery died after two days.",
  "max_tokens": 10
}'
```

### Vision

```bash
belt app run anthropic/claude-sonnet-5-5 --input '{
  "text": "What is wrong in this UI screenshot?",
  "images": ["https://example.com/screenshot.png"]
}'
```

### Multi-turn Conversation

```bash
belt app run anthropic/claude-sonnet-5-5 --input '{
  "context": [
    {"role": "user", "text": "My name is Ada and I write Rust."},
    {"role": "assistant", "text": "Nice to meet you, Ada."}
  ],
  "text": "Suggest a weekend project for me."
}'
```

### Structured Output

```bash
belt app run anthropic/claude-sonnet-5-5 --input '{
  "text": "Extract the company and amount: Acme Corp raised $12M in Series A.",
  "response_format": {
    "type": "json_schema",
    "name": "funding",
    "json_schema": {
      "type": "object",
      "properties": {"company": {"type": "string"}, "amount": {"type": "string"}},
      "required": ["company", "amount"]
    }
  }
}'
```

### Tool Use

```bash
belt app run anthropic/claude-opus-5-5 --input '{
  "text": "What is the weather in Paris?",
  "tools": [{
    "type": "function",
    "function": {
      "name": "get_weather",
      "description": "Get current weather for a city",
      "parameters": {"type": "object", "properties": {"city": {"type": "string"}}, "required": ["city"]}
    }
  }],
  "tool_choice": {"mode": "auto"}
}'
```

The call comes back in `tool_calls`. Send the result as a `context` message with `"role": "tool"` and `tool_call_id`.

## Claude Code Setup

1. Install Claude Code and log in with your Claude account.
2. Run `claude setup-token` and copy the token (starts with `sk-ant-oat01-`, valid one year).
3. Save it on inference.sh:

```bash
belt credentials connect claude-code
```

4. Run any Claude Code app:

```bash
belt app run anthropic/claude-code-opus-5-5 --input '{"text": "Refactor this function to be iterative: <code>"}'
```

The token is used only for your own runs, never shared with your workspace. Check how much of your plan is used (5-hour and weekly windows, with reset times):

```bash
belt app run anthropic/claude-code-opus-5-5 --function usage --input '{}'
```

`usage` has no inference.sh charge; it sends one token as a quota check against your plan.

## Pricing

| Model | Input | Output | Cache read | Cache write |
|-------|-------|--------|------------|-------------|
| Fable 5.1 | $10.00/M | $50.00/M | $0.25/M | $12.50/M |
| Opus 5.5 | $4.00/M | $20.00/M | $0.20/M | $5.00/M |
| Sonnet 5.5 | $2.00/M | $10.00/M | $0.20/M | $2.50/M |
| Haiku 5.5 (<=100K tokens) | $0.10/M | $0.50/M | - | - |
| Haiku 5.5 (>100K tokens) | $0.50/M | $2.50/M | - | - |

Claude Code apps bill your Claude plan. Estimate a run: `belt app estimate <app-id> --input input.json`.

## Related Skills

```bash
# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# All LLMs in one place
npx skills add inference-sh/skills@llm-models

# Other providers
npx skills add inference-sh/skills@gpt
npx skills add inference-sh/skills@gemini

# Agents and tool use
npx skills add inference-sh/skills@agent-tools

# Search + summarize papers with Claude
npx skills add inference-sh/skills@academic-research
```

Browse all apps: `belt app list`

## Documentation

- [Claude Code Subscription](https://inference.sh/docs/integrations/claude-code-subscription) - Using your own Claude plan
- [Agents Overview](https://inference.sh/docs/concepts/agents) - Building AI agents
