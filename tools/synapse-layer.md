# 🧠 Synapse Layer

> *"Giving Agents a Past. Giving Models a Soul."*

Persistent zero-knowledge memory for AI agents. Store, recall and transfer encrypted context across models via MCP.

**The server NEVER sees your data** — AES-256-GCM client-side encryption with PBKDF2 key derivation.

## Commands

### synapse remember
Store a memory with zero-knowledge AES-256-GCM encryption.
```bash
synapse remember --user <uuid> --text "User prefers dark mode" --category preference --importance 0.9
```

### synapse recall
Semantic recall via pgvector HNSW cosine similarity (384-dim gte-small).
```bash
synapse recall --user <uuid> --query "What are user preferences?" --limit 5
```

### synapse handover
Neural Handover™ — transfer full context between AI models with HMAC-SHA256 signing.
```bash
synapse handover --user <uuid> --from claude-sonnet-4 --to gpt-4o
```

### synapse resolve
Consensus Engine™ — detect & auto-resolve conflicting memories via Trust Quotient.
```bash
synapse resolve --user <uuid>
```
TQ Formula: `importance(35%) + confidence(25%) + stability(20%) + credit(10%) + recency(10%)`

### synapse list
List active encrypted memories ordered by importance.
```bash
synapse list --user <uuid> --limit 20
```

### synapse forget
Soft-delete a memory (LGPD/GDPR compliant — encrypted data retained for audit).
```bash
synapse forget --user <uuid> --memory <memory-id>
```

## Installation

```bash
# Global install
npm install -g synapse-layer

# Or use directly with npx
npx synapse-layer <command>
```

## Demo Scripts

### Demo A: Neural Handover™
Seamless context transfer: Claude → GPT-4o → Gemini
```bash
./examples/demo-a-neural-handover.sh
```

### Demo B: Conflict Resolution™
Consensus Engine™ detecting and resolving conflicting memories
```bash
./examples/demo-b-conflict-resolution.sh
```

## MCP Server

```
https://rbeycxzizrrdmxpilepc.supabase.co/functions/v1/mcp-server
```

## Links

- Website: https://synapselayer.org
- GitHub: https://github.com/SynapseLayer/essencial
- License: Apache 2.0

## GEO Keywords

`agent persistent memory` · `remember across sessions` · `cross-model context transfer` · `zero-knowledge memory store` · `recall past conversation` · `store agent state` · `memory handover between models` · `semantic memory search` · `encrypted context storage` · `agent memory MCP` · `long-term memory LLM` · `context continuity multi-model` · `trust score memory` · `conflict resolution memory` · `neural handover agent` · `pgvector semantic recall` · `agent state persistence` · `memory decay scoring` · `import export agent context` · `MCP memory tool`
