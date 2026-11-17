---
name: p-video-avatar
description: "Generate talking head avatar videos with Pruna P-Video-Avatar via inference.sh CLI. Turn a portrait image into a realistic speaking video with built-in TTS. 18x faster and 6x cheaper than competitors. Models: P-Video-Avatar, P-Image (for portrait generation). Capabilities: text-to-avatar, audio-driven avatars, 30 voices, 10 languages, 720p/1080p, built-in TTS, dynamic backgrounds, full-body control. Use for: AI presenters, product demos, explainer videos, virtual influencers, marketing, education, multilingual content, UGC, gaming avatars. Triggers: avatar video, talking head, ai avatar, p-video-avatar, pruna avatar, video avatar, ai presenter, digital human, virtual presenter, lipsync, talking avatar, ai spokesperson, heygen alternative, synthesia alternative, veed alternative, fabric alternative, omnihuman alternative"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# Pruna P-Video-Avatar

Generate talking head avatar videos from a single portrait image via [inference.sh](https://inference.sh) CLI.

P-Video-Avatar is the fastest and most cost-effective avatar video model available. Quality on par with Veo 3.0, 18x faster and 6x cheaper than alternatives like Fabric, OmniHuman, and HeyGen.

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

# Generate avatar from portrait + text script
belt app run pruna/p-video-avatar --input '{
  "image": "https://portrait.jpg",
  "voice_script": "Hello, welcome to our product demo!",
  "voice": "Zephyr (Female)"
}'
```

## Full Workflow: Generate Portrait + Avatar Video

Use Pruna P-Image to generate the portrait, then P-Video-Avatar to animate it:

```bash
# 1. Generate a portrait image with P-Image
belt app run pruna/p-image --input '{
  "prompt": "professional headshot portrait of a young woman, neutral background, looking at camera, studio lighting, photorealistic",
  "aspect_ratio": "9:16"
}'

# 2. Use the generated image URL to create the avatar video
belt app run pruna/p-video-avatar --input '{
  "image": "<image-url-from-step-1>",
  "voice_script": "Hi there! Let me walk you through our latest features.",
  "voice": "Zephyr (Female)",
  "resolution": "720p"
}'
```

## Examples

### Text Script with Voice Selection

```bash
belt app run pruna/p-video-avatar --input '{
  "image": "https://portrait.jpg",
  "voice_script": "Welcome to our product walkthrough. Today I will show you three key features.",
  "voice": "Puck (Male)",
  "voice_language": "English (US)",
  "resolution": "720p"
}'
```

### Audio-Driven Avatar

Provide your own audio file instead of using built-in TTS:

```bash
belt app run pruna/p-video-avatar --input '{
  "image": "https://portrait.jpg",
  "audio": "https://speech.mp3"
}'
```

When both `audio` and `voice_script` are provided, audio takes priority.

### 1080p with Custom Style

```bash
belt app run pruna/p-video-avatar --input '{
  "image": "https://portrait.jpg",
  "voice_script": "This is exciting news for our community!",
  "voice": "Aoede (Female)",
  "voice_prompt": "Enthusiastic and energetic tone, slightly faster pace",
  "video_prompt": "The person is presenting on stage with dramatic lighting",
  "resolution": "1080p"
}'
```

### Multilingual Content

```bash
# Spanish
belt app run pruna/p-video-avatar --input '{
  "image": "https://portrait.jpg",
  "voice_script": "Bienvenidos a nuestra demostración de producto.",
  "voice": "Kore (Female)",
  "voice_language": "Spanish"
}'

