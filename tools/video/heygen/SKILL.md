---
name: heygen
description: "HeyGen avatar videos, photo animation, lipsync, video translation, TTS and voice cloning via inference.sh CLI. Apps: heygen/avatar-video (Avatar IV/V engines, up to 4K), heygen/create-avatar (digital twin, photo or prompt avatars), heygen/photo-video, heygen/lipsync, heygen/video-agent (prompt to finished video), heygen/video-translate (30+ languages with voice cloning and lip-sync), heygen/text-to-speech (Starfish), heygen/voice-clone (instant clone). Use for: AI presenters, digital twins, talking photos, dubbing and localizing videos, re-syncing lips to new audio, custom voices, prompt-to-video explainers. Triggers: heygen, heygen avatar, heygen api, digital twin, avatar iv, avatar v, photo avatar, talking photo, video translate, video translation, translate video, dub video, heygen lipsync, lip sync, heygen tts, starfish tts, voice clone, instant voice clone, video agent, ai spokesperson, ai presenter"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# HeyGen

Create avatar videos, animate photos, translate and lipsync videos, and generate or clone voices with HeyGen via [inference.sh](https://inference.sh) CLI.

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

# Animate a portrait photo into a talking video
belt app run heygen/photo-video --input '{
  "image": "https://portrait.jpg",
  "audio_url": "https://speech.mp3"
}'
```

## Apps

| App | App ID | What It Does | Pricing |
|-----|--------|--------------|---------|
| Avatar Video | `heygen/avatar-video` | Talking video from a HeyGen avatar ID + script, Avatar IV or V engine, 720p/1080p/4K | Photo avatar $0.05/s (720p/1080p), $0.067/s (4K); digital twin $0.067/s (720p/1080p), $0.083/s (4K) |
| Create Avatar | `heygen/create-avatar` | New avatar from video (digital twin), photo, or text prompt; returns a look ID | $1.00/call |
| Photo Video | `heygen/photo-video` | Animate a portrait photo with script+voice or an audio file | $0.05/s (720p/1080p), $0.067/s (4K) |
| Lip Sync | `heygen/lipsync` | Re-sync an existing video's lips to new audio | $0.033/s speed, $0.067/s precision |
| Video Agent | `heygen/video-agent` | Full video from a natural language prompt; agent picks avatar, script, production | $0.033/s |
| Video Translate | `heygen/video-translate` | Translate a video into 30+ languages with voice cloning and lip-sync | $0.033/s speed or audio-only, $0.067/s precision |
| Text to Speech | `heygen/text-to-speech` | Starfish TTS, plain text or SSML, speed and language control | $0.0007/s |
| Voice Clone | `heygen/voice-clone` | Instant clone from one short recording, optionally speak text in it | $0.0007/s of audio; clone-only runs free |

## Choosing an App

| You have | You want | Use |
|----------|----------|-----|
| A portrait photo | A one-off talking video | `heygen/photo-video` |
| Footage, a photo, or a description | A reusable avatar | `heygen/create-avatar`, then `heygen/avatar-video` |
| A HeyGen avatar ID | A scripted video | `heygen/avatar-video` |
| Only an idea | A finished video | `heygen/video-agent` |
| A video in one language | The same video in another | `heygen/video-translate` |
| A video and new audio | Lips matching the new audio | `heygen/lipsync` |
| Text | Speech audio | `heygen/text-to-speech` |
| A voice recording | A reusable voice ID | `heygen/voice-clone` |

## Examples

### Avatar Video

```bash
belt app run heygen/avatar-video --input '{
  "avatar_id": "<avatar-look-id>",
  "script": "Welcome to our quarterly update. Here are three things that changed.",
  "engine": "avatar_iv",
  "resolution": "1080p",
  "aspect_ratio": "16:9"
}'
```

- `engine`: `avatar_iv` (default) or `avatar_v`. Avatar V gives more natural motion and lip-sync but needs an eligible avatar look.
- `resolution`: `720p`, `1080p`, `4k`. `aspect_ratio`: `16:9`, `9:16`.
- `voice_id` is optional; the avatar's default voice is used when omitted.
- `expressiveness` (`high`, `medium`, `low`) and `motion_prompt` only apply to photo avatars.
- `remove_background` and `output_format` (`mp4`, `webm`) are also available.

List available avatars and voices with the `list_resources` function:

```bash
belt app run heygen/avatar-video --function list_resources --input '{
  "resource_type": "both",
  "limit": 10
}'
```

`resource_type` is `avatars`, `voices`, or `both`.

### Photo Video (Talking Photo)

With a script and voice:

```bash
belt app run heygen/photo-video --input '{
  "image": "https://portrait.jpg",
  "script": "Hi, I am excited to show you what we built.",
  "voice_id": "<voice-id>",
  "expressiveness": "medium",
  "motion_prompt": "nodding slowly",
  "aspect_ratio": "9:16"
}'
```

`voice_id` is required when `script` is set. Pass `audio_url` instead of `script` + `voice_id` to lip-sync to your own audio.

### Create an Avatar

```bash
# From a photo
belt app run heygen/create-avatar --input '{
  "type": "photo",
  "name": "Product Presenter",
  "file": "https://portrait.jpg"
}'

