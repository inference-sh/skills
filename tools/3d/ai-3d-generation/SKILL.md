---
name: ai-3d-generation
description: "Generate 3D models, rig characters and create PBR materials via inference.sh CLI. Models: Tripo 3.1, Meshy 7.1, Hunyuan 3D 3.1, Rodin Gen-2.5, HunyuanImage-to-3D 2, Rodin 3D Generator. Rigging: Tripo Auto Rig, Hunyuan 3D Auto Rigging, TokenRig. Materials: PATINA text-to-material, image-to-material, extract-material. Capabilities: text-to-3D, image-to-3D, multiview-to-3D, PBR textures up to 8K, quad and low-poly topology, part segmentation, auto rigging, preset animations, seamless tiling PBR maps. Use for: game assets, 3D printing, AR/VR models, product visualization, character rigging, texture libraries. Triggers: 3d model, text to 3d, image to 3d, 3d generation, 3d asset, glb, fbx, obj, stl, usdz, mesh generation, tripo, meshy, hunyuan 3d, rodin, hyper3d, auto rig, rigging, skinning, mixamo, pbr material, pbr texture, seamless texture, tileable texture, normal map, roughness map, patina"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# AI 3D Generation

Generate 3D models, rig them, and create PBR materials via [inference.sh](https://inference.sh) CLI.

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

belt app run tripo/tripo-3-1 --input '{"prompt": "a low-poly treasure chest with gold trim"}'
```

## 3D Model Generation

| Model | App ID | Inputs | Best For |
|-------|--------|--------|----------|
| Tripo 3.1 | `tripo/tripo-3-1` | text, image, or front + left/back/right views | PBR textures up to 8K, Ultra geometry, quad and smart low-poly, part segmentation |
| Meshy 7.1 | `meshy/meshy-7-1` | text, 1 image, or 2-4 views | Widest export choice: GLB, FBX, OBJ, STL, USDZ, 3MF; A/T-pose for characters |
| Hunyuan 3D 3.1 | `tencent/hunyuan-3d-3-1` | text, image, or front + up to 7 views | Face count up to 1.5M, geometry-only option |
| Rodin Gen-2.5 | `hyper3d/rodin-gen-2-5` | text or 1-5 images | Five quality tiers, raw or quad meshes, textures up to 8K |
| HunyuanImage-to-3D 2 | `infsh/hunyuan-image-to-3d-2` | image and/or text, extra views | Built-in background, floater and face cleanup |
| Rodin 3D Generator | `infsh/rodin-3d-generator` | 1-5 images (+ optional prompt) | Image-to-3D with preset mesh sizes |

## Rigging

| Model | App ID | Input | Best For |
|-------|--------|-------|----------|
| Tripo Auto Rig | `tripo/tripo-rig` | GLB, FBX, OBJ or STL (up to 150 MB) | Bipeds, quadrupeds, other creatures; Mixamo/Unity/Unreal/VRM bone naming; up to 5 preset animations |
| Hunyuan 3D Auto Rigging | `tencent/hunyuan-3d-auto-rigging` | T-posed FBX or OBJ (up to 60 MB) | Humanoid characters, one of 48 preset animations |
| TokenRig | `infsh/tokenrig` | GLB, glTF, FBX, OBJ, DAE or .blend | Skeleton and skin weights in one pass, keeps original textures |

## PBR Materials

| Model | App ID | Input | Output |
|-------|--------|-------|--------|
| PATINA Text to Material | `patina/text-to-material` | text (optional image/mask) | Seamless tiling texture up to 8K + PBR maps |
| PATINA Image to Material | `patina/image-to-material` | photo or render | basecolor, normal, roughness, metalness, height maps |
| PATINA Extract Material | `patina/extract-material` | image + text describing the region | Seamless texture + PBR maps from part of an image |

## Examples

### Text-to-3D

```bash
belt app run tripo/tripo-3-1 --input '{
  "prompt": "a medieval wooden shield with iron rivets",
  "texture_quality": "detailed",
  "pbr": true
}'
```

### Image-to-3D

```bash
belt app run tripo/tripo-3-1 --input '{
  "image": "https://your-product-photo.jpg",
  "geometry_quality": "detailed",
  "orientation": "align_image"
}'
```

### Multiview-to-3D

Front view in `image`, other sides in their own fields:

```bash
belt app run tripo/tripo-3-1 --input '{
  "image": "https://front.jpg",
  "left_image": "https://left.jpg",
  "back_image": "https://back.jpg",
  "right_image": "https://right.jpg"
}'
```

### Game-Ready Low-Poly

```bash
belt app run tripo/tripo-3-1 --input '{
  "prompt": "a stylized pine tree",
  "smart_low_poly": true,
  "face_limit": 5000
}'
```

### Editable Parts

Part segmentation needs `texture` and `pbr` off:

```bash
belt app run tripo/tripo-3-1 --input '{
  "prompt": "a vintage desk lamp",
  "generate_parts": true,
  "texture": false,
  "pbr": false
}'
```

### 3D Printing (Meshy, STL)

```bash
belt app run meshy/meshy-7-1 --input '{
  "prompt": "a chess knight piece",
  "should_texture": false,
  "format": "stl"
}'
```

### Character in T-Pose (Meshy)

```bash
belt app run meshy/meshy-7-1 --input '{
  "images": ["https://character-front.jpg", "https://character-side.jpg"],
  "pose_mode": "t-pose",
  "enable_pbr": true,
  "format": "fbx"
}'
```

### Hunyuan 3D with Views

```bash
belt app run tencent/hunyuan-3d-3-1 --input '{
  "image": "https://front.jpg",
  "views": ["https://left.jpg", "https://back.jpg"],
  "enable_pbr": true,
  "face_count": 200000
}'
```

### Rodin High Quality Quad Mesh

```bash
belt app run hyper3d/rodin-gen-2-5 --input '{
  "images": ["https://object-1.jpg", "https://object-2.jpg"],
  "tier": "Gen-2.5-High",
  "mesh_mode": "Quad",
  "quality": "high",
  "material": "PBR",
  "texture_delight": true
}'
```

### Rig a Character (Tripo)

```bash
belt app run tripo/tripo-rig --input '{
  "model": "https://your-character.glb",
  "rig_type": "biped",
  "spec": "mixamo",
  "out_format": "fbx",
  "animations": ["preset:idle", "preset:walk", "preset:run"]
}'
```

### Rig with Preset Animation (Hunyuan)

The character must stand in a T-pose with its arms out:

```bash
belt app run tencent/hunyuan-3d-auto-rigging --input '{
  "model": "https://t-pose-character.fbx",
  "animation": "idle-1"
}'
```

### Rig and Keep Textures (TokenRig)

```bash
belt app run infsh/tokenrig --input '{
  "model": "https://your-model.glb",
  "output_format": "glb",
  "keep_original": true
}'
```

### Tileable Material from Text

```bash
belt app run patina/text-to-material --input '{
  "prompt": "weathered red brick wall with moss in the mortar",
  "tiling_mode": "both",
  "upscale_factor": 2
}'
```

### PBR Maps from a Photo

```bash
belt app run patina/image-to-material --input '{
  "image": "https://wood-floor-photo.jpg"
}'
```

### Extract a Material from an Image

```bash
belt app run patina/extract-material --input '{
  "image": "https://room-photo.jpg",
  "prompt": "the marble countertop"
}'
```

## Workflow: Image to Rigged Character

```bash
# 1. Generate a character image
belt app run openai/gpt-image-2-6-flare --input '{
  "prompt": "full body game character, knight in armor, T-pose, plain white background"
}'

