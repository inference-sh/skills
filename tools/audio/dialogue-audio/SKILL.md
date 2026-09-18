---
name: dialogue-audio
description: "Multi-speaker dialogue audio creation with ElevenLabs and Dia TTS. Covers speaker tags, emotion control, pacing, conversation flow, and post-production. Use for: podcasts, audiobooks, explainers, character dialogue, conversational content. Triggers: dialogue audio, multi speaker, conversation audio, dia tts, two speakers, podcast audio, character voices, voice acting, dialogue generation, conversation tts, multi voice, speaker tags, dialogue recording, elevenlabs dialogue, eleven labs conversation"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# Dialogue Audio

Create realistic multi-speaker dialogue with Dia TTS via [inference.sh](https://inference.sh) CLI.

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

# Two-speaker conversation
belt app run falai/dia-tts --input '{
  "prompt": "[S1] Have you tried the new feature yet? [S2] Not yet, but I heard it saves a ton of time. [S1] It really does. I cut my workflow in half. [S2] Okay, I am definitely trying it today."
}'
```


## Speaker Tags

Dia TTS uses `[S1]` and `[S2]` to distinguish two speakers.

| Tag | Role | Voice |
|-----|------|-------|
| `[S1]` | Speaker 1 | Automatically assigned voice A |
| `[S2]` | Speaker 2 | Automatically assigned voice B |

**Rules:**
- Always start each speaker turn with the tag
- Tags must be uppercase: `[S1]` not `[s1]`
- Maximum 2 speakers per generation
- Each speaker maintains consistent voice within a session

## Emotion & Expression Control

Dia TTS interprets punctuation and non-speech cues for emotional delivery.

### Punctuation Effects

| Punctuation | Effect | Example |
|-------------|--------|---------|
| `.` | Neutral, declarative, medium pause | "This is important." |
| `!` | Emphasis, excitement, energy | "This is amazing!" |
| `?` | Rising intonation, questioning | "Are you sure about that?" |
| `...` | Hesitation, trailing off, long pause | "I thought it would work... but it didn't." |
| `,` | Short breath pause | "First, we analyze. Then, we act." |
| `—` or `--` | Interruption or pivot | "I was going to say — never mind." |

### Non-Speech Sounds

Dia TTS supports parenthetical sound descriptions:

```
(laughs)      — laughter
(sighs)       — exasperation or relief
(clears throat) — attention-getting pause
(whispers)    — softer delivery
(gasps)       — surprise
```

### Examples with Emotion

```bash
# Excited conversation
belt app run falai/dia-tts --input '{
  "prompt": "[S1] Guess what happened today! [S2] What? Tell me! [S1] We hit ten thousand users! [S2] (gasps) No way! That is incredible! [S1] I know... I still cannot believe it."
}'

# Serious/thoughtful dialogue
belt app run falai/dia-tts --input '{
  "prompt": "[S1] We need to talk about the timeline. [S2] (sighs) I know. It is tight. [S1] Can we cut anything from the scope? [S2] Maybe... but it would mean dropping the analytics dashboard. [S1] That is a tough trade-off."
}'

# Teaching/explaining
belt app run falai/dia-tts --input '{
  "prompt": "[S1] So how does it actually work? [S2] Great question. Think of it like a pipeline. Data comes in on one end, gets processed in the middle, and comes out transformed on the other side. [S1] Like an assembly line? [S2] Exactly! Each step adds something."
}'
```

## Pacing Control

### Pause Hierarchy

| Technique | Pause Length | Use For |
|-----------|-------------|---------|
| Comma `,` | ~0.3 seconds | Between clauses, list items |
| Period `.` | ~0.5 seconds | Between sentences |
| Ellipsis `...` | ~1.0 seconds | Dramatic pause, thinking, hesitation |
| New speaker tag | ~0.3 seconds | Natural turn-taking gap |

### Speed Control

- **Shorter sentences** = faster perceived pace
- **Longer sentences with commas** = measured, thoughtful pace
- **Questions followed by answers** = engaging back-and-forth rhythm

```bash
# Fast-paced, energetic
belt app run falai/dia-tts --input '{
  "prompt": "[S1] Ready? [S2] Ready. [S1] Let us go! Three features. Five minutes. [S2] Hit it! [S1] Feature one: real-time sync."
}'

