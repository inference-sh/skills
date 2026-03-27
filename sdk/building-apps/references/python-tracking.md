# Python: Tracking Usage (Output Metadata)

Enable usage-based pricing by reporting what your app processes.

> **CRITICAL:** Output classes that include `output_meta` MUST extend `BaseAppOutput`, not `BaseModel`. Using `BaseModel` will silently drop `output_meta` from the response — everything else works, but usage tracking is lost.

## Basic Structure

```python
from inferencesh import BaseAppOutput, OutputMeta, TextMeta, ImageMeta, VideoMeta, AudioMeta

class AppOutput(BaseAppOutput):  # NOT BaseModel!
    result: File = Field(description="Generated output")
    # output_meta is inherited from BaseAppOutput
```

## OutputMeta — Required Fields by Category

Always populate all required fields for the category. Read actual values from output files — don't hardcode.

| Category | Required Fields | Notes |
|----------|----------------|-------|
| **image** | `width`, `height`, `count` | Read dimensions from actual output file |
| **video** | `width`, `height`, `seconds` | Read from output file metadata |
| **audio** | `seconds` | Read from output file metadata |
| **text** | `tokens` | Use actual tokenizer counts |

## MetaItem Types

| Type | Class | Fields |
|------|-------|--------|
| Text | `TextMeta` | `tokens` |
| Image | `ImageMeta` | `width`, `height`, `resolution_mp`, `steps`, `count` |
| Video | `VideoMeta` | `width`, `height`, `resolution`, `seconds` |
| Audio | `AudioMeta` | `seconds` |
| Raw | `RawMeta` | `cost` (dollar cents) |

## Examples

### LLM/Text Generation

Track both input (prompt) and output (completion) tokens:

```python
from inferencesh.models.usage import OutputMeta, TextMeta

return AppOutput(
    response=generated_text,
    output_meta=OutputMeta(
        inputs=[TextMeta(tokens=prompt_tokens)],
        outputs=[TextMeta(tokens=completion_tokens)]
    )
)
```

### Image Generation

```python
return AppOutput(
    image=File(path=output_path),
    output_meta=OutputMeta(
        outputs=[ImageMeta(
            width=1024,
            height=1024,
            resolution_mp=1.05,
            steps=20,
            count=1
        )]
    )
)
```

### Video Generation

```python
return AppOutput(
    video=File(path=output_path),
    output_meta=OutputMeta(
        outputs=[VideoMeta(
            width=1280,
            height=720,
            resolution="720p",
            seconds=5.0
        )]
    )
)
```

### Audio Generation

```python
return AppOutput(
    audio=File(path=output_path),
    output_meta=OutputMeta(
        outputs=[AudioMeta(seconds=30.0)]
    )
)
```

## Custom Data

Use `extra` for app-specific pricing factors:

```python
output_meta=OutputMeta(
    outputs=[ImageMeta(
        width=1024,
        height=1024,
        extra={
            "model": "sdxl-turbo",
            "lora_count": 2
        }
    )]
)
```

## Best Practices

1. **Always populate `output_meta`** if usage varies per request
2. **Use accurate token counts** from the actual tokenizer
3. **Report actual dimensions** — don't hardcode
4. **Include relevant `extra` data** for pricing flexibility