# From a text prompt
belt app run heygen/create-avatar --input '{
  "type": "prompt",
  "name": "Friendly Teacher",
  "prompt": "a friendly middle-aged teacher in a bright classroom, warm smile"
}'

# Digital twin from video footage
belt app run heygen/create-avatar --input '{
  "type": "digital_twin",
  "name": "My Twin",
  "file": "https://footage-of-me.mp4"
}'
```

The output has `avatar.id` (the look ID to pass as `avatar_id` in `heygen/avatar-video`), `avatar.supported_engines`, and `group.id`. Pass `avatar_group_id` to attach a new look to an existing character. Digital twins can return `group.consent_status` and `group.consent_url` when consent is pending.

### Video Agent (Prompt to Video)

```bash
belt app run heygen/video-agent --input '{
  "prompt": "A 45-second explainer about how solar panels convert sunlight into electricity, friendly presenter, simple diagrams",
  "orientation": "landscape"
}'
```

`orientation` is `landscape` or `portrait` (auto-detected if omitted). Optional `avatar_id`, `voice_id`, and `style_id` pin specific choices.

### Lip Sync

```bash
belt app run heygen/lipsync --input '{
  "video": "https://original.mp4",
  "audio": "https://new-voiceover.mp3",
  "mode": "precision"
}'
```

`mode` is `speed` (default) or `precision`. Optional: `enable_caption`, `enable_dynamic_duration` (default true), `disable_music_track`, `start_time`/`end_time` for a partial sync. Inputs must be publicly accessible URLs.

### Text to Speech

```bash
belt app run heygen/text-to-speech --input '{
  "text": "Thanks for watching. See you next week.",
  "voice_id": "<voice-id>",
  "speed": 1.1,
  "locale": "en-US"
}'
```

- `text`: 1-5000 characters. `input_type`: `text` or `ssml`.
- `speed`: 0.5-2.0. `language` (ISO code, e.g. `es`) or `locale` (BCP-47, e.g. `pt-BR`).
- The voice must support the Starfish engine.

### Voice Clone

```bash
belt app run heygen/voice-clone --input '{
  "audio": "https://my-voice-60s.mp3",
  "text": "This sentence is spoken in my cloned voice.",
  "voice_name": "My Voice",
  "keep_voice": true
}'
```

- 30-60 seconds of clean, single-speaker speech works best. `remove_background_noise` defaults to true.
- `keep_voice` defaults to false: the clone is deleted after generating audio because the HeyGen account has limited clone slots. It is forced on when no `text` is given.
- Output: `audio`, `voice_id`, `kept`, `preview_audio_url`. A kept `voice_id` works with `heygen/text-to-speech` and avatar videos.

Manage cloned voices:

```bash
belt app run heygen/voice-clone --function list_voices --input '{"voice_type": "private", "limit": 20}'
belt app run heygen/voice-clone --function delete_voice --input '{"voice_id": "<voice-id>"}'
```

`voice_type`: `private` (this workspace's clones) or `public` (HeyGen catalog).

## Workflows

### Create an Avatar, Then Make Videos With It

```bash
# 1. Create a photo avatar (one-time, $1.00)
belt app run heygen/create-avatar --input '{
  "type": "photo",
  "name": "Brand Host",
  "file": "https://host-portrait.jpg"
}' -o avatar.json

