---
name: ai-rag-pipeline
description: "Build RAG (Retrieval Augmented Generation) pipelines with web search and LLMs. Tools: Tavily Search, Exa Search, Exa Answer, Claude, GPT, Gemini. Capabilities: research, fact-checking, grounded responses, knowledge retrieval. Use for: AI agents, research assistants, fact-checkers, knowledge bases. Triggers: rag, retrieval augmented generation, grounded ai, search and answer, research agent, fact checking, knowledge retrieval, ai research, search + llm, web grounded, perplexity alternative, ai with sources, citation, research pipeline"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# AI RAG Pipeline

Build RAG (Retrieval Augmented Generation) pipelines via [inference.sh](https://inference.sh) CLI.

![AI RAG Pipeline](https://cloud.inference.sh/app/files/u/4mg21r6ta37mpaz6ktzwtt8krr/01kgndqjxd780zm2j3rmada6y8.jpeg)

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

# Simple RAG: Search + LLM
SEARCH=$(belt app run tavily/search-assistant --input '{"query": "latest AI developments 2024"}')
belt app run anthropic/claude-sonnet-5-5 --input "{
  \"text\": \"Based on this research, summarize the key trends: $SEARCH\"
}"
```


## What is RAG?

RAG combines:
1. **Retrieval**: Fetch relevant information from external sources
2. **Augmentation**: Add retrieved context to the prompt
3. **Generation**: LLM generates response using the context

This produces more accurate, up-to-date, and verifiable AI responses.

## RAG Pipeline Patterns

### Pattern 1: Simple Search + Answer

```
[User Query] -> [Web Search] -> [LLM with Context] -> [Answer]
```

### Pattern 2: Multi-Source Research

```
[Query] -> [Multiple Searches] -> [Aggregate] -> [LLM Analysis] -> [Report]
```

### Pattern 3: Extract + Process

```
[URLs] -> [Content Extraction] -> [Chunking] -> [LLM Summary] -> [Output]
```

## Available Tools

### Search Tools

| Tool | App ID | Best For |
|------|--------|----------|
| Tavily Search | `tavily/search-assistant` | AI-powered search with answers |
| Exa Search | `exa/search` | Neural search, semantic matching |
| Exa Answer | `exa/answer` | Direct factual answers |

### Extraction Tools

| Tool | App ID | Best For |
|------|--------|----------|
| Tavily Extract | `tavily/extract` | Clean content from URLs |
| Exa Extract | `exa/extract` | Analyze web content |

### LLM Tools

| Model | App ID | Best For |
|-------|--------|----------|
| Claude Sonnet 5.5 | `anthropic/claude-sonnet-5-5` | Complex analysis |
| Claude Haiku 5.5 | `anthropic/claude-haiku-5-5` | Fast processing |
| GPT-6.1 Sol | `openai/gpt-6-1-sol` | General purpose |
| Gemini 4 Argon | `google/gemini-4-argon` | Long context |

## Pipeline Examples

### Basic RAG Pipeline

```bash
# 1. Search for information
SEARCH_RESULT=$(belt app run tavily/search-assistant --input '{
  "query": "What are the latest breakthroughs in quantum computing 2024?"
}')

# 2. Generate grounded response
belt app run anthropic/claude-sonnet-5-5 --input "{
  \"text\": \"You are a research assistant. Based on the following search results, provide a comprehensive summary with citations.

Search Results:
$SEARCH_RESULT

Provide a well-structured summary with source citations.\"
}"
```

### Multi-Source Research

```bash
# Search multiple sources
TAVILY=$(belt app run tavily/search-assistant --input '{"query": "electric vehicle market trends 2024"}')
EXA=$(belt app run exa/search --input '{"query": "EV market analysis latest reports"}')

# Combine and analyze
belt app run anthropic/claude-sonnet-5-5 --input "{
  \"text\": \"Analyze these research results and identify common themes and contradictions.

Source 1 (Tavily):
$TAVILY

Source 2 (Exa):
$EXA

Provide a balanced analysis with sources.\"
}"
```

### URL Content Analysis

```bash
# 1. Extract content from specific URLs
CONTENT=$(belt app run tavily/extract --input '{
  "urls": [
    "https://example.com/research-paper",
    "https://example.com/industry-report"
  ]
}')

