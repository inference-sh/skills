---
name: ai-image-generation
description: "Generate AI images with GPT-Image-2.5, FLUX, Gemini, Grok, Seedream, Reve and 50+ models via inference.sh CLI. Models: GPT-Image-2.5 Flare, GPT-Image-2.5 Sunburst, GPT-Image-2, FLUX Dev LoRA, FLUX.2 Klein LoRA, Gemini 3 Pro Image, Grok Imagine, Seedream 4.5, Reve, ImagineArt. Capabilities: text-to-image, image-to-image, inpainting, LoRA, image editing, upscaling, text rendering. Use for: AI art, product mockups, concept art, social media graphics, marketing visuals, illustrations. Triggers: flux, image generation, ai image, text to image, stable diffusion, generate image, ai art, midjourney alternative, dall-e alternative, text2img, t2i, image generator, ai picture, create image with ai, generative ai, ai illustration, grok image, gemini image, gpt image, openai image, chatgpt image"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# AI Image Generation

Generate images with 50+ AI models via [inference.sh](https://inference.sh) CLI.

![AI Image Generation](https://cloud.inference.sh/app/files/u/4mg21r6ta37mpaz6ktzwtt8krr/01kg0v0nz7wv0qwqjtq1cam52z.jpeg)

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

# Generate an image with FLUX
belt app run falai/flux-dev-lora --input '{"prompt": "a cat astronaut in space"}'
```


## Available Models

| Model | App ID | Best For |
|-------|--------|----------|
| **GPT-Image-2.5 Flare** | `openai/gpt-image-2-5-flare` | Best default: text-to-image, editing, inpainting, transparent PNG |
| **GPT-Image-2.5 Sunburst** | `openai/gpt-image-2-5-sunburst` | Precise edits that preserve subject and composition |
| GPT-Image-2 | `openai/gpt-image-2` | Previous generation |
| FLUX Dev LoRA | `falai/flux-dev-lora` | High quality with custom styles |
| FLUX.2 Klein LoRA | `falai/flux-2-klein-lora` | Fast with LoRA support (4B/9B) |
| **P-Image** | `pruna/p-image` | Fast, economical, multiple aspects |
| **P-Image-LoRA** | `pruna/p-image-lora` | Fast with preset LoRA styles |
| **P-Image-Edit** | `pruna/p-image-edit` | Fast image editing |
| Gemini 3 Pro | `google/gemini-3-pro-image` | Google's latest |
| Gemini 2.5 Flash | `google/gemini-2-5-flash-image` | Fast Google model |
| Grok Imagine | `xai/grok-imagine-image` | xAI's model, multiple aspects |
| Seedream 4.5 | `bytedance/seedream-4-5` | 2K-4K cinematic quality |
| Seedream 4.0 | `bytedance/seedream-4-0` | High quality 2K-4K |
| Seedream 3.0 | `bytedance/seedream-3-0-t2i` | Accurate text rendering |
| Reve | `falai/reve` | Natural language editing, text rendering |
| ImagineArt 1.5 Pro | `falai/imagine-art-1-5-pro-preview` | Ultra-high-fidelity 4K |
| FLUX Klein 4B | `pruna/flux-2-klein-4b` | Ultra-cheap ($0.0001/image) |
| Topaz Upscaler | `falai/topaz-image-upscaler` | Professional upscaling |

## Browse All Image Apps

```bash
belt app list --category image
```

## Examples

### GPT-Image-2.5 Flare

```bash
belt app run openai/gpt-image-2-5-flare --input '{
  "prompt": "professional product photo of sneakers, studio lighting",
  "quality": "high"
}'
```

### GPT-Image-2.5 Sunburst Editing

```bash
belt app run openai/gpt-image-2-5-sunburst --input '{
  "prompt": "change the background to a beach at sunset",
  "images": ["https://your-image.jpg"]
}'
```

### Text-to-Image with FLUX

```bash
belt app run falai/flux-dev-lora --input '{
  "prompt": "professional product photo of a coffee mug, studio lighting"
}'
```

### Fast Generation with FLUX Klein

```bash
belt app run falai/flux-2-klein-lora --input '{"prompt": "sunset over mountains"}'
```

### Google Gemini 3 Pro

```bash
belt app run google/gemini-3-pro-image --input '{
  "prompt": "photorealistic landscape with mountains and lake"
}'
```

### Grok Imagine

```bash
belt app run xai/grok-imagine-image --input '{
  "prompt": "cyberpunk city at night",
  "aspect_ratio": "16:9"
}'
```

### Reve (with Text Rendering)

```bash
belt app run falai/reve --input '{
  "prompt": "A poster that says HELLO WORLD in bold letters"
}'
```

### Seedream 4.5 (4K Quality)

```bash
belt app run bytedance/seedream-4-5 --input '{
  "prompt": "cinematic portrait of a woman, golden hour lighting"
}'
```

### Image Upscaling

```bash
belt app run falai/topaz-image-upscaler --input '{"image": "https://..."}'
```

### Stitch Multiple Images

```bash
belt app run infsh/stitch-images --input '{
  "images": ["https://img1.jpg", "https://img2.jpg"],
  "direction": "horizontal"
}'
```

## Related Skills

```bash
# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# Pruna P-Image (fast & economical)
npx skills add inference-sh/skills@p-image

# GPT-Image-2.5 / GPT-Image-2 (OpenAI)
npx skills add inference-sh/skills@gpt-image

# FLUX-specific skill
npx skills add inference-sh/skills@flux-image

# Upscaling & enhancement
npx skills add inference-sh/skills@image-upscaling

# Background removal
npx skills add inference-sh/skills@background-removal

# Video generation
npx skills add inference-sh/skills@ai-video-generation

# AI avatars from images
npx skills add inference-sh/skills@ai-avatar-video
```

Browse all apps: `belt app list`

## Documentation

- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
- [Image Generation Example](https://inference.sh/docs/examples/image-generation) - Complete image generation guide
- [Apps Overview](https://inference.sh/docs/apps/overview) - Understanding the app ecosystem

