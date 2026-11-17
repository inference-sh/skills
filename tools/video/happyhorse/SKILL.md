---
name: happyhorse
description: "Generate and edit videos with Alibaba HappyHorse 1.0 models via inference.sh CLI. Models: HappyHorse T2V, I2V, R2V, Video Edit. Capabilities: text-to-video, image-to-video, reference-to-video, video editing with natural language, character preservation, 720P/1080P, up to 15 seconds. Use for: physically realistic video, video editing, character-consistent content, product demos, social media. Triggers: happyhorse, happy horse, alibaba video, happyhorse 1.0, dashscope video, alibaba happyhorse, video editing ai, ai video editor"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# HappyHorse 1.0 Video Generation

Generate and edit physically realistic videos with Alibaba's HappyHorse 1.0 models via [inference.sh](https://inference.sh) CLI.

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

belt app run alibaba/happyhorse-1-0-t2v --input '{"prompt": "a horse galloping across a sunlit meadow"}'
```


## HappyHorse Models

| Model | App ID | Best For |
|-------|--------|----------|
| T2V | `alibaba/happyhorse-1-0-t2v` | Text-to-video, physically realistic motion |
| I2V | `alibaba/happyhorse-1-0-i2v` | Animate a single image |
| R2V | `alibaba/happyhorse-1-0-r2v` | Preserve characters from up to 9 reference images |
| Video Edit | `alibaba/happyhorse-1-0-video-edit` | Edit existing videos with natural language |

All models support 720P/1080P resolution, up to 15 seconds duration.

## Examples

### Text-to-Video

```bash
belt app run alibaba/happyhorse-1-0-t2v --input '{
  "prompt": "a golden retriever running through autumn leaves in a park, slow motion",
  "duration": 10,
  "resolution": "1080P",
  "ratio": "16:9"
}'
```

### Image-to-Video

Animate a still image:

```bash
belt app run alibaba/happyhorse-1-0-i2v --input '{
  "first_frame": "https://your-image.jpg",
  "prompt": "gentle camera zoom, clouds moving in the sky",
  "duration": 8,
  "resolution": "720P"
}'
```

### Reference-to-Video (Character Preservation)

Generate videos that preserve characters from reference images (up to 9):

```bash
belt app run alibaba/happyhorse-1-0-r2v --input '{
  "prompt": "a woman walking through a busy market street",
  "reference_images": ["https://portrait.jpg"],
  "duration": 10,
  "resolution": "720P"
}'
```

### Multi-Character Reference

```bash
belt app run alibaba/happyhorse-1-0-r2v --input '{
  "prompt": "two friends sitting at a cafe having coffee",
  "reference_images": ["https://person1.jpg", "https://person2.jpg"],
  "ratio": "16:9"
}'
```

### Video Editing

Edit existing videos using natural language instructions:

```bash
belt app run alibaba/happyhorse-1-0-video-edit --input '{
  "video": "https://your-video.mp4",
  "prompt": "change the background to a snowy mountain landscape"
}'
```

### Video Editing with Reference Images

```bash
belt app run alibaba/happyhorse-1-0-video-edit --input '{
  "video": "https://your-video.mp4",
  "prompt": "replace the person with the character from the reference image",
  "reference_images": ["https://character.jpg"]
}'
```

### Video Editing with Audio Control

```bash
belt app run alibaba/happyhorse-1-0-video-edit --input '{
  "video": "https://your-video.mp4",
  "prompt": "make the scene look like a rainy day",
  "audio_setting": "origin"
}'
```

## Pricing

| Resolution | Price |
|------------|-------|
| 720P | $0.14 per second |
| 1080P | $0.24 per second |

Video Edit is billed on input + output duration.

## Parameters (T2V)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `prompt` | string | required | Text description of the video |
| `duration` | integer | 5 | Duration in seconds (3–15) |
| `resolution` | enum | 1080P | 720P or 1080P |
| `ratio` | enum | 16:9 | 16:9, 9:16, 1:1, 4:3, 3:4 |
| `seed` | integer | random | Reproducible generation |
| `watermark` | boolean | false | Add HappyHorse watermark |

## Parameters (I2V)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `first_frame` | file | required | First frame image (JPEG, PNG, WebP) |
| `prompt` | string | - | Optional text description |
| `duration` | integer | 5 | Duration in seconds (3–15) |
| `resolution` | enum | 1080P | 720P or 1080P |
| `seed` | integer | random | Reproducible generation |

## Parameters (R2V)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `prompt` | string | required | Text description of the scene |
| `reference_images` | array | required | Up to 9 character reference images |
| `duration` | integer | 5 | Duration in seconds (3–15) |
| `resolution` | enum | 1080P | 720P or 1080P |
| `ratio` | enum | 16:9 | 16:9, 9:16, 1:1, 4:3, 3:4 |
| `seed` | integer | random | Reproducible generation |

## Parameters (Video Edit)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `video` | file | required | Video to edit (MP4/MOV, H.264) |
| `prompt` | string | required | Editing instruction |
| `reference_images` | array | - | Up to 5 reference images |
| `audio_setting` | enum | auto | auto (model decides) or origin (keep original audio) |
| `resolution` | enum | 1080P | 720P or 1080P |
| `seed` | integer | random | Reproducible generation |

## Search HappyHorse Apps

```bash
belt app search "happyhorse"
```

## Related Skills

```bash
# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# All video generation models
npx skills add inference-sh/skills@ai-video-generation

# Seedance 2.5
npx skills add inference-sh/skills@seedance

# Google Veo
npx skills add inference-sh/skills@google-veo

# Image generation (for image-to-video)
npx skills add inference-sh/skills@ai-image-generation
```

Browse all video apps: `belt app list --category video`

## Documentation

- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
- [Streaming Results](https://inference.sh/docs/api/sdk/streaming) - Real-time progress updates
- [Content Pipeline Example](https://inference.sh/docs/examples/content-pipeline) - Building media workflows