# 2. Analyze extracted content
belt app run anthropic/claude-sonnet-5-5 --input "{
  \"text\": \"Analyze these documents and extract key insights:

$CONTENT

Provide:
1. Key findings
2. Data points
3. Recommendations\"
}"
```

### Fact-Checking Pipeline

```bash
# Claim to verify
CLAIM="AI will replace 50% of jobs by 2030"

# 1. Search for evidence
EVIDENCE=$(belt app run tavily/search-assistant --input "{
  \"query\": \"$CLAIM evidence studies research\"
}")

# 2. Verify claim
belt app run anthropic/claude-sonnet-5-5 --input "{
  \"text\": \"Fact-check this claim: '$CLAIM'

Based on the following evidence:
$EVIDENCE

Provide:
1. Verdict (True/False/Partially True/Unverified)
2. Supporting evidence
3. Contradicting evidence
4. Sources\"
}"
```

### Research Report Generator

```bash
TOPIC="Impact of generative AI on creative industries"

# 1. Initial research
OVERVIEW=$(belt app run tavily/search-assistant --input "{\"query\": \"$TOPIC overview\"}")
STATISTICS=$(belt app run exa/search --input "{\"query\": \"$TOPIC statistics data\"}")
OPINIONS=$(belt app run tavily/search-assistant --input "{\"query\": \"$TOPIC expert opinions\"}")

# 2. Generate comprehensive report
belt app run anthropic/claude-sonnet-5-5 --input "{
  \"text\": \"Generate a comprehensive research report on: $TOPIC

Research Data:
== Overview ==
$OVERVIEW

== Statistics ==
$STATISTICS

== Expert Opinions ==
$OPINIONS

Format as a professional report with:
- Executive Summary
- Key Findings
- Data Analysis
- Expert Perspectives
- Conclusion
- Sources\"
}"
```

### Quick Answer with Sources

```bash
# Use Exa Answer for direct factual questions
belt app run exa/answer --input '{
  "query": "What is the current market cap of NVIDIA?"
}'
```

## Best Practices

### 1. Query Optimization

```bash
# Bad: Too vague
"AI news"

# Good: Specific and contextual
"latest developments in large language models January 2024"
```

### 2. Context Management

```bash
# Summarize long search results before sending to LLM
SEARCH=$(belt app run tavily/search-assistant --input '{"query": "..."}')

# If too long, summarize first
SUMMARY=$(belt app run anthropic/claude-haiku-5-5 --input "{
  \"text\": \"Summarize these search results in bullet points: $SEARCH\"
}")

# Then use summary for analysis
belt app run anthropic/claude-sonnet-5-5 --input "{
  \"text\": \"Based on this research summary, provide insights: $SUMMARY\"
}"
```

### 3. Source Attribution

Always ask the LLM to cite sources:

```bash
belt app run anthropic/claude-sonnet-5-5 --input '{
  "text": "... Always cite sources in [Source Name](URL) format."
}'
```

### 4. Iterative Research

```bash
# First pass: broad search
INITIAL=$(belt app run tavily/search-assistant --input '{"query": "topic overview"}')

# Second pass: dive deeper based on findings
DEEP=$(belt app run tavily/search-assistant --input '{"query": "specific aspect from initial search"}')
```

## Pipeline Templates

### Agent Research Tool

```bash
#!/bin/bash
# research.sh - Reusable research function

research() {
  local query="$1"

  # Search
  local results=$(belt app run tavily/search-assistant --input "{\"query\": \"$query\"}")

  # Analyze
  belt app run anthropic/claude-haiku-5-5 --input "{
    \"text\": \"Summarize: $results\"
  }"
}

research "your query here"
```

## Related Skills

```bash
# Web search tools
npx skills add inference-sh/skills@web-search

# LLM models
npx skills add inference-sh/skills@llm-models

# Content pipelines
npx skills add inference-sh/skills@ai-content-pipeline

# Full platform skill
npx skills add inference-sh/skills@infsh-cli
```

Browse all apps: `belt app list`

## Documentation

- [Adding Tools to Agents](https://inference.sh/docs/agents/adding-tools) - Agent tool integration
- [Building a Research Agent](https://inference.sh/blog/guides/research-agent) - Full guide

