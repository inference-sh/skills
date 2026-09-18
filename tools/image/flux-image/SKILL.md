---
name: flux-image
description: "Generate images with FLUX models (Black Forest Labs) via inference.sh CLI. Models: FLUX Dev LoRA, FLUX.2 Klein LoRA with custom style adaptation. Capabilities: text-to-image, image-to-image, LoRA fine-tuning, custom styles. Triggers: flux, flux.2, flux dev, flux schnell, flux pro, black forest labs, flux image, flux ai, flux model, flux lora"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# FLUX Image Generation

Generate images with FLUX models via [inference.sh](https://inference.sh) CLI.

![FLUX Image Generation](https://cloud.inference.sh/app/files/u/4mg21r6ta37mpaz6ktzwtt8krr/01kg0v0nz7wv0qwqjtq1cam52z.jpeg)

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

belt app run falai/flux-dev-lora --input '{"prompt": "a futuristic city at night"}'
```


## FLUX Models

| Model | App ID | Speed | Quality | Use Case |
|-------|--------|-------|---------|----------|
| FLUX Dev LoRA | `falai/flux-dev-lora` | Medium | Highest | Production, custom styles |
| FLUX.2 Klein LoRA | `falai/flux-2-klein-lora` | Fastest | Good | Fast iteration, 4B/9B sizes |
| **FLUX Dev (Pruna)** | `pruna/flux-dev` | Fast | High | Optimized, speed modes |
| **FLUX Dev LoRA (Pruna)** | `pruna/flux-dev-lora` | Fast | High | LoRA with optimization |
| **FLUX Klein 4B (Pruna)** | `pruna/flux-2-klein-4b` | Fastest | Good | Ultra-cheap ($0.0001/img) |

## Examples

### High-Quality Generation

```bash
belt app run falai/flux-dev-lora --input '{
  "prompt": "professional product photo of headphones, studio lighting, white background"
}'
```

### Fast Generation (Klein)

```bash
belt app run falai/flux-2-klein-lora --input '{"prompt": "abstract art, colorful"}'
```

### With LoRA Custom Styles

```bash
belt app sample falai/flux-dev-lora --save input.json

# Edit to add "loras": [{"path": "<lora-url>", "scale": 1.0}] for custom style
belt app run falai/flux-dev-lora --input input.json
```

### Image-to-Image

```bash
belt app run falai/flux-dev-lora --input '{
  "prompt": "transform to watercolor style",
  "image": "https://your-image.jpg"
}'
```

## For Other Image Tasks

```bash
# Image editing with natural language
belt app run falai/reve --input '{"prompt": "change background to beach"}'

# Upscaling
belt app run falai/topaz-image-upscaler --input '{"image": "https://..."}'
```

## Related Skills

```bash
# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# Pruna P-Image (fast & economical)
npx skills add inference-sh/skills@p-image

# All image generation models
npx skills add inference-sh/skills@ai-image-generation

# Upscaling
npx skills add inference-sh/skills@image-upscaling
```

Browse all apps: `belt app list`

## Documentation

- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
- [Image Generation Example](https://inference.sh/docs/examples/image-generation) - Complete image generation guide
- [Streaming Results](https://inference.sh/docs/api/sdk/streaming) - Real-time progress updates

