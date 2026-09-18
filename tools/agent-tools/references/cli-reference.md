# CLI Reference

## Installation

```bash
curl -fsSL https://cli.inference.sh | sh
```

## Global Commands

| Command | Description |
|---------|-------------|
| `belt help` | Show help |
| `belt version` | Show CLI version |
| `belt update` | Update CLI to latest |
| `belt login` | Authenticate |
| `belt me` | Show current user |

## App Commands

### Your Apps

| Command | Description |
|---------|-------------|
| `belt app list` | List your deployed apps |
| `belt app list --search <query>` | Search your apps |
| `belt app search <query>` | Search your apps (shortcut) |
| `belt app list -l` | Detailed table view |

### Store (Public Apps)

| Command | Description |
|---------|-------------|
| `belt app list` | Browse the public app store |
| `belt app list --category <cat>` | Filter by category (image, video, audio, text, other) |
| `belt app search <query>` | Search the store |
| `belt app list --featured` | Show featured apps |
| `belt app list --new` | Sort by newest |
| `belt app list --page <n>` | Pagination |
| `belt app list -l` | Detailed table view |
| `belt app list --save <file>` | Save to JSON file |
| `belt app get <app>` | Get app details |
| `belt app get <app> --json` | Get app details as JSON |

### Execution

| Command | Description |
|---------|-------------|
| `belt app run <app> --input <file>` | Run app with input file |
| `belt app run <app> --input '<json>'` | Run with inline JSON |
| `belt app run <app> --input <file> --no-wait` | Run without waiting for completion |
| `belt app sample <app>` | Show sample input |
| `belt app sample <app> --save <file>` | Save sample to file |

## Task Commands

| Command | Description |
|---------|-------------|
| `belt task get <task-id>` | Get task status and result |
| `belt task get <task-id> --json` | Get task as JSON |
| `belt task get <task-id> --save <file>` | Save task result to file |

### Development

| Command | Description |
|---------|-------------|
| `belt app init` | Create new app (interactive) |
| `belt app init <name>` | Create new app with name |
| `belt app test --input <file>` | Test app locally |
| `belt app deploy` | Deploy app |
| `belt app deploy --dry-run` | Validate without deploying |
| `belt app pull <id>` | Pull app source |
| `belt app pull --all` | Pull all your apps |

## Environment Variables

| Variable | Description |
|----------|-------------|
| `INFSH_API_KEY` | API key (overrides config) |

## Shell Completions

```bash
# Bash
belt completion bash > /etc/bash_completion.d/infsh

# Zsh
belt completion zsh > "${fpath[1]}/_infsh"

# Fish
belt completion fish > ~/.config/fish/completions/infsh.fish
```

## App Name Format

Apps use the format `namespace/app-name`:

- `falai/flux-dev-lora` - fal.ai's FLUX 2 Dev
- `google/veo-3-1` - Google's Veo 3
- `infsh/sdxl` - inference.sh's SDXL
- `bytedance/seedance-2-0` - ByteDance's Seedance 2.0
- `bytedance/seedance-2-0-fast` - ByteDance's Seedance 2.0 Fast
- `xai/grok-imagine-image` - xAI's Grok

Version pinning: `namespace/app-name@version`

## Documentation

- [CLI Setup](https://inference.sh/docs/extend/cli-setup) - Complete CLI installation guide
- [Running Apps](https://inference.sh/docs/apps/running) - How to run apps via CLI
- [Creating an App](https://inference.sh/docs/extend/creating-app) - Build your own apps
- [Deploying](https://inference.sh/docs/extend/deploying) - Deploy apps to the cloud
