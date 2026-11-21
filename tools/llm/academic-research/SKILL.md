---
name: academic-research
description: "Search and fetch academic papers and archives via inference.sh CLI. Apps: arXiv Search, arXiv Paper, bioRxiv Search, bioRxiv Paper, ChemRxiv Search, ChemRxiv Paper, Internet Archive Search, Metadata, Download, Wayback Machine, You.com Search, Deep Research, Contents, Finance Research. Capabilities: preprint search with field prefixes and category filters, paper metadata and PDF download, medRxiv lookup, archive.org items, Wayback snapshots, cited deep research. Use for: literature review, paper discovery, citation lookup, research agents, summarizing papers, historical web pages. Triggers: arxiv, arxiv search, biorxiv, medrxiv, chemrxiv, preprint, research paper, academic search, literature search, paper summary, internet archive, archive.org, wayback machine, deep research, you.com, scholarly search, scientific papers"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# Academic Research

Search preprint servers, the Internet Archive and the web via [inference.sh](https://inference.sh) CLI.

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

# Search arXiv
belt app run arxiv/search --input '{"query": "ti:diffusion AND cat:cs.CV", "max_results": 5}'
```

## Available Apps

### Preprint Servers

| App | App ID | Description |
|-----|--------|-------------|
| arXiv Search | `arxiv/search` | Query with field prefixes, boolean operators, category filter, sorting |
| arXiv Paper | `arxiv/paper` | Full metadata for one arXiv ID, optional PDF download |
| bioRxiv Search | `biorxiv/search` | bioRxiv and medRxiv preprints by date range, optional category |
| bioRxiv Paper | `biorxiv/paper` | One bioRxiv or medRxiv paper by DOI, optional PDF download |
| ChemRxiv Search | `chemrxiv/search` | ChemRxiv preprints by keyword (via Crossref) |
| ChemRxiv Paper | `chemrxiv/paper` | One ChemRxiv paper by DOI (via Crossref) |

### Internet Archive

| App | App ID | Description |
|-----|--------|-------------|
| Archive Search | `archive/search` | Search archive.org with Lucene-like syntax |
| Archive Metadata | `archive/metadata` | Item metadata and file list |
| Archive Download | `archive/download` | Download one file from an item |
| Wayback | `archive/wayback` | Check whether a URL has a Wayback Machine snapshot |

### You.com

| App | App ID | Description |
|-----|--------|-------------|
| Web Search | `you/search` | Web search with snippets and domain filters |
| Deep Research | `you/research` | Multi-step web research with cited sources |
| Contents | `you/contents` | Clean markdown or HTML from up to 10 URLs |
| Finance Research | `you/finance-research` | Financial research with filings and macro data |

## Examples

### arXiv Search

`query` supports field prefixes `ti:`, `au:`, `abs:`, `cat:`, `all:` and the operators `AND`, `OR`, `ANDNOT`.

```bash
belt app run arxiv/search --input '{
  "query": "abs:\"retrieval augmented generation\" ANDNOT ti:survey",
  "category": "cs.CL",
  "sort_by": "submittedDate",
  "sort_order": "descending",
  "max_results": 20
}'
```

`sort_by`: `relevance`, `lastUpdatedDate`, `submittedDate`. Paginate with `start`.

### arXiv Paper (with PDF)

```bash
belt app run arxiv/paper --input '{"arxiv_id": "2301.07041", "fetch_pdf": true}'
```

Returns `paper` metadata (title, authors, abstract, links) and `pdf` when requested.

### bioRxiv / medRxiv Search

Search is by date range, not keyword. Filter with `category`.

```bash
belt app run biorxiv/search --input '{
  "start_date": "2026-10-01",
  "end_date": "2026-10-31",
  "category": "neuroscience",
  "server": "biorxiv"
}'
```

Set `"server": "medrxiv"` for medRxiv. Paginate with the returned `cursor`.

### bioRxiv Paper

```bash
belt app run biorxiv/paper --input '{"doi": "10.1101/2023.01.01.123456", "fetch_pdf": true}'
```

### ChemRxiv

```bash
belt app run chemrxiv/search --input '{"query": "metal organic frameworks CO2 capture", "rows": 10, "sort": "published-desc"}'

belt app run chemrxiv/paper --input '{"doi": "10.26434/chemrxiv-2023-fw8n4"}'
```

`sort`: `relevance`, `published-desc`, `published-asc`.

### Internet Archive

```bash
# Search items (mediatype: texts, movies, audio, software, image, ...)
belt app run archive/search --input '{"query": "darwin origin of species", "mediatype": "texts", "sort": "downloads desc", "rows": 5}'

# Item metadata and file list
belt app run archive/metadata --input '{"identifier": "greatgatsby00fitzgerald"}'

# Download one file from the item (use a filename from the metadata file list)
belt app run archive/download --input '{"identifier": "<identifier>", "filename": "<filename>"}'

# Wayback snapshot (timestamp format YYYYMMDDhhmmss, optional)
belt app run archive/wayback --input '{"url": "example.com", "timestamp": "20150101000000"}'
```

### You.com Search and Contents

```bash
belt app run you/search --input '{
  "query": "protein structure prediction benchmarks",
  "count": 10,
  "include_domains": ["nature.com", "science.org"],
  "recent_past_month": true
}'

belt app run you/contents --input '{"urls": ["https://arxiv.org/abs/2301.07041"], "format": "markdown"}'
```

### You.com Deep Research

```bash
belt app run you/research --input '{
  "query": "What are the main approaches to long-context evaluation in LLMs since 2025?",
  "research_effort": "deep"
}'
```

`research_effort`: `lite`, `standard` (default), `deep`, `exhaustive`. Returns `answer` and `sources`.

### You.com Finance Research

```bash
belt app run you/finance-research --input '{
  "query": "Compare R&D spending trends of the top three semiconductor companies over the last 5 years",
  "research_effort": "deep"
}'
```

`research_effort`: `deep` (default), `exhaustive`.

## Workflow: Search Papers + Summarize

```bash
# 1. Find recent papers
belt app run arxiv/search --input '{
  "query": "abs:\"speculative decoding\"",
  "sort_by": "submittedDate",
  "max_results": 10
}' --save papers.json

# 2. Pull full metadata for the most relevant one
belt app run arxiv/paper --input '{"arxiv_id": "<arxiv-id-from-results>"}' --save paper.json

# 3. Summarize with Claude Sonnet
belt app run anthropic/claude-sonnet-5-5 --input '{
  "system_prompt": "You are a research assistant. Cite arXiv IDs for every claim.",
  "text": "Summarize the main methods, results and open problems across these papers:\n\n<papers.json results>"
}'
```

For PDFs, pass the downloaded file to the LLM's `files` input instead of pasting text.

## Pricing

| App | Pricing |
|-----|---------|
| `you/search` | $0.005/call |
| `you/research` | $0.001-$0.15/call (pass-through) |
| `you/finance-research` | $0.11/request |
| `you/contents` | Pass-through at upstream cost |

Check any app with `belt app pricing <app-id>`.

## Related Skills

```bash
# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# Web search (Tavily, Exa)
npx skills add inference-sh/skills@web-search

# RAG pipelines (search + LLM)
npx skills add inference-sh/skills@ai-rag-pipeline

# Claude for summarization
npx skills add inference-sh/skills@claude

# All LLMs
npx skills add inference-sh/skills@llm-models
```

Browse all apps: `belt app list`

## Documentation

- [Adding Tools to Agents](https://inference.sh/docs/agents/adding-tools) - Equip agents with search
- [Building a Research Agent](https://inference.sh/blog/guides/research-agent) - LLM + search integration guide
