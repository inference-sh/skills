---
name: elevenlabs-voice-isolator
description: "ElevenLabs voice isolator - remove background noise and isolate vocals from audio via inference.sh CLI. Capabilities: noise removal, voice extraction, audio cleanup, background removal. Use for: podcast cleanup, interview audio, music vocals, noisy recordings, audio restoration. Triggers: voice isolator, noise removal, background removal, isolate voice, clean audio, remove background noise, audio cleanup, voice extraction, elevenlabs isolator, eleven labs noise, vocal isolation, denoise, audio restoration, voice separation"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# ElevenLabs Voice Isolator

Remove background noise and isolate voices from audio via [inference.sh](https://inference.sh) CLI.

![Voice Isolator](https://cloud.inference.sh/u/4mg21r6ta37mpaz6ktzwtt8krr/01jz00krptarq4bwm89g539aea.png)

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

# Isolate voice from noisy audio
belt app run elevenlabs/voice-isolator --input '{"audio": "https://noisy-recording.mp3"}'
```


## Supported Formats

| Format | Max Size | Max Duration |
|--------|----------|-------------|
| WAV | 500MB | 1 hour |
| MP3 | 500MB | 1 hour |
| FLAC | 500MB | 1 hour |
| OGG | 500MB | 1 hour |
| AAC | 500MB | 1 hour |

## Examples

### Clean Up a Recording

```bash
# Remove background noise from a podcast recording
belt app run elevenlabs/voice-isolator --input '{"audio": "https://noisy-podcast.mp3"}'
```

### Clean Interview Audio

```bash
# Isolate speaker from café background noise
belt app run elevenlabs/voice-isolator --input '{"audio": "https://cafe-interview.mp3"}'
```

### Extract Vocals from Music

```bash
# Separate vocals from instrumental
belt app run elevenlabs/voice-isolator --input '{"audio": "https://song.mp3"}'
```

## What It Removes

- Ambient/environmental noise
- Background music
- Reverb and echo
- Wind noise
- Traffic and crowd noise
- Electrical hum/buzz
- Other non-voice sounds

## Workflow: Clean → Transcribe

```bash
# 1. Isolate voice from noisy recording
belt app run elevenlabs/voice-isolator --input '{
  "audio": "https://noisy-meeting.mp3"
}' > cleaned.json

# 2. Transcribe the clean audio
belt app run elevenlabs/stt --input '{
  "audio": "<cleaned-audio-url>",
  "diarize": true
}'
```

## Workflow: Clean → Voice Change

```bash
# 1. Clean up the audio
belt app run elevenlabs/voice-isolator --input '{
  "audio": "https://raw-recording.mp3"
}' > cleaned.json

# 2. Transform the voice
belt app run elevenlabs/voice-changer --input '{
  "audio": "<cleaned-audio-url>",
  "voice": "george"
}'
```

## Workflow: Clean → Add to Video

```bash
# 1. Clean the voiceover
belt app run elevenlabs/voice-isolator --input '{
  "audio": "https://raw-voiceover.mp3"
}' > cleaned.json

# 2. Merge with video
belt app run infsh/video-audio-merger --input '{
  "video_file": "video.mp4",
  "audio_file": "<cleaned-audio-url>"
}'
```

## Use Cases

- **Podcasts**: Clean up recordings with background noise
- **Interviews**: Remove café/office ambient sounds
- **Music**: Extract vocals for remixes or karaoke
- **Video Production**: Clean dialogue audio
- **Archival**: Restore old or degraded recordings
- **Meetings**: Improve recording clarity
- **Voice Cloning Prep**: Clean source audio for better cloning results

## Related Skills

```bash
# ElevenLabs voice changer (transform voice after cleaning)
npx skills add inference-sh/skills@elevenlabs-voice-changer

# ElevenLabs STT (transcribe clean audio)
npx skills add inference-sh/skills@elevenlabs-stt

# ElevenLabs TTS (generate clean speech from text)
npx skills add inference-sh/skills@elevenlabs-tts

# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli
```

Browse all audio apps: `belt app list --category audio`
