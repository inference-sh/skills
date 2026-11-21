---
name: image-editing
description: "Edit existing images with Bria, Reve and Phota via inference.sh CLI. Apps: Bria Eraser, Bria Gen Fill, Bria Expand, Bria Replace Background, Bria Increase Resolution, Bria Product Cutout, Bria Product Packshot, Bria Product Shadow, Bria Image Edit, Reve Edit, Reve Remix, Phota Edit, Phota Enhance. Capabilities: object removal, mask-based generative fill, outpainting, background replacement, instruction-based editing, multi-image remix, identity-preserving edits, photo enhancement, 2x/4x upscaling, product cutouts, packshots and shadows. Use for: photo retouching, e-commerce product images, removing objects, changing backgrounds, extending images. Triggers: edit image, image editing, photo editing, remove object, erase object, inpainting, generative fill, outpainting, expand image, extend canvas, uncrop, replace background, change background, edit with text, instruction edit, bria, reve edit, reve remix, phota, enhance photo, increase resolution, product cutout, packshot, retouch"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# Image Editing

Edit existing images via [inference.sh](https://inference.sh) CLI: remove objects, fill and extend, swap backgrounds, upscale, and prepare product shots.

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

belt app run reve/edit --input '{
  "image": "https://your-photo.jpg",
  "edit_instruction": "Remove all people in the background"
}'
```

## Apps by Task

| Task | App ID | Input | Notes |
|------|--------|-------|-------|
| **Remove** objects | `bria/erase` | image + mask | White = erase, black = keep |
| **Add / replace** in a region | `bria/gen-fill` | image + mask + prompt | Generates prompt content in the masked area |
| **Edit with text** | `bria/edit` | image + instruction | Natural language edits |
| **Edit with text** | `reve/edit` | image + edit_instruction | Optional upscale and transparent background |
| **Edit with identity** | `phota/edit` | 1-10 images + prompt | Keeps trained subject identities, 1K or 4K output |
| **Combine** references | `reve/remix` | 1-6 images + prompt | Refer to images as `0`, `1`, ... in the prompt |
| **Background** replace | `bria/replace-background` | image + prompt or ref_image | Modes: fast, base, high_control |
| **Background** remove | `reve/edit` | image + `remove_background: true` | Transparent output |
| **Extend canvas** | `bria/expand` | image + aspect_ratio or canvas size | Outpainting up to 5000x5000 |
| **Upscale** | `bria/increase-resolution` | image | 2x or 4x, max 8192x8192 |
| **Enhance** photo | `phota/enhance` | image | Lighting, composition, color, sharpness |
| **Product** cutout | `bria/product-cutout` | product image | Transparent background, max 12MB |
| **Product** packshot | `bria/product-packshot` | product image or cutout | 2000x2000, hex or transparent background |
| **Product** shadow | `bria/product-shadow` | product cutout | Regular or floating shadow |

Mask-based apps (`bria/erase`, `bria/gen-fill`) give exact control over the edited region. Instruction-based apps (`bria/edit`, `reve/edit`, `phota/edit`) need no mask.

## Remove

### Erase with a Mask

The mask must match the image aspect ratio.

```bash
belt app run bria/erase --input '{
  "image": "https://street-photo.jpg",
  "mask": "https://person-mask.png"
}'
```

### Erase with an Instruction

```bash
belt app run reve/edit --input '{
  "image": "https://beach-photo.jpg",
  "edit_instruction": "Remove the trash can on the left"
}'
```

## Add or Replace

### Generative Fill

```bash
belt app run bria/gen-fill --input '{
  "image": "https://living-room.jpg",
  "mask": "https://empty-corner-mask.png",
  "prompt": "a tall green potted monstera plant",
  "negative_prompt": "blurry, distorted"
}'
```

### Instruction Edit (Bria)

```bash
belt app run bria/edit --input '{
  "image": "https://portrait.jpg",
  "instruction": "change the shirt color to navy blue"
}'
```

### Instruction Edit (Reve) with Upscale

`test_time_scaling` (1-5) spends more time for quality; values above 1 cost more.

```bash
belt app run reve/edit --input '{
  "image": "https://room.jpg",
  "edit_instruction": "Replace the sofa with a mid-century leather couch",
  "test_time_scaling": 2,
  "upscale_factor": 2
}'
```

### Identity-Preserving Edit (Phota)

Reference trained profiles in the prompt with `[[profile_id]]`.

```bash
belt app run phota/edit --input '{
  "prompt": "put [[my-profile]] in a tailored grey suit, studio lighting",
  "images": ["https://headshot.jpg"],
  "profile_ids": ["my-profile"],
  "resolution": "4K",
  "num_output_images": 2
}'
```

### Remix Multiple Images

```bash
belt app run reve/remix --input '{
  "images": ["https://woman.jpg", "https://vintage-car.jpg"],
  "prompt": "The woman from 0 driving the car from 1 along a coastal road",
  "aspect_ratio": "16:9"
}'
```

## Background

### Replace Background from Text

```bash
belt app run bria/replace-background --input '{
  "image": "https://product-on-desk.jpg",
  "prompt": "a sunlit marble kitchen counter with soft morning light, blurred plants and a window in the background, warm natural tones",
  "mode": "high_control"
}'
```

### Replace Background from a Reference Image

```bash
belt app run bria/replace-background --input '{
  "image": "https://portrait.jpg",
  "ref_image": "https://office-background.jpg"
}'
```

### Transparent Background

```bash
belt app run reve/edit --input '{
  "image": "https://mascot.jpg",
  "edit_instruction": "Keep the mascot unchanged",
  "remove_background": true
}'
```

### Extend the Canvas

```bash
belt app run bria/expand --input '{
  "image": "https://square-photo.jpg",
  "aspect_ratio": "16:9",
  "prompt": "continuation of the forest scene"
}'
```

## Restore and Upscale

### Upscale 4x

```bash
belt app run bria/increase-resolution --input '{
  "image": "https://small-image.jpg",
  "desired_increase": 4
}'
```

### Enhance a Photo

```bash
belt app run phota/enhance --input '{
  "image": "https://dim-phone-photo.jpg",
  "output_format": "jpg"
}'
```

## Product and E-commerce

### Cutout

```bash
belt app run bria/product-cutout --input '{
  "image": "https://sneaker-photo.jpg"
}'
```

### Packshot on White

```bash
belt app run bria/product-packshot --input '{
  "image": "https://sneaker-photo.jpg",
  "background_color": "#FFFFFF"
}'
```

### Floating Shadow

```bash
belt app run bria/product-shadow --input '{
  "image": "https://sneaker-cutout.png",
  "shadow_type": "float",
  "shadow_intensity": 40
}'
```

### Workflow: Raw Photo to Listing Image

```bash
# 1. Cut out the product
belt app run bria/product-cutout --input '{"image": "https://raw-product.jpg"}'

