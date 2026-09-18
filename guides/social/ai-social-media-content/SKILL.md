---
name: ai-social-media-content
description: "Create AI-powered social media content for TikTok, Instagram, YouTube, Twitter/X. Generate: images, videos, reels, shorts, thumbnails, captions, hashtags. Tools: FLUX, Veo, Seedance, Wan, Kokoro TTS, Claude for copywriting. Use for: content creators, social media managers, influencers, brands. Triggers: social media content, tiktok, instagram reels, youtube shorts, twitter post, content creator, ai influencer, social content, reels, shorts, viral content, thumbnail generator, caption generator, hashtag generator, ugc content"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# AI Social Media Content

Create social media content for all platforms via [inference.sh](https://inference.sh) CLI.

![AI Social Media Content](https://cloud.inference.sh/app/files/u/4mg21r6ta37mpaz6ktzwtt8krr/01kg2c0egyg243mnyth4y6g51q.jpeg)

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

# Generate a TikTok-style video
belt app run google/veo-3-1-fast --input '{
  "prompt": "POV walking through a neon-lit Tokyo street at night, vertical format 9:16, cinematic"
}'
```


## Platform Formats

| Platform | Aspect Ratio | Duration | Resolution |
|----------|--------------|----------|------------|
| TikTok | 9:16 vertical | 15-60s | 1080x1920 |
| Instagram Reels | 9:16 vertical | 15-90s | 1080x1920 |
| Instagram Feed | 1:1 or 4:5 | - | 1080x1080 |
| YouTube Shorts | 9:16 vertical | <60s | 1080x1920 |
| YouTube Thumbnail | 16:9 | - | 1280x720 |
| Twitter/X | 16:9 or 1:1 | <140s | 1920x1080 |

## Content Workflows

### TikTok / Reels Video

```bash
# Generate trending-style content
belt app run google/veo-3-1-fast --input '{
  "prompt": "Satisfying slow motion video of paint being mixed, vibrant colors swirling together, vertical 9:16, ASMR aesthetic, viral TikTok style"
}'
```

### Instagram Carousel Images

```bash
# Generate cohesive carousel images
for i in 1 2 3 4 5; do
  belt app run falai/flux-dev --input "{
    \"prompt\": \"Minimalist lifestyle flat lay photo $i/5, morning coffee routine, neutral tones, Instagram aesthetic, consistent style\"
  }" > "carousel_$i.json"
done
```

### YouTube Thumbnail

```bash
# Eye-catching thumbnail
belt app run falai/flux-dev --input '{
  "prompt": "YouTube thumbnail, shocked face emoji, bright yellow background, bold text area on right, attention-grabbing, high contrast, professional"
}'
```

### Twitter/X Visual Post

```bash
# Generate image for tweet
belt app run falai/flux-dev --input '{
  "prompt": "Tech infographic style image showing AI trends, modern design, data visualization aesthetic, shareable"
}'

# Post with Twitter automation
belt app run x/post-create --input '{
  "text": "The future of AI is here. Here are the top 5 trends reshaping tech in 2024 🧵",
  "media": ["<image-url>"]
}'
```

### Talking Head Content

```bash
# 1. Write script with Claude
belt app run openrouter/claude-sonnet-45 --input '{
  "prompt": "Write a 30-second engaging script about productivity tips for a TikTok. Conversational, hook in first 3 seconds."
}' > script.json

# 2. Generate voiceover
belt app run falai/kokoro-tts --input '{
  "prompt": "<script>",
  "voice": "af_sarah"
}' > voice.json

