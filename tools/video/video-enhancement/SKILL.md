---
name: video-enhancement
description: "Upscale, restore, denoise, interpolate and edit existing videos via inference.sh CLI. Models: Topaz Proteus, Starlight, Astra, Nyx denoise, Apollo/Chronos/Aion frame interpolation, Themis deblur, colorization, Hyperion SDR to HDR, RIFE, Bria video upscale, background removal, green screen, background replacement, video eraser, Pruna P-Video-Upscale. Use for: upscale video to 1080p/4K/8K, restore old or compressed footage, deinterlace, remove noise and grain, slow motion, 60fps conversion, colorize black and white video, remove or replace video background, erase objects from video. Triggers: video upscale, upscale video, video upscaler, 4k video, enhance video, video enhancement, restore video, topaz video, topaz video ai, proteus, starlight, astra, denoise video, video noise reduction, deinterlace, frame interpolation, slow motion, increase fps, 60fps, rife, colorize video, sdr to hdr, deblur video, video background removal, green screen video, replace video background, video eraser"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# Video Enhancement

Improve existing videos via [inference.sh](https://inference.sh) CLI: upscale, restore, denoise, raise frame rate, and remove or replace backgrounds and objects.

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

belt app run pruna/p-video-upscale --input '{
  "video": "https://your-video.mp4",
  "target": "4k"
}'
```

## Apps by Task

### Upscale

| App | App ID | Approach | Pricing |
|-----|--------|----------|---------|
| P-Video-Upscale | `pruna/p-video-upscale` | Upscale to 720p/1080p/2k/4k target, keeps aspect ratio and audio, optional interpolation to 60fps | $0.01/s 720p, $0.02/s 1080p, $0.04/s 2k, $0.08/s 4k; 2x above 30fps |
| Topaz Proteus | `topaz/proteus` | Precision upscaling with manual controls, deinterlacing, face recovery, CGI enhancement | $0.0714/Topaz credit |
| Topaz Starlight | `topaz/starlight` | Generative upscaling | $0.0714/Topaz credit |
| Topaz Astra | `topaz/astra` | Creative AI-guided upscaling with prompt and creativity controls | $0.0714/Topaz credit |
| Topaz Video Upscale | `topaz/video-upscale` | Proteus-family upscaling selected by Topaz model codes | not published |
| Topaz Video Upscaler (fal) | `falai/topaz-video-upscaler` | Simple upscale factor, optional fps interpolation | $0.01/s up to 720p, $0.02/s 720p-1080p, $0.08/s above 1080p; 60fps 2x |
| Bria Video Increase Resolution | `bria/video-increase-resolution` | 2x or 4x super-resolution, output up to 8K, max 60s input | $0.02/s |

### Denoise and Restore

| App | App ID | Approach | Pricing |
|-----|--------|----------|---------|
| Topaz Denoise | `topaz/denoise` | Nyx models for noise, compression and artifact removal | $0.0714/Topaz credit |
| Topaz Video Utilities | `topaz/video-utilities` | Motion deblur (themis-2), colorization, SDR to HDR (hyperion-2) | $0.0714/Topaz credit |

### Frame Rate and Slow Motion

| App | App ID | Approach | Pricing |
|-----|--------|----------|---------|
| Topaz Frame Interpolation | `topaz/frame-interpolation` | Apollo, Chronos, Aion models; slowmo up to 16x, 15-240fps | $0.0714/Topaz credit |
| RIFE Video Interpolation | `infsh/rife-video-interpolation` | Target 24/30/60/120fps, fast and slow-motion modes | GPU time, per ms |

### Background

| App | App ID | Approach | Pricing |
|-----|--------|----------|---------|
| Bria Video Background Removal | `bria/video-rmbg` | Transparent or solid color background | $0.0033/s |
| Bria Video Green Screen | `bria/video-green-screen` | Green or blue screen behind the foreground | $0.0033/s |
| Bria Video Replace Background | `bria/video-replace-background` | New background from an image or video | $0.0033/s |

### Object Removal

| App | App ID | Approach | Pricing |
|-----|--------|----------|---------|
| Bria Video Eraser | `bria/video-eraser` | Erase regions defined by a mask video; max 5s, 750p | $0.02/s |

Topaz apps bill in Topaz credits. Check any app with `belt app pricing <app-id>` and estimate a run with `belt app run <app-id> --estimate --input '{...}'`.

## Proteus vs Starlight vs Astra

| | Proteus | Starlight | Astra |
|---|---------|-----------|-------|
| Type | Precision upscaling and enhancement | Generative upscaling | Creative, AI-guided upscaling |
| Control | Model choice plus sliders: compression, details, noise, halo, blur, grain, recover detail | Model choice only (hq has extra options) | `prompt`, `creativity`, `realism`, `sharpness` |
| Also does | Deinterlacing, face recovery, CGI/animation | - | Text-guided detail (prompt limits input to 450 frames) |

- **Proteus** when you need faithful, tunable cleanup of real footage, interlaced sources, faces, or CGI.
- **Starlight** for generative upscaling where the model reconstructs detail.
- **Astra** when you want to steer the result with a prompt and creativity level.

## Examples

### Topaz Proteus

```bash
belt app run topaz/proteus --input '{
  "video": "https://old-footage.mp4",
  "model": "proteus",
  "scale": 2,
  "compression": 0.3,
  "noise": 0.2,
  "output_container": "mp4"
}'
```

Models: `proteus` (general), `proteus-natural` (natural look), `rhea` (detail-rich), `theia-detail`/`theia-fidelity` (temporal), `artemis-hq`/`artemis-mq`/`artemis-lq` and other `artemis-*` (legacy footage), `dione-dv`/`dione-tv` and other `dione-*` (deinterlace), `gaia-cg`/`gaia-hq` (CGI/animation), `iris-mq`/`iris-lq` (face recovery). Input max 500MB.

Deinterlace an old broadcast:

```bash
belt app run topaz/proteus --input '{
  "video": "https://tv-recording.mp4",
  "model": "dione-tv",
  "video_type": "interlaced",
  "scale": 2
}'
```

`video_type`: `progressive`, `interlaced`, `progressive-interlaced`. `focus_fix`: `none`, `normal`, `strong`. `output_container`: `mp4`, `mov`, `mkv`.

### Topaz Starlight

```bash
belt app run topaz/starlight --input '{
  "video": "https://low-res.mp4",
  "model": "precise-2.5",
  "scale": 2
}'
```

Models: `precise-2.5` (best quality, default), `hq`, `mini` (lightweight), `sharp` (detail emphasis), `fast-2` (speed). `upscaling_factor`, `low_sharpness`, and `keep_original` apply to the `hq` model only.

### Topaz Astra

```bash
belt app run topaz/astra --input '{
  "video": "https://clip.mp4",
  "scale": 2,
  "prompt": "crisp details, natural skin texture, sharp foliage",
  "creativity": 0.4,
  "sharpness": 0.6
}'
```

`creativity`, `realism`, `sharpness` are 0-1. Setting `prompt` limits input to 450 frames.

### Other Upscalers

```bash
# Topaz Video Upscale: Topaz model codes (prob-4 default, pnat-1, rhea-1, ahq-12, gcg-5, iris-3, ...)
belt app run topaz/video-upscale --input '{"video": "https://clip.mp4", "model": "prob-4", "scale": 2}'

