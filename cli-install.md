# Install CLI

```sh
curl -fsSL cli.inference.sh | sh
```

Or via package managers:

```sh
brew install inference-sh/tap/belt       # macOS / Linux
npx @inferencesh/belt                    # run without installing
scoop bucket add inference https://github.com/inference-sh/scoop-bucket && scoop install belt  # Windows
```

Then authenticate:

```sh
belt login
```

## What does the installer do?

The install script detects your OS and architecture, downloads the correct binary from dist.inference.sh, verifies its SHA-256 checksum, and places it in your PATH. That's it — no elevated permissions, no background processes, no telemetry. If you have cosign installed, the installer also verifies the Sigstore signature automatically.
