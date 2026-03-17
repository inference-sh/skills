---
name: elevenlabs-sound-effects
description: "Generate AI sound effects from text descriptions with ElevenLabs via inference.sh CLI. Capabilities: text-to-sound-effect, custom duration, royalty-free audio. Use for: video production, game audio, podcasts, films, presentations, social media. Triggers: sound effects, sfx, sound generation, ai sound effects, generate sound, foley, audio effects, sound design, text to sound, elevenlabs sound, eleven labs sfx, ambient sound, cinematic sound, game sound effects"
allowed-tools: Bash(infsh *)
---

# ElevenLabs Sound Effects

Generate sound effects from text descriptions via [inference.sh](https://inference.sh) CLI.

![Sound Effects](https://cloud.inference.sh/u/4mg21r6ta37mpaz6ktzwtt8krr/01jz01qvx0gdcyvhvhpfjjb6s4.png)

## Quick Start

> Requires inference.sh CLI (`infsh`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
infsh login

# Generate a sound effect
infsh app run elevenlabs/sound-effects --input '{"text": "Thunder rumbling in the distance"}'
```


## Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `text` | string | Description of the sound effect (max 1000 chars) |
| `duration_seconds` | number | Duration 0.5-22 seconds (optional, auto if omitted) |
| `prompt_influence` | number | 0-1, how literal to interpret prompt (default: 0.3) |

## Examples

### Cinematic Effects

```bash
# Epic trailer hit
infsh app run elevenlabs/sound-effects --input '{"text": "Cinematic braam, deep bass impact"}'

# Suspense drone
infsh app run elevenlabs/sound-effects --input '{
  "text": "Dark atmospheric drone, tension building, horror",
  "duration_seconds": 10
}'

# Whoosh transition
infsh app run elevenlabs/sound-effects --input '{
  "text": "Fast cinematic whoosh transition",
  "duration_seconds": 1.5
}'
```

### Nature & Environment

```bash
# Rain
infsh app run elevenlabs/sound-effects --input '{
  "text": "Heavy rain on a tin roof with occasional thunder",
  "duration_seconds": 15
}'

# Forest ambience
infsh app run elevenlabs/sound-effects --input '{
  "text": "Forest ambience with birds chirping and gentle wind",
  "duration_seconds": 20
}'

# Ocean waves
infsh app run elevenlabs/sound-effects --input '{
  "text": "Ocean waves crashing on a beach, calming",
  "duration_seconds": 15
}'
```

### Game Audio

```bash
# Power-up
infsh app run elevenlabs/sound-effects --input '{
  "text": "Retro game power-up sound, ascending tones",
  "duration_seconds": 1
}'

# Explosion
infsh app run elevenlabs/sound-effects --input '{
  "text": "Sci-fi laser explosion, futuristic",
  "duration_seconds": 3
}'

# UI click
infsh app run elevenlabs/sound-effects --input '{
  "text": "Soft UI button click, subtle and clean",
  "duration_seconds": 0.5
}'
```

### Everyday Sounds

```bash
# Doorbell
infsh app run elevenlabs/sound-effects --input '{"text": "Classic doorbell ring"}'

# Typing
infsh app run elevenlabs/sound-effects --input '{
  "text": "Mechanical keyboard typing, fast, clicky",
  "duration_seconds": 5
}'

# Notification
infsh app run elevenlabs/sound-effects --input '{
  "text": "Pleasant notification chime, positive",
  "duration_seconds": 1
}'
```

## Prompt Influence

Control how literally the model interprets your description:

| Value | Effect | Best For |
|-------|--------|----------|
| 0.0 | Very loose interpretation | Creative, surprising results |
| 0.3 | Balanced (default) | General purpose |
| 0.7 | Close to description | Specific sound needs |
| 1.0 | Very literal | Exact sound reproduction |

```bash
# Loose interpretation - creative result
infsh app run elevenlabs/sound-effects --input '{
  "text": "Magical fairy dust sparkle",
  "prompt_influence": 0.1
}'

# Literal interpretation - precise result
infsh app run elevenlabs/sound-effects --input '{
  "text": "Single gunshot, pistol, indoor range",
  "prompt_influence": 0.8
}'
```

## Prompt Tips

**Be specific**: "Heavy rain on metal roof" > "rain sound"

**Include context**: "Footsteps on gravel, slow walking pace" > "footsteps"

**Describe mood**: "Eerie wind howling through abandoned building" > "wind"

**Specify material**: "Glass shattering on concrete floor" > "breaking glass"

## Workflow: Add SFX to Video

```bash
# 1. Generate sound effect
infsh app run elevenlabs/sound-effects --input '{
  "text": "Dramatic reveal swoosh with bass drop",
  "duration_seconds": 2
}' > sfx.json

# 2. Merge with video
infsh app run infsh/media-merger --input '{
  "media": ["video.mp4", "<sfx-url>"]
}'
```

## Use Cases

- **Video Production**: Transitions, impacts, ambience
- **Game Development**: UI sounds, effects, environments
- **Podcasts**: Stingers, transitions, atmosphere
- **Film/Animation**: Foley, ambience, scoring elements
- **Presentations**: Attention-grabbing sound cues
- **Social Media**: Short-form content audio

## Related Skills

```bash
# ElevenLabs music generation
npx skills add inference-sh/skills@elevenlabs-music

# ElevenLabs TTS (combine voice with effects)
npx skills add inference-sh/skills@elevenlabs-tts

# AI music generation (Diffrythm, Tencent)
npx skills add inference-sh/skills@ai-music-generation

# Full platform skill (all 150+ apps)
npx skills add inference-sh/skills@infsh-cli
```

Browse all audio apps: `infsh app list --category audio`
