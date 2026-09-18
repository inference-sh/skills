---
name: talking-head-production
description: "Talking head video production with AI avatars, lipsync, and voiceover. Recommended: P-Video-Avatar (fastest, cheapest, built-in TTS). Also covers OmniHuman, PixVerse, Fabric. Portrait requirements, audio quality, production workflows. Use for: spokesperson videos, course content, social media, presentations, demos. Triggers: talking head, avatar video, lipsync, lip sync, ai spokesperson, virtual presenter, ai presenter, omnihuman, talking avatar, video presenter, ai talking head, presenter video, ai face video, p-video-avatar"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# Talking Head Production

Create talking head videos with AI avatars and lipsync via [inference.sh](https://inference.sh) CLI.

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

# Recommended: P-Video-Avatar (built-in TTS, fastest, cheapest)
belt app run pruna/p-video-avatar --input '{
  "image": "https://portrait.jpg",
  "voice_script": "Welcome to our product tour. Today I will show you three features that will save you hours every week.",
  "voice": "Zephyr (Female)"
}'
```


## Portrait Requirements

The source portrait image is critical. Poor portraits = poor video output.

### Must Have

| Requirement | Why | Spec |
|------------|-----|------|
| **Center-framed** | Avatar needs face in predictable position | Face centered in frame |
| **Head and shoulders** | Body visible for natural gestures | Crop below chest |
| **Eyes to camera** | Creates connection with viewer | Direct frontal gaze |
| **Neutral expression** | Starting point for animation | Slight smile OK, not laughing/frowning |
| **Clear face** | Model needs to detect features | No sunglasses, heavy shadows, or obstructions |
| **High resolution** | Detail preservation | Min 512x512 face region, ideally 1024x1024+ |

### Generate a Portrait

```bash
# Generate a professional portrait with P-Image
belt app run pruna/p-image --input '{
  "prompt": "professional headshot portrait of a friendly business person, soft studio lighting, clean grey background, head and shoulders, direct eye contact, neutral pleasant expression, photorealistic",
  "aspect_ratio": "9:16"
}'
```

### Background Options

| Type | When to Use |
|------|-------------|
| Solid color | Professional, clean, easy to composite |
| Soft bokeh | Natural, lifestyle feel |
| Office/studio | Business context |
| Dynamic (P-Video-Avatar) | Use `video_prompt` to set background |

## Model Selection

**Start with P-Video-Avatar** — it's 18x faster and 6x cheaper than alternatives, with built-in TTS.

| Model | App ID | Built-in TTS | Best For |
|-------|--------|-------------|----------|
| **P-Video-Avatar** | `pruna/p-video-avatar` | **Yes (30 voices, 10 langs)** | **Best overall: speed, cost, quality** |
| OmniHuman 1.5 | `bytedance/omnihuman-1-5` | No | Multi-character, gestures |
| OmniHuman 1.0 | `bytedance/omnihuman-1-0` | No | Single character |
| Fabric 1.0 | `falai/fabric-1-0` | Yes | Image talks with lipsync |
| PixVerse Lipsync | `falai/pixverse-lipsync` | Yes | Lipsync an existing video |

### Cost & Speed Comparison

| Model | Speed (per sec of video) | Cost per second |
|-------|-------------------------|----------------|
| **P-Video-Avatar** | **~1.83s/s** | **$0.025** |
| OmniHuman 1.5 | ~28s/s (15x slower) | $0.16 (6.4x more) |
| Fabric 1.0 | ~34s/s (18x slower) | $0.14 (5.6x more) |

## Production Workflows

### Simple: Text Script -> Video (P-Video-Avatar)

No separate TTS step needed — P-Video-Avatar has built-in voices:

```bash
belt app run pruna/p-video-avatar --input '{
  "image": "https://portrait.jpg",
  "voice_script": "Hi there! I am excited to share something with you today.",
  "voice": "Puck (Male)",
  "voice_language": "English (US)",
  "resolution": "720p"
}'
```

### With Style Control

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

### Audio-Driven (Any Model)

Provide your own audio file:

```bash
# P-Video-Avatar with custom audio
belt app run pruna/p-video-avatar --input '{
  "image": "https://portrait.jpg",
  "audio": "https://speech.mp3"
}'

# OmniHuman with custom audio
belt app run bytedance/omnihuman-1-5 --input '{
  "image": "https://portrait.jpg",
  "audio": "https://speech.mp3"
}'
```

### Full Workflow: Generate Portrait + Avatar

```bash
# 1. Generate a portrait image
belt app run pruna/p-image --input '{
  "prompt": "professional headshot portrait of a young woman, neutral background, looking at camera, studio lighting, photorealistic",
  "aspect_ratio": "9:16"
}'

# 2. Create avatar video with built-in TTS
belt app run pruna/p-video-avatar --input '{
  "image": "<image-url-from-step-1>",
  "voice_script": "Hi there! Let me walk you through our latest features.",
  "voice": "Zephyr (Female)"
}'
```

### With Separate TTS (for non-TTS models)

```bash
# 1. Generate speech
belt app run falai/dia-tts --input '{
  "prompt": "[S1] Your narration script here."
}'

