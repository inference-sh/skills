---
name: nano-banana
description: "Generate images with Google Gemini native image models via inference.sh CLI. Models: Gemini 3 Pro Image, Gemini 2.5 Flash Image. Capabilities: text-to-image, image editing, multi-image input. Triggers: nano banana, gemini image, gemini 3 pro image, gemini 2.5 flash image, google image generation, native image generation, gemini native image"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# Nano Banana - Gemini Native Image Generation

Generate images with Google Gemini native image models via [inference.sh](https://inference.sh) CLI.

![Nano Banana](https://cloud.inference.sh/u/33sqbmzt3mrg2xxphnhw5g5ear/01k8d6xa9cwawrvzk9cgtsexfc.png)

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

belt app run google/gemini-3-pro-image --input '{"prompt": "a banana in space, photorealistic"}'
```


## Models

| Model | App ID | Speed | Quality |
|-------|--------|-------|---------|
| Gemini 3 Pro Image | `google/gemini-3-pro-image` | Slower | Best |
| Gemini 2.5 Flash Image | `google/gemini-2-5-flash-image` | Fast | Excellent |

## Search Gemini Image Apps

```bash
belt app search "gemini image"
```

## Examples

### Basic Text-to-Image

```bash
belt app run google/gemini-3-pro-image --input '{
  "prompt": "A futuristic cityscape at sunset with flying cars"
}'
```

### Multiple Images

```bash
belt app run google/gemini-2-5-flash-image --input '{
  "prompt": "Minimalist logo design for a coffee shop",
  "num_images": 4
}'
```

### Custom Aspect Ratio

```bash
belt app run google/gemini-3-pro-image --input '{
  "prompt": "Panoramic mountain landscape with northern lights",
  "aspect_ratio": "16:9"
}'
```

### Image Editing (with input image)

```bash
belt app run google/gemini-2-5-flash-image --input '{
  "prompt": "Add a rainbow in the sky",
  "images": ["https://example.com/landscape.jpg"]
}'
```

### High Resolution (4K)

```bash
belt app run google/gemini-3-pro-image --input '{
  "prompt": "Detailed illustration of a medieval castle",
  "resolution": "4K"
}'
```

### With Google Search Grounding

```bash
belt app run google/gemini-3-pro-image --input '{
  "prompt": "Current weather in Tokyo visualized as an artistic scene",
  "enable_google_search": true
}'
```

## Input Options

| Parameter | Type | Description |
|-----------|------|-------------|
| `prompt` | string | **Required.** What to generate or change |
| `images` | array | Input images for editing (up to 14) |
| `num_images` | integer | Number of images to generate |
| `aspect_ratio` | string | Output ratio: "1:1", "16:9", "9:16", "4:3", "3:4", "auto" |
| `resolution` | string | "1K", "2K", "4K" (Gemini 3 Pro only) |
| `output_format` | string | Output format for images |
| `enable_google_search` | boolean | Enable real-time info grounding |

## Prompt Tips

**Styles**: photorealistic, illustration, watercolor, oil painting, digital art, anime, 3D render

**Composition**: close-up, wide shot, aerial view, macro, portrait, landscape

**Lighting**: natural light, studio lighting, golden hour, dramatic shadows, neon

**Details**: add specific details about textures, colors, mood, atmosphere

## Sample Workflow

```bash
# 1. Generate sample input to see all options
belt app sample google/gemini-3-pro-image --save input.json

# 2. Edit the prompt
# 3. Run
belt app run google/gemini-3-pro-image --input input.json
```

## Related Skills

```bash
# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# All image generation models
npx skills add inference-sh/skills@ai-image-generation

# Video generation (for image-to-video)
npx skills add inference-sh/skills@ai-video-generation
```

Browse all image apps: `belt app list --category image`

## Documentation

- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
- [Streaming Results](https://inference.sh/docs/api/sdk/streaming) - Real-time progress updates
- [File Handling](https://inference.sh/docs/api/sdk/files) - Working with images

