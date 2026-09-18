# Discovering Apps

## Your Apps

```bash
belt app list
belt app list --search "flux"
belt app search "flux"
belt app list -l  # detailed
```

## Browse the Public Store

```bash
belt app list
```

## Pagination

```bash
belt app list --page 2
```

## Filter by Category

```bash
belt app list --category image
belt app list --category video
belt app list --category audio
belt app list --category text
belt app list --category other
```

## Search the Store

```bash
belt app search "flux"
belt app search "video generation"
belt app search "tts" -l
belt app search "image" --category image
```

## Featured Apps

```bash
belt app list --featured
```

## Newest First

```bash
belt app list --new
```

## Detailed View

```bash
belt app list -l
```

Shows table with app name, category, description, and featured status.

## Save to File

```bash
belt app list --save apps.json
```

## Get App Details

```bash
belt app get falai/flux-dev-lora
belt app get falai/flux-dev-lora --json
```

Shows full app info including input/output schema.

## Popular Apps by Category

### Image Generation
- `falai/flux-dev-lora` - FLUX.2 Dev (high quality)
- `falai/flux-2-klein-lora` - FLUX.2 Klein (fastest)
- `infsh/sdxl` - Stable Diffusion XL
- `google/gemini-3-pro-image` - Gemini 3 Pro
- `xai/grok-imagine-image` - Grok image generation

### Video Generation
- `google/veo-3-1-fast` - Veo 3.1 Fast
- `google/veo-3-1` - Veo 3
- `bytedance/seedance-2-0` - Seedance 2.0
- `bytedance/seedance-2-0-fast` - Seedance 2.0 Fast
- `infsh/ltx-video-2` - LTX Video 2 (with audio)
- `bytedance/omnihuman-1-5` - OmniHuman avatar

### Audio
- `infsh/dia-tts` - Conversational TTS
- `falai/kokoro-tts` - Kokoro TTS
- `infsh/fast-whisper-large-v3` - Fast transcription
- `infsh/diffrythm` - Music generation

## Documentation

- [Browsing the Grid](https://inference.sh/docs/apps/browsing-grid) - Visual app browsing
- [Apps Overview](https://inference.sh/docs/apps/overview) - Understanding apps
- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps
