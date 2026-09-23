# Python: Secrets & OAuth Credentials

## Declaring Secrets

In `inf.yml`:

```yaml
secrets:
  - key: OPENAI_API_KEY
    description: OpenAI API key
    optional: false

  - key: WEBHOOK_SECRET
    description: Optional webhook secret
    optional: true
```

| Property | Type | Description |
|----------|------|-------------|
| `key` | string | Environment variable name |
| `description` | string | Shown to users |
| `optional` | boolean | If false, app won't run without it |

## Accessing Secrets

```python
import os

class App(BaseApp):
    async def setup(self, config):
        api_key = os.environ.get("OPENAI_API_KEY")
        if not api_key:
            raise ValueError("OPENAI_API_KEY required")
        self.client = OpenAI(api_key=api_key)
```

## Common Patterns

### External API Access

```python
from openai import OpenAI

class App(BaseApp):
    async def setup(self, config):
        self.client = OpenAI(api_key=os.environ.get("OPENAI_API_KEY"))
```

### HuggingFace Token

```yaml
secrets:
  - key: HF_TOKEN
    description: HuggingFace token for gated models
```

```python
from huggingface_hub import snapshot_download

self.model_path = snapshot_download(
    repo_id="meta-llama/Llama-2-7b",
    token=os.environ.get("HF_TOKEN")
)
```

## Tips

- Use specific names (`OPENAI_API_KEY` not `API_KEY`)
- Validate in `setup()`, fail fast
- Never log secret values

---

## OAuth Credentials

Access external services (Google Sheets, Drive) on behalf of users through OAuth.

### Declaring Credentials

In `inf.yml`:

```yaml
credentials:
  - key: google.sheets
    description: Read/write Google Sheets
    optional: false

  - key: google.drive
    description: Access to Google Drive files
    optional: true
```

| Property | Type | Description |
|----------|------|-------------|
| `key` | string | Credential identifier |
| `description` | string | Shown to users |
| `optional` | boolean | If false, app won't run without it |

### Available Credentials

```bash
belt app credentials list
```

| Key | Description |
|-----|-------------|
| `google.sheets` | Read/write Sheets |
| `google.sheets.readonly` | Read-only Sheets |
| `google.drive` | Google Drive files |
| `google.sa` | Service account |

### Accessing Credentials

#### OAuth Credentials

```python
import os, json

class App(BaseApp):
    async def setup(self, config):
        creds_json = os.environ.get("GOOGLE_OAUTH_CREDENTIALS")
        if creds_json:
            self.credentials = json.loads(creds_json)
```

#### Service Account

```python
from google.oauth2 import service_account

class App(BaseApp):
    async def setup(self, config):
        sa_json = os.environ.get("GOOGLE_SA_CREDENTIALS")
        if sa_json:
            self.credentials = service_account.Credentials.from_service_account_info(
                json.loads(sa_json)
            )
```

### Secrets vs Credentials

| Feature | Secrets | Credentials |
|---------|---------|--------------|
| User provides | Raw value (API key) | OAuth authorization |
| Refresh | Manual | Automatic |
| Scope control | None | Fine-grained |
| Best for | API keys | OAuth services |

### Best Practices

1. **Request minimal scopes** — use `readonly` if you only read
2. **Clear descriptions** — explain why access is needed
3. **Handle missing gracefully** — check if optional credentials exist
