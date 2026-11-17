---
name: kling
description: "Generate video and images with Kling AI models via inference.sh CLI. Models: Kling Video V4, V4 Flash, O1 (Omni), V3, Kling Avatar, Lip Sync, Video-to-Audio, Image V3, Image 3O, Virtual Try-On. Capabilities: text-to-video, image-to-video with start and end frames, 3-30s duration, native audio, automatic shot-splitting, reference-image and reference-video editing, talking avatars, lip sync, sound effects and music for video, text-to-image up to 4K, clothing try-on. Use for: cinematic clips, product videos, social media, dubbing, digital presenters, fashion try-on. Triggers: kling, kling ai, kling 4, kling v4, kling 4.0, kling flash, kling o1, kling omni, kling 3, kling v3, kling avatar, kling lip sync, kling lipsync, kolors, kling image, kling try-on, virtual try-on, kuaishou video"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# Kling Video and Image Generation

Generate video, avatars, lip sync, audio and images with Kling AI models via [inference.sh](https://inference.sh) CLI.

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

belt app run klingai/video-v4 --input '{"prompt": "a red fox trotting through fresh snow at dawn"}'
```

## Models

| Model | App ID | Best For |
|-------|--------|----------|
| Kling Video V4 | `klingai/video-v4` | Text/image-to-video, start and end frames, 3-30s, 720p/1080p, native audio |
| Kling Video V4 Flash | `klingai/video-v4-flash` | Faster, cheaper V4. 720p, 3-20s, first-frame image-to-video |
| Kling Video O1 | `klingai/video-o1` | Omni model: reference images, reference video for style transfer or editing |
| Kling Video V3 | `klingai/video-v3` | Up to 4K, multi-shot, 3-15s, synchronized audio |
| Kling Avatar | `klingai/avatar` | Talking head video from one face photo and audio |
| Kling Lip Sync | `klingai/lip-sync` | Drive mouth movement in an existing video with audio |
| Kling Video to Audio | `klingai/video-to-audio` | Add sound effects and background music to a 3-20s video |
| Kling Image V3 | `klingai/image-v3` | Text-to-image at 1K/2K |
| Kling Image 3O | `klingai/image-3o` | Text-to-image and image-to-image up to 4K, series images |
| Kling Virtual Try-On | `klingai/virtual-tryon` | Put a clothing item on a person photo |

V4 and V4 Flash bill per second and split shots automatically. A `duration` field takes precedence over a duration named in the prompt.

Older versions (Video V2.5, V2.6, Image V1 to V2.1, Image O1) are still available:

```bash
belt app search "kling"
```

## Examples

### Text-to-Video (V4)

```bash
belt app run klingai/video-v4 --input '{
  "prompt": "a chef plating a dessert in a busy restaurant kitchen, close-up, steam rising",
  "duration": 10,
  "resolution": "1080p",
  "aspect_ratio": "16:9"
}'
```

### Image-to-Video with Start and End Frames (V4)

```bash
belt app run klingai/video-v4 --input '{
  "prompt": "the flower slowly blooms as morning light hits it",
  "image": "https://closed-bud.jpg",
  "end_image": "https://open-flower.jpg",
  "duration": 5
}'
```

With an `image`, the video follows the image aspect ratio; `aspect_ratio` applies to text-to-video only.

### Fast Vertical Clip (V4 Flash)

```bash
belt app run klingai/video-v4-flash --input '{
  "prompt": "a skateboarder kickflips down a stair set, handheld camera",
  "aspect_ratio": "9:16",
  "duration": 8
}'
```

### Reference Images (O1)

Reference inputs in the prompt as `@image_1`, `@image_2`. Up to 7 reference images without a video, 4 with one.

```bash
belt app run klingai/video-o1 --input '{
  "prompt": "@image_1 walks through the market from @image_2, evening light",
  "reference_images": ["https://character.jpg", "https://market.jpg"],
  "aspect_ratio": "16:9",
  "duration": 8
}'
```

### Edit a Video (O1)

`reference_video_type` is `feature` (use the video as a style/motion reference) or `base` (edit the video directly).

```bash
belt app run klingai/video-o1 --input '{
  "prompt": "change the season in @video_1 to winter with falling snow",
  "reference_video": "https://your-video.mp4",
  "reference_video_type": "base"
}'
```

### 4K Multi-Shot (V3)

```bash
belt app run klingai/video-v3 --input '{
  "prompt": "a lighthouse on a cliff during a storm, waves crashing, lightning",
  "resolution": "4k",
  "duration": 10,
  "sound": true,
  "multi_shot": true
}'
```

### Talking Avatar

```bash
belt app run klingai/avatar --input '{
  "image": "https://face-photo.jpg",
  "audio": "https://speech.mp3",
  "prompt": "while talking, nods and smiles warmly",
  "mode": "pro",
  "aspect_ratio": "9:16"
}'
```

### Lip Sync an Existing Video

```bash
belt app run klingai/lip-sync --input '{
  "video": "https://speaker.mp4",
  "audio": "https://new-dialogue.mp3",
  "original_audio_volume": 0
}'
```

### Add Sound to a Video

```bash
belt app run klingai/video-to-audio --input '{
  "video": "https://beach-clip.mp4",
  "sound_effect_prompt": "ocean waves, seagulls, gentle wind",
  "bgm_prompt": "calm acoustic guitar"
}'
```

### Images

```bash
# Image V3, 2K
belt app run klingai/image-v3 --input '{
  "prompt": "minimalist product shot of a ceramic mug on linen, soft daylight",
  "resolution": "2k",
  "aspect_ratio": "4:3",
  "n": 2
}'

