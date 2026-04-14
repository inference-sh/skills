# inference.sh skills

AI agent skills for 250+ models via [inference.sh](https://inference.sh) CLI. Generate images, videos, call LLMs, search the web, and more.

![inference.sh](https://cloud.inference.sh/app/files/u/4mg21r6ta37mpaz6ktzwtt8krr/01kgvqa60jjrqa47j3g5s6ce6v.jpeg)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## This fork: Prospect 360 B2B and the full inference.sh catalog

This repo (**[rafaelgrasa/skills](https://github.com/rafaelgrasa/skills)**) is a public fork of [`inference-sh/skills`](https://github.com/inference-sh/skills). It contains **the same skill catalog as upstream** plus the extra skill **[prospect-360-b2b](tools/llm/prospect-360-b2b/)** (deep B2B prospect research and sales meeting prep).

Pick **one** of the install paths below:

| Option | What you get | Command(s) |
|--------|----------------|------------|
| **A — Recommended** | **All inference.sh skills + Prospect 360** (everything in this fork) | `npx skills add rafaelgrasa/skills -g -y` |
| **B — Minimal** | **Only Prospect 360** + its required web-search dependency | `npx skills add rafaelgrasa/skills@prospect-360-b2b -g -y`<br>`npx skills add inference-sh/skills@web-search -g -y` |
| **C — Upstream only** | Official catalog **without** Prospect 360 (until it lands in `inference-sh/skills`) | `npx skills add inference-sh/skills -g -y` |

- **Option A** clones this fork once and installs **every** bundled skill (same coverage as `inference-sh/skills`, plus **prospect-360-b2b**).
- **Option B** is best if you only want Prospect 360; the second line installs **web-search** from the official repo (required by Prospect 360).
- **Option C** is the standard upstream install; use **A** or **B** if you need Prospect 360 today.

More detail: **[tools/llm/prospect-360-b2b/README.md](tools/llm/prospect-360-b2b/README.md)**.

---

## Contents

- [This fork: Prospect 360 B2B and the full inference.sh catalog](#this-fork-prospect-360-b2b-and-the-full-inferencesh-catalog)
- [Install as Claude Code Plugin](#claude-code-plugin)
- [Install as Skills](#install-as-skills)
- [CLI Setup](#cli-setup)
- [Available Skills](#available-skills)
- [Documentation](#documentation)

---

## Claude Code Plugin

Install all skills as a Claude Code plugin:

```bash
/plugin install inference-sh
```

Or add from GitHub:

```bash
/plugin marketplace add inference-sh/skills
/plugin install inference-sh@inference-sh-skills
```

After install, skills are available as `/inference-sh:flux-image`, `/inference-sh:google-veo`, etc.

---

## Install as Skills

### All skills

**From this fork (inference.sh catalog + Prospect 360):**

```bash
npx skills add rafaelgrasa/skills -g -y
```

**From upstream only (no Prospect 360 until merged):**

```bash
npx skills add inference-sh/skills -g -y
```

### Specific skills

**Prospect 360 only** (use this fork’s skill id until it exists on `inference-sh/skills`):

```bash
npx skills add rafaelgrasa/skills@prospect-360-b2b -g -y
npx skills add inference-sh/skills@web-search -g -y
```

**Other examples** (official repo):

```bash
npx skills add inference-sh/skills@flux-image
npx skills add inference-sh/skills@google-veo
npx skills add inference-sh/skills@llm-models
npx skills add inference-sh/skills@web-search
```

After **prospect-360-b2b** is merged into `inference-sh/skills`, you may also use:

```bash
npx skills add inference-sh/skills@prospect-360-b2b -g -y
```

### Manual

```bash
cp -r tools/* ui/* sdk/* guides/* ~/.claude/skills/
```

---

## CLI Setup

> Requires inference.sh CLI (`infsh`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
infsh login
```

Browse apps: `infsh app list`

---

## Available Skills

### Tools

| Skill | Description |
|-------|-------------|
| [ai-image-generation](tools/image/) | 50+ image models (FLUX, Gemini, Reve, etc.) |
| [ai-video-generation](tools/video/) | 40+ video models (Veo, Seedance, Wan, etc.) |
| [llm-models](tools/llm/) | Claude, Gemini, Kimi, GLM |
| [prospect-360-b2b](tools/llm/prospect-360-b2b/) | B2B prospect research & sales meeting prep |
| [web-search](tools/llm/) | Tavily, Exa search |
| [twitter-automation](tools/social/) | X/Twitter API |

### SDKs

| Skill | Description |
|-------|-------------|
| [javascript-sdk](sdk/javascript-sdk/) | JS/TS SDK with streaming, tools, React |
| [python-sdk](sdk/python-sdk/) | Python SDK with async, streaming |

### UI Components

| Skill | Description |
|-------|-------------|
| [agent-ui](ui/agent-ui/) | Full agent interface |
| [chat-ui](ui/chat-ui/) | Chat components |
| [tools-ui](ui/tools-ui/) | Tool call/result rendering |

### Guides

| Category | Topics |
|----------|--------|
| [prompting](guides/prompting/) | Prompt engineering, video prompts |
| [design](guides/design/) | Landing pages, thumbnails, logos |
| [video](guides/video/) | Storyboards, explainers, ads |
| [writing](guides/writing/) | Blogs, case studies, newsletters |
| [social](guides/social/) | LinkedIn, Twitter threads, carousels |
| [product](guides/product/) | Competitor analysis, personas, launches |

---

## Documentation

- [Getting Started](https://inference.sh/docs/getting-started/introduction)
- [Running Apps](https://inference.sh/docs/apps/running)
- [CLI Setup](https://inference.sh/docs/extend/cli-setup)
- [API & SDK](https://inference.sh/docs/api/overview)

**Links:** [Website](https://inference.sh) | [App Store](https://app.inference.sh) | [Docs](https://inference.sh/docs) | [Blog](https://inference.sh/blog)
