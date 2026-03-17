---
name: elevenlabs-dubbing
description: "ElevenLabs automatic dubbing - translate and dub audio/video into 29 languages while preserving speaker voice via inference.sh CLI. Capabilities: auto speaker detection, voice-preserving translation, video dubbing, audio localization. Use for: content localization, video translation, multilingual content, international distribution. Triggers: dubbing, dub video, translate audio, video translation, audio translation, localize content, elevenlabs dubbing, eleven labs dub, multilingual dub, voice translation, auto dub, language dub, content localization"
allowed-tools: Bash(infsh *)
---

# ElevenLabs Dubbing

Automatically dub audio and video into 29 languages via [inference.sh](https://inference.sh) CLI.

![Dubbing](https://cloud.inference.sh/u/4mg21r6ta37mpaz6ktzwtt8krr/01jz00krptarq4bwm89g539aea.png)

## Quick Start

> Requires inference.sh CLI (`infsh`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
infsh login

# Dub English video to Spanish
infsh app run elevenlabs/dubbing --input '{
  "audio": "https://video.mp4",
  "target_lang": "es"
}'
```


## Supported Languages

| Code | Language | Code | Language |
|------|----------|------|----------|
| `en` | English | `ko` | Korean |
| `es` | Spanish | `ru` | Russian |
| `fr` | French | `tr` | Turkish |
| `de` | German | `nl` | Dutch |
| `it` | Italian | `sv` | Swedish |
| `pt` | Portuguese | `da` | Danish |
| `pl` | Polish | `fi` | Finnish |
| `hi` | Hindi | `no` | Norwegian |
| `ar` | Arabic | `cs` | Czech |
| `zh` | Chinese | `el` | Greek |
| `ja` | Japanese | `he` | Hebrew |
| `hu` | Hungarian | `id` | Indonesian |
| `ms` | Malay | `ro` | Romanian |
| `th` | Thai | `uk` | Ukrainian |
| `vi` | Vietnamese | | |

## Supported Input Formats

- MP3, MP4, WAV, MOV

## Examples

### Dub Video to Spanish

```bash
infsh app run elevenlabs/dubbing --input '{
  "audio": "https://english-video.mp4",
  "target_lang": "es"
}'
```

### Dub Audio to French

```bash
infsh app run elevenlabs/dubbing --input '{
  "audio": "https://podcast-episode.mp3",
  "target_lang": "fr"
}'
```

### Specify Source Language

```bash
# Skip auto-detection, specify source
infsh app run elevenlabs/dubbing --input '{
  "audio": "https://german-video.mp4",
  "source_lang": "de",
  "target_lang": "en"
}'
```

### Multi-Language Distribution

```bash
# Dub to multiple languages
for lang in es fr de ja ko; do
  infsh app run elevenlabs/dubbing --input "{
    \"audio\": \"https://video.mp4\",
    \"target_lang\": \"$lang\"
  }" > "dubbed_${lang}.json"
  echo "Dubbed to $lang"
done
```

## Features

- **Auto Speaker Detection**: Identifies multiple speakers automatically
- **Voice Preservation**: Maintains original speaker voice characteristics
- **Timing**: Matches original speech timing and pacing
- **Multi-Speaker**: Handles videos with multiple speakers

## Workflow: Localize Content Pipeline

```bash
# 1. Start with original video
# 2. Dub to target language
infsh app run elevenlabs/dubbing --input '{
  "audio": "https://original-video.mp4",
  "target_lang": "es"
}' > dubbed.json

# 3. Add subtitles in target language
infsh app run elevenlabs/stt --input '{
  "audio": "<dubbed-audio-url>",
  "language_code": "spa"
}' > transcript.json

# 4. Caption the dubbed video
infsh app run infsh/caption-videos --input '{
  "video_url": "<dubbed-video-url>",
  "captions": "<transcript>"
}'
```

## Use Cases

- **Content Creators**: Reach international audiences
- **E-learning**: Localize courses for global students
- **Marketing**: Adapt campaigns for different markets
- **Podcasts**: Distribute in multiple languages
- **Corporate**: Multilingual training and communications
- **Film/TV**: Quick dubbing for distribution

## Related Skills

```bash
# ElevenLabs TTS (generate speech in any language)
npx skills add inference-sh/skills@elevenlabs-tts

# ElevenLabs STT (transcribe dubbed content)
npx skills add inference-sh/skills@elevenlabs-stt

# ElevenLabs voice changer (transform voices)
npx skills add inference-sh/skills@elevenlabs-voice-changer

# Full platform skill (all 150+ apps)
npx skills add inference-sh/skills@infsh-cli
```

Browse all audio apps: `infsh app list --category audio`
