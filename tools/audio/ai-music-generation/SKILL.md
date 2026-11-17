---
name: ai-music-generation
description: "Generate AI music and songs with MiniMax Music, ElevenLabs, Diffrythm, Tencent Song Generation via inference.sh CLI. Models: MiniMax Music 3.0 (up to 5 min, vocals or instrumental), ElevenLabs Music (up to 10 min, commercial license), Diffrythm (fast song generation), Tencent Song Generation (full songs with vocals). Capabilities: text-to-music, song generation, instrumental, lyrics to song, soundtrack creation. Use for: background music, social media content, game soundtracks, podcasts, royalty-free music. Triggers: music generation, ai music, generate song, ai composer, text to music, song generator, create music with ai, suno alternative, udio alternative, ai song, ai soundtrack, generate soundtrack, ai jingle, music ai, beat generator, elevenlabs music, eleven labs music, minimax music"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# AI Music Generation

Generate music and songs via [inference.sh](https://inference.sh) CLI.

![AI Music Generation](https://cloud.inference.sh/u/4mg21r6ta37mpaz6ktzwtt8krr/01jz01qvx0gdcyvhvhpfjjb6s4.png)

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

# Generate a song
belt app run minimax/music-3-0 --input '{"prompt": "upbeat electronic dance track", "is_instrumental": true}'
```


## Available Models

| Model | App ID | Best For |
|-------|--------|----------|
| MiniMax Music 3.0 | `minimax/music-3-0` | Up to 5 min, vocals with lyrics or instrumental |
| ElevenLabs Music | `elevenlabs/music` | Up to 10 min, commercial license |
| Diffrythm | `infsh/diffrythm` | Fast song generation from LRC lyrics |
| Tencent Song | `infsh/tencent-song-generation` | Full songs with vocals |

## Browse Audio Apps

```bash
belt app list --category audio
```

## Examples

### Instrumental Track

```bash
belt app run minimax/music-3-0 --input '{
  "prompt": "cinematic orchestral soundtrack, epic and dramatic",
  "is_instrumental": true
}'
```

### Song with Vocals

```bash
belt app sample infsh/tencent-song-generation --save input.json

# Edit input.json:
# {
#   "descriptions": "pop song about summer love",
#   "lyrics": "Walking on the beach with you..."
# }

belt app run infsh/tencent-song-generation --input input.json
```

### Background Music for Video

```bash
belt app run elevenlabs/music --input '{
  "prompt": "calm lo-fi hip hop beat, study music, relaxing",
  "duration_seconds": 60
}'
```

### Podcast Intro

```bash
belt app run elevenlabs/music --input '{
  "prompt": "short podcast intro jingle, professional, tech themed",
  "duration_seconds": 10
}'
```

### Game Soundtrack

```bash
belt app run infsh/diffrythm --input '{
  "style_prompt": "retro 8-bit video game music, adventure theme, chiptune",
  "lyrics": "[00:00.00]Press start, the quest begins\n[00:05.00]Pixel heroes, let the journey in"
}'
```

## Prompt Tips

**Genre keywords**: pop, rock, electronic, jazz, classical, hip-hop, lo-fi, ambient, orchestral

**Mood keywords**: happy, sad, energetic, calm, dramatic, epic, mysterious, uplifting

**Instrument keywords**: piano, guitar, synth, drums, strings, brass, choir

**Structure keywords**: intro, verse, chorus, bridge, outro, loop

## Use Cases

- **Social Media**: Background music for videos
- **Podcasts**: Intro/outro jingles
- **Games**: Soundtracks and effects
- **Videos**: Background scores
- **Ads**: Commercial jingles
- **Content Creation**: Royalty-free music

## Related Skills

```bash
# ElevenLabs music (up to 10 min, commercial license)
npx skills add inference-sh/skills@elevenlabs-music

# ElevenLabs sound effects (combine with music)
npx skills add inference-sh/skills@elevenlabs-sound-effects

# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# Text-to-speech
npx skills add inference-sh/skills@text-to-speech

# Video generation (add music to videos)
npx skills add inference-sh/skills@ai-video-generation

# Speech-to-text
npx skills add inference-sh/skills@speech-to-text
```

Browse all apps: `belt app list`

## Documentation

- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
- [Content Pipeline Example](https://inference.sh/docs/examples/content-pipeline) - Building media workflows
- [Apps Overview](https://inference.sh/docs/apps/overview) - Understanding the app ecosystem

