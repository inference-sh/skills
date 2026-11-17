---
name: p-video
description: "Generate videos with Pruna P-Video 2 and WAN models via inference.sh CLI. Models: P-Video 2, WAN-T2V, WAN-I2V. Capabilities: text-to-video, image-to-video, audio support, 720p/1080p, up to 20s, fast inference. Pruna optimizes models for speed without quality loss. Triggers: pruna video, p-video, p-video 2, pruna ai video, fast video generation, optimized video, wan t2v, wan i2v, economic video generation, cheap video generation, pruna text to video, pruna image to video"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# Pruna P-Video Generation

Generate videos with Pruna's optimized video models via [inference.sh](https://inference.sh) CLI.

![P-Video Generation](https://cloud.inference.sh/app/files/u/4mg21r6ta37mpaz6ktzwtt8krr/01kkgymcjx9g2tv51m602jssn3.jpeg)

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

belt app run pruna/p-video-2 --input '{"prompt": "drone shot flying over a forest at sunset"}'
```


## Pruna Video Models

Pruna optimizes AI models for speed without sacrificing quality.

| Model | App ID | Best For |
|-------|--------|----------|
| P-Video 2 | `pruna/p-video-2` | Text-to-video, image-to-video, with audio, up to 20s |
| WAN-T2V | `pruna/wan-t2v` | Text-to-video, 480p/720p |
| WAN-I2V | `pruna/wan-i2v` | Animate images, 480p/720p |

## Examples

### Text-to-Video

```bash
belt app run pruna/p-video-2 --input '{
  "prompt": "waves crashing on a beach at sunset",
  "duration": 5,
  "resolution": "720p"
}'
```

### Image-to-Video

```bash
belt app run pruna/p-video-2 --input '{
  "prompt": "gentle camera movement, clouds drifting",
  "image": "https://your-image.jpg"
}'
```

### With Audio

P-Video 2 supports audio input that syncs with the video:

```bash
belt app run pruna/p-video-2 --input '{
  "prompt": "person talking in an interview setting",
  "audio": "https://your-audio.mp3"
}'
```

### WAN Text-to-Video

Fast and economical text-to-video:

```bash
belt app run pruna/wan-t2v --input '{
  "prompt": "a cat playing with a ball of yarn",
  "resolution": "720p",
  "duration": 5
}'
```

### WAN Image-to-Video

Animate any still image:

```bash
belt app run pruna/wan-i2v --input '{
  "prompt": "gentle movement, natural motion, subtle breathing",
  "image": "https://portrait.jpg",
  "resolution": "720p"
}'
```

### 1080p High Quality

```bash
belt app run pruna/p-video-2 --input '{
  "prompt": "cinematic landscape with dramatic clouds",
  "resolution": "1080p",
  "duration": 5
}'
```

### Draft Mode (Faster, Cheaper)

```bash
belt app run pruna/p-video-2 --input '{
  "prompt": "quick concept test video",
  "draft": true
}'
```

## Resolution Options

| Model | Resolutions | Pricing |
|-------|-------------|---------|
| P-Video 2 | 720p, 1080p | Per second, varies by resolution/draft |
| WAN-T2V | 480p, 720p | $0.05 (480p), $0.10 (720p) per video |
| WAN-I2V | 480p, 720p | $0.05 (480p), $0.11 (720p) per video |

## Browse All Pruna Apps

```bash
belt app list --namespace pruna
```

## Related Skills

```bash
# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# All video generation models
npx skills add inference-sh/skills@ai-video-generation

# Image-to-video guide
npx skills add inference-sh/skills@image-to-video

# Pruna image generation
npx skills add inference-sh/skills@p-image

# Text-to-speech (for video narration)
npx skills add inference-sh/skills@text-to-speech
```

Browse all apps: `belt app list`

## Documentation

- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
- [Streaming Results](https://inference.sh/docs/api/sdk/streaming) - Real-time progress updates
- [Content Pipeline Example](https://inference.sh/docs/examples/content-pipeline) - Building media workflows

