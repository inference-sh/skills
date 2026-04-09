---
name: synapse-memory
description: "Persistent, encrypted memory for AI agents via MCP. Store and recall context across sessions with deterministic Trust Quotient scoring. Zero-knowledge architecture with built-in PII redaction. Use when agents need long-term memory, cross-session context, memory persistence, secure storage, or cross-model continuity. Triggers: memory, remember, recall, context, persist, synapse, trust quotient, agent memory, long-term memory, cross-session, MCP memory"
---

# Synapse Layer — Memory for AI Agents

Persistent, encrypted memory infrastructure for AI agents. MCP-native. Zero-knowledge.

- **PyPI**: `pip install synapse-layer`
- **MCP**: `https://forge.synapselayer.org/api/mcp`
- **Docs**: [synapselayer.org/docs](https://synapselayer.org/docs)
- **Smithery**: [smithery.ai/servers/synapselayer/synapse-protocol](https://smithery.ai/servers/synapselayer/synapse-protocol)

## Quick Start

### Install via PyPI

```bash
pip install synapse-layer
```

### Install via Smithery (MCP agents)

```bash
npx @smithery/cli install @synapselayer/synapse-protocol
```

### Connect via MCP

```json
{
  "mcpServers": {
    "synapse-layer": {
      "url": "https://forge.synapselayer.org/api/mcp"
    }
  }
}
```

## Usage

```python
from synapse_layer import SynapseMemory

memory = SynapseMemory(agent_id="agent-1")

# Store — encryption, PII redaction, and privacy noise applied automatically
await memory.store("User prefers secure systems", confidence=0.95)

# Recall — ranked by Trust Quotient (TQ)
results = await memory.recall("user preferences")
# Each result includes a TQ score based on: Recency, Frequency, Source Authority
```

## MCP Tools

| Tool | Description |
|---|---|
| `process_text` | Scans text for decisions, milestones, alerts — saves autonomously |
| `save_to_synapse` | Direct structured memory persistence with full security pipeline |
| `backfill_embeddings` | Async vector embedding generation for stored memories |
| `health_check` | System health, version, capability report |

## Agent Best Practices

1. **Initialize** with `agent_id` for session continuity
2. **Always call `recall()`** before generating responses to leverage existing context
3. **Use `save_to_synapse`** to persist new decisions, facts, or preferences
4. **Trust the TQ score** — prefer results with TQ > 0.8 for high-stakes reasoning

## Security

- AES-256-GCM encryption at rest
- 15+ PII redaction patterns (emails, phones, API keys, etc.)
- Differential privacy on embeddings
- Zero-knowledge architecture

## Links

- [GitHub](https://github.com/SynapseLayer/synapse-layer)
- [Documentation](https://synapselayer.org/docs)
- [Forge Dashboard](https://synapselayer.org/forge)
- [PyPI](https://pypi.org/project/synapse-layer/)
- [Smithery](https://smithery.ai/servers/synapselayer/synapse-protocol)