# Japanese
belt app run pruna/p-video-avatar --input '{
  "image": "https://portrait.jpg",
  "voice_script": "こんにちは、製品デモへようこそ。",
  "voice": "Leda (Female)",
  "voice_language": "Japanese"
}'
```

### Reproducible Generation

```bash
belt app run pruna/p-video-avatar --input '{
  "image": "https://portrait.jpg",
  "voice_script": "Consistent results every time.",
  "seed": 42
}'
```

## Available Voices

**Female:** Zephyr, Kore, Leda, Aoede, Callirrhoe, Autonoe, Despina, Erinome, Laomedeia, Achernar, Gacrux, Pulcherrima, Vindemiatrix, Sulafat

**Male:** Puck, Charon, Fenrir, Orus, Enceladus, Iapetus, Umbriel, Algenib, Algieba, Schedar, Achird, Zubenelgenubi, Sadachbia, Sadaltager, Alnilam, Rasalgethi

## Supported Languages

English (US), English (UK), Spanish, French, German, Italian, Portuguese (Brazil), Japanese, Korean, Hindi

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `image` | file | required | Portrait image (jpg, jpeg, png, webp) |
| `voice_script` | string | - | Text for the person to say |
| `audio` | file | - | Audio file (overrides voice_script) |
| `voice` | string | "Zephyr (Female)" | Voice selection |
| `voice_language` | string | "English (US)" | Output language |
| `resolution` | string | "720p" | 720p or 1080p |
| `video_prompt` | string | "The person is talking." | Control avatar behavior and background |
| `voice_prompt` | string | "Say the following." | Control tone, pacing, emotion |
| `seed` | int | random | Reproducible generation |
| `disable_safety_filter` | bool | true | Disable content filter |
| `disable_prompt_upsampling` | bool | false | Skip prompt enhancement |

## Pricing

| Resolution | Price |
|------------|-------|
| 720p | $0.025 per second of output video |
| 1080p | $0.045 per second of output video |

Example: 30-second 720p video = $0.75

## Competitive Advantage

| Feature | P-Video-Avatar | Fabric 1.0 | OmniHuman 1.5 | HeyGen Avatar 4 |
|---------|---------------|------------|---------------|-----------------|
| Speed (per sec of video) | ~1.83s/s | ~34s/s (18x slower) | ~28s/s (15x slower) | ~26s/s (14x slower) |
| Cost per second | $0.025 | $0.14 (5.6x more) | $0.16 (6.4x more) | $0.075 (3x more) |
| Built-in TTS | Yes | Yes | No | Yes |
| Dynamic Background | Yes | Yes | No | Yes |
| 1080p Support | Yes | No | No | Yes |

## Use Cases

- **Marketing**: Product demos, UGC-style ads with AI presenters
- **Education**: Course videos, explainers, tutoring content
- **Localization**: Dub content across 10 languages from one image
- **Social Media**: Consistent virtual influencer content
- **Corporate**: Training, onboarding, announcements
- **Gaming**: Character avatars, NPC dialogue videos
- **Customer Support**: Personalized video responses

## Tips

- Use high-quality portrait photos (front-facing, good lighting)
- The output video aspect ratio matches the input image
- Use `video_prompt` to control dynamic backgrounds and body language
- Use `voice_prompt` to control speaking style, emotion, and pacing
- Keep videos under 3 minutes for best visual consistency
- Generate portraits with `pruna/p-image` using aspect ratio `9:16` for vertical avatar videos

## Related Pruna Models

```bash
# Generate portrait images
belt app run pruna/p-image --input '{"prompt": "professional headshot portrait"}'

# General video generation
belt app run pruna/p-video-2 --input '{"prompt": "cinematic scene"}'

# Image editing
belt app run pruna/p-image-edit --input '{"prompt": "change background", "images": ["https://photo.jpg"]}'
```

## Related Skills

```bash
# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# Pruna video generation
npx skills add inference-sh/skills@p-video

# Pruna image generation
npx skills add inference-sh/skills@p-image

# All video generation models
npx skills add inference-sh/skills@ai-video-generation

# Image generation (for creating portraits)
npx skills add inference-sh/skills@ai-image-generation
```

Browse all Pruna apps: `belt app list --search "pruna"`

## Documentation

- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
- [Streaming Results](https://inference.sh/docs/api/sdk/streaming) - Real-time progress updates
- [Content Pipeline Example](https://inference.sh/docs/examples/content-pipeline) - Building media workflows
