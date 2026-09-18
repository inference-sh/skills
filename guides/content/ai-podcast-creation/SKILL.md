---
name: ai-podcast-creation
description: "Create AI-powered podcasts with text-to-speech, music, and audio editing. Tools: Kokoro TTS, DIA TTS, Chatterbox, AI music generation, ffmpeg for audio assembly. Capabilities: multi-voice conversations, background music, intro/outro, full episodes. Use for: podcast production, audiobooks, voice content, audio newsletters. Triggers: podcast, ai podcast, text to speech podcast, audio content, voice over, ai audiobook, multi voice, conversation ai, notebooklm alternative, audio generation, podcast automation, ai narrator, voice content, audio newsletter, podcast maker"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# AI Podcast Creation

Create AI-powered podcasts and audio content via [inference.sh](https://inference.sh) CLI.

![AI Podcast Creation](https://cloud.inference.sh/u/4mg21r6ta37mpaz6ktzwtt8krr/01jz00krptarq4bwm89g539aea.png)

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

# Generate podcast segment
belt app run falai/kokoro-tts --input '{
  "prompt": "Welcome to the AI Frontiers podcast. Today we explore the latest developments in generative AI.",
  "voice": "am_michael"
}'
```


## Available Voices

### Kokoro TTS

| Voice ID | Description | Best For |
|----------|-------------|----------|
| `af_sarah` | American female, warm | Host, narrator |
| `af_nicole` | American female, professional | News, business |
| `am_michael` | American male, authoritative | Documentary, tech |
| `am_adam` | American male, conversational | Casual podcast |
| `bf_emma` | British female, refined | Audiobooks |
| `bm_george` | British male, classic | Formal content |

### DIA TTS (Conversational)

| Voice ID | Description | Best For |
|----------|-------------|----------|
| `dia-conversational` | Natural conversation | Dialogue, interviews |

### Chatterbox

| Voice ID | Description | Best For |
|----------|-------------|----------|
| `chatterbox-default` | Expressive | Casual, entertainment |

## Podcast Workflows

### Simple Narration

```bash
# Single voice podcast segment
belt app run falai/kokoro-tts --input '{
  "prompt": "Your podcast script here. Make it conversational and engaging. Add natural pauses with punctuation.",
  "voice": "am_michael"
}'
```

### Multi-Voice Conversation

```bash
# Host introduction
belt app run falai/kokoro-tts --input '{
  "prompt": "Welcome back to Tech Talk. Today I have a special guest to discuss AI developments.",
  "voice": "am_michael"
}' > host_intro.json

# Guest response
belt app run falai/kokoro-tts --input '{
  "prompt": "Thanks for having me. I am excited to share what we have been working on.",
  "voice": "af_sarah"
}' > guest_response.json

# Merge into conversation (no inference.sh app concatenates audio; use ffmpeg locally)
curl -L -o host.mp3 "<host-url>"
curl -L -o guest.mp3 "<guest-url>"
ffmpeg -i host.mp3 -i guest.mp3 -filter_complex "acrossfade=d=0.5" conversation.mp3
```

### Full Episode Pipeline

```bash
# 1. Generate script with Claude
belt app run openrouter/claude-sonnet-45 --input '{
  "prompt": "Write a 5-minute podcast script about the impact of AI on creative work. Format as a two-person dialogue between HOST and GUEST. Include natural conversation, questions, and insights."
}' > script.json

# 2. Generate intro music
belt app run infsh/ai-music --input '{
  "prompt": "Podcast intro music, upbeat, modern, tech feel, 15 seconds"
}' > intro_music.json

# 3. Generate host segments
belt app run falai/kokoro-tts --input '{
  "prompt": "<host-lines>",
  "voice": "am_michael"
}' > host.json

# 4. Generate guest segments
belt app run falai/kokoro-tts --input '{
  "prompt": "<guest-lines>",
  "voice": "af_sarah"
}' > guest.json

# 5. Generate outro music
belt app run infsh/ai-music --input '{
  "prompt": "Podcast outro music, matching intro style, fade out, 10 seconds"
}' > outro_music.json

# 6. Merge everything (no inference.sh app concatenates audio; use ffmpeg locally)
curl -L -o intro.mp3 "<intro-music>"
curl -L -o host.mp3 "<host>"
curl -L -o guest.mp3 "<guest>"
curl -L -o outro.mp3 "<outro-music>"
ffmpeg -i intro.mp3 -i host.mp3 -i guest.mp3 -i outro.mp3 -filter_complex \
  "[0][1]acrossfade=d=1[a];[a][2]acrossfade=d=1[b];[b][3]acrossfade=d=1" episode.mp3
