---
name: ai-voice-cloning
description: "AI voice generation, text-to-speech, and voice synthesis via inference.sh CLI. Models: Inworld TTS-2 (100+ languages, emotion/non-verbal steering), Inworld TTS 1.5 (ultra-low latency), ElevenLabs (22+ premium voices, 32 languages), Kokoro TTS, DIA, Chatterbox, Higgs, VibeVoice for natural speech. Capabilities: multiple voices, emotions, accents, long-form narration, conversation, voice transformation, delivery mode control, character voices. Use for: voiceovers, audiobooks, podcasts, video narration, accessibility, gaming NPCs, avatar audio, UGC. Triggers: voice cloning, tts, text to speech, ai voice, voice generation, voice synthesis, voice over, narration, speech synthesis, ai narrator, elevenlabs, eleven labs, natural voice, realistic speech, voice ai, voice changer, inworld, inworld tts, character voice, npc voice"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# AI Voice Generation

Generate natural AI voices via [inference.sh](https://inference.sh) CLI.

![AI Voice Generation](https://cloud.inference.sh/u/4mg21r6ta37mpaz6ktzwtt8krr/01jz00krptarq4bwm89g539aea.png)

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

# Generate speech
belt app run falai/kokoro-tts --input '{
  "prompt": "Hello! This is an AI-generated voice that sounds natural and engaging.",
  "voice": "af_sarah"
}'
```


## Available Models

| Model | App ID | Best For |
|-------|--------|----------|
| **Inworld TTS-2** | `inworld/text-to-speech-2` | **100+ languages, emotion/non-verbal steering, delivery modes** |
| Inworld TTS 1.5 Max | `inworld/text-to-speech-1-5-max` | Low latency (<200ms), 15 languages |
| Inworld TTS 1.5 Mini | `inworld/text-to-speech-1-5-mini` | Ultra-low latency (~120ms), 15 languages, real-time |
| ElevenLabs TTS | `elevenlabs/tts` | Premium quality, 22+ voices, 32 languages |
| ElevenLabs Voice Changer | `elevenlabs/voice-changer` | Transform existing voice recordings |
| Kokoro TTS | `falai/kokoro-tts` | Natural, multiple voices |
| DIA | `infsh/dia-tts` | Conversational, expressive |
| Chatterbox | `infsh/chatterbox` | Casual, entertainment |
| Higgs | `infsh/higgs-audio` | Professional narration |
| VibeVoice | `infsh/vibevoice` | Emotional range |

## Kokoro Voice Library

### American English

| Voice ID | Gender | Style |
|----------|--------|-------|
| `af_sarah` | Female | Warm, friendly |
| `af_nicole` | Female | Professional |
| `af_sky` | Female | Youthful |
| `am_michael` | Male | Authoritative |
| `am_adam` | Male | Conversational |
| `am_echo` | Male | Clear, neutral |

### British English

| Voice ID | Gender | Style |
|----------|--------|-------|
| `bf_emma` | Female | Refined |
| `bf_isabella` | Female | Warm |
| `bm_george` | Male | Classic |
| `bm_lewis` | Male | Modern |

## Inworld TTS — Character & Emotion Voices

Inworld TTS-2 is purpose-built for character voices, gaming, and expressive speech. Use `[brackets]` inline for emotion, non-verbals, and delivery control:

```bash
# Expressive character voice with emotion steering
belt app run inworld/text-to-speech-2 --input '{
  "text": "[excited] Oh wow, you actually found the ancient artifact! [gasp] I cannot believe it... [whisper] We need to keep this between us.",
  "voice_id": "Sarah",
  "delivery_mode": "CREATIVE"
}'

# Calm narrator with stable delivery
belt app run inworld/text-to-speech-2 --input '{
  "text": "The sun set behind the mountains, casting long shadows across the valley. A new chapter was about to begin.",
  "voice_id": "Sarah",
  "delivery_mode": "STABLE"
}'
```

**Delivery modes:** `STABLE` (consistent, narration), `BALANCED` (natural, default), `CREATIVE` (expressive, characters)

**Steering examples:** `[laugh]`, `[sigh]`, `[whisper]`, `[excited]`, `[sad]`, `[angry]`, `[pause]`, `[gasp]`

**Built-in voices** (271+ across 15 languages): `Sarah`, `Alex`, `Ashley`, `Dennis`, `Hana`, `Blake`, `Luna`, `Clive`, and many more. Browse all at the [Inworld TTS Playground](https://platform.inworld.ai/tts-playground).

### Low-Latency for Real-Time / Conversational AI

```bash
# Ultra-fast response for chatbots & game NPCs (~120ms)
belt app run inworld/text-to-speech-1-5-mini --input '{
  "text": "Welcome, traveler. What brings you to our village?",
  "voice_id": "Clive",
  "speaking_rate": 0.9
}'
```

## Voice Generation Examples

### Professional Narration

```bash
belt app run falai/kokoro-tts --input '{
  "prompt": "Welcome to our quarterly earnings call. Today we will discuss the financial performance and strategic initiatives for the past quarter.",
  "voice": "am_michael",
  "speed": 1.0
}'
```

### Conversational Style

```bash
belt app run infsh/dia-tts --input '{
  "text": "Hey, so I was thinking about that project we discussed. What if we tried a different approach?",
  "voice": "conversational"
}'
```

### Audiobook Narration

```bash
belt app run falai/kokoro-tts --input '{
  "prompt": "Chapter One. The morning mist hung low over the valley as Sarah made her way down the winding path. She had been walking for hours.",
  "voice": "bf_emma",
  "language": "british-english",
  "speed": 0.9
}'
```

### Video Voiceover

```bash
belt app run falai/kokoro-tts --input '{
  "prompt": "Introducing the next generation of productivity. Work smarter, not harder.",
  "voice": "af_nicole",
  "speed": 1.1
}'
```

### Podcast Host

```bash
belt app run falai/kokoro-tts --input '{
  "prompt": "Welcome back to Tech Talk! Im your host, and today we are diving deep into the world of artificial intelligence.",
  "voice": "am_adam"
}'
```

## Multi-Voice Conversation

```bash
# Generate dialogue between two speakers
# Speaker 1
belt app run falai/kokoro-tts --input '{
  "prompt": "Have you seen the latest AI developments? Its incredible how fast things are moving.",
  "voice": "am_michael"
}' > speaker1.json

