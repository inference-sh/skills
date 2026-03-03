# inference.sh skills — ai agent capabilities for claude code, copilot & more

![inference.sh](https://cloud.inference.sh/app/files/u/4mg21r6ta37mpaz6ktzwtt8krr/01kgvqa60jjrqa47j3g5s6ce6v.jpeg)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

ai agent skills for running 150+ ai models via the [inference.sh](https://inference.sh) cli — the ai agent runtime for serverless ai inference.

compatible with claude code, github copilot, and other ai coding assistants. generate images with flux, create videos with veo, call llms like claude and gpt, search the web with tavily, and more.

## Installation

### Option 1: Install All Skills

```bash
npx skills add inference-sh/skills
```

### Option 2: Install Specific Skills

```bash
# Image generation
npx skills add inference-sh/skills@ai-image-generation
npx skills add inference-sh/skills@flux-image

# Video generation
npx skills add inference-sh/skills@ai-video-generation
npx skills add inference-sh/skills@google-veo
npx skills add inference-sh/skills@ai-avatar-video

# LLMs (Claude, Gemini, Kimi, etc.)
npx skills add inference-sh/skills@llm-models

# Web search (Tavily, Exa)
npx skills add inference-sh/skills@web-search

# Utilities
npx skills add inference-sh/skills@image-upscaling
npx skills add inference-sh/skills@background-removal

# Social
npx skills add inference-sh/skills@twitter-automation
```

### Manual Installation

Copy to your skills directory:

```bash
# Claude Code
cp -r tools/* ui/* sdk/* guides/* ~/.claude/skills/

# GitHub Copilot
cp -r tools/* ui/* sdk/* guides/* ~/.copilot/skills/

# Project-level
cp -r tools/* ui/* sdk/* guides/* .claude/skills/
```

## Quick Start

```bash
# Install CLI
curl -fsSL https://cli.inference.sh | sh

# Login
infsh login

# Generate an image
infsh app run falai/flux-dev-lora --input '{"prompt": "a cat astronaut"}'

# Generate a video
infsh app run google/veo-3-1-fast --input '{"prompt": "drone over mountains"}'

# Call Claude
infsh app run openrouter/claude-sonnet-45 --input '{"prompt": "Hello world"}'

# Web search
infsh app run tavily/search-assistant --input '{"query": "latest AI news"}'

# Post to Twitter
infsh app run x/post-tweet --input '{"text": "Hello from AI!"}'
```

## Task Tracking

When you run an app, the CLI shows the task ID. For long-running tasks:

```bash
# Run without waiting
infsh app run google/veo-3 --input input.json --no-wait

# Check task status anytime
infsh task get <task-id>
```

## Repository Structure

```
skills/
├── tools/                    # App & model running skills
│   ├── image/                # Image generation & processing
│   ├── video/                # Video generation
│   ├── audio/                # Audio & speech
│   ├── llm/                  # Language models & search
│   ├── social/               # Social media automation
│   ├── utilities/            # Browser, executor, discovery
│   └── agent-tools/          # Core agent capabilities
├── sdk/                      # SDK documentation
│   ├── javascript-sdk/
│   └── python-sdk/
├── ui/                       # UI components
│   ├── agent-ui/
│   ├── chat-ui/
│   ├── tools-ui/
│   └── widgets-ui/
└── guides/                   # How-to guides & workflows
    ├── content/              # Content pipelines
    ├── design/               # Visual design
    ├── photo/                # Photography
    ├── product/              # Product marketing
    ├── prompting/            # Prompt engineering
    ├── social/               # Social media content
    ├── video/                # Video production
    └── writing/              # Written content
```

## Available Skills

### Tools

| Skill | Description | Path |
|-------|-------------|------|
| **ai-image-generation** | 50+ image models | `tools/image/` |
| **ai-video-generation** | 40+ video models | `tools/video/` |
| **llm-models** | Claude, Gemini, Kimi, GLM | `tools/llm/` |
| **web-search** | Tavily, Exa search | `tools/llm/` |
| **twitter-automation** | X/Twitter API | `tools/social/` |
| **flux-image** | FLUX models | `tools/image/` |
| **google-veo** | Google Veo | `tools/video/` |
| **ai-avatar-video** | Talking heads | `tools/video/` |
| **image-upscaling** | Upscalers | `tools/image/` |
| **background-removal** | BG removal | `tools/image/` |

### UI Components

| Skill | Description |
|-------|-------------|
| **agent-ui** | Full agent interface |
| **chat-ui** | Chat components |
| **tools-ui** | Tool call/result components |
| **widgets-ui** | Widget renderer |

### SDKs

| Skill | Description |
|-------|-------------|
| **javascript-sdk** | JS/TS SDK with streaming, tools, React |
| **python-sdk** | Python SDK with async, streaming |

### Guides

| Category | Skills |
|----------|--------|
| **prompting** | prompt-engineering, video-prompting-guide |
| **writing** | technical-blog, case-study, press-release, seo, newsletter |
| **design** | landing-page, email, og-image, youtube-thumbnail, logo, etc. |
| **video** | storyboard, explainer, video-ads, talking-head |
| **social** | linkedin, twitter-thread, carousel, social-content |
| **product** | competitor-teardown, customer-persona, changelog, launch |

## 150+ AI Apps

| Category | Examples |
|----------|----------|
| **Image** | FLUX, Gemini 3 Pro, Grok Imagine, Seedream 4.5, Reve, Topaz |
| **Video** | Veo 3.1, Seedance, Wan 2.5, OmniHuman, Fabric, HunyuanVideo |
| **LLMs** | Claude Opus/Sonnet/Haiku, Gemini 3 Pro, Kimi K2, GLM-4.6 |
| **Search** | Tavily Search, Tavily Extract, Exa Search, Exa Answer |
| **Social** | Twitter/X posting, DMs, likes, retweets, follows |

Browse all apps:

```bash
infsh app list
infsh app list --category image
infsh app list --category video
infsh app list --category audio
infsh app list --search "flux"
```

## Documentation

- [Getting Started](https://inference.sh/docs/getting-started/introduction) - Introduction to inference.sh
- [What is inference.sh?](https://inference.sh/docs/getting-started/what-is-inference) - Platform overview
- [Apps Overview](https://inference.sh/docs/apps/overview) - Understanding the app ecosystem
- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
- [CLI Setup](https://inference.sh/docs/extend/cli-setup) - Installing the CLI
- [API & SDK](https://inference.sh/docs/api/overview) - Programmatic access

### Blog

- [Agent Skills Overview](https://inference.sh/blog/skills/skills-overview) - The open standard for AI capabilities
- [Workflows vs Agents](https://inference.sh/blog/concepts/workflows-vs-agents) - When to use each
- [Why Agent Runtimes Matter](https://inference.sh/blog/agent-runtime/why-runtimes-matter) - Runtime benefits
- [Building a Research Agent](https://inference.sh/blog/guides/research-agent) - LLM + search integration
- [From Demo to Production](https://inference.sh/blog/guides/demo-to-production) - Production best practices

## Links

- **Website**: [inference.sh](https://inference.sh)
- **App Store**: [app.inference.sh](https://app.inference.sh)
- **Docs**: [inference.sh/docs](https://inference.sh/docs)
- **Blog**: [inference.sh/blog](https://inference.sh/blog)
- **CLI Install**: `curl -fsSL https://cli.inference.sh | sh`