```

### NotebookLM-Style Content

Generate podcast-style discussions from documents.

```bash
# 1. Extract key points
belt app run openrouter/claude-sonnet-45 --input '{
  "prompt": "Read this document and create a podcast script where two hosts discuss the key points in an engaging, conversational way. Include questions, insights, and natural dialogue.\n\nDocument:\n<your-document-content>"
}' > discussion_script.json

# 2. Generate Host A
belt app run falai/kokoro-tts --input '{
  "prompt": "<host-a-lines>",
  "voice": "am_michael"
}' > host_a.json

# 3. Generate Host B
belt app run falai/kokoro-tts --input '{
  "prompt": "<host-b-lines>",
  "voice": "af_sarah"
}' > host_b.json

# 4. Interleave and merge (no inference.sh app concatenates audio; use ffmpeg locally)
curl -L -o a1.mp3 "<host-a-1>"; curl -L -o b1.mp3 "<host-b-1>"
curl -L -o a2.mp3 "<host-a-2>"; curl -L -o b2.mp3 "<host-b-2>"
ffmpeg -i a1.mp3 -i b1.mp3 -i a2.mp3 -i b2.mp3 -filter_complex \
  "[0][1]acrossfade=d=0.3[a];[a][2]acrossfade=d=0.3[b];[b][3]acrossfade=d=0.3" conversation.mp3
```

### Audiobook Chapter

```bash
# Long-form narration
belt app run falai/kokoro-tts --input '{
  "prompt": "Chapter One. It was a dark and stormy night when the first AI achieved consciousness...",
  "voice": "bf_emma",
  "language": "british-english",
  "speed": 0.9
}'
```

## Audio Enhancement

### Add Background Music

```bash
# 1. Generate podcast audio
belt app run falai/kokoro-tts --input '{
  "prompt": "<podcast-script>",
  "voice": "am_michael"
}' > podcast.json

# 2. Generate ambient music
belt app run infsh/ai-music --input '{
  "prompt": "Soft ambient background music for podcast, subtle, non-distracting, loopable"
}' > background.json

# 3. Mix with lower background volume (no inference.sh app mixes audio; use ffmpeg locally)
curl -L -o podcast.mp3 "<podcast-url>"
curl -L -o background.mp3 "<background-url>"
ffmpeg -i podcast.mp3 -stream_loop -1 -i background.mp3 -filter_complex \
  "[1]volume=0.15[bg];[0][bg]amix=inputs=2:duration=first" mixed.mp3
```

### Add Sound Effects

```bash
# Transition sounds between segments
belt app run infsh/ai-music --input '{
  "prompt": "Short podcast transition sound, whoosh, 2 seconds"
}' > transition.json
```

## Script Writing Tips

### Prompt for Claude

```bash
belt app run openrouter/claude-sonnet-45 --input '{
  "prompt": "Write a podcast script with these requirements:
  - Topic: [YOUR TOPIC]
  - Duration: 5 minutes (about 750 words)
  - Format: Two hosts (HOST_A and HOST_B)
  - Tone: Conversational, informative, engaging
  - Include: Hook intro, 3 main points, call to action
  - Mark speaker changes clearly

  Make it sound natural, not scripted. Add verbal fillers like \"you know\" and \"I mean\" occasionally."
}'
```

## Podcast Templates

### Interview Format

```
HOST: Introduction and welcome
GUEST: Thank you, happy to be here
HOST: First question about background
GUEST: Response with story
HOST: Follow-up question
GUEST: Deeper insight
... continue pattern ...
HOST: Closing question
GUEST: Final thoughts
HOST: Thank you and outro
```

### Solo Episode

```
Introduction with hook
Topic overview
Point 1 with examples
Point 2 with examples
Point 3 with examples
Summary and takeaways
Call to action
Outro
```

### News Roundup

```
Intro music
Welcome and date
Story 1: headline + details
Story 2: headline + details
Story 3: headline + details
Analysis/opinion segment
Outro
```

## Best Practices

1. **Natural punctuation** - Use commas and periods for pacing
2. **Short sentences** - Easier to speak and listen
3. **Varied voices** - Different speakers prevent monotony
4. **Background music** - Subtle, at 10-15% volume
5. **Crossfades** - Smooth transitions between segments
6. **Edit scripts** - Remove filler before generating

## Related Skills

```bash
# Text-to-speech models
npx skills add inference-sh/skills@text-to-speech

# AI music generation
npx skills add inference-sh/skills@ai-music-generation

# LLM for scripts
npx skills add inference-sh/skills@llm-models

# Content pipelines
npx skills add inference-sh/skills@ai-content-pipeline

# Full platform skill
npx skills add inference-sh/skills@infsh-cli
```

Browse all apps: `belt app list --category audio`