# Speaker 2
belt app run falai/kokoro-tts --input '{
  "prompt": "I know, right? Just last week I tried that new image generator and was blown away.",
  "voice": "af_sarah"
}' > speaker2.json

# Merge conversation (no inference.sh app concatenates audio; use ffmpeg locally)
curl -L -o speaker1.mp3 "<speaker1-url>"
curl -L -o speaker2.mp3 "<speaker2-url>"
ffmpeg -i speaker1.mp3 -i speaker2.mp3 -filter_complex "acrossfade=d=0.3" conversation.mp3
```

## Long-Form Content

### Chunked Processing

For content over 5000 characters, split into chunks:

```bash
# Process long text in chunks
TEXT="Your very long text here..."

# Split and generate
# Chunk 1
belt app run falai/kokoro-tts --input '{
  "prompt": "<chunk-1>",
  "voice": "bf_emma",
  "language": "british-english"
}' > chunk1.json

# Chunk 2
belt app run falai/kokoro-tts --input '{
  "prompt": "<chunk-2>",
  "voice": "bf_emma",
  "language": "british-english"
}' > chunk2.json

# Merge chunks (no inference.sh app concatenates audio; use ffmpeg locally)
curl -L -o chunk1.mp3 "<chunk1-url>"
curl -L -o chunk2.mp3 "<chunk2-url>"
ffmpeg -i chunk1.mp3 -i chunk2.mp3 -filter_complex "acrossfade=d=0.1" narration.mp3
```

## Voice + Video Workflow

### Add Voiceover to Video

```bash
# 1. Generate voiceover
belt app run falai/kokoro-tts --input '{
  "prompt": "This stunning footage shows the beauty of nature in its purest form.",
  "voice": "am_michael"
}' > voiceover.json

# 2. Merge with video
belt app run infsh/video-audio-merger --input '{
  "video_file": "https://your-video.mp4",
  "audio_file": "<voiceover-url>"
}'
```

### Create Talking Head

```bash
# 1. Generate speech
belt app run falai/kokoro-tts --input '{
  "prompt": "Hi, Im excited to share some updates with you today.",
  "voice": "af_sarah"
}' > speech.json

# 2. Animate with avatar
belt app run bytedance/omnihuman-1-5 --input '{
  "image": "https://portrait.jpg",
  "audio": "<speech-url>"
}'
```

## Speed and Pacing

| Speed | Effect | Use For |
|-------|--------|---------|
| 0.8 | Slow, deliberate | Audiobooks, meditation |
| 0.9 | Slightly slow | Education, tutorials |
| 1.0 | Normal | General purpose |
| 1.1 | Slightly fast | Commercials, energy |
| 1.2 | Fast | Quick announcements |

```bash
# Slow narration
belt app run falai/kokoro-tts --input '{
  "prompt": "Take a deep breath. Let yourself relax.",
  "voice": "bf_emma",
  "language": "british-english",
  "speed": 0.8
}'
```

## Punctuation for Pacing

Use punctuation to control speech rhythm:

| Punctuation | Effect |
|-------------|--------|
| Period `.` | Full pause |
| Comma `,` | Brief pause |
| `...` | Extended pause |
| `!` | Emphasis |
| `?` | Question intonation |
| `-` | Quick break |

```bash
belt app run falai/kokoro-tts --input '{
  "prompt": "Wait... Did you hear that? Something is coming. Something big!",
  "voice": "am_adam"
}'
```

## Best Practices

1. **Match voice to content** - Professional voice for business, casual for social
2. **Use punctuation** - Control pacing with periods and commas
3. **Keep sentences short** - Easier to generate and sounds more natural
4. **Test different voices** - Same text sounds different across voices
5. **Adjust speed** - Slightly slower often sounds more natural
6. **Break long content** - Process in chunks for consistency

## Use Cases

- **Voiceovers** - Video narration, commercials
- **Audiobooks** - Full book narration
- **Podcasts** - AI hosts and guests
- **E-learning** - Course narration
- **Accessibility** - Screen reader content
- **IVR** - Phone system messages
- **Content localization** - Translate and voice

## Related Skills

```bash
# ElevenLabs TTS (premium, 22+ voices)
npx skills add inference-sh/skills@elevenlabs-tts

# ElevenLabs voice changer (transform recordings)
npx skills add inference-sh/skills@elevenlabs-voice-changer

# All TTS models
npx skills add inference-sh/skills@text-to-speech

# Podcast creation
npx skills add inference-sh/skills@ai-podcast-creation

# AI avatars
npx skills add inference-sh/skills@ai-avatar-video

# Video generation
npx skills add inference-sh/skills@ai-video-generation

# Full platform skill
npx skills add inference-sh/skills@infsh-cli
```

Browse audio apps: `belt app list --category audio`

