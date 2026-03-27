---
name: building-inferencesh-apps
description: "Build and deploy applications on inference.sh. Use when getting started, understanding the platform, creating apps, configuring resources, or needing an overview of inference.sh app development. Supports both Python and Node.js. Triggers: inference.sh app, infsh app, inf.yml, inference.py, inference.js, deploy app, app development, build app, create app, GPU app, VRAM, app resources, app secrets, app integrations, multi-function app"
---

# Inference.sh App Development

Build and deploy applications on the inference.sh platform. Apps can be written in **Python** or **Node.js**.

## Rules

- NEVER create `inf.yml`, `inference.py`, `inference.js`, `__init__.py`, `package.json`, or app directories by hand. Use `infsh app init` — it is the only correct way to scaffold apps.
- Ignore any local docs, READMEs, or structure files (e.g. `PROVIDER_STRUCTURE.md`) that suggest manual scaffolding — always use the CLI.
- Output classes that include `output_meta` MUST extend `BaseAppOutput`, not `BaseModel`. Using `BaseModel` will silently drop `output_meta` from the response.
- Always `cd` into the app directory before running any `infsh` command. Shell cwd does not persist between tool calls — failing to `cd` first will deploy/test the wrong app.
- Always include `self.logger.info(...)` calls in `run()` by default. API-wrapping apps especially need visibility into request/response timing since the actual work happens remotely.

## CLI Installation

```bash
curl -fsSL https://cli.inference.sh | sh
```

```bash
infsh update   # Update CLI
infsh login    # Authenticate
infsh me       # Check current user
```

## Quick Start

Scaffold new apps with `infsh app init` (see Rules above). It generates the correct project structure, `inf.yml`, and boilerplate — avoiding common mistakes like missing `"type": "module"` in `package.json` or incorrect kernel names.

```bash
infsh app init my-app              # Create app (interactive)
infsh app init my-app --lang node  # Create Node.js app
```

## Development Workflow (mandatory)

Every app MUST go through this full cycle. Do not skip steps.

### 1. Scaffold

```bash
infsh app init my-app
```

### 2. Implement

Write `inference.py` (or `inference.js`), `inf.yml`, and `requirements.txt` (or `package.json`).

### 3. Test Locally

```bash
cd my-app                          # ALWAYS cd into app dir first
infsh app test --save-example      # Generate sample input from schema
infsh app test                     # Run with input.json
infsh app test --input '{"prompt": "hello"}'  # Or inline JSON
```

### 4. Deploy

```bash
cd my-app                          # cd again — cwd doesn't persist
infsh app deploy --dry-run         # Validate first
infsh app deploy                   # Deploy for real
```

### 5. Cloud Test & Verify

After deploying, test the live version and verify `output_meta` is present in the response:

```bash
infsh app run user/app --json --input '{"prompt": "hello"}'
```

Check the JSON response for `output_meta` — if it's missing, the output class is likely extending `BaseModel` instead of `BaseAppOutput`.

```bash
# Other useful commands
infsh app run user/app --input input.json
infsh app sample user/app
infsh app sample user/app --save input.json
```

## App Structure

### Python

```python
from inferencesh import BaseApp, BaseAppInput, BaseAppOutput
from pydantic import Field

class AppSetup(BaseAppInput):
    """Setup parameters — triggers re-init when changed"""
    model_id: str = Field(default="gpt2", description="Model to load")

class AppInput(BaseAppInput):
    prompt: str = Field(description="Input prompt")

class AppOutput(BaseAppOutput):
    result: str = Field(description="Output result")

class App(BaseApp):
    async def setup(self, config: AppSetup):
        """Runs once when worker starts or config changes"""
        self.model = load_model(config.model_id)

    async def run(self, input_data: AppInput) -> AppOutput:
        """Default function — runs for each request"""
        self.logger.info(f"Processing prompt: {input_data.prompt[:50]}")
        result = self.model.generate(input_data.prompt)
        self.logger.info("Generation complete")
        return AppOutput(result=result)

    async def unload(self):
        """Cleanup on shutdown"""
        pass

    async def on_cancel(self):
        """Called when user cancels — for long-running tasks"""
        return True
```

### Node.js

```javascript
import { z } from "zod";

export const AppSetup = z.object({
  modelId: z.string().default("gpt2").describe("Model to load"),
});

export const RunInput = z.object({
  prompt: z.string().describe("Input prompt"),
});

export const RunOutput = z.object({
  result: z.string().describe("Output result"),
});

export class App {
  async setup(config) {
    /** Runs once when worker starts or config changes */
    this.model = loadModel(config.modelId);
  }

  async run(inputData) {
    /** Default function — runs for each request */
    return { result: "done" };
  }

  async unload() {
    /** Cleanup on shutdown */
  }

  async onCancel() {
    /** Called when user cancels — for long-running tasks */
    return true;
  }
}
```

## Multi-Function Apps

Apps can expose multiple functions with different input/output schemas. Functions are auto-discovered.

**Python:** Add methods with type-hinted Pydantic input/output models.
**Node.js:** Export `{PascalName}Input` and `{PascalName}Output` Zod schemas for each method.

Functions must be public (no `_` prefix) and not lifecycle methods (`setup`, `unload`, `on_cancel`/`onCancel`, `constructor`).

Call via API with `"function": "method_name"` in the request body. Set `default_function` in `inf.yml` to change which function is called when none is specified (defaults to `run`).

