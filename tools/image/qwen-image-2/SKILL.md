---
name: qwen-image-2
description: "Generate and edit images with Alibaba Qwen-Image-2.0 models via inference.sh CLI. Models: Qwen-Image-2.0 (fast), Qwen-Image-2.0-Pro (professional text rendering). Capabilities: text-to-image, multi-image editing, complex text rendering. Triggers: qwen image, qwen-image, alibaba image, dashscope image, qwen image 2, qwen image pro"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# Qwen-Image - Alibaba Image Generation

Generate and edit images with Alibaba Qwen-Image-2.0 models via [inference.sh](https://inference.sh) CLI.

![Qwen-Image-2.0](https://cloud.inference.sh/app/files/u/4mg21r6ta37mpaz6ktzwtt8krr/01kjtcbmde0jccmb4t36q04ctx.jpeg)

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

belt app run alibaba/qwen-image-2 --input '{"prompt": "A serene mountain landscape at sunset"}'
```


## Models

| Model | App ID | Speed | Text Rendering | Best For |
|-------|--------|-------|----------------|----------|
| Qwen-Image-2.0 | `alibaba/qwen-image-2` | Fast | Good | General use |
| Qwen-Image-2.0-Pro | `alibaba/qwen-image-2-pro` | Standard | Professional | Posters, text-heavy designs |

## Search Qwen Image Apps

```bash
belt app search "qwen image"
```

## Examples

### Basic Text-to-Image

```bash
belt app run alibaba/qwen-image-2 --input '{
  "prompt": "A futuristic cityscape at sunset with flying cars"
}'
```

### Multiple Images

```bash
belt app run alibaba/qwen-image-2 --input '{
  "prompt": "Minimalist logo design for a coffee shop",
  "num_images": 4
}'
```

### Custom Resolution

```bash
belt app run alibaba/qwen-image-2-pro --input '{
  "prompt": "Panoramic mountain landscape with northern lights",
  "width": 1536,
  "height": 1024
}'
```

### Text-Heavy Poster (Pro)

```bash
belt app run alibaba/qwen-image-2-pro --input '{
  "prompt": "Poster with title \"Summer Sale!\" in bold red text at the top. Subtitle \"50% Off Everything\" in blue below. Beach background with palm trees.",
  "width": 1024,
  "height": 1536,
  "prompt_extend": false
}'
```

### Image Editing (Multi-Reference)

```bash
belt app run alibaba/qwen-image-2 --input '{
  "prompt": "Make the girl from Image 1 wear the dress from Image 2 in the pose from Image 3",
  "reference_images": [
    "https://example.com/person.jpg",
    "https://example.com/dress.jpg",
    "https://example.com/pose.jpg"
  ]
}'
```

### With Negative Prompt

```bash
belt app run alibaba/qwen-image-2-pro --input '{
  "prompt": "Professional headshot portrait, studio lighting",
  "negative_prompt": "low resolution, blurry, deformed, oversaturated"
}'
```

### Reproducible with Seed

```bash
belt app run alibaba/qwen-image-2 --input '{
  "prompt": "Abstract geometric art in blue and gold",
  "seed": 12345
}'
```

## Input Options

| Parameter | Type | Description |
|-----------|------|-------------|
| `prompt` | string | **Required.** What to generate or edit (max 800 chars) |
| `reference_images` | array | Input images for editing (1-3 images) |
| `num_images` | integer | Number of images to generate (1-6) |
| `width` | integer | Output width in pixels (512-2048) |
| `height` | integer | Output height in pixels (512-2048) |
| `watermark` | boolean | Add "Qwen-Image" watermark |
| `negative_prompt` | string | Content to avoid (max 500 chars) |
| `prompt_extend` | boolean | Enable prompt rewriting (default: true) |
| `seed` | integer | Random seed for reproducibility (0-2147483647) |

**Size constraint:** Total pixels must be between 512×512 and 2048×2048.

## Output

| Field | Type | Description |
|-------|------|-------------|
| `images` | array | The generated or edited images (PNG format) |
| `output_meta` | object | Metadata with dimensions and count |

## Prompt Tips

**For Text Rendering (use Pro model):**
- Put exact text in quotes: `"Title: \"Hello World!\""`
- Specify font style, color, position
- Set `prompt_extend: false` for precise control

**Styles**: photorealistic, illustration, watercolor, oil painting, digital art, anime, 3D render

**Composition**: close-up, wide shot, aerial view, macro, portrait, landscape

**Lighting**: natural light, studio lighting, golden hour, dramatic shadows, neon

## Sample Workflow

```bash
# 1. Generate sample input to see all options
belt app sample alibaba/qwen-image-2-pro --save input.json

# 2. Edit the prompt
# 3. Run
belt app run alibaba/qwen-image-2-pro --input input.json
```

## Model Comparison

| Feature | qwen-image-2 | qwen-image-2-pro |
|---------|--------------|------------------|
| Speed | Faster | Standard |
| Text Rendering | Good | Professional |
| Realism | Standard | Fine-grained |
| Semantic Adherence | Good | Enhanced |

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

