# Synapse Layer: The Memory Standard for Agents

**"Giving Agents a Past. Giving Models a Soul."**

[![Python SDK](https://img.shields.io/badge/Python_SDK-1.0.2-blue)](https://pypi.org/project/synapse-layer/)
[![A2A Protocol](https://img.shields.io/badge/A2A_Protocol-v1.0-green)](https://synapselayer.org/docs/sdk/a2a-protocol)
[![MCP Compatible](https://img.shields.io/badge/MCP-Compatible-purple)](https://synapselayer.org)
[![Zero-Knowledge](https://img.shields.io/badge/Zero--Knowledge-AES--256--GCM-red)](https://synapselayer.org/docs)
[![License](https://img.shields.io/badge/License-Apache_2.0-yellow)](https://github.com/SynapseLayer/essencial/blob/main/LICENSE)

Synapse Layer is a high-performance, decentralized memory protocol that solves the **"Agent Amnesia"** problem. It allows AI agents to maintain persistent context across sessions, platforms, and models.

---

## The Problem: Agent Amnesia

Every time an AI agent starts a new session, it forgets everything. Users repeat themselves. Context is lost. Agents make the same mistakes. This is Agent Amnesia — and it costs productivity, trust, and money.

WITHOUT SYNAPSE LAYER:

```
Session 1: "I prefer Portuguese and dark mode"
Session 2: Agent has no memory. User repeats.
Session 3: Agent has no memory. User gives up.
```

WITH SYNAPSE LAYER:

```
Session 1: "I prefer Portuguese and dark mode" --> stored (TQ: 0.95)
Session 2: Agent recalls: "PT-BR + dark mode" --> seamless
Session 3: Agent recalls: "PT-BR + dark mode" --> seamless
```

---

## Key Features

### Zero-Knowledge Privacy

All memories are encrypted **client-side** using AES-256-GCM before leaving your device. The server stores only encrypted blobs. Even Synapse Layer cannot read your agent's thoughts.

```python
# The server NEVER sees this content
memory = SynapseMemory(
    api_key="your-key",
    encryption_key="your-private-aes-key",  # Never sent to server
    user_id="user-001"
)
```

### Neural Handover(TM)

Transfer complete AI context between different models without losing state. HMAC-SHA256 signed for integrity.

```python
# Agent A (Claude) creates handover
handover = await client.create_handover(
    user_id="user-123",
    target_model="gpt-4o",
    summary="Analyzed Q3 data; found 15% revenue growth"
)
# handover.token is HMAC-SHA256 signed

# Agent B (GPT-4o) receives full context automatically
# Zero memory loss between models
```

**Supported Model Adapters:**

| Model Family | Format | Optimized For |
|---|---|---|
| Claude | XML tags | Structured context parsing |
| GPT | Markdown | Header-based navigation |
| Gemini | JSON structured | Structured output mode |
| Llama (large) | [CONTEXT] blocks | Efficient token usage |
| Llama (small) | [CTX] ultra-compact | 8k context windows |
| Mistral | [CONTEXT] blocks | Compact format |

### Trust Quotient (TQ)

Every memory has a quality score. High-quality memories are promoted; low-quality memories decay automatically.

```
TQ = (confidence_score * 0.4) + (recency_score * 0.3) + (usage_normalized * 0.3)
```

**High TQ Example:**
```
"User purchased premium plan on 2026-04-01"
confidence=0.99, recency=0.99, usage=0.85
TQ = (0.99*0.4) + (0.99*0.3) + (0.85*0.3) = 0.948 (Excellent)
```

**Low TQ Example:**
```
"User might prefer dark mode"
confidence=0.35, recency=0.18, usage=0.05
TQ = (0.35*0.4) + (0.18*0.3) + (0.05*0.3) = 0.209 (Poor -> will decay)
```

### Conflict Resolution Engine(TM)

When two memories contradict each other, Synapse automatically resolves the conflict using TQ scores. The highest-TQ memory becomes canonical.

### Model Agnostic

Works with LangChain, CrewAI, AutoGPT, and any MCP client. No vendor lock-in.

---

## Quick Start

### Via Inference CLI

```bash
inference skill add synapse-memory
```

### Python SDK

```bash
pip install synapse-layer
pip install synapse-layer[langchain]   # LangChain adapter
pip install synapse-layer[crewai]      # CrewAI tools
pip install synapse-layer[all]         # Everything
```

### Basic Usage

```python
import asyncio
from synapse_layer import SynapseA2AClient

async def main():
    client = SynapseA2AClient(api_key="your-api-key")

    async with client:
        # Store a memory
        result = await client.store_memory(
            user_id="user-123",
            content="User prefers Portuguese language and dark mode",
            source_type="user_input",
            confidence=0.95
        )
        print(f"Stored: {result.task_id} | TQ: {result.tq_score}")

        # Recall memories
        recalled = await client.recall_memory(
            user_id="user-123",
            query="language preference",
            limit=5
        )
        print(f"Found {len(recalled.output)} memories")

asyncio.run(main())
```

### LangChain Integration

```python
from langchain.chains import ConversationChain
from langchain_anthropic import ChatAnthropic
from synapse_layer import SynapseMemory

memory = SynapseMemory(
    api_key="your-api-key",
    user_id="user-001",
    recall_limit=10
)

chain = ConversationChain(
    llm=ChatAnthropic(model="claude-3-5-sonnet-20241022"),
    memory=memory,
    verbose=True
)

response = chain.run(input="What did I tell you last week?")
```

### CrewAI Integration

```python
from crewai import Agent, Crew
from synapse_layer import (
    SynapseStoreMemoryTool,
    SynapseRecallMemoryTool,
    SynapseHandoverTool
)

researcher = Agent(
    role="Researcher",
    goal="Gather and store findings with persistent memory",
    tools=[
        SynapseStoreMemoryTool(api_key="your-api-key"),
        SynapseRecallMemoryTool(api_key="your-api-key"),
        SynapseHandoverTool(api_key="your-api-key"),
    ]
)
```

---

## Architecture

```
CLIENT LAYER
  Python SDK (Async)  |  TypeScript SDK  |  Any MCP Client
         |                    |                   |
         +--------------------+-------------------+
                              |
                    AES-256-GCM Encryption
                    (Client-side only)
                              |
                    encrypted blobs + embeddings
                              |
SERVER LAYER
  Supabase Edge Functions (A2A Protocol / JSON-RPC 2.0)
  store_memory | recall_memory | create_handover
  resolve_conflict | forget_memory
                              |
  PostgreSQL + pgvector (HNSW)
  Region: sa-east-1 (Sao Paulo)
  - Encrypted memory blobs (AES-256-GCM)
  - 384-dim embeddings (semantic search)
  - Trust Quotient scores (TQ ranking)
  - Audit trails (GDPR/LGPD compliance)
```

---

## Plugin System

Synapse Layer v1.0.0 introduces an extensible Plugin System for domain-specific conflict resolution.

```python
from synapse_layer.plugins import MemoryResolverPlugin, MemoryCandidate, ResolverDecision

class MyCustomResolver(MemoryResolverPlugin):
    @property
    def name(self) -> str:
        return "my_resolver"

    def can_handle(self, candidates: list) -> bool:
        return any("custom_tag" in c.metadata for c in candidates)

    def resolve(self, candidates: list) -> ResolverDecision:
        # Your domain-specific logic here
        return ResolverDecision(winner_memory_id=candidates[0].memory_id)
```

Included: **MedicalTruthResolver** — validates medical memories by source priority (peer-reviewed > clinical guidelines > specialist notes > patient self-reports).

---

## Compliance & Security

| Feature | Standard | Status |
|---------|----------|--------|
| Encryption | AES-256-GCM | Active |
| Integrity | HMAC-SHA256 | Active |
| Data Residency | sa-east-1 (Sao Paulo) | Active |
| GDPR Compliance | Soft-delete + audit trails | Active |
| LGPD Compliance | Data minimization | Active |
| License | Apache 2.0 | Active |

---

## Links

| Resource | URL |
|----------|-----|
| Website | https://synapselayer.org |
| Docs | https://synapselayer.org/docs |
| GitHub | https://github.com/SynapseLayer/essencial |
| PyPI | https://pypi.org/project/synapse-layer/ |
| Agent Card | https://synapselayer.org/.well-known/agent-card.json |
| Issues | https://github.com/SynapseLayer/essencial/issues |
| Email | founder.synapselayer@proton.me |

---

Built in Sao Paulo, Brazil. Apache 2.0 — Open-source and free for commercial use.
