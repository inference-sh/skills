---
name: ai-podcast
description: "Generate multi-person talking head podcast videos from scratch using AI — character creation, TTS, avatar animation, and video stitching. Use when the user wants to create a podcast, talking head video, or multi-speaker conversation video."
allowed-tools: Bash, Read, Write, Agent, Glob, Grep
---

## AI Podcast Generator

Create multi-person talking head podcast videos using the inference.sh pipeline: portrait generation → TTS audio → avatar video → merge. Supports real humans (via Phota), 3D mascots, illustrated characters, and mixed casts.

Use when the user wants to create a podcast, talking head video, demo reel, promotional conversation, or any multi-speaker video content.

### Pipeline Overview

```
Characters (images) → TTS (audio per turn) → Avatar (video per turn) → Merge (final video)
```

### Process

#### Step 1: Character Creation

Choose the right tool per character type:

| Character Type | Tool | Notes |
|---------------|------|-------|
| **Real human (new)** | `pruna/p-image` | 16:9, `prompt_upsampling: true`. Quick, no training needed, but identity won't be consistent across multiple generations. |
| **Real human (consistent ID)** | `phota/generate` with `[[profile_id]]` | Consistent identity across all shots. Requires a trained Phota profile first (see below). |
| **Brand mascot / logo character** | `google/gemini-3-pro-image-preview` | Pass logo + character sheet as reference images |
| **Illustrated / stylized** | `google/gemini-3-pro-image-preview` | Pass style reference as input image |

**Training a Phota identity (optional but recommended for humans):**

If you need a real human character with consistent identity across multiple angles and shots, train a Phota profile first:

```bash
infsh app run phota/train --input '{
  "images": ["url1.jpg", "url2.jpg", ...],
  "wait": true
}' --save profile.json
```

- Requires 30-50 face images of the subject
- Training takes a few minutes with `wait: true`
- Returns a `profile_id` you then use in `phota/generate` as `[[profile_id]]` in prompts
- The profile is reusable forever — train once, generate unlimited shots

If you don't need cross-shot consistency (e.g. single-speaker video, one angle only), `pruna/p-image` is simpler and cheaper.

**Character sheets first, podcast frames second:**
1. Generate a character sheet (plain white background, multiple angles) for each character
2. Then place characters into the podcast studio setting using the sheet as reference

**For branded characters (logo on clothing):**
1. Generate the character with a plain version of the garment
2. Use `phota/edit` with the logo as a second reference image to add the logo
3. Always pass the logo image alongside character references when generating new angles

#### Step 2: Alternate Angles

Generate at least 2 angles per character for visual variety:

| Angle | When to use |
|-------|-------------|
| **Front/medium** | Establishing shots, opening, closing |
| **Close-up** | Reactions, emotional moments, punchy lines |

For close-ups, prompt for "tight framing, chest up, shallow depth of field" — not "turned to the side" (which just makes them look away).

**Identity consistency rules:**
- For real humans with a Phota profile: use `phota/generate` or `phota/edit` for new angles — Gemini does not preserve facial identity and will produce a different person
- For real humans without a Phota profile: try to generate all needed angles in one go with `pruna/p-image`, or consider training a Phota profile if you need many shots
- For mascots/illustrations: Gemini 3 Pro is fine, pass the established frame as reference

**Framing rule:** Use tight framing on individual speakers. Wide shots with multiple seats show empty chairs when only one person is on screen.

#### Step 3: QA Frames

Before proceeding, visually inspect all frames for:
- Extra people in the background
- Multiple microphones (should be single mic per shot)
- Wrong or distorted logos
- Inconsistent character identity across angles
- Weird artifacts (extra limbs, merged objects)

Fix issues before generating video — re-rendering video is the most expensive step in the pipeline.

#### Step 4: Write the Script

**Rules for natural conversation:**
- Write it like a real conversation, NOT like people reading ad copy in turns
- Include reactions ("wait, hold on", "that is wild"), interruptions, and follow-up questions
- Vary turn length — short reactions (1 sentence) mixed with longer explanations (2-3 sentences)
- The host should ask real questions, not set up obvious talking points
- Keep total duration target in mind: ~2.5 words/second for natural speech at 1.05x rate

**Duration guide:**
| Target | Words |
|--------|-------|
| 15s | ~38 words |
| 30s | ~75 words |
| 60s | ~150 words |

