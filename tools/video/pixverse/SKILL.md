---
name: pixverse
description: "Generate, extend, compose and edit videos with PixVerse models via inference.sh CLI. Models: PixVerse V6, C1, Extend, Fusion, Modify, Lip Sync, Avatar, plus PixVerse Lipsync on fal.ai. Capabilities: text-to-video, image-to-video, 360p-1080p, 5-15s, video continuation, multi-subject composition from reference images, text-guided video editing with masks and reference images, lip sync from audio or TTS, talking avatars from a portrait. Use for: social clips, longer sequences built by extension, putting specific people, products or pets into one scene, swapping subjects in footage, dubbing, talking presenters. Triggers: pixverse, pixverse v6, pixverse c1, pixverse extend, pixverse fusion, pixverse modify, pixverse lipsync, pixverse lip sync, pixverse avatar, extend video, video continuation, compose subjects video, reference to video"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# PixVerse Video Generation

Generate, extend, compose, edit and lip sync videos with PixVerse models via [inference.sh](https://inference.sh) CLI.

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

belt app run pixverse/v6 --input '{"prompt": "a hot air balloon rising over misty valleys at sunrise, gentle drift, cinematic wide shot"}'
```

## Models

| Model | App ID | Best For |
|-------|--------|----------|
| PixVerse V6 | `pixverse/v6` | Versatile text-to-video and image-to-video, 360p-1080p, 5-15s |
| PixVerse C1 | `pixverse/c1` | Advanced text-to-video and image-to-video, 360p-1080p, 5-15s |
| PixVerse Extend | `pixverse/extend` | Continue an existing video by 5s or 8s |
| PixVerse Fusion | `pixverse/fusion` | Compose 1-3 subject/background images into one video scene |
| PixVerse Modify | `pixverse/modify` | Edit video with text: swap subjects, add elements, restyle regions |
| PixVerse Lip Sync | `pixverse/lip-sync` | Sync mouth movement in a video to audio or TTS |
| PixVerse Avatar | `pixverse/avatar` | Talking avatar video from a portrait image and audio or TTS |
| PixVerse Lipsync (fal.ai) | `falai/pixverse-lipsync` | Lip sync with audio or TTS from 14 named voices |

V6 and C1 share the same inputs and are billed per second. Prompts of 25-200 words are recommended. List all PixVerse apps with `belt app search "pixverse"`.

## Examples

### Text-to-Video (V6)

```bash
belt app run pixverse/v6 --input '{
  "prompt": "a golden retriever puppy chases soap bubbles across a sunny backyard lawn, low camera following at ground level, shallow depth of field, warm afternoon light",
  "quality": "1080p",
  "duration": 8,
  "aspect_ratio": "16:9",
  "negative_prompt": "blurry, distorted faces"
}'
```

### Image-to-Video (V6)

With an `image`, the video uses the image aspect ratio; `aspect_ratio` applies to text-to-video only.

```bash
belt app run pixverse/v6 --input '{
  "prompt": "the woman turns toward the camera and smiles as wind moves her hair",
  "image": "https://portrait.jpg",
  "quality": "720p",
  "duration": 5
}'
```

### Fast Motion (C1)

```bash
belt app run pixverse/c1 --input '{
  "prompt": "a motorcycle races along a coastal highway, camera tracking alongside, sea spray, late afternoon sun",
  "motion_mode": "fast",
  "quality": "720p",
  "aspect_ratio": "9:16",
  "duration": 5
}'
```

### Extend a Video

Source video: MP4/MOV, max 50MB, max 1920px, max 30s. `duration` is 5 or 8; 1080p supports 5s only. Fast motion mode is available for 5s, not at 1080p.

```bash
belt app run pixverse/extend --input '{
  "video": "https://my-clip.mp4",
  "prompt": "the puppy catches a bubble and tumbles onto the grass",
  "duration": 8,
  "quality": "720p"
}'
```

### Fusion: Compose Subjects into One Scene

Give each image a unique `ref_name` and a `type` of `subject` or `background`. In the prompt, prefix the name with `@` and follow it with a space.

```bash
belt app run pixverse/fusion --input '{
  "prompt": "@dog runs up to @girl on @beach and she throws a frisbee",
  "image_references": [
    {"image": "https://my-dog.jpg", "ref_name": "dog", "type": "subject"},
    {"image": "https://girl.jpg", "ref_name": "girl", "type": "subject"},
    {"image": "https://beach.jpg", "ref_name": "beach", "type": "background"}
  ],
  "model": "v6",
  "quality": "720p",
  "duration": 5,
  "aspect_ratio": "16:9"
}'
```

`model` is `v6` (default) or `c1`.

### Modify a Video

Reference images are addressed as `@img0`, `@img1`, `@img2` (max 3). Mask selections are addressed as `@selection0`, `@selection1`. Output is 540p. Source video: max 100MB, max 1920px, max 30s.

```bash
belt app run pixverse/modify --input '{
  "video": "https://street-scene.mp4",
  "prompt": "replace the red car with the car in @img0",
  "reference_images": ["https://blue-sports-car.jpg"]
}'
```

### Lip Sync with Audio

Source video: MP4/MOV/WebM, max 100MB, max 60s. Audio: MP3/WAV/M4A/AAC, max 60s. Provide either `audio` or `tts_text`.

```bash
belt app run pixverse/lip-sync --input '{
  "video": "https://presenter.mp4",
  "audio": "https://voiceover.mp3"
}'
```

### Lip Sync with TTS

`tts_text` is max 200 chars and requires `tts_speaker_id` (`auto` picks a voice).

```bash
belt app run pixverse/lip-sync --input '{
  "video": "https://presenter.mp4",
  "tts_text": "Welcome back to the channel. Today we are testing three new cameras.",
  "tts_speaker_id": "auto"
}'
```

### Talking Avatar

Portrait: PNG/JPG/WebP, max 20MB. Audio max 30s, or `tts_text` of 30-200 chars with `tts_speaker_id`.

```bash
belt app run pixverse/avatar --input '{
  "image": "https://headshot.jpg",
  "tts_text": "Hi, I am your onboarding guide. Let me show you around the dashboard.",
  "tts_speaker_id": "auto",
  "quality": "1080p",
  "prompt": "friendly expression, small hand gestures"
}'
```

### Lip Sync on fal.ai

```bash
belt app run falai/pixverse-lipsync --input '{
  "video": "https://speaker.mp4",
  "text": "Thanks for watching, see you next week.",
  "voice_id": "Emily"
}'
```

`voice_id` options: Emily, James, Isabella, Liam, Chloe, Adrian, Harper, Ava, Sophia, Julia, Mason, Jack, Oliver, Ethan, Auto (default). Pass `audio` (WAV/MP3) instead of `text` to use your own recording.

## Workflows

### Generate, then Extend

```bash
# 1. Generate the opening clip
belt app run pixverse/v6 --input '{
  "prompt": "a lighthouse keeper climbs the spiral stairs carrying a lantern, night, storm outside",
  "quality": "720p",
  "duration": 8
}'

