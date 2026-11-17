---
name: seedance
description: "Generate videos with ByteDance Seedance 2.5 via inference.sh CLI. Unified model for text-to-video, image-to-video, and reference-to-video with synchronized audio, up to 1080p, 4-30s duration. Draft mode for fast previews. Studio variant with private asset library for portrait consistency. Use for: social media videos, music videos, product demos, animated content, AI video with sound. Triggers: seedance, seedance 2, seedance 2.5, bytedance video, seedance t2v, seedance i2v, seedance r2v, video with audio, bytedance seedance, seedance studio"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# Seedance 2.5 Video Generation

Generate videos with synchronized audio using ByteDance's Seedance 2.5 via [inference.sh](https://inference.sh) CLI.

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

belt app run bytedance/seedance-2-5 --input '{
  "prompt": "a jazz band performing in a dimly lit club",
  "generate_audio": true
}'
```


## Models

| Model | App ID | Best For |
|-------|--------|----------|
| Seedance 2.5 | `bytedance/seedance-2-5` | Best quality, up to 1080p, 4-30s |
| Seedance 2.5 Studio | `bytedance/seedance-2-5-studio` | Quality + private asset library for portrait consistency |

Both models support text-to-video, image-to-video, multimodal reference-to-video, and synchronized audio generation. Set `"draft": true` for a fast, low-cost 480p preview; pass its `draft_task_id` later to render the chosen take at 1080p. The Studio variant automatically upload reference images to the BytePlus private virtual portrait library for enhanced character consistency - particularly useful for faces and branded characters.

## Modes

The model determines the generation mode from your inputs. These modes are **mutually exclusive** - use either first-frame/last-frame OR reference inputs, not both.

| Mode | Inputs | Description |
|------|--------|-------------|
| Text-to-Video | `prompt` only | Generate video from text description |
| Image-to-Video | `prompt` + `image` | Animate a still image (first frame) |
| First+Last Frame | `prompt` + `image` + `end_image` | Control start and end frames |
| Multimodal Reference | `prompt` + `reference_images`/`reference_videos`/`reference_audios` | Guide generation with reference material |

## Examples

### Text-to-Video with Audio

```bash
belt app run bytedance/seedance-2-5 --input '{
  "prompt": "ocean waves crashing on rocks during a storm, dramatic cinematic shot",
  "generate_audio": true,
  "duration": 10,
  "ratio": "16:9"
}'
```

### Draft Mode (Cheaper)

```bash
belt app run bytedance/seedance-2-5 --input '{
  "prompt": "a butterfly landing on a flower in slow motion",
  "generate_audio": true,
  "draft": true
}'
```

### Image-to-Video

Animate a still image into a video:

```bash
belt app run bytedance/seedance-2-5 --input '{
  "image": "https://your-image.jpg",
  "prompt": "gentle camera movement, leaves rustling in the wind",
  "generate_audio": true
}'
```

### Image-to-Video with Start and End Frames

```bash
belt app run bytedance/seedance-2-5 --input '{
  "image": "https://start-frame.jpg",
  "end_image": "https://end-frame.jpg",
  "prompt": "smooth transition between scenes",
  "generate_audio": true
}'
```

### Multi-Image Reference

Use multiple reference images to guide character appearance, outfits, and scene elements:

```bash
belt app run bytedance/seedance-2-5 --input '{
  "prompt": "The girl from Image 1 wearing the outfit from Image 2 walks through the cafe from Image 3",
  "reference_images": [
    "https://character-portrait.jpg",
    "https://outfit-reference.jpg",
    "https://cafe-scene.jpg"
  ],
  "generate_audio": true,
  "duration": 8
}'
```

### Video Editing (Replace Elements)

```bash
belt app run bytedance/seedance-2-5 --input '{
  "prompt": "Replace the perfume in Video 1 with the face cream from Image 1, preserving all original motions and camera work",
  "reference_images": ["https://face-cream.jpg"],
  "reference_videos": ["https://original-video.mp4"],
  "generate_audio": true
}'
```

### Video Extension (Stitch Clips)

```bash
belt app run bytedance/seedance-2-5 --input '{
  "prompt": "Video 1 transitions smoothly into Video 2, then the camera enters the painting from Video 3",
  "reference_videos": [
    "https://clip1.mp4",
    "https://clip2.mp4",
    "https://clip3.mp4"
  ],
  "generate_audio": true,
  "duration": 8
}'
```

### Reference with Audio

```bash
belt app run bytedance/seedance-2-5 --input '{
  "prompt": "The musician from Image 1 performs the song from Audio 1, voice style referenced from Audio 1",
  "reference_images": ["https://musician.jpg"],
  "reference_audios": ["https://music.mp3"],
  "generate_audio": true
}'
```

### Studio Mode (Portrait Consistency)

The Studio variant uploads images to BytePlus's private asset library for enhanced face/character consistency:

```bash
belt app run bytedance/seedance-2-5-studio --input '{
  "prompt": "The person in Image 1 smiles at the camera, golden hour lighting, cinematic",
  "reference_images": ["https://portrait.jpg"],
  "safety_identifier": "user-abc123",
  "generate_audio": true
}'
```

### Product Ad with Multiple References

```bash
belt app run bytedance/seedance-2-5 --input '{
  "prompt": "First-person POV product ad. Opening frame is Image 1, hand picks up the product. Camera pushes into close-up showing details. Use the camera movement style from Video 1. Background music from Audio 1.",
  "reference_images": ["https://product-hero.jpg", "https://product-detail.jpg"],
  "reference_videos": ["https://camera-style.mp4"],
  "reference_audios": ["https://bgm.mp3"],
  "generate_audio": true,
  "ratio": "9:16",
  "duration": 11
}'
```

## Prompt Guide

Reference assets in your prompt using **type + index**: `Image 1`, `Image 2`, `Video 1`, `Audio 1`. The index is the position within that type in the arrays you provide. Do NOT use asset IDs in prompts.

**Multimodal reference formula:**
- Image reference: "Refer to the [subject] from [Image N] to generate [scene], keeping [subject] consistent"
- Video reference: "Refer to the [camera movement/action] from [Video N]"
- Audio reference: "[Character] says: [dialogue], voice style referenced from [Audio N]"

**Video editing formula:**
- Add: "At [timing] of [Video N], add [element]"
- Remove: "Remove [element] from [Video N], keeping the rest unchanged"
- Modify: "Replace [element] in [Video N] with [new element]"

**Video extension formula:**
- Forward: "Generate content after [Video N]: [description]"
- Backward: "Extend the opening of [Video N]: [description]"
- Stitch: "[Video 1] + [transition] + followed by [Video 2]"

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `prompt` | string | required | Text description of the video |
| `generate_audio` | boolean | true | Generate synchronized audio |
| `duration` | integer | -1 | Duration in seconds (4-30), or -1 for auto |
| `ratio` | enum | adaptive | 21:9, 16:9, 4:3, 1:1, 3:4, 9:16, or adaptive |
| `resolution` | enum | 720p | 480p, 720p, 1080p |
| `seed` | integer | -1 | Seed for reproducibility (-1 for random) |
| `watermark` | boolean | false | Add watermark to output |
| `draft` | boolean | false | Fast 480p preview; returns a `draft_task_id` to render the final 1080p take |
| `safety_identifier` | string | - | Unique end-user identifier for safety policy (max 64 chars, hash of user ID recommended) |
| `image` | file | - | First-frame image (mutually exclusive with reference inputs) |
| `end_image` | file | - | Last-frame image (requires `image`) |
| `reference_images` | file[] | - | Reference images, up to 30 (mutually exclusive with image/end_image) |
| `reference_videos` | file[] | - | Reference videos, up to 10. 2-30s each, total max 30s. mp4/mov |
| `reference_audios` | file[] | - | Reference audios, up to 10. 2-30s each, total max 30s. wav/mp3. Audio-only input is supported |

## Pricing

| Model | Pricing |
|-------|---------|
| Seedance 2.5 | $6.40-$11.70/M tokens (varies by resolution and input type) |
| Seedance 2.5 Studio | $6.40-$11.70/M tokens |

Token formula: `(width x height x fps x duration) / 1024`

## Search Seedance Apps

```bash
belt app search "seedance"
```

## Related Skills

```bash
# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# All video generation models
npx skills add inference-sh/skills@ai-video-generation

# Google Veo
npx skills add inference-sh/skills@google-veo

# Image generation (for image-to-video)
npx skills add inference-sh/skills@ai-image-generation

# AI avatars & lipsync
npx skills add inference-sh/skills@ai-avatar-video
```

Browse all video apps: `belt app list --category video`

## Documentation

- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
- [Streaming Results](https://inference.sh/docs/api/sdk/streaming) - Real-time progress updates
- [Content Pipeline Example](https://inference.sh/docs/examples/content-pipeline) - Building media workflows
