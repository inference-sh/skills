---
name: p-image
description: "Generate images with Pruna P-Image models via inference.sh CLI. Models: P-Image, P-Image-LoRA, P-Image-Edit, P-Image-Edit-LoRA. Capabilities: text-to-image, image editing, LoRA styles, multi-image compositing, fast inference. Pruna optimizes models for speed without quality loss. Triggers: pruna, p-image, pruna image, fast image generation, optimized flux, pruna ai, p image, fast ai image, economic image generation, cheap image generation"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# Pruna P-Image Generation

Generate images with Pruna's optimized P-Image models via [inference.sh](https://inference.sh) CLI.

![P-Image Generation](https://cloud.inference.sh/app/files/u/4mg21r6ta37mpaz6ktzwtt8krr/01kkgym0yqys16pqrg9h8ctk2y.jpeg)

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

belt app run pruna/p-image --input '{"prompt": "a futuristic cityscape at sunset"}'
```


## P-Image Models

Pruna optimizes AI models for speed without sacrificing quality.

| Model | App ID | Best For |
|-------|--------|----------|
| P-Image | `pruna/p-image` | Fast text-to-image, multiple aspect ratios |
| P-Image-LoRA | `pruna/p-image-lora` | Custom styles with preset LoRAs |
| P-Image-Edit | `pruna/p-image-edit` | Image editing with multi-image support |
| P-Image-Edit-LoRA | `pruna/p-image-edit-lora` | Stylized image editing |

## Examples

### Text-to-Image

```bash
belt app run pruna/p-image --input '{
  "prompt": "professional product photo of sneakers, studio lighting",
  "aspect_ratio": "1:1"
}'
```

### With LoRA Presets

P-Image-LoRA includes built-in style presets:

```bash
belt app run pruna/p-image-lora --input '{
  "prompt": "portrait of a woman in golden hour light",
  "lora_preset": "photos-realism"
}'
```

Available presets: `photos-realism`, `pixel-art`, `japanese-modern-look`, `cinematic-movie-style`, `graffiti-splash`, `neon-punk`, `anime-2-5d`, `ethereal-portrait`, `retro-90s-style`, `ink-sketchbook`, `paper-cut`

### Image Editing

```bash
belt app run pruna/p-image-edit --input '{
  "prompt": "change the background to a beach",
  "images": ["https://your-image.jpg"]
}'
```

### Multi-Image Compositing

```bash
belt app run pruna/p-image-edit --input '{
  "prompt": "combine these images into a collage",
  "images": ["https://img1.jpg", "https://img2.jpg", "https://img3.jpg"]
}'
```

### Custom Aspect Ratios

```bash
belt app run pruna/p-image --input '{
  "prompt": "landscape mountain scene",
  "aspect_ratio": "16:9"
}'
```

Supported ratios: `1:1`, `16:9`, `9:16`, `4:3`, `3:4`, `3:2`, `2:3`, or `custom` with width/height

## Other Pruna Models

Pruna offers optimized versions of popular models:

```bash
# FLUX Dev (optimized)
belt app run pruna/flux-dev --input '{"prompt": "..."}'

# FLUX Klein 4B (extremely fast, $0.0001/image)
belt app run pruna/flux-2-klein-4b --input '{"prompt": "..."}'

# Qwen Image
belt app run pruna/qwen-image --input '{"prompt": "..."}'

# Z-Image Turbo (ultra-fast)
belt app run pruna/z-image-turbo --input '{"prompt": "..."}'

# WAN Image Small (batch generation)
belt app run pruna/wan-image-small --input '{"prompt": "..."}'
```

## Browse All Pruna Apps

```bash
belt app list --namespace pruna
```

## Related Skills

```bash
# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# All image generation models
npx skills add inference-sh/skills@ai-image-generation

# Pruna video generation
npx skills add inference-sh/skills@p-video

# FLUX models
npx skills add inference-sh/skills@flux-image
```

Browse all apps: `belt app list`

## Documentation

- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
- [Image Generation Example](https://inference.sh/docs/examples/image-generation) - Complete image generation guide
- [Streaming Results](https://inference.sh/docs/api/sdk/streaming) - Real-time progress updates

