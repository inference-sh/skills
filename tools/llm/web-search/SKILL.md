---
name: web-search
description: "Web search and content extraction with Tavily and Exa via inference.sh CLI. Apps: Tavily Search, Tavily Extract, Exa Search, Exa Answer, Exa Extract. Capabilities: AI-powered search, content extraction, direct answers, research. Use for: research, RAG pipelines, fact-checking, content aggregation, agents. Triggers: web search, tavily, exa, search api, content extraction, research, internet search, ai search, search assistant, web scraping, rag, perplexity alternative"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# Web Search & Extraction

Search the web and extract content via [inference.sh](https://inference.sh) CLI.

![Web Search & Extraction](https://cloud.inference.sh/app/files/u/4mg21r6ta37mpaz6ktzwtt8krr/01kgndqjxd780zm2j3rmada6y8.jpeg)

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

# Search the web
belt app run tavily/search-assistant --input '{"query": "latest AI developments 2024"}'
```


## Available Apps

### Tavily

| App | App ID | Description |
|-----|--------|-------------|
| Search Assistant | `tavily/search-assistant` | AI-powered search with answers |
| Extract | `tavily/extract` | Extract content from URLs |

### Exa

| App | App ID | Description |
|-----|--------|-------------|
| Search | `exa/search` | Smart web search with AI |
| Answer | `exa/answer` | Direct factual answers |
| Extract | `exa/extract` | Extract and analyze web content |

## Examples

### Tavily Search

```bash
belt app run tavily/search-assistant --input '{
  "query": "What are the best practices for building AI agents?"
}'
```

Returns AI-generated answers with sources and images.

### Tavily Extract

```bash
belt app run tavily/extract --input '{
  "urls": ["https://example.com/article1", "https://example.com/article2"]
}'
```

Extracts clean text and images from multiple URLs.

### Exa Search

```bash
belt app run exa/search --input '{
  "query": "machine learning frameworks comparison"
}'
```

Returns highly relevant links with context.

### Exa Answer

```bash
belt app run exa/answer --input '{
  "query": "What is the population of Tokyo?"
}'
```

Returns direct factual answers.

### Exa Extract

```bash
belt app run exa/extract --input '{
  "url": "https://example.com/research-paper"
}'
```

Extracts and analyzes web page content.

## Workflow: Research + LLM

```bash
# 1. Search for information
belt app run tavily/search-assistant --input '{
  "query": "latest developments in quantum computing"
}' > search_results.json

# 2. Analyze with Claude
belt app run anthropic/claude-sonnet-5-5 --input '{
  "text": "Based on this research, summarize the key trends: <search-results>"
}'
```

## Workflow: Extract + Summarize

```bash
# 1. Extract content from URL
belt app run tavily/extract --input '{
  "urls": ["https://example.com/long-article"]
}' > content.json

# 2. Summarize with LLM
belt app run anthropic/claude-haiku-5-5 --input '{
  "text": "Summarize this article in 3 bullet points: <content>"
}'
```

## Use Cases

- **Research**: Gather information on any topic
- **RAG**: Retrieval-augmented generation
- **Fact-checking**: Verify claims with sources
- **Content aggregation**: Collect data from multiple sources
- **Agents**: Build research-capable AI agents

## Related Skills

```bash
# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# LLM models (combine with search for RAG)
npx skills add inference-sh/skills@llm-models

# Image generation
npx skills add inference-sh/skills@ai-image-generation
```

Browse all apps: `belt app list`

## Documentation

- [Adding Tools to Agents](https://inference.sh/docs/agents/adding-tools) - Equip agents with search
- [Building a Research Agent](https://inference.sh/blog/guides/research-agent) - LLM + search integration guide
- [Tool Integration Tax](https://inference.sh/blog/tools/integration-tax) - Why pre-built tools matter

