# Authentication & Setup

## Install the CLI

```bash
curl -fsSL https://cli.inference.sh | sh
```

## Login

```bash
belt login
```

This opens a browser for authentication. After login, credentials are stored locally.

### Non-interactive login

```bash
belt login --key <api-key>
```

Without a TTY, `belt login` prints a URL; the browser then shows a claim code. Run `belt login --code <code>` on the same machine within 5 minutes.

## Check Authentication

```bash
belt me
```

Shows your user info if authenticated.

## Environment Variable

For CI/CD, scripts or agents, set your API key (read on every command, no login needed):

```bash
export INFSH_API_KEY=your-api-key
```

The environment variable overrides the config file.

## Update CLI

```bash
belt update
```

Or reinstall:

```bash
curl -fsSL https://cli.inference.sh | sh
```

## Troubleshooting

| Error | Solution |
|-------|----------|
| "not authenticated" | Run `belt login` |
| "command not found" | Reinstall CLI or add to PATH |
| "API key invalid" | Check `INFSH_API_KEY` or re-login |

## Documentation

- [CLI Setup](https://inference.sh/docs/extend/cli-setup) - Complete CLI installation guide
- [API Authentication](https://inference.sh/docs/api/authentication) - API key management
- [Secrets](https://inference.sh/docs/secrets/overview) - Managing credentials