# Fal Topaz Upscaler: target_fps 16-60; H265 output unless H264_output is true
belt app run falai/topaz-video-upscaler --input '{"video": "https://clip.mp4", "upscale_factor": 2, "target_fps": 60, "H264_output": true}'

# Bria: desired_increase 2 or 4 (required), max 60s input, output up to 8K
belt app run bria/video-increase-resolution --input '{"video_url": "https://clip.mp4", "desired_increase": 4}'
```

`topaz/video-upscale` uses capitalized enums (`video_type`: `Progressive`; `focus_fix_level`: `None`). Prefer `topaz/proteus` for readable model names.

### Topaz Denoise

```bash
belt app run topaz/denoise --input '{
  "video": "https://noisy-night.mp4",
  "model": "nyx-xl",
  "noise": 0.5,
  "compression": 0.3
}'
```

Models: `nyx` (balanced), `nyx-high-fidelity` (detail preservation), `nyx-xl` (heavy noise), `nyx-fast` (speed). Sliders `compression`, `details`, `noise`, `halo`, `blur` take -1 to 1; `grain` 0-0.1.

### Deblur, Colorize, SDR to HDR

```bash
belt app run topaz/video-utilities --input '{"video": "https://shaky.mp4", "model": "themis-2"}'      # motion deblur
belt app run topaz/video-utilities --input '{"video": "https://bw-film.mp4", "model": "colorization"}' # b&w to color
belt app run topaz/video-utilities --input '{"video": "https://sdr.mp4", "model": "hyperion-2"}'    # SDR to HDR
```

The enhancement sliders and `video_type`/`focus_fix` apply to `themis-2` only.

### Frame Interpolation

```bash
# 60fps with Topaz Apollo
belt app run topaz/frame-interpolation --input '{
  "video": "https://24fps.mp4",
  "model": "apollo",
  "target_fps": 60
}'

