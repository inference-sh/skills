# inference.sh Skills

AI agent skills for running 150+ AI apps via the inference.sh CLI.

## Installation

### Option 1: Install All Skills

```bash
npx skills add inference-sh/skills
```

### Option 2: Install Specific Skills

```bash
# Main platform skill
npx skills add inference-sh/skills@inference-sh

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
cp -r skills/* ~/.claude/skills/

# GitHub Copilot
cp -r skills/* ~/.copilot/skills/

# Project-level
cp -r skills/* .claude/skills/
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

## Available Skills

| Skill | Description | Triggers |
|-------|-------------|----------|
| **[inference-sh](inference-sh/SKILL.md)** | Main platform skill | inference.sh, run ai |
| **[ai-image-generation](ai-image-generation/SKILL.md)** | 50+ image models | flux, gemini image, grok, ai art |
| **[ai-video-generation](ai-video-generation/SKILL.md)** | 40+ video models | veo, seedance, text to video |
| **[llm-models](llm-models/SKILL.md)** | Claude, Gemini, Kimi, GLM | claude api, openrouter, llm |
| **[web-search](web-search/SKILL.md)** | Tavily, Exa search | tavily, exa, web search, rag |
| **[twitter-automation](twitter-automation/SKILL.md)** | X/Twitter API | tweet, twitter bot |
| **[flux-image](flux-image/SKILL.md)** | FLUX models | flux.2, flux lora |
| **[google-veo](google-veo/SKILL.md)** | Google Veo | veo 3, vertex ai |
| **[ai-avatar-video](ai-avatar-video/SKILL.md)** | Talking heads | omnihuman, lipsync, heygen alt |
| **[image-upscaling](image-upscaling/SKILL.md)** | Upscalers | upscale, topaz |
| **[background-removal](background-removal/SKILL.md)** | BG removal | remove background |

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

## Links

- **Website**: [inference.sh](https://inference.sh)
- **App Store**: [app.inference.sh](https://app.inference.sh)
- **Docs**: [app.inference.sh/docs](https://app.inference.sh/docs)
- **CLI Install**: `curl -fsSL https://cli.inference.sh | sh`