# 3. Create AI avatar
belt app run bytedance/omnihuman-1-5 --input '{
  "image": "https://your-avatar.jpg",
  "audio": "<voice-url>"
}'
```

## Content Type Templates

### Trending/Viral Style

```bash
belt app run google/veo-3-1 --input '{
  "prompt": "Satisfying compilation style video, oddly satisfying content, smooth transitions, ASMR quality, vertical 9:16"
}'
```

### Tutorial/How-To

```bash
belt app run google/veo-3-1 --input '{
  "prompt": "Hands demonstrating a craft tutorial, overhead shot, clean workspace, step-by-step motion, warm lighting, vertical format"
}'
```

### Product Showcase

```bash
belt app run bytedance/seedance-2-0 --input '{
  "prompt": "Product unboxing aesthetic, sleek packaging reveal, soft lighting, premium feel, satisfying unwrap",
  "ratio": "9:16",
  "generate_audio": true
}'
```

### Lifestyle/Aesthetic

```bash
belt app run google/veo-3-1-fast --input '{
  "prompt": "Day in my life aesthetic, morning routine montage, golden hour lighting, cozy apartment, coffee steam rising, vertical format"
}'
```

### Behind the Scenes

```bash
belt app run google/veo-3-1-fast --input '{
  "prompt": "Behind the scenes of creative workspace, artist at work, authentic candid moments, documentary style, vertical 9:16"
}'
```

## Caption & Hashtag Generation

```bash
# Generate engaging caption
belt app run openrouter/claude-haiku-45 --input '{
  "prompt": "Write an engaging Instagram caption for a sunset beach photo. Include a hook, value, and call to action. Add 10 relevant hashtags."
}'
```

### Hook Formulas

```bash
belt app run openrouter/claude-haiku-45 --input '{
  "prompt": "Generate 5 viral TikTok hooks for a video about morning routines. Use proven patterns like: curiosity gap, bold claim, relatable struggle, before/after, or tutorial format."
}'
```

## Multi-Platform Repurposing

### Long to Short Pipeline

```bash
# Take a concept and create multiple formats
CONCEPT="productivity hack: 2-minute rule"

# TikTok vertical
belt app run google/veo-3-1-fast --input "{
  \"prompt\": \"$CONCEPT visualization, vertical 9:16, quick cuts, text overlays style\"
}"

# Twitter square
belt app run falai/flux-dev --input "{
  \"prompt\": \"$CONCEPT infographic, square format, minimal design, shareable\"
}"

# YouTube thumbnail
belt app run falai/flux-dev --input "{
  \"prompt\": \"$CONCEPT thumbnail, surprised person, bold text space, 16:9\"
}"
```

## Batch Content Creation

```bash
# Generate a week of content
TOPICS=("morning routine" "productivity tips" "coffee aesthetic" "workspace tour" "night routine")

for topic in "${TOPICS[@]}"; do
  belt app run google/veo-3-1-fast --input "{
    \"prompt\": \"$topic content for social media, aesthetic, vertical 9:16, engaging\"
  }" > "content_${topic// /_}.json"
done
```

## Best Practices

1. **Hook in first 3 seconds** - Start with most engaging moment
2. **Vertical first** - 9:16 for TikTok, Reels, Shorts
3. **Consistent aesthetic** - Match brand colors and style
4. **Text-safe zones** - Leave space for platform UI elements
5. **Trending audio** - Add popular sounds separately
6. **Batch create** - Generate multiple pieces at once

## Platform-Specific Tips

### TikTok
- Fast cuts, trending sounds
- Text overlays important
- Hook immediately

### Instagram
- High visual quality
- Carousel for engagement
- Aesthetic consistency

### YouTube Shorts
- Clear value proposition
- Subscribe CTAs work
- Repurpose longer content

### Twitter/X
- Single striking image
- Controversial hooks work
- Thread potential

## Related Skills

```bash
# Video generation
npx skills add inference-sh/skills@ai-video-generation

# Image generation
npx skills add inference-sh/skills@ai-image-generation

# Twitter automation
npx skills add inference-sh/skills@twitter-automation

# Text-to-speech for voiceovers
npx skills add inference-sh/skills@text-to-speech

# Full platform skill
npx skills add inference-sh/skills@infsh-cli
```

Browse all apps: `belt app list`