# Slow, contemplative
belt app run falai/dia-tts --input '{
  "prompt": "[S1] I have been thinking about this for a while... and I think we need to change direction. [S2] What do you mean? [S1] The market has shifted. What worked last year... is not working now."
}'
```

## Conversation Structure Patterns

### Interview Format

```bash
belt app run falai/dia-tts --input '{
  "prompt": "[S1] Welcome to the show. Today we have a special guest. Tell us about yourself. [S2] Thanks for having me! I am a product designer, and I have been building tools for creators for about ten years. [S1] What got you started in design? [S2] Honestly? I was terrible at coding but loved making things look good. (laughs) So design was the natural path."
}'
```

### Tutorial / Explainer

```bash
belt app run falai/dia-tts --input '{
  "prompt": "[S1] Can you walk me through the setup process? [S2] Sure. Step one, install the CLI. It takes about thirty seconds. [S1] And then? [S2] Step two, run the login command. It will open your browser for authentication. [S1] That sounds simple. [S2] It is! Step three, you are ready to run your first app."
}'
```

### Debate / Discussion

```bash
belt app run falai/dia-tts --input '{
  "prompt": "[S1] I think we should go with option A. It is faster to implement. [S2] But option B scales better long-term. [S1] Sure, but we need something shipping this quarter. [S2] Fair point... what if we do A now with a migration path to B? [S1] That could work. Let us prototype it."
}'
```

## Post-Production Tips

### Volume Normalization

Both speakers should be at consistent volume. If one is louder:

```bash
# Merge with balanced audio
belt app run infsh/video-audio-merger --input '{
  "video_file": "talking-head.mp4",
  "audio_file": "dialogue.mp3"
}'
```

### Adding Background/Music

```bash
# Merge dialogue with background music (no inference.sh app mixes audio; use ffmpeg locally)
ffmpeg -i dialogue.mp3 -stream_loop -1 -i background-music.mp3 -filter_complex \
  "[1]volume=0.15[bg];[0][bg]amix=inputs=2:duration=first" mixed.mp3
```

### Segmenting Long Conversations

For conversations longer than ~30 seconds, generate in segments:

```bash
# Segment 1: Introduction
belt app run falai/dia-tts --input '{
  "prompt": "[S1] Welcome back to another episode..."
}'

# Segment 2: Main content
belt app run falai/dia-tts --input '{
  "prompt": "[S1] So let us dive into today s topic..."
}'

# Segment 3: Wrap-up
belt app run falai/dia-tts --input '{
  "prompt": "[S1] Great conversation today..."
}'

# Merge all segments (no inference.sh app concatenates audio; use ffmpeg locally)
ffmpeg -i segment1.mp3 -i segment2.mp3 -i segment3.mp3 -filter_complex \
  "[0][1][2]concat=n=3:v=0:a=1" conversation.mp3
```

## Script Writing Tips

| Do | Don't |
|----|-------|
| Write how people talk | Write how people write |
| Short sentences (< 15 words) | Long academic sentences |
| Contractions ("can't", "won't") | Formal ("cannot", "will not") |
| Natural fillers ("So,", "Well,") | Every sentence perfectly formed |
| Vary sentence length | All sentences same length |
| Include reactions ("Exactly!", "Hmm.") | One-sided monologues |
| Read it aloud before generating | Assume it sounds right |

## Common Mistakes

| Mistake | Problem | Fix |
|---------|---------|-----|
| Monologues longer than 3 sentences | Sounds like a lecture, not conversation | Break into exchanges |
| No emotional variation | Flat, robotic delivery | Use punctuation and non-speech cues |
| Missing speaker tags | Voices don't alternate | Start every turn with `[S1]` or `[S2]` |
| Formal written language | Sounds unnatural spoken | Use contractions, short sentences |
| No pauses between topics | Feels rushed | Use `...` or scene breaks |
| All same energy level | Monotonous | Vary between high/low energy moments |

## Related Skills

```bash
# ElevenLabs dialogue (22+ voices, voice direction)
npx skills add inference-sh/skills@elevenlabs-dialogue

npx skills add inference-sh/skills@text-to-speech
npx skills add inference-sh/skills@ai-podcast-creation
npx skills add inference-sh/skills@ai-avatar-video
```

Browse all apps: `belt app list`

