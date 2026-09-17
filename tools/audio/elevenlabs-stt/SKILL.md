---
name: elevenlabs-stt
description: "ElevenLabs speech-to-text with Scribe models and forced alignment via inference.sh CLI. Models: Scribe v1/v2 (98%+ accuracy, 90+ languages). Capabilities: transcription, speaker diarization, audio event tagging, word-level timestamps, forced alignment, subtitle generation. Use for: meeting transcription, subtitles, podcast transcripts, lip-sync timing, karaoke. Triggers: elevenlabs stt, elevenlabs transcription, scribe, elevenlabs speech to text, forced alignment, word alignment, subtitle timing, diarization, speaker identification, audio event detection, eleven labs transcribe"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# ElevenLabs Speech-to-Text

High-accuracy transcription with Scribe models via [inference.sh](https://inference.sh) CLI.

![ElevenLabs STT](https://cloud.inference.sh/u/4mg21r6ta37mpaz6ktzwtt8krr/01jz025e88nkvw55at1rqtj5t8.png)

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

# Transcribe audio
belt app run elevenlabs/stt --input '{"audio": "https://audio.mp3"}'
```


## Available Models

| Model | ID | Best For |
|-------|----|----------|
| Scribe v2 | `scribe_v2` | Latest, highest accuracy (default) |
| Scribe v1 | `scribe_v1` | Stable, proven |

- 98%+ transcription accuracy
- 90+ languages with auto-detection

## Examples

### Basic Transcription

```bash
belt app run elevenlabs/stt --input '{"audio": "https://meeting-recording.mp3"}'
```

### With Speaker Identification

```bash
belt app run elevenlabs/stt --input '{
  "audio": "https://meeting.mp3",
  "diarize": true
}'
```

### Audio Event Tagging

Detect laughter, applause, music, and other non-speech events:

```bash
belt app run elevenlabs/stt --input '{
  "audio": "https://podcast.mp3",
  "tag_audio_events": true
}'
```

### Specify Language

```bash
belt app run elevenlabs/stt --input '{
  "audio": "https://spanish-audio.mp3",
  "language_code": "spa"
}'
```

### Full Options

```bash
belt app run elevenlabs/stt --input '{
  "audio": "https://conference.mp3",
  "model": "scribe_v2",
  "diarize": true,
  "tag_audio_events": true,
  "language_code": "eng"
}'
```

## Forced Alignment

Get precise word-level and character-level timestamps by aligning known text to audio. Useful for subtitles, lip-sync, and karaoke.

```bash
belt app run elevenlabs/forced-alignment --input '{
  "audio": "https://narration.mp3",
  "text": "This is the exact text spoken in the audio file."
}'
```

### Output Format

```json
{
  "words": [
    {"text": "This", "start": 0.0, "end": 0.3},
    {"text": "is", "start": 0.35, "end": 0.5},
    {"text": "the", "start": 0.55, "end": 0.65}
  ],
  "text": "This is the exact text spoken in the audio file."
}
```

### Forced Alignment Use Cases

- **Subtitles**: Precise timing for video captions
- **Lip-sync**: Align audio to animated characters
- **Karaoke**: Word-by-word timing for lyrics
- **Accessibility**: Synchronized transcripts

## Workflow: Video Subtitles

```bash
# 1. Transcribe video audio
belt app run elevenlabs/stt --input '{
  "audio": "https://video.mp4",
  "diarize": true
}' > transcript.json

# 2. Build caption segments (start, end, text) from the word timestamps in step 1
belt app run infsh/caption-videos --input '{
  "video_file": "https://video.mp4",
  "segments": [{"start": 0.0, "end": 2.5, "text": "<text-from-step-1>"}]
}'
```

## Supported Languages

90+ languages including: English, Spanish, French, German, Italian, Portuguese, Chinese, Japanese, Korean, Arabic, Hindi, Russian, Turkish, Dutch, Swedish, and many more. Leave `language_code` empty for automatic detection.

## Use Cases

- **Meetings**: Transcribe recordings with speaker identification
- **Podcasts**: Generate transcripts with audio event tags
- **Subtitles**: Create timed captions for videos
- **Research**: Interview transcription with diarization
- **Accessibility**: Make audio content searchable and accessible
- **Lip-sync**: Forced alignment for animation timing

## Related Skills

```bash
# ElevenLabs TTS (reverse direction)
npx skills add inference-sh/skills@elevenlabs-tts

# ElevenLabs dubbing (translate audio)
npx skills add inference-sh/skills@elevenlabs-dubbing

# Other STT models (Whisper)
npx skills add inference-sh/skills@speech-to-text

# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli
```

Browse all audio apps: `belt app list --category audio`