# Image 3O, 4K with a reference image
belt app run klingai/image-3o --input '{
  "prompt": "the same sneaker on a wet city street at night, neon reflections",
  "image": "https://sneaker.jpg",
  "resolution": "4k",
  "aspect_ratio": "3:2"
}'
```

### Virtual Try-On

```bash
belt app run klingai/virtual-tryon --input '{
  "human_image": "https://person-full-body.jpg",
  "cloth_image": "https://jacket-product.jpg"
}'
```

V1.5 (default, `kolors-virtual-try-on-v1-5`) accepts upper and lower garments merged into one clothing image.

## Workflow: Image to Video with Sound

```bash
# 1. Generate a keyframe
belt app run klingai/image-v3 --input '{"prompt": "astronaut standing on a red desert planet, wide shot", "aspect_ratio": "16:9"}'

# 2. Animate it with V4
belt app run klingai/video-v4 --input '{"prompt": "the astronaut starts walking toward the horizon, dust blowing", "image": "<image-url-from-step-1>"}'
```

## Parameters (Video V4)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `prompt` | string | required | Text description of the video |
| `image` | file | - | Start frame (jpg/png, max 30MB, min 300px per side, aspect ratio 2:5 to 5:2) |
| `end_image` | file | - | End frame, requires `image` |
| `resolution` | enum | 1080p | 720p, 1080p |
| `aspect_ratio` | enum | 16:9 | 21:9, 16:9, 9:16, 1:1 (text-to-video only) |
| `duration` | integer | 5 | 3-30 seconds, billed per second |

V4 Flash takes `prompt`, `image`, `aspect_ratio` (21:9, 16:9, 9:16, 1:1) and `duration` (3-20). It has no `end_image` or `resolution` and outputs 720p.

## Parameters (Video O1)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `prompt` | string | required | Use `@image_1`, `@video_1` to reference inputs. Max 2500 chars |
| `image` / `end_image` | file | - | First and end frame |
| `reference_images` | file[] | [] | Up to 7 (4 with a reference video) |
| `reference_video` | file | - | Video for style/motion reference or editing |
| `reference_video_type` | string | feature | `feature` or `base` |
| `resolution` | enum | 1080p | 720p, 1080p |
| `aspect_ratio` | enum | - | 16:9, 9:16, 1:1. Required for text-to-video |
| `duration` | integer | 5 | Text-to-video: 5 or 10. With reference images: 3-10 |
| `watermark` | boolean | false | Add watermark |

## Parameters (Video V3)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `prompt` | string | required | Max 3072 chars (2500 recommended) |
| `image` / `end_image` | file | - | Start and end frame |
| `sound` | boolean | true | Generate synchronized audio |
| `multi_shot` | boolean | true | Multi-shot generation via prompt format |
| `resolution` | enum | 1080p | 720p, 1080p, 4k |
| `aspect_ratio` | enum | 16:9 | 16:9, 9:16, 1:1 |
| `duration` | integer | 5 | 3-15 seconds |

## Pricing

| App | Price |
|-----|-------|
| `klingai/video-v4` | $0.157/s at 720p, $0.235/s at 1080p |
| `klingai/video-v4-flash` | $0.094/s at 720p |
| `klingai/avatar` | $0.056/s (std), $0.112/s (pro) |
| `klingai/lip-sync` | $0.014/s, 5s minimum |
| `klingai/video-to-audio` | $0.0088 per video |
| `klingai/image-v3` | $0.0035 per image |
| `klingai/image-3o` | $0.028 per image (1K/2K), $0.056 (4K) |

For O1, V3 and Try-On, check `belt app pricing <app-id>`. Estimate a run before paying: `belt app estimate <app-id> --input input.json`.

## Sample Workflow

```bash
# 1. Generate sample input to see all options
belt app sample klingai/video-v4 --save input.json

# 2. Edit the prompt
# 3. Run
belt app run klingai/video-v4 --input input.json
```

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
npx skills add inference-sh/skills@seedance
npx skills add inference-sh/skills@google-veo
npx skills add inference-sh/skills@runway
npx skills add inference-sh/skills@pixverse
```

Browse all video apps: `belt app list --category video`

## Documentation

- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
- [Streaming Results](https://inference.sh/docs/api/sdk/streaming) - Real-time progress updates
- [Content Pipeline Example](https://inference.sh/docs/examples/content-pipeline) - Building media workflows
