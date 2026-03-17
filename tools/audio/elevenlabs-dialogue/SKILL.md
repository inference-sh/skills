---
name: elevenlabs-dialogue
description: "ElevenLabs multi-speaker dialogue generation - create conversations with different voices in a single audio file via inference.sh CLI. Capabilities: multi-voice dialogue, script-based generation, voice direction, conversation audio. Use for: podcasts, audiobooks, explainers, tutorials, character dialogue, video scripts. Triggers: elevenlabs dialogue, eleven labs dialogue, multi speaker, conversation audio, dialogue generation, text to dialogue, multi voice, voice acting, podcast dialogue, character voices, script to audio, elevenlabs conversation, two speakers"
allowed-tools: Bash(infsh *)
---

# ElevenLabs Dialogue

Generate multi-speaker dialogue audio via [inference.sh](https://inference.sh) CLI.

![Dialogue](https://cloud.inference.sh/u/4mg21r6ta37mpaz6ktzwtt8krr/01jz00krptarq4bwm89g539aea.png)

## Quick Start

> Requires inference.sh CLI (`infsh`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
infsh login

# Generate dialogue
infsh app run elevenlabs/text-to-dialogue --input '{
  "segments": [
    {"text": "Have you tried the new feature?", "voice": "george"},
    {"text": "Not yet, but I heard it is amazing.", "voice": "aria"},
    {"text": "You should check it out today.", "voice": "george"}
  ]
}'
```


## Voice Options

22+ premium voices available for each speaker:

### Popular Pairings

| Pairing | Voices | Best For |
|---------|--------|----------|
| Interview | `george` + `aria` | Professional Q&A |
| Casual Chat | `brian` + `sarah` | Relaxed conversation |
| Tutorial | `daniel` + `jessica` | Instructional |
| Debate | `adam` + `alice` | Contrasting perspectives |
| Podcast | `charlie` + `bella` | Entertainment |

### All Voices

Female: `aria`, `alice`, `bella`, `jessica`, `laura`, `lily`, `sarah`, `matilda`

Male: `george`, `adam`, `bill`, `brian`, `callum`, `charlie`, `chris`, `daniel`, `eric`, `harry`, `liam`, `river`, `roger`, `will`

## Voice Direction

Add directions in square brackets to control delivery:

```bash
infsh app run elevenlabs/text-to-dialogue --input '{
  "segments": [
    {"text": "[excitedly] Guess what happened today!", "voice": "aria"},
    {"text": "[curiously] What? Tell me!", "voice": "george"},
    {"text": "[proudly] We hit ten thousand users!", "voice": "aria"},
    {"text": "[surprised] No way, that is incredible!", "voice": "george"}
  ]
}'
```

### Direction Keywords

| Direction | Effect |
|-----------|--------|
| `[excitedly]` | Energetic, upbeat delivery |
| `[sadly]` | Somber, emotional tone |
| `[whispering]` | Soft, quiet speech |
| `[angrily]` | Intense, forceful delivery |
| `[sarcastically]` | Ironic intonation |
| `[curiously]` | Questioning, intrigued |
| `[proudly]` | Confident, accomplished |
| `[nervously]` | Hesitant, uncertain |
| `[cheerfully]` | Happy, bright |

## Examples

### Podcast Episode

```bash
infsh app run elevenlabs/text-to-dialogue --input '{
  "segments": [
    {"text": "Welcome back to Tech Talk! Today we are discussing the latest in AI.", "voice": "george"},
    {"text": "Thanks for having me. This is such an exciting topic right now.", "voice": "aria"},
    {"text": "So let us start with the big question. How is AI changing creative work?", "voice": "george"},
    {"text": "Great question. I think the biggest shift is in accessibility. Tools that used to require specialized skills are now available to everyone.", "voice": "aria"},
    {"text": "Can you give us a specific example?", "voice": "george"},
    {"text": "Sure. Take audio production. A year ago, you needed a studio and voice actors. Now you can generate professional dialogue with AI voices.", "voice": "aria"}
  ]
}'
```

### Tutorial / Explainer

```bash
infsh app run elevenlabs/text-to-dialogue --input '{
  "segments": [
    {"text": "Can you walk me through the setup process?", "voice": "jessica"},
    {"text": "Sure. Step one, install the CLI. It takes about thirty seconds.", "voice": "daniel"},
    {"text": "And then what?", "voice": "jessica"},
    {"text": "Step two, run the login command. It opens your browser for authentication.", "voice": "daniel"},
    {"text": "That sounds simple enough.", "voice": "jessica"},
    {"text": "It is. Step three, you are ready to run your first app.", "voice": "daniel"}
  ]
}'
```

### Audiobook Dialogue

```bash
infsh app run elevenlabs/text-to-dialogue --input '{
  "segments": [
    {"text": "[whispering] Do you hear that?", "voice": "lily"},
    {"text": "[nervously] Hear what? I do not hear anything.", "voice": "harry"},
    {"text": "Exactly. The forest has gone completely silent.", "voice": "lily"},
    {"text": "[worried] That is not a good sign, is it?", "voice": "harry"},
    {"text": "[firmly] We need to move. Now.", "voice": "lily"}
  ]
}'
```

### Product Demo

```bash
infsh app run elevenlabs/text-to-dialogue --input '{
  "segments": [
    {"text": "So what makes this different from other solutions?", "voice": "brian"},
    {"text": "Three things. Speed, quality, and simplicity.", "voice": "alice"},
    {"text": "That sounds too good to be true.", "voice": "brian"},
    {"text": "[confidently] Let me show you. Watch this.", "voice": "alice"}
  ]
}'
```

### Customer Support Training

```bash
infsh app run elevenlabs/text-to-dialogue --input '{
  "segments": [
    {"text": "[frustrated] I have been waiting for twenty minutes and my issue is still not resolved.", "voice": "adam"},
    {"text": "[empathetically] I completely understand your frustration, and I apologize for the wait. Let me look into this right away.", "voice": "sarah"},
    {"text": "I just need my account access restored.", "voice": "adam"},
    {"text": "Of course. I can see the issue here. Give me just a moment and I will have this fixed for you.", "voice": "sarah"},
    {"text": "Okay, thank you.", "voice": "adam"},
    {"text": "[cheerfully] All done! Your access has been restored. Is there anything else I can help with?", "voice": "sarah"}
  ]
}'
```

## Tips

1. **Vary sentence length** - Mix short reactions with longer explanations
2. **Include reactions** - "Exactly!", "Interesting.", "Hmm." make dialogue natural
3. **Use directions sparingly** - One or two per exchange, not every line
4. **Keep segments short** - Under 3 sentences per turn for natural pacing
5. **Assign distinct voices** - Use contrasting voices for clarity
6. **Write for speaking** - Use contractions, informal language

## Workflow: Dialogue + Music

```bash
# 1. Generate dialogue
infsh app run elevenlabs/text-to-dialogue --input '{
  "segments": [
    {"text": "Welcome to the show.", "voice": "george"},
    {"text": "Great to be here.", "voice": "aria"}
  ]
}' > dialogue.json

# 2. Generate background music
infsh app run elevenlabs/music --input '{
  "prompt": "Soft podcast background music, non-intrusive",
  "duration_seconds": 30
}' > music.json

# 3. Merge
infsh app run infsh/media-merger --input '{
  "media": ["<dialogue-url>", "<music-url>"]
}'
```

## Use Cases

- **Podcasts**: Multi-host episodes, interviews
- **Audiobooks**: Character dialogue scenes
- **E-learning**: Instructor and student exchanges
- **Explainers**: Q&A format tutorials
- **Training**: Customer service scenarios
- **Video Scripts**: Pre-production dialogue testing

## Related Skills

```bash
# ElevenLabs TTS (single-voice narration)
npx skills add inference-sh/skills@elevenlabs-tts

# ElevenLabs music (background for dialogue)
npx skills add inference-sh/skills@elevenlabs-music

# Dia TTS dialogue (free alternative with S1/S2 tags)
npx skills add inference-sh/skills@dialogue-audio

# Full platform skill (all 150+ apps)
npx skills add inference-sh/skills@infsh-cli
```

Browse all audio apps: `infsh app list --category audio`
