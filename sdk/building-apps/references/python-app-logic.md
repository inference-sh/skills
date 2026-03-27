# Python App Logic (inference.py)

The `inference.py` file contains your app's logic with setup, run, and unload methods.

## Structure

```python
from inferencesh import BaseApp, BaseAppInput, BaseAppOutput
from pydantic import Field

class AppSetup(BaseAppInput):
    """Setup parameters — runs once when config changes"""
    model_id: str = Field(default="gpt2", description="Model to load")

class AppInput(BaseAppInput):
    prompt: str = Field(description="What to generate")
    style: str = Field(default="modern", description="Style")

class AppOutput(BaseAppOutput):
    result: str = Field(description="Generated output")

class App(BaseApp):
    async def setup(self, config: AppSetup):
        """Runs once when worker starts or config changes"""
        self.model = load_model(config.model_id)

    async def run(self, input_data: AppInput) -> AppOutput:
        """Runs for each request"""
        return AppOutput(result="done")

    async def unload(self):
        """Cleanup on shutdown"""
        pass
```

## Field Types

| Type | Usage |
|------|-------|
| `str`, `int`, `float`, `bool` | Basic types |
| `File` | File upload/output (`.path` for local path) |
| `Optional[T]` | Nullable |
| `List[T]` | Array |
| `Literal["a", "b"]` | Enum dropdown |

## Setup Parameters

Use `AppSetup` to define parameters that trigger re-initialization when changed:

```python
class AppSetup(BaseAppInput):
    model_id: str = Field(default="gpt2", description="Model to load")
    precision: str = Field(default="fp16", description="Model precision")

class App(BaseApp):
    async def setup(self, config: AppSetup):
        from transformers import AutoModel
        self.model = AutoModel.from_pretrained(config.model_id)
```

## File Handling

```python
# Input: auto-downloaded
image_path = input_data.image.path

# Output: auto-uploaded
return AppOutput(image=File(path="/tmp/output.png"))
```

## Multi-Function Apps

Apps can expose multiple functions with different input/output types.

> **IMPORTANT:** If any output class needs `output_meta`, it MUST extend `BaseAppOutput`, not `BaseModel`. Using `BaseModel` will silently drop `output_meta` from the response.

```python
from inferencesh import BaseApp, BaseAppInput, BaseAppOutput
from pydantic import Field

class GreetInput(BaseAppInput):
    name: str = Field(default="World", description="Name to greet")

class GreetOutput(BaseAppOutput):
    message: str = Field(description="Greeting message")

class ReverseInput(BaseAppInput):
    text: str = Field(description="Text to reverse")

class ReverseOutput(BaseAppOutput):
    reversed_text: str = Field(description="Reversed text")

class App(BaseApp):
    async def run(self, input_data: GreetInput) -> GreetOutput:
        """Default function."""
        return GreetOutput(message=f"Hello, {input_data.name}!")

    async def greet(self, input_data: GreetInput) -> GreetOutput:
        """Custom greeting."""
        return GreetOutput(message=f"Welcome, {input_data.name}!")

    async def reverse(self, input_data: ReverseInput) -> ReverseOutput:
        """Reverse text."""
        return ReverseOutput(reversed_text=input_data.text[::-1])
```

Functions are auto-discovered if they:
- Are public (no `_` prefix)
- Have type hints for input and return
- Use Pydantic models

Call via API with `"function": "reverse"` in the request body.

### Default Function

By default, `run` is called when no function is specified. To change this, set `default_function` in `inf.yml`:

```yaml
default_function: greet
```

When set, requests without an explicit `function` parameter will call `greet` instead of `run`.

## The on_cancel Hook

For long-running tasks:

```python
async def on_cancel(self):
    """Called when user cancels — must return quickly"""
    self.cancel_flag = True
    return True
```