# 4x slow motion, large motion model
belt app run topaz/frame-interpolation --input '{
  "video": "https://action.mp4",
  "model": "aion",
  "slowmo": 4
}'

# RIFE to 60fps, quality mode
belt app run infsh/rife-video-interpolation --input '{
  "video": "https://clip.mp4",
  "target_fps": 60,
  "fast_mode": false
}'
```

Topaz models: `apollo` (best quality), `apollo-fast`, `chronos` (optical flow), `chronos-fast`, `aion` (large motion). `slowmo` 1-16, `target_fps` 15-240. RIFE `target_fps`: `24`, `30`, `60`, `120`; `slow_motion: true` plays at half the interpolated rate.

### Background Removal, Green Screen, Replacement

```bash
# Transparent background
belt app run bria/video-rmbg --input '{
  "video_url": "https://talking.mp4",
  "background_color": "Transparent"
}'

# Green screen for editing
belt app run bria/video-green-screen --input '{
  "video_url": "https://talking.mp4",
  "green_shade": "chroma_green"
}'

# Replace with an image or video
belt app run bria/video-replace-background --input '{
  "video_url": "https://talking.mp4",
  "background_url": "https://office-background.jpg"
}'
```

- `background_color`: `Transparent` (default), `Black`, `White`, `Gray`, `Red`, `Green`, `Blue`, `Yellow`, `Cyan`, `Magenta`, `Orange`.
- `green_shade`: `broadcast_green` (default), `chroma_green`, `blue_screen`.
- Inputs up to 60s and 16K. Audio is kept by default (`preserve_audio`).

### Erase Objects

```bash
belt app run bria/video-eraser --input '{
  "video_url": "https://clip-5s.mp4",
  "mask_url": "https://mask-video.mp4"
}'
```

The mask is a video: white erases, black keeps. Input max 5s and 750p.

Bria output presets (`output_container_and_codec`): `mp4_h264` (default), `mp4_h265`, `webm_vp9`, `mov_h265`, `mov_proresks`, `mkv_h264`, `mkv_h265`, `mkv_vp9`, `gif`.

## Workflows

### Restore Old Footage

```bash
# 1. Remove noise and compression artifacts
belt app run topaz/denoise --input '{"video": "https://vhs-transfer.mp4", "model": "nyx-high-fidelity"}' -o step1.json

# 2. Deinterlace and upscale
belt app run topaz/proteus --input '{"video": "<video-url-from-step-1>", "model": "dione-tv", "video_type": "interlaced", "scale": 2}' -o step2.json

# 3. Smooth to 60fps
belt app run topaz/frame-interpolation --input '{"video": "<video-url-from-step-2>", "target_fps": 60}'
```

### Upscale AI-Generated Video

```bash
# 1. Generate a 720p clip (see ai-video-generation skill), then:
belt app run pruna/p-video-upscale --input '{
  "video": "<generated-video-url>",
  "target": "4k",
  "fps": 60
}'
```

`fps` (16-60) interpolates above the source rate and bills at the higher rate. `turbo: true` is faster at the same price.

## Tips

- All three Topaz upscalers (Proteus, Starlight, Astra) take `scale` 1-4.
- For transparent output from `bria/video-rmbg`, the app says to use an alpha-supporting `output_container_and_codec` preset.

## Related Skills

```bash
# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# Image upscaling
npx skills add inference-sh/skills@image-upscaling

# Image background removal
npx skills add inference-sh/skills@background-removal

# Video generation (generate, then enhance)
npx skills add inference-sh/skills@ai-video-generation

# Avatar videos
npx skills add inference-sh/skills@ai-avatar-video
```

Browse all video apps: `belt app list --category video`

## Documentation

- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
- [Content Pipeline Example](https://inference.sh/docs/examples/content-pipeline) - Building media workflows
- [Streaming Results](https://inference.sh/docs/api/sdk/streaming) - Real-time progress updates
