---
name: text-to-speech
description: "Convert text to natural speech with Inworld TTS, ElevenLabs, DIA TTS, Kokoro, Chatterbox, and more via inference.sh CLI. Models: Inworld TTS-2 (100+ languages, emotion steering), Inworld TTS 1.5 (ultra-low latency), ElevenLabs (premium, 22+ voices, 32 languages), DIA TTS (conversational), Kokoro TTS, Chatterbox, Higgs Audio, VibeVoice (podcasts). Capabilities: text-to-speech, voice cloning, multi-speaker dialogue, podcast generation, expressive speech, emotion/delivery steering, character voices. Use for: voiceovers, audiobooks, podcasts, accessibility, video narration, IVR, voice assistants, gaming characters, avatar audio. Triggers: text to speech, tts, voice generation, ai voice, speech synthesis, voice over, generate speech, ai narrator, voice cloning, text to audio, elevenlabs, eleven labs, voice ai, ai voiceover, speech generator, natural voice, inworld, inworld tts, character voice, game voice, npc voice"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# Text-to-Speech

Convert text to natural speech via [inference.sh](https://inference.sh) CLI.

![Text-to-Speech](https://cloud.inference.sh/u/4mg21r6ta37mpaz6ktzwtt8krr/01jz00krptarq4bwm89g539aea.png)

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

# Generate speech
belt app run infsh/kokoro-tts --input '{"text": "Hello, welcome to our product demo."}'
```


## Available Models

| Model | App ID | Best For |
|-------|--------|----------|
| **Inworld TTS-2** | `inworld/text-to-speech-2` | **100+ languages, emotion steering with [brackets], delivery modes** |
| Inworld TTS 1.5 Max | `inworld/text-to-speech-1-5-max` | Low latency (<200ms), 15 languages |
| Inworld TTS 1.5 Mini | `inworld/text-to-speech-1-5-mini` | Ultra-low latency (~120ms), 15 languages |
| ElevenLabs TTS | `elevenlabs/tts` | Premium quality, 22+ voices, 32 languages |
| DIA TTS | `infsh/dia-tts` | Conversational, expressive |
| Kokoro TTS | `infsh/kokoro-tts` | Fast, natural |
| Chatterbox | `infsh/chatterbox` | General purpose |
| Higgs Audio | `infsh/higgs-audio` | Emotional control |
| VibeVoice | `infsh/vibevoice` | Podcasts, long-form |

## Browse All Audio Apps

```bash
belt app list --category audio
```

## Examples

### Basic Text-to-Speech

```bash
belt app run infsh/kokoro-tts --input '{"text": "Welcome to our tutorial."}'
```

### Inworld TTS-2 — Emotion Steering

Inworld TTS-2 supports natural-language steering with `[brackets]` — control emotion, volume, speed, and non-verbals inline with text:

```bash
belt app run inworld/text-to-speech-2 --input '{
  "text": "I have some [exciting] news to share with you! [pause] We just hit one million users. [laugh]",
  "voice_id": "Sarah",
  "delivery_mode": "CREATIVE"
}'
```

Delivery modes: `STABLE` (consistent), `BALANCED` (natural, default), `CREATIVE` (expressive).

**Built-in voices** (271+ across 15 languages): `Sarah`, `Alex`, `Ashley`, `Dennis`, `Hana`, `Blake`, `Luna`, `Clive`, and many more. Browse all voices in the [Inworld TTS Playground](https://platform.inworld.ai/tts-playground) or list programmatically via `GET https://api.inworld.ai/voices/v1/voices`.

### Inworld TTS 1.5 — Low Latency

For real-time applications where speed matters:

```bash
# Max quality at low latency (<200ms)
belt app run inworld/text-to-speech-1-5-max --input '{
  "text": "Welcome back! How can I help you today?",
  "voice_id": "Ashley"
}'

# Ultra-low latency (~120ms) for conversational AI
belt app run inworld/text-to-speech-1-5-mini --input '{
  "text": "Sure, let me look that up for you.",
  "voice_id": "Dennis"
}'
```

### Conversational TTS with DIA

```bash
belt app sample infsh/dia-tts --save input.json

# Edit input.json:
# {
#   "text": "Hey! How are you doing today? I'm really excited to share this with you.",
#   "voice": "conversational"
# }

belt app run infsh/dia-tts --input input.json
```

### Long-form Audio (Podcasts)

```bash
belt app sample infsh/vibevoice --save input.json

# Edit input.json with your podcast script
belt app run infsh/vibevoice --input input.json
```

### Expressive Speech with Higgs

```bash
belt app sample infsh/higgs-audio --save input.json

# {
#   "text": "This is absolutely incredible!",
#   "emotion": "excited"
# }

belt app run infsh/higgs-audio --input input.json
```

## Use Cases

- **Voiceovers**: Product demos, explainer videos
- **Audiobooks**: Convert text to spoken word
- **Podcasts**: Generate podcast episodes
- **Accessibility**: Make content accessible
- **IVR**: Phone system voice prompts
- **Video Narration**: Add narration to videos
- **Gaming / NPCs**: Character voices with emotion steering (Inworld TTS-2)
- **Conversational AI**: Ultra-low latency responses (Inworld TTS 1.5 Mini)
- **Avatar / UGC Videos**: Generate speech for talking head avatars

## Combine with Video

The easiest way to create a talking head video is P-Video-Avatar with built-in TTS — no separate audio step:

```bash
belt app run pruna/p-video-avatar --input '{
  "image": "https://portrait.jpg",
  "voice_script": "Your script here",
  "voice": "Zephyr (Female)"
}'
```

For models without built-in TTS (OmniHuman, PixVerse), generate speech first:

```bash
# 1. Generate speech with Inworld TTS-2 (emotion steering)
belt app run inworld/text-to-speech-2 --input '{
  "text": "[friendly] Your script here. [excited] This is the best part!",
  "voice_id": "Sarah",
  "delivery_mode": "CREATIVE"
}' > speech.json

# 2. Use the audio URL with OmniHuman for avatar video
belt app run bytedance/omnihuman-1-5 --input '{
  "image": "https://portrait.jpg",
  "audio": "<audio-url-from-step-1>"
}'
```

## Related Skills

```bash
# ElevenLabs TTS (premium, 22+ voices)
npx skills add inference-sh/skills@elevenlabs-tts

# ElevenLabs dialogue (multi-speaker)
npx skills add inference-sh/skills@elevenlabs-dialogue

# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# AI avatars (combine TTS with talking heads)
npx skills add inference-sh/skills@ai-avatar-video

# AI music generation
npx skills add inference-sh/skills@ai-music-generation

# Speech-to-text (transcription)
npx skills add inference-sh/skills@speech-to-text

# Video generation
npx skills add inference-sh/skills@ai-video-generation
```

Browse all apps: `belt app list`

## Documentation

- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
- [Audio Transcription Example](https://inference.sh/docs/examples/audio-transcription) - Audio processing workflows
- [Apps Overview](https://inference.sh/docs/apps/overview) - Understanding the app ecosystem