#### Step 5: Generate TTS Audio

Use `inworld/text-to-speech-2` for each turn.

```bash
infsh app run inworld/text-to-speech-2 --input '{
  "text": "...",
  "voice_id": "...",
  "speaking_rate": 1.05,
  "audio_encoding": "MP3"
}' --save output.json
```

**Voice selection:**
- Generate samples with the same line across candidate voices BEFORE committing
- Let the user listen and approve voices
- Good podcast voices: Tyler, Nate, Lauren, Kelsey, Naomi, Anjali (EN_US)
- Use `inworld/text-to-speech-2:voices` to list all available voices

**Speaking rate:**
- Default to **1.05** for natural podcast pacing
- Use **1.1** for short snappy reactions
- **NEVER go below 1.0** — sounds slow and disengaging
- Keep rate consistent per character across all their turns

All TTS turns can run in parallel (cheap, fast ~2-8s each).

#### Step 6: Generate Video Clips

Use `pruna/p-video-avatar` for each turn.

```bash
infsh app run pruna/p-video-avatar --input '{
  "image": "<character_frame_url>",
  "audio": "<tts_audio_url>",
  "resolution": "720p",
  "video_prompt": "..."
}' --save output.json
```

**Critical: Run clips SEQUENTIALLY, not in parallel.** Parallel runs hit the same GPU and cause CUDA OOM failures. Each clip takes 15-90s depending on audio length.

**Angle assignment plan:** Alternate between front and close-up shots across turns for visual variety. Example for 6 turns:

```
T1: Speaker A — front
T2: Speaker B — front
T3: Speaker C — front (or close-up)
T4: Speaker A — close-up
T5: Speaker B — close-up
T6: Speaker A — front
```

#### Step 7: Merge

Use `infsh/media-merger` to stitch all clips into the final video.

```bash
# Build input JSON
{
  "media_files": [
    {"file": "<clip1_url>"},
    {"file": "<clip2_url>"},
    ...
  ],
  "fps": 24,
  "output_format": "mp4"
}

infsh app run infsh/media-merger --input merger_input.json --save final.json
```

Merger is free and takes 2-6 minutes depending on total duration.

### Rules

1. **Gemini does not preserve human facial identity** — generating alternate angles of a real human with Gemini will produce a different person. For identity-consistent human shots, use Phota with a trained profile_id, or generate all angles in a single batch. This was learned after Gemini produced an entirely different face for a close-up that was supposed to match the front shot.

2. **NEVER run p-video-avatar clips in parallel** — they compete for GPU memory and fail with CUDA OOM. Run them sequentially. This was learned after 2 of 3 parallel runs failed.

3. **NEVER set speaking_rate below 1.0** — it sounds artificial and disengaging. Default to 1.05. Learned from user feedback that 0.9 rate "felt weird and disengaging."

4. **ALWAYS QA frames before generating video** — video generation is the most expensive step in the pipeline. Catching a double mic or wrong logo in the image stage is cheap to fix. Catching it after video generation means re-rendering the entire clip.

5. **ALWAYS use tight framing for individual speaker shots** — wide/establishing shots show empty seats where other speakers should be. Frame from waist or chest up so no empty chairs are visible.

6. **ALWAYS pass the logo as a reference image** when generating branded characters — describing a logo in text produces wrong results. Pass the actual logo file as a second image input.

7. **ALWAYS get voice approval before full production** — generate samples with the same line across 5-8 candidate voices and let the user pick before committing to the full script.

8. **Script should read like a conversation, not an ad** — people reading ad copy in turns sounds fake. Include reactions, interruptions, varied turn lengths, and genuine questions. The host should have personality, not just set up talking points.

### App Reference

| App | Purpose |
|-----|---------|
| `pruna/p-image` | Generate portraits from text |
| `phota/train` | Train identity profile from 30-50 face images |
| `phota/generate` | Generate images with trained identity via `[[profile_id]]` |
| `phota/edit` | Edit images preserving identity of known subjects |
| `google/gemini-3-pro-image-preview` | Image gen/edit, mascots, style transfer |
| `inworld/text-to-speech-2` | Text to speech, 100+ languages, voice steering |
| `pruna/p-video-avatar` | Portrait + audio → talking head video |
| `infsh/media-merger` | Concatenate video clips into one video |

Use `belt task cost <task-id>` to check the cost of any individual task.