# 2. Add a natural drop shadow
belt app run bria/product-shadow --input '{
  "image": "<cutout-url-from-step-1>",
  "shadow_type": "regular"
}'

# 3. Upscale for zoom views
belt app run bria/increase-resolution --input '{
  "image": "<url-from-step-2>",
  "desired_increase": 2
}'
```

## Pricing

| App | Price |
|-----|-------|
| `bria/erase` | $0.02/image |
| `bria/expand` | $0.02/image |
| `bria/increase-resolution` | $0.02/image |
| `bria/product-cutout` | $0.018/image |
| `bria/product-shadow` | $0.018/image |
| `bria/gen-fill` | $0.03/image |
| `bria/replace-background` | $0.03/image |
| `bria/product-packshot` | $0.03/image |
| `bria/edit` | $0.03/image |
| `phota/edit` | $0.09/image standard, $0.18/image 4K |
| `phota/enhance` | $0.13/image |

Check any app with `belt app pricing <app-id>`.

## Tips

- Get the full input schema for any app: `belt app sample <app-id>`
- `bria/replace-background` prompts work best in English at 50-110 words.
- `bria/expand` without `aspect_ratio` needs `canvas_size`, `original_image_size` and `original_image_location`.
- Pass a seed (`seed` on Bria apps) to reproduce a result.

## Related Skills

```bash
# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# Image generation
npx skills add inference-sh/skills@ai-image-generation

# Background removal
npx skills add inference-sh/skills@background-removal

# Upscaling
npx skills add inference-sh/skills@image-upscaling

# Product photography
npx skills add inference-sh/skills@ai-product-photography
npx skills add inference-sh/skills@product-photography
```

Browse all image apps: `belt app list --category image`

## Documentation

- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
- [Image Generation Example](https://inference.sh/docs/examples/image-generation) - Complete image workflow guide
- [Apps Overview](https://inference.sh/docs/apps/overview) - Understanding the app ecosystem
