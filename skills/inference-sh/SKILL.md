---
name: inference-sh
description: |
  Run 100+ AI models via inference.sh CLI - image generation, video creation, TTS, music, transcription, and more.
  Use when running AI apps, generating images/videos/audio, or working with models like FLUX, Veo, Whisper, SDXL.
  Triggers: inference.sh, infsh, ai model, run ai, serverless ai, ai api
allowed-tools: Bash(infsh *)
---

# inference.sh

Run AI models in the cloud with a simple CLI. No GPU required.

## Quick Start

```bash
# Install CLI
curl -fsSL https://cli.inference.sh | sh

# Login
infsh login

# Run an image generation app
infsh app run falai/flux-dev-lora --input '{"prompt": "a cat astronaut"}'
```

## Quick Reference

| Task | Command |
|------|---------|
| Install CLI | `curl -fsSL https://cli.inference.sh \| sh` |
| Login | `infsh login` |
| Check auth | `infsh me` |
| List all apps | `infsh app list` |
| Search apps | `infsh app list --search "flux"` |
| Filter by category | `infsh app list --category image` |
| Get app details | `infsh app get falai/flux-dev-lora` |
| Generate sample input | `infsh app sample falai/flux-dev-lora --save input.json` |
| Run app | `infsh app run falai/flux-dev-lora --input input.json` |
| Run with inline JSON | `infsh app run falai/flux-dev-lora --input '{"prompt": "hello"}'` |
| Run without waiting | `infsh app run <app> --input input.json --no-wait` |
| Check task status | `infsh task get <task-id>` |

## Categories

| Category | Command | Examples |
|----------|---------|----------|
| Image | `infsh app list --category image` | FLUX, SDXL, Gemini, Grok, Seedream |
| Video | `infsh app list --category video` | Veo, Seedance, Wan, LTX, OmniHuman |
| Audio | `infsh app list --category audio` | TTS, Whisper, music generation |
| Text | `infsh app list --category text` | Search, OCR, code execution |

## Workflow

1. **Find an app**: `infsh app list --search "your query"`
2. **Get details**: `infsh app get user/app-name`
3. **Generate sample**: `infsh app sample user/app-name --save input.json`
4. **Edit input**: Modify `input.json` as needed
5. **Run**: `infsh app run user/app-name --input input.json`

## Related Skills

```bash
# Image generation (FLUX, Gemini, Grok, Seedream)
npx skills add inference-sh/skills@ai-image-generation

# Video generation (Veo, Seedance, Wan, OmniHuman)
npx skills add inference-sh/skills@ai-video-generation

# LLMs (Claude, Gemini, Kimi, GLM via OpenRouter)
npx skills add inference-sh/skills@llm-models

# Web search (Tavily, Exa)
npx skills add inference-sh/skills@web-search

# AI avatars & lipsync (OmniHuman, Fabric, PixVerse)
npx skills add inference-sh/skills@ai-avatar-video

# Twitter/X automation
npx skills add inference-sh/skills@twitter-automation

# Model-specific
npx skills add inference-sh/skills@flux-image
npx skills add inference-sh/skills@google-veo

# Utilities
npx skills add inference-sh/skills@image-upscaling
npx skills add inference-sh/skills@background-removal
```

## Reference Files

- [Authentication & Setup](references/authentication.md)
- [Discovering Apps](references/app-discovery.md)
- [Running Apps](references/running-apps.md)
- [CLI Reference](references/cli-reference.md)
