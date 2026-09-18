---
name: elevenlabs-voice-changer
description: "ElevenLabs voice changer - transform any voice to a different voice while preserving speech content and emotion via inference.sh CLI. Models: eleven_multilingual_sts_v2 (70+ languages), eleven_english_sts_v2. Capabilities: speech-to-speech, voice transformation, accent change, voice disguise. Use for: content creation, voice acting, privacy, dubbing, character voices. Triggers: voice changer, speech to speech, voice transformation, change voice, voice swap, voice conversion, voice disguise, eleven labs voice changer, elevenlabs sts, transform voice, ai voice changer, voice modifier"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# ElevenLabs Voice Changer

Transform any voice into a different voice via [inference.sh](https://inference.sh) CLI.

![Voice Changer](https://cloud.inference.sh/u/4mg21r6ta37mpaz6ktzwtt8krr/01jz00krptarq4bwm89g539aea.png)

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

# Transform voice
belt app run elevenlabs/voice-changer --input '{"audio": "https://recording.mp3", "voice": "aria"}'
```


## Available Models

| Model | ID | Best For |
|-------|----|----------|
| Multilingual STS v2 | `eleven_multilingual_sts_v2` | 70+ languages (default) |
| English STS v2 | `eleven_english_sts_v2` | English-optimized |

## Voice Options

Same 22+ premium voices as ElevenLabs TTS:

| Voice | Style |
|-------|-------|
| `george` | British, authoritative (default) |
| `aria` | American, conversational |
| `alice` | British, confident |
| `brian` | American, conversational |
| `charlie` | Australian, natural |
| `daniel` | British, commanding |
| `jessica` | American, expressive |
| `sarah` | American, friendly |
| `adam`, `bella`, `bill`, `callum`, `chris`, `eric`, `harry`, `laura`, `liam`, `lily`, `matilda`, `river`, `roger`, `will` | Various styles |

## Examples

### Basic Voice Transformation

```bash
# Change voice to British male
belt app run elevenlabs/voice-changer --input '{
  "audio": "https://my-recording.mp3",
  "voice": "george"
}'

# Change voice to American female
belt app run elevenlabs/voice-changer --input '{
  "audio": "https://my-recording.mp3",
  "voice": "aria"
}'
```

### Choose Output Format

```bash
belt app run elevenlabs/voice-changer --input '{
  "audio": "https://recording.mp3",
  "voice": "daniel",
  "output_format": "mp3_44100_192"
}'
```

### English-Optimized Model

```bash
belt app run elevenlabs/voice-changer --input '{
  "audio": "https://english-speech.mp3",
  "voice": "brian",
  "model": "eleven_english_sts_v2"
}'
```

## Workflow: Voice-Over Replacement

```bash
# 1. Record yourself reading the script (any quality mic)
# 2. Transform to professional voice
belt app run elevenlabs/voice-changer --input '{
  "audio": "https://my-rough-recording.mp3",
  "voice": "george"
}' > professional.json

# 3. Add to video
belt app run infsh/video-audio-merger --input '{
  "video_file": "video.mp4",
  "audio_file": "<professional-audio-url>"
}'
```

## Workflow: Character Voices

```bash
# Record one actor, create multiple characters
# Character 1: British narrator
belt app run elevenlabs/voice-changer --input '{
  "audio": "https://actor-line1.mp3",
  "voice": "george"
}' > char1.json

# Character 2: Young female
belt app run elevenlabs/voice-changer --input '{
  "audio": "https://actor-line2.mp3",
  "voice": "lily"
}' > char2.json

# Character 3: Casual male
belt app run elevenlabs/voice-changer --input '{
  "audio": "https://actor-line3.mp3",
  "voice": "charlie"
}' > char3.json
```

## Use Cases

- **Content Creation**: Transform your voice for videos and podcasts
- **Voice Acting**: Create multiple characters from one performance
- **Privacy**: Anonymize voice in recordings
- **Dubbing**: Replace voices in video content
- **Accessibility**: Convert to preferred voice characteristics
- **Prototyping**: Test different voices before hiring talent

## Related Skills

```bash
# ElevenLabs TTS (generate from text instead)
npx skills add inference-sh/skills@elevenlabs-tts

# ElevenLabs voice isolator (clean audio first)
npx skills add inference-sh/skills@elevenlabs-voice-isolator

# ElevenLabs dubbing (translate to other languages)
npx skills add inference-sh/skills@elevenlabs-dubbing

# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli
```

Browse all audio apps: `belt app list --category audio`