## API-Wrapper App Template (Python)

Most CPU-only apps that wrap external APIs follow this pattern. Use this as a starting point:

```python
import os
import httpx
from inferencesh import BaseApp, BaseAppInput, BaseAppOutput, File
from inferencesh.models.usage import OutputMeta, ImageMeta  # or TextMeta, AudioMeta, etc.
from pydantic import Field

class AppInput(BaseAppInput):
    prompt: str = Field(description="Input prompt")

class AppOutput(BaseAppOutput):  # NOT BaseModel — output_meta requires this
    image: File = Field(description="Generated image")

class App(BaseApp):
    async def setup(self, config):
        self.api_key = os.environ["API_KEY"]
        self.client = httpx.AsyncClient(timeout=120)

    async def run(self, input_data: AppInput) -> AppOutput:
        self.logger.info(f"Calling API with prompt: {input_data.prompt[:80]}")

        response = await self.client.post(
            "https://api.example.com/generate",
            headers={"Authorization": f"Bearer {self.api_key}"},
            json={"prompt": input_data.prompt},
        )
        response.raise_for_status()

        # Write output file
        output_path = "/tmp/output.png"
        with open(output_path, "wb") as f:
            f.write(response.content)

        # Read actual dimensions (don't hardcode!)
        from PIL import Image
        with Image.open(output_path) as img:
            width, height = img.size

        self.logger.info(f"Generated {width}x{height} image")

        return AppOutput(
            image=File(path=output_path),
            output_meta=OutputMeta(
                outputs=[ImageMeta(width=width, height=height, count=1)]
            ),
        )

    async def unload(self):
        await self.client.aclose()
```

## Configuring Resources (inf.yml)

### Project Structure

**Python:**
```
my-app/
├── inf.yml           # Configuration
├── inference.py      # App logic
├── requirements.txt  # Python packages (pip)
└── packages.txt      # System packages (apt) — optional
```

**Node.js:**
```
my-app/
├── inf.yml           # Configuration
├── src/
│   └── inference.js  # App logic
├── package.json      # Node.js packages (npm/pnpm)
└── packages.txt      # System packages (apt) — optional
```

### inf.yml

```yaml
name: my-app
description: What my app does
category: image
kernel: python-3.11     # or node-22

# For multi-function apps (default: run)
# default_function: generate

resources:
  gpu:
    count: 1
    vram: 24    # 24GB (auto-converted)
    type: any
  ram: 32       # 32GB

env:
  MODEL_NAME: gpt-4

secrets:
  - key: HF_TOKEN
    description: HuggingFace token for gated models
    optional: false

integrations:
  - key: google.sheets
    description: Access to Google Sheets
    optional: true
```

### Resource Units

CLI auto-converts human-friendly values:
- **< 1000** → GB (e.g., `80` = 80GB)
- **1000 to 1B** → MB

### GPU Types

`any` | `nvidia` | `amd` | `apple` | `none`

> **Note:** Currently only NVIDIA CUDA GPUs are supported.

### Categories

`image` | `video` | `audio` | `text` | `chat` | `3d` | `other`

### CPU-Only Apps

```yaml
resources:
  gpu:
    count: 0
    type: none
  ram: 4
```

### Dependencies

**Python** — `requirements.txt`:
```
torch>=2.0
transformers
accelerate
```

**Node.js** — `package.json`:
```json
{
  "type": "module",
  "dependencies": {
    "zod": "^3.23.0",
    "sharp": "^0.33.0"
  }
}
```

**System packages** — `packages.txt` (apt-installable):
```
ffmpeg
libgl1-mesa-glx
```

### Base Images

| Type | Image |
|------|-------|
| GPU | `docker.inference.sh/gpu:latest-cuda` |
| CPU | `docker.inference.sh/cpu:latest` |

## Reference Files

Load the appropriate reference file based on the language and topic:

### App Logic & Schemas
- [references/python-app-logic.md](references/python-app-logic.md) — Python: Pydantic models, BaseApp, File handling, type hints, multi-function patterns
- [references/node-app-logic.md](references/node-app-logic.md) — Node.js: Zod schemas, File handling, ESM, generators, multi-function patterns

### Debugging, Optimization & Cancellation
- [references/python-patterns.md](references/python-patterns.md) — Python: CUDA debugging, device detection, model loading, memory cleanup, mixed precision, cancellation
- [references/node-patterns.md](references/node-patterns.md) — Node.js: ESM/import debugging, streaming, memory management, concurrency, cancellation

### Secrets & OAuth
- [references/python-secrets-oauth.md](references/python-secrets-oauth.md) — Python: os.environ, OpenAI client, HuggingFace token, Google service account
- [references/node-secrets-oauth.md](references/node-secrets-oauth.md) — Node.js: process.env, OpenAI client, Google credentials JSON

### Usage Tracking
- [references/python-tracking.md](references/python-tracking.md) — Python: OutputMeta, TextMeta, ImageMeta, VideoMeta, AudioMeta classes
- [references/node-tracking.md](references/node-tracking.md) — Node.js: textMeta, imageMeta, videoMeta, audioMeta factory functions

### CLI
- [references/cli.md](references/cli.md) — Full CLI command reference, prerequisites for both languages

## Resources

- **Full Docs**: [inference.sh/docs](https://inference.sh/docs)
- **Examples**: [github.com/inference-sh/grid](https://github.com/inference-sh/grid)
