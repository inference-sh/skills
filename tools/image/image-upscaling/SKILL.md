---
name: image-upscaling
description: "Upscale and enhance images with Real-ESRGAN, Thera, Topaz, FLUX Upscaler via inference.sh CLI. Models: Real-ESRGAN, Thera (any size), FLUX Dev Upscaler, Topaz Image Upscaler. Use for: enhance low-res images, upscale AI art, restore old photos, increase resolution. Triggers: upscale image, image upscaler, enhance image, increase resolution, real esrgan, ai upscale, super resolution, image enhancement, upscaling, enlarge image, higher resolution, 4k upscale, hd upscale"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# Image Upscaling

Upscale and enhance images via [inference.sh](https://inference.sh) CLI.

![Image Upscaling](https://cloud.inference.sh/u/33sqbmzt3mrg2xxphnhw5g5ear/01k8d77p126y82zfecnt46hy4h.png)

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

belt app run infsh/real-esrgan --input '{"input_file": "https://your-image.jpg"}'
```


## Available Upscalers

| Model | App ID | Best For |
|-------|--------|----------|
| Topaz Image Upscaler | `falai/topaz-image-upscaler` | Professional quality, any image |

## Examples

### Upscale Any Image

```bash
belt app run falai/topaz-image-upscaler --input '{"image": "https://low-res-image.jpg"}'
```

### Workflow: Generate and Upscale

```bash
# 1. Generate image with FLUX Klein (fast)
belt app run falai/flux-2-klein-lora --input '{"prompt": "landscape painting"}' > image.json

# 2. Upscale the result
belt app run falai/topaz-image-upscaler --input '{"image": "<url-from-step-1>"}'
```

## Use Cases

- **AI Art**: Upscale generated images for print
- **Old Photos**: Restore and enhance resolution
- **Web Images**: Prepare for high-DPI displays
- **Print**: Increase resolution for large prints
- **Thumbnails**: Create high-res versions

## Related Skills

```bash
# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# Image generation (generate then upscale)
npx skills add inference-sh/skills@ai-image-generation

# FLUX models
npx skills add inference-sh/skills@flux-image

# Background removal
npx skills add inference-sh/skills@background-removal
```

Browse all image apps: `belt app list --category image`

## Documentation

- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
- [Image Generation Example](https://inference.sh/docs/examples/image-generation) - Complete image workflow guide
- [Apps Overview](https://inference.sh/docs/apps/overview) - Understanding the app ecosystem

