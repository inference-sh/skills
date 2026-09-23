# CLI Command Reference

## Prerequisites

### Python Apps — uv (Required)

```bash
# macOS / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

### Node.js Apps — Node.js v20+ (Required)

```bash
# macOS / Linux (via fnm)
curl -fsSL https://fnm.vercel.app/install | bash
fnm install 22

# Or via nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
nvm install 22
```

### Hardware

| App Type | Development |
|----------|-------------|
| CPU apps | Any machine |
| GPU apps | NVIDIA CUDA GPU required |

## Installation

```bash
curl -fsSL https://cli.inference.sh | sh
belt login
belt me  # Check current user
```

## App Commands

### Development

```bash
# Create
belt app init my-app              # Non-interactive (Python default)
belt app init my-app --lang node  # Non-interactive (Node.js)
belt app init                     # Interactive

# Test locally
belt app test                     # Test with input.json
belt app test --input '{"k":"v"}' # Test with inline JSON
belt app test --input in.json     # Test with input file
belt app test --save-example      # Generate sample input.json

# Deploy
belt app deploy                   # Deploy from current directory
belt app deploy --dry-run         # Validate without deploying
```

### Running Apps (Cloud)

```bash
belt app run user/app --input input.json
belt app run user/app@version --input '{"prompt": "hello"}'

# Generate sample input for an app
belt app sample user/app
belt app sample user/app --save input.json
```

### Managing Apps

```bash
# Your apps
belt app list                     # List your deployed apps
belt app list -l                  # Detailed list

# Browse store
belt app list                    # Browse available apps
belt app list --featured         # Featured apps
belt app list --category image   # Filter by category

# Get app details
belt app get user/app             # View app info and schemas
belt app get user/app --json      # Output as JSON

# Pull apps
belt app pull [id]                # Pull an app
belt app pull --all               # Pull all apps
belt app pull --all --force       # Overwrite existing
```

## Credential Commands

```bash
belt app credentials list         # List credential keys valid in inf.yml
```

## General Commands

```bash
belt help                         # Get help
belt [command] --help             # Command help
belt version                      # View version
belt update                       # Update CLI
belt completion bash              # Shell completions (bash/zsh/fish)
```

## Environment Variables

| Variable | Description |
|----------|-------------|
| `INFSH_API_KEY` | API key (overrides config file) |
