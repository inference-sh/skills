---
name: speech-to-text
description: "Transcribe audio to text with ElevenLabs Scribe and OpenAI GPT Transcribe via inference.sh CLI. Models: ElevenLabs Scribe v2 (98%+ accuracy, diarization, word timestamps), GPT Transcribe (fast, low cost, key-term hints). Capabilities: transcription, multi-language, timestamps, speaker diarization, audio event tagging, realtime microphone transcription. Use for: meeting transcription, subtitles, podcast transcripts, voice notes. Triggers: speech to text, transcription, gpt transcribe, openai transcribe, audio to text, transcribe audio, voice to text, stt, automatic transcription, subtitles generation, transcribe meeting, audio transcription, whisper, elevenlabs stt, scribe, eleven labs transcribe"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# Speech-to-Text

Transcribe audio to text via [inference.sh](https://inference.sh) CLI.

![Speech-to-Text](https://cloud.inference.sh/u/4mg21r6ta37mpaz6ktzwtt8krr/01jz025e88nkvw55at1rqtj5t8.png)

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

belt app run openai/gpt-transcribe --input '{"audio": "https://audio.mp3"}'
```


## Available Models

| Model | App ID | Best For |
|-------|--------|----------|
| ElevenLabs Scribe v2 | `elevenlabs/stt` | 98%+ accuracy, diarization, 90+ languages |
| GPT Transcribe | `openai/gpt-transcribe` | Fast, low-cost transcription; key-term hints |

## Examples

### Basic Transcription

```bash
belt app run openai/gpt-transcribe --input '{"audio": "https://meeting.mp3"}'
```

### With Timestamps

```bash
belt app sample elevenlabs/stt --save input.json

# {
#   "audio": "https://podcast.mp3",
#   "diarize": true
# }

belt app run elevenlabs/stt --input input.json
```

The result's `words` array carries word-level timestamps and speaker labels.

### Language Hints and Key Terms

```bash
belt app run openai/gpt-transcribe --input '{
  "audio": "https://french-audio.mp3",
  "languages": ["fr"],
  "keyterms": ["inference.sh", "Seedance"]
}'
```

### From Video

```bash
# Extract audio from video first
belt app run infsh/video-audio-extractor --input '{"video_file": "https://video.mp4"}' > audio.json

# Transcribe the extracted audio
belt app run openai/gpt-transcribe --input '{"audio": "<audio-url>"}'
```

## Workflow: Video Subtitles

```bash
# 1. Extract the audio track (Scribe takes audio files, not video)
belt app run infsh/video-audio-extractor --input '{
  "video_file": "https://video.mp4"
}' > audio.json

# 2. Transcribe it
belt app run elevenlabs/stt --input '{
  "audio": "<audio_file-from-step-1>"
}' > transcript.json

# 3. Group the word timestamps into caption segments
belt app run infsh/caption-videos --input '{
  "video_file": "https://video.mp4",
  "segments": [{"start": 0.0, "end": 2.5, "text": "<segments-from-step-2>"}]
}'
```

## Supported Languages

Both models support 90+ languages including:
English, Spanish, French, German, Italian, Portuguese, Chinese, Japanese, Korean, Arabic, Hindi, Russian, and many more.

## Use Cases

- **Meetings**: Transcribe recordings
- **Podcasts**: Generate transcripts
- **Subtitles**: Create captions for videos
- **Voice Notes**: Convert to searchable text
- **Interviews**: Transcription for research
- **Accessibility**: Make audio content accessible

## Output Format

Returns JSON with:
- `text`: Full transcription
- `words`: Word-level timestamps and speakers (`elevenlabs/stt`)
- `language_code` / `languages`: Detected language

## Related Skills

```bash
# ElevenLabs STT (98%+ accuracy, diarization)
npx skills add inference-sh/skills@elevenlabs-stt

# ElevenLabs TTS (reverse direction)
npx skills add inference-sh/skills@elevenlabs-tts

# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# Text-to-speech (reverse direction)
npx skills add inference-sh/skills@text-to-speech

# Video generation (add captions)
npx skills add inference-sh/skills@ai-video-generation

# AI avatars (lipsync with transcripts)
npx skills add inference-sh/skills@ai-avatar-video
```

Browse all audio apps: `belt app list --category audio`

## Documentation

- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
- [Audio Transcription Example](https://inference.sh/docs/examples/audio-transcription) - Complete transcription guide
- [Apps Overview](https://inference.sh/docs/apps/overview) - Understanding the app ecosystem

