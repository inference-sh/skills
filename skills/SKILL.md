---
name: inference-sh
description: |
  Run 150+ AI apps via inference.sh CLI - image generation, video creation, LLMs, search, 3D, Twitter automation.
  Models: FLUX, Veo, Gemini, Grok, Claude, Seedance, OmniHuman, Tavily, Exa, OpenRouter, and many more.
  Use when running AI apps, generating images/videos, calling LLMs, web search, or automating Twitter.
  Triggers: inference.sh, infsh, ai model, run ai, serverless ai, ai api, flux, veo, claude api,
  image generation, video generation, openrouter, tavily, exa search, twitter api, grok
allowed-tools: Bash(infsh *)
---

# inference.sh

Run 150+ AI apps in the cloud with a simple CLI. No GPU required.

## Install CLI

```bash
curl -fsSL https://cli.inference.sh | sh
infsh login
```

## Quick Examples

```bash
# Generate an image
infsh app run falai/flux-dev-lora --input '{"prompt": "a cat astronaut"}'

# Generate a video
infsh app run google/veo-3-1-fast --input '{"prompt": "drone over mountains"}'

# Call Claude
infsh app run openrouter/claude-sonnet-45 --input '{"prompt": "Explain quantum computing"}'

# Web search
infsh app run tavily/search-assistant --input '{"query": "latest AI news"}'

# Post to Twitter
infsh app run x/post-tweet --input '{"text": "Hello from AI!"}'

# Generate 3D model
infsh app run infsh/rodin-3d-generator --input '{"prompt": "a wooden chair"}'
```

## Commands

| Task | Command |
|------|---------|
| List all apps | `infsh app list` |
| Search apps | `infsh app list --search "flux"` |
| Filter by category | `infsh app list --category image` |
| Get app details | `infsh app get google/veo-3-1-fast` |
| Generate sample input | `infsh app sample google/veo-3-1-fast --save input.json` |
| Run app | `infsh app run google/veo-3-1-fast --input input.json` |
| Run without waiting | `infsh app run <app> --input input.json --no-wait` |
| Check task status | `infsh task get <task-id>` |

## What's Available

| Category | Examples |
|----------|----------|
| **Image** | FLUX, Gemini 3 Pro, Grok Imagine, Seedream 4.5, Reve, Topaz Upscaler |
| **Video** | Veo 3.1, Seedance 1.5, Wan 2.5, OmniHuman, Fabric, HunyuanVideo Foley |
| **LLMs** | Claude Opus/Sonnet/Haiku, Gemini 3 Pro, Kimi K2, GLM-4, any OpenRouter model |
| **Search** | Tavily Search, Tavily Extract, Exa Search, Exa Answer, Exa Extract |
| **3D** | Rodin 3D Generator |
| **Twitter/X** | post-tweet, post-create, dm-send, user-follow, post-like, post-retweet |
| **Utilities** | Media merger, caption videos, image stitching, audio extraction |

## Specialized Skills

Install focused skills for specific tasks:

```bash
# Image generation (FLUX, Gemini, Grok, Seedream, Reve)
npx skills add inference-sh/skills@ai-image-generation

# Video generation (Veo, Seedance, Wan, OmniHuman)
npx skills add inference-sh/skills@ai-video-generation

# LLMs via OpenRouter (Claude, Gemini, Kimi, GLM)
npx skills add inference-sh/skills@llm-models

# Web search (Tavily, Exa)
npx skills add inference-sh/skills@web-search

# AI avatars & lipsync (OmniHuman, Fabric, PixVerse)
npx skills add inference-sh/skills@ai-avatar-video

# Twitter/X automation
npx skills add inference-sh/skills@twitter-automation

# Model-specific skills
npx skills add inference-sh/skills@flux-image
npx skills add inference-sh/skills@google-veo

# Utility skills
npx skills add inference-sh/skills@image-upscaling
npx skills add inference-sh/skills@background-removal
```

## Reference

- [Authentication & Setup](inference-sh/references/authentication.md)
- [Discovering Apps](inference-sh/references/app-discovery.md)
- [Running Apps](inference-sh/references/running-apps.md)
- [CLI Reference](inference-sh/references/cli-reference.md)
