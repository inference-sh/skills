# Node.js: Secrets & OAuth Credentials

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

```javascript
export class App {
  async setup(config) {
    const apiKey = process.env.OPENAI_API_KEY;
    if (!apiKey) {
      throw new Error("OPENAI_API_KEY required");
    }
    this.apiKey = apiKey;
  }
}
```

## Common Patterns

### External API Access

```javascript
import OpenAI from "openai";

export class App {
  async setup(config) {
    this.client = new OpenAI({
      apiKey: process.env.OPENAI_API_KEY,
    });
  }
}
```

### HuggingFace Token

```yaml
secrets:
  - key: HF_TOKEN
    description: HuggingFace token for gated models
```

```javascript
export class App {
  async setup(config) {
    this.hfToken = process.env.HF_TOKEN;
  }
}
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

```javascript
export class App {
  async setup(config) {
    const credsJson = process.env.GOOGLE_OAUTH_CREDENTIALS;
    if (credsJson) {
      this.credentials = JSON.parse(credsJson);
    }
  }
}
```

#### Service Account

```javascript
export class App {
  async setup(config) {
    const saJson = process.env.GOOGLE_SA_CREDENTIALS;
    if (saJson) {
      this.serviceAccount = JSON.parse(saJson);
    }
  }
}
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