# 2. Use avatar.id from step 1 as avatar_id
belt app run heygen/avatar-video --input '{
  "avatar_id": "<avatar.id-from-step-1>",
  "script": "Here is what is new this week.",
  "aspect_ratio": "9:16",
  "resolution": "1080p"
}'
```

Reuse the same `avatar_id` for every future video. Check `avatar.supported_engines` before setting `"engine": "avatar_v"`.

### Translate a Video

```bash
belt app run heygen/video-translate --input '{
  "video": "https://original-english.mp4",
  "output_language": "Spanish (Spain)",
  "mode": "precision",
  "speaker_num": 2,
  "enable_caption": true
}'
```

- `output_language` takes names like `Spanish (Spain)`, `Chinese (Mandarin, Simplified)`, `French`.
- `input_language` is auto-detected if omitted.
- `translate_audio_only: true` translates the audio track and keeps the original frames.
- `speaker_num` improves speaker separation in multi-speaker videos.
- `start_time`/`end_time` translate only part of the video.

### Your Own Voice on an Avatar

```bash
# 1. Clone and keep the voice
belt app run heygen/voice-clone --input '{
  "audio": "https://my-voice-60s.mp3",
  "voice_name": "Founder Voice"
}' -o voice.json

# 2. Use voice_id from step 1 in an avatar video
belt app run heygen/avatar-video --input '{
  "avatar_id": "<avatar-look-id>",
  "voice_id": "<voice_id-from-step-1>",
  "script": "A quick message from our founder."
}'
```

With no `text`, the clone is kept automatically and only the `voice_id` is returned.

### Replace the Voiceover on Existing Footage

```bash
# 1. Generate new speech
belt app run heygen/text-to-speech --input '{
  "text": "Our new pricing starts today.",
  "voice_id": "<voice-id>"
}' -o speech.json

# 2. Re-sync the presenter's lips to the new audio
belt app run heygen/lipsync --input '{
  "video": "https://presenter.mp4",
  "audio": "<audio-url-from-step-1>",
  "mode": "precision"
}'
```

## Tips

- Check pricing with `belt app pricing heygen/<app>`; use `--estimate` before long or 4K renders.
- `precision` mode in lipsync and video-translate costs about twice `speed` mode.

## Related Skills

```bash
# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# Other avatar and lipsync models (P-Video-Avatar, OmniHuman, Fabric)
npx skills add inference-sh/skills@ai-avatar-video

# Talking head production workflows
npx skills add inference-sh/skills@talking-head-production

# ElevenLabs dubbing (alternative to video-translate)
npx skills add inference-sh/skills@elevenlabs-dubbing

# Voice cloning with other providers
npx skills add inference-sh/skills@ai-voice-cloning

# Video generation
npx skills add inference-sh/skills@ai-video-generation
```

Browse all HeyGen apps: `belt app search "heygen"`

## Documentation

- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
- [Content Pipeline Example](https://inference.sh/docs/examples/content-pipeline) - Building media workflows
- [Streaming Results](https://inference.sh/docs/api/sdk/streaming) - Real-time progress updates
