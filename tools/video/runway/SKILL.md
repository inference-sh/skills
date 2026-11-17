---
name: runway
description: "Generate and transform video and images with Runway models via inference.sh CLI. Models: Gen-4.5, Gen-4 Turbo, Aleph 2.0, Act-Two, Gen-4 Image, Gen-4 Image Turbo. Capabilities: text-to-video, image-to-video, 2-10s duration, video-to-video transformation with keyframe guidance, character performance transfer from a reference video, text-to-image with tagged reference images. Use for: cinematic clips, animating stills, restyling footage, reframing aspect ratio, character animation, consistent characters across images. Triggers: runway, runwayml, runway gen 4, gen-4, gen 4.5, gen-4.5, gen-4 turbo, runway aleph, aleph 2, act-two, act two, runway act, runway image, gen-4 image, runway video, performance transfer"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# Runway Video and Image Generation

Generate, transform and animate video and images with Runway models via [inference.sh](https://inference.sh) CLI.

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

belt app run runway/gen-4-5 --input '{"prompt": "a paper boat drifting down a rain-soaked city gutter, macro shot"}'
```

## Models

| Model | App ID | Best For |
|-------|--------|----------|
| Gen-4.5 | `runway/gen-4-5` | Highest quality text-to-video and image-to-video, 2-10s |
| Gen-4 Turbo | `runway/gen-4-turbo` | Fast image-to-video (image required) |
| Aleph 2.0 | `runway/aleph-2` | Video-to-video transformation with text and keyframe guidance |
| Act-Two | `runway/act-two` | Animate a character image or video with a reference performance |
| Gen-4 Image | `runway/gen-4-image` | Text-to-image with optional tagged reference images |
| Gen-4 Image Turbo | `runway/gen-4-image-turbo` | Fast image generation guided by a required reference image |

Runway apps use pixel ratios (`1280:720`) instead of `16:9`. Aleph 2.0 is the exception: its `target_aspect_ratio` uses `16:9` style values.

## Search Runway Apps

```bash
belt app search "runway"
```

## Examples

### Text-to-Video (Gen-4.5)

Text-to-video supports `1280:720` or `720:1280` only.

```bash
belt app run runway/gen-4-5 --input '{
  "prompt": "a lone hiker crossing a glacier at sunset, wide drone shot, slow push in",
  "ratio": "1280:720",
  "duration": 10
}'
```

### Image-to-Video (Gen-4.5)

Image-to-video adds `1104:832`, `832:1104`, `960:960` and `1584:672`.

```bash
belt app run runway/gen-4-5 --input '{
  "prompt": "the cat turns its head toward the window, curtains moving in the breeze",
  "image": "https://cat-photo.jpg",
  "ratio": "960:960",
  "duration": 5
}'
```

### Fast Image-to-Video (Gen-4 Turbo)

```bash
belt app run runway/gen-4-turbo --input '{
  "prompt": "slow orbit around the product, soft studio light",
  "image": "https://product.jpg",
  "ratio": "720:1280",
  "duration": 5
}'
```

### Transform a Video (Aleph 2.0)

```bash
belt app run runway/aleph-2 --input '{
  "video": "https://street-clip.mp4",
  "prompt": "turn the scene into a snowy winter evening with warm street lights"
}'
```

### Reframe and Guide with Keyframes (Aleph 2.0)

Up to 5 keyframes, each with a reference image at a timestamp. `range` sets how many seconds around the timestamp the guidance applies.

```bash
belt app run runway/aleph-2 --input '{
  "video": "https://interview.mp4",
  "prompt": "restyle as a hand-painted watercolor animation",
  "target_aspect_ratio": "9:16",
  "keyframes": [
    {"seconds": 0, "image": "https://watercolor-style-frame.jpg"},
    {"seconds": 4, "image": "https://watercolor-style-frame-2.jpg", "range": 1}
  ]
}'
```

### Performance Transfer (Act-Two)

The character mimics the performance in `reference_video` (3-30s, max 16MB).

```bash
belt app run runway/act-two --input '{
  "character": "https://cartoon-character.png",
  "character_type": "image",
  "reference_video": "https://me-talking.mp4",
  "body_control": true,
  "expression_intensity": 4,
  "ratio": "1280:720"
}'
```

Use `"character_type": "video"` to drive a character that is already in a video (max 16MB).

### Text-to-Image (Gen-4 Image)

```bash
belt app run runway/gen-4-image --input '{
  "prompt": "editorial photo of a vintage espresso machine on a marble counter",
  "ratio": "1920:1080"
}'
```

### Tagged References (Gen-4 Image)

Tag reference images and use `@tagName` in the prompt to keep characters or objects consistent.

```bash
belt app run runway/gen-4-image --input '{
  "prompt": "@maya sitting in @cafe reading a book, morning light",
  "reference_images": [
    {"image": "https://maya-portrait.jpg", "tag": "maya"},
    {"image": "https://cafe-interior.jpg", "tag": "cafe"}
  ],
  "ratio": "1360:768"
}'
```

### Fast Image Edit (Gen-4 Image Turbo)

```bash
belt app run runway/gen-4-image-turbo --input '{
  "prompt": "the character standing on @rooftop at night, city lights behind",
  "image": "https://hero-character.jpg",
  "additional_references": [{"image": "https://rooftop.jpg", "tag": "rooftop"}],
  "ratio": "1024:1024"
}'
```

## Workflow: Consistent Character to Video

```bash
# 1. Create a keyframe with a tagged character reference
belt app run runway/gen-4-image --input '{
  "prompt": "@maya on a train platform in the rain",
  "reference_images": [{"image": "https://maya-portrait.jpg", "tag": "maya"}],
  "ratio": "1280:720"
}'

# 2. Animate the keyframe
belt app run runway/gen-4-5 --input '{
  "prompt": "the train arrives, her coat moves in the wind, she looks up",
  "image": "<image-url-from-step-1>",
  "ratio": "1280:720",
  "duration": 8
}'
```

## Parameters (Gen-4.5)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `prompt` | string | required | Text description, max 1000 chars |
| `image` | file | - | Start image for image-to-video |
| `ratio` | enum | 1280:720 | 1280:720, 720:1280 (text-to-video); also 1104:832, 832:1104, 960:960, 1584:672 with an image |
| `duration` | integer | 5 | 2-10 seconds |
| `seed` | integer | - | Reproducible results |

Gen-4 Turbo takes the same fields, with `image` required.

## Parameters (Aleph 2.0)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `video` | file | required | Input video, max 16MB |
| `prompt` | string | - | Transformation guidance |
| `target_aspect_ratio` | enum | - | 16:9, 4:3, 3:2, 1:1, 2:3, 3:4, 9:16, 21:9 |
| `keyframes` | object[] | - | Up to 5 `{seconds, image, range}` entries |
| `seed` | integer | - | Reproducible results |

## Parameters (Act-Two)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `character` | file | required | Image (max 5MB) or video (max 16MB) |
| `character_type` | enum | image | image, video |
| `reference_video` | file | required | Performance video, 3-30s, max 16MB |
| `ratio` | enum | 1280:720 | 1280:720, 720:1280, 960:960, 1104:832, 832:1104, 1584:672 |
| `body_control` | boolean | false | Enable non-facial movement and gestures |
| `expression_intensity` | integer | - | 1-5, higher is more expressive |
| `seed` | integer | - | Reproducible results |

## Parameters (Gen-4 Image / Turbo)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `prompt` | string | required | Use `@tagName` to reference tagged images |
| `image` | file | required (Turbo only) | Reference image that Turbo transforms |
| `reference_images` | object[] | - | Gen-4 Image: `{image, tag}` entries |
| `additional_references` | object[] | - | Turbo: extra `{image, tag}` entries |
| `ratio` | enum | 1280:720 | 1280:720, 720:1280, 720:720, 960:720, 720:960, 1680:720, 1920:1080, 1080:1920, 1024:1024, 1080:1080, 1168:880, 1360:768, 1440:1080, 1080:1440, 1808:768, 2112:912 |
| `seed` | integer | - | Reproducible results |

## Pricing

| App | Price |
|-----|-------|
| `runway/gen-4-5` | $0.12/sec |
| `runway/gen-4-turbo` | $0.05/sec |
| `runway/aleph-2` | $0.28/sec |
| `runway/act-two` | $0.05/sec |
| `runway/gen-4-image` | $0.05/image (720p), $0.08/image (1080p) |
| `runway/gen-4-image-turbo` | $0.02/image |

Estimate before running: `belt app estimate runway/gen-4-5 --input input.json`.

## Sample Workflow

```bash
# 1. Generate sample input to see all options
belt app sample runway/gen-4-5 --save input.json

# 2. Edit the prompt
# 3. Run
belt app run runway/gen-4-5 --input input.json
```

## Related Skills

```bash
# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# All video generation models
npx skills add inference-sh/skills@ai-video-generation

# Image-to-video workflows
npx skills add inference-sh/skills@image-to-video

# Video prompting
npx skills add inference-sh/skills@video-prompting-guide

# Other video models
npx skills add inference-sh/skills@kling
npx skills add inference-sh/skills@pixverse
npx skills add inference-sh/skills@seedance
npx skills add inference-sh/skills@google-veo
```

Browse all video apps: `belt app list --category video`

## Documentation

- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
- [Streaming Results](https://inference.sh/docs/api/sdk/streaming) - Real-time progress updates
- [Content Pipeline Example](https://inference.sh/docs/examples/content-pipeline) - Building media workflows