# 2. Turn it into a 3D model
belt app run tripo/tripo-3-1 --input '{"image": "<image-url-from-step-1>"}'

# 3. Rig and animate it
belt app run tripo/tripo-rig --input '{
  "model": "<model-url-from-step-2>",
  "animations": ["preset:walk"]
}'
```

## Pricing

| App | Pricing |
|-----|---------|
| `tripo/tripo-3-1` | $0.01/credit, $0.10-$0.50/run (text: 10-40 credits, image/multiview: 20-50 credits) |
| `tripo/tripo-rig` | $0.01/credit; rig check free, auto rig 25 credits, 10 credits per animation |
| `meshy/meshy-7-1` | $0.0127/credit (20 base + 10-15 texture + 5 hi-res geometry) |
| `tencent/hunyuan-3d-3-1` | $0.02/credit; Geometry 15, Normal 25, +10 for views/PBR/face count |
| `tencent/hunyuan-3d-auto-rigging` | $0.20/call |
| `hyper3d/rodin-gen-2-5` | $0.58/credit; Extreme-High tier +0.5 credits, extreme-high texture +2 credits |
| `infsh/rodin-3d-generator` | $0.40/generation |
| `infsh/tokenrig` | GPU time, billed per millisecond |
| `patina/text-to-material` | $0.01 base + $0.02/MP texture + $0.01/MP per map (+ upscaling) |
| `patina/image-to-material` | $0.01 base + $0.01/MP per map |
| `patina/extract-material` | $0.10 base + $0.02/MP texture + $0.01/MP per map (+ upscaling) |

Estimate before running: `belt app estimate <app-id> --input input.json`

## Tips

- Get the full input schema for any app: `belt app sample <app-id>`
- Turning texturing off (`texture: false`, `should_texture: false`, `generate_type: "Geometry"`) returns a bare mesh and costs less.
- For multiview input, put the front view first.
- For rigging with Hunyuan, generate the character in a T-pose (Meshy `pose_mode: "t-pose"` or Rodin `t_a_pose: true`).

## Search 3D Apps

```bash
belt app list --category 3d
belt app search "3d"
```

## Related Skills

```bash
# Full platform skill (all apps)
npx skills add inference-sh/skills@infsh-cli

# Image generation (reference images for image-to-3D)
npx skills add inference-sh/skills@ai-image-generation

# Background removal (clean input images)
npx skills add inference-sh/skills@background-removal

# Product photography
npx skills add inference-sh/skills@ai-product-photography
```

Browse all 3D apps: `belt app list --category 3d`

## Documentation

- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
- [Streaming Results](https://inference.sh/docs/api/sdk/streaming) - Real-time progress updates
- [Apps Overview](https://inference.sh/docs/apps/overview) - Understanding the app ecosystem
