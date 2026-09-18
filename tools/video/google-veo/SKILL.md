---
name: google-veo
description: "Generate videos with Google Veo models via inference.sh CLI. Models: Veo 3.1, Veo 3.1 Fast. Capabilities: text-to-video, cinematic output, high quality video generation. Triggers: veo, google veo, veo 3, veo 2, veo 3.1, vertex ai video, google video generation, google video ai, veo model, veo video"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# Google Veo Video Generation

Generate videos with Google Veo models via [inference.sh](https://inference.sh) CLI.

![Google Veo Video Generation](https://cloud.inference.sh/app/files/u/4mg21r6ta37mpaz6ktzwtt8krr/01kg2c0egyg243mnyth4y6g51q.jpeg)

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

belt app run google/veo-3-1-fast --input '{"prompt": "drone shot over a mountain lake"}'
```


## Veo Models

| Model | App ID | Speed | Quality |
|-------|--------|-------|---------|
| Veo 3.1 | `google/veo-3-1` | Slower | Best |
| Veo 3.1 Fast | `google/veo-3-1-fast` | Fast | Excellent |

## Search Veo Apps

```bash
belt app search "veo"
```

## Examples

### Cinematic Shot

```bash
belt app run google/veo-3-1-fast --input '{
  "prompt": "Cinematic drone shot flying through a misty forest at sunrise, volumetric lighting"
}'
```

### Product Demo

```bash
belt app run google/veo-3-1 --input '{
  "prompt": "Sleek smartphone rotating on a dark reflective surface, studio lighting"
}'
```

### Nature Scene

```bash
belt app run google/veo-3-1-fast --input '{
  "prompt": "Timelapse of clouds moving over a mountain range, golden hour"
}'
```

### Action Shot

```bash
belt app run google/veo-3-1 --input '{
  "prompt": "Slow motion water droplet splashing into a pool, macro shot"
}'
```

### Urban Scene

```bash
belt app run google/veo-3-1-fast --input '{
  "prompt": "Busy city street at night with neon signs and rain reflections, Tokyo style"
}'
```

## Prompt Tips

**Camera movements**: drone shot, tracking shot, pan, zoom, dolly, steadicam

**Lighting**: golden hour, blue hour, studio lighting, volumetric, neon, natural

**Style**: cinematic, documentary, commercial, artistic, realistic

**Timing**: slow motion, timelapse, real-time

## Sample Workflow

```bash
# 1. Generate sample input to see all options
belt app sample google/veo-3-1-fast --save input.json

# 2. Edit the prompt
# 3. Run
belt app run google/veo-3-1-fast --input input.json
```

## Related Skills

```bash
# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# All video generation models
npx skills add inference-sh/skills@ai-video-generation

# AI avatars & lipsync
npx skills add inference-sh/skills@ai-avatar-video

# Image generation (for image-to-video)
npx skills add inference-sh/skills@ai-image-generation
```

Browse all video apps: `belt app list --category video`

## Documentation

- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
- [Streaming Results](https://inference.sh/docs/api/sdk/streaming) - Real-time progress updates
- [Content Pipeline Example](https://inference.sh/docs/examples/content-pipeline) - Building media workflows