# 2. Create talking head
belt app run bytedance/omnihuman-1-5 --input '{
  "image": "https://portrait.jpg",
  "audio": "<audio-url-from-step-1>"
}'
```

### Multi-Character Conversation

OmniHuman 1.5 supports up to 2 characters:

```bash
# 1. Generate dialogue with two speakers
belt app run falai/dia-tts --input '{
  "prompt": "[S1] So tell me about the new feature. [S2] Sure! We built a dashboard that shows real-time analytics. [S1] That sounds great. How long did it take? [S2] About two weeks from concept to launch."
}'

# 2. Create video with two characters
belt app run bytedance/omnihuman-1-5 --input '{
  "image": "https://two-person-portrait.png",
  "audio": "<audio-url>"
}'
```

### Long-Form (Stitched Clips)

For content longer than ~60 seconds, split into segments:

```bash
# Generate clips with same portrait for consistency
belt app run pruna/p-video-avatar --input '{"image": "https://portrait.jpg", "voice_script": "Segment one..."}' --no-wait
belt app run pruna/p-video-avatar --input '{"image": "https://portrait.jpg", "voice_script": "Segment two..."}' --no-wait
belt app run pruna/p-video-avatar --input '{"image": "https://portrait.jpg", "voice_script": "Segment three..."}' --no-wait

# Merge all segments
belt app run infsh/media-merger --input '{
  "media_files": [
    {"file": "segment1.mp4"},
    {"file": "segment2.mp4"},
    {"file": "segment3.mp4"}
  ]
}'
```

### Multilingual Content

P-Video-Avatar supports 10 languages with built-in TTS:

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

### Dub Existing Video

```bash
# 1. Transcribe original video
belt app run infsh/fast-whisper-large-v3 --input '{"audio": "https://video.mp4"}'

# 2. Translate text (manually or with LLM)

# 3. Generate speech in new language
belt app run falai/kokoro-tts --input '{"prompt": "<translated-text>"}'

# 4. Lipsync original video with new audio
belt app run infsh/latentsync-1-6 --input '{
  "video_path": "https://original-video.mp4",
  "audio_path": "<new-audio-url>"
}'
```

## Audio Quality (for non-TTS workflows)

When providing your own audio, quality directly impacts lipsync accuracy.

| Parameter | Target | Why |
|-----------|--------|-----|
| Background noise | None/minimal | Noise confuses lipsync timing |
| Volume | Consistent throughout | Prevents sync drift |
| Sample rate | 44.1kHz or 48kHz | Standard quality |
| Format | MP3 128kbps+ or WAV | Compatible with all tools |

## Available Voices (P-Video-Avatar)

**Female:** Zephyr, Kore, Leda, Aoede, Callirrhoe, Autonoe, Despina, Erinome, Laomedeia, Achernar, Gacrux, Pulcherrima, Vindemiatrix, Sulafat

**Male:** Puck, Charon, Fenrir, Orus, Enceladus, Iapetus, Umbriel, Algenib, Algieba, Schedar, Achird, Zubenelgenubi, Sadachbia, Sadaltager, Alnilam, Rasalgethi

**Languages:** English (US), English (UK), Spanish, French, German, Italian, Portuguese (Brazil), Japanese, Korean, Hindi

## Framing Guidelines

```
┌─────────────────────────────────┐
│          Headroom (minimal)     │
│  ┌───────────────────────────┐  │
│  │                           │  │
│  │     ● ─ ─ Eyes at 1/3 ─ ─│─ │ ← Eyes at top 1/3 line
│  │    /|\                    │  │
│  │     |   Head & shoulders  │  │
│  │    / \  visible           │  │
│  │                           │  │
│  └───────────────────────────┘  │
│       Crop below chest          │
└─────────────────────────────────┘
```

## Common Mistakes

| Mistake | Problem | Fix |
|---------|---------|-----|
| Low-res portrait | Blurry face, poor lipsync | Use 1024x1024+ face region |
| Profile/side angle | Lipsync can't track mouth well | Use frontal or near-frontal |
| Noisy audio | Lipsync drifts, looks unnatural | Use built-in TTS or record clean |
| Too-long clips | Quality degrades | Split into segments, stitch |
| Sunglasses/obstruction | Face features hidden | Clear face required |
| Inconsistent lighting | Uncanny when animated | Even, soft lighting |

## Related Skills

```bash
# Dedicated P-Video-Avatar skill
npx skills add inference-sh/skills@p-video-avatar

# All avatar models
npx skills add inference-sh/skills@ai-avatar-video

# All video generation models
npx skills add inference-sh/skills@ai-video-generation

# Text-to-speech
npx skills add inference-sh/skills@text-to-speech

# Image generation (for portraits)
npx skills add inference-sh/skills@ai-image-generation
```

Browse all apps: `belt app list`