# 2. Continue it (repeat for longer sequences; the source must stay under 30s)
belt app run pixverse/extend --input '{
  "video": "<video-url-from-step-1>",
  "prompt": "he reaches the top and lights the great lamp, beam sweeps across the sea",
  "duration": 8,
  "quality": "720p"
}'
```

### Fusion of Your Own Subjects

```bash
# Product + model + location in one shot
belt app run pixverse/fusion --input '{
  "prompt": "@model holds up @bottle and smiles inside @studio , slow push in",
  "image_references": [
    {"image": "https://model.jpg", "ref_name": "model", "type": "subject"},
    {"image": "https://perfume-bottle.png", "ref_name": "bottle", "type": "subject"},
    {"image": "https://studio.jpg", "ref_name": "studio", "type": "background"}
  ],
  "aspect_ratio": "9:16",
  "duration": 8
}'
```

### Lip Sync a Generated Clip

```bash
# 1. Generate a talking-head shot
belt app run pixverse/v6 --input '{
  "prompt": "medium close-up of a news anchor at a desk looking into the camera, studio lighting, speaking",
  "quality": "1080p",
  "duration": 10
}'

# 2. Make the anchor say your script
belt app run pixverse/lip-sync --input '{
  "video": "<video-url-from-step-1>",
  "audio": "https://script-read.mp3"
}'
```

## Parameters (V6 / C1)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `prompt` | string | required | Video description (25-200 words recommended) |
| `image` | file | - | Image for image-to-video (PNG/JPG/WebP, max 10000px) |
| `quality` | enum | 720p | 360p, 540p, 720p, 1080p |
| `duration` | integer | 5 | 5-15 seconds, billed per second |
| `aspect_ratio` | enum | 16:9 | 16:9, 9:16, 1:1, 4:3, 3:4 (text-to-video only) |
| `negative_prompt` | string | - | What to avoid |
| `motion_mode` | enum | normal | normal, fast |
| `seed` | integer | - | 0-2147483647 |

## Parameters (Extend)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `video` | file | required | MP4/MOV, max 50MB, max 1920px, max 30s |
| `prompt` | string | required | What happens next, max 2048 chars |
| `quality` | enum | 720p | 360p, 540p, 720p, 1080p |
| `duration` | integer | 5 | 5 or 8 (1080p: 5 only) |
| `motion_mode` | enum | normal | normal, fast (5s only, not at 1080p) |
| `negative_prompt` | string | - | What to avoid |
| `seed` | integer | - | Reproducible results |

## Parameters (Fusion)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `prompt` | string | required | Use `@ref_name ` (with trailing space) for each reference |
| `image_references` | object[] | required | 1-3 `{image, ref_name, type}`; `type` is subject (default) or background |
| `model` | enum | v6 | v6, c1 |
| `quality` | enum | 720p | 360p, 540p, 720p, 1080p |
| `duration` | integer | 5 | 5-15 seconds |
| `aspect_ratio` | enum | 16:9 | 16:9, 9:16, 1:1, 4:3, 3:4 |
| `seed` | integer | - | Reproducible results |

## Pricing

Prices vary by resolution and duration. Check with `belt app pricing <app-id>` and estimate a specific input with `belt app estimate <app-id> --input input.json`.

## Related Skills

```bash
# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# All video generation models
npx skills add inference-sh/skills@ai-video-generation

# Image-to-video workflows
npx skills add inference-sh/skills@image-to-video

# AI avatars & lipsync
npx skills add inference-sh/skills@ai-avatar-video

# Video prompting
npx skills add inference-sh/skills@video-prompting-guide

# Other video models
npx skills add inference-sh/skills@kling
npx skills add inference-sh/skills@runway
npx skills add inference-sh/skills@seedance
npx skills add inference-sh/skills@google-veo
```

Browse all video apps: `belt app list --category video`

## Documentation

- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
- [Streaming Results](https://inference.sh/docs/api/sdk/streaming) - Real-time progress updates
- [Content Pipeline Example](https://inference.sh/docs/examples/content-pipeline) - Building media workflows
