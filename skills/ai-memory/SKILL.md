---
name: ai-memory
description: AI Memory is a feature that allows you to store and retrieve information from your AI assistant. This can be useful for keeping track of important information, such as your schedule, contacts, and other data.
license: Complete terms in LICENSE.txt
---

## AI Memory Guide

Overview

AI Memory is a feature that allows you to store and retrieve information from your AI assistant. This can be useful for keeping track of important information, such as your schedule, contacts, and other data.

### Quickstart

> Ingest your preference data into the AI Memory.

```python
import cognee

PREFERENCE_DATA = "" # get from the app 
await cognee.add(PREFERENCE_DATA)
await cognee.cognify()
```

> Retrieve your preference data from the AI Memory.

```python
import asyncio
import cognee
async def main():
    user_query=""
    answers = await cognee.search(
        query_text=user_query,
        query_type=cognee.SearchType.GRAPH_COMPLETION
    )
    for answer in answers:
        print(answer)

asyncio.run(main())
```

output format for the retriever: 

```
{'search_result': ['Audi was founded in the early 1900s.'], 'dataset_id': UUID('de430473-b286-5641-9081-655751d6684e'), 'dataset_name': 'main_dataset', 'dataset_tenant_id': None}
```

### Important-Concepts

#### NodeSets

Tag and organize data at the dataset level. Pass tags when adding data:

```python
await cognee.add("text", node_set=["projectA", "finance"])
```

**What happens:** Tags become graph nodes with `belongs_to_set` edges, enabling filtered searches within tagged subsets.

**When to use:**
- Group data by project, domain, or user
- Filter searches to specific subsets
- Organize memory at the dataset level

**Flow:**
- **Add**: Tags attached to datasets/documents
- **Cognify**: Tags materialize as NodeSet nodes in graph, entities inherit parent NodeSet links
- **Search**: Scope queries to specific NodeSets for targeted retrieval

**Example use case:** Tag patient medications as `["medications", "daily"]` and emergency contacts as `["emergency"]` to enable targeted queries.

#### Ontologies

Optional RDF/OWL file for entity validation and enrichment:

```python
await cognee.cognify(datasets=["my_dataset"], ontology_file_path="subset.owl")
```

**What it does:** Validates and enriches extracted entities against a reference vocabulary. Matched entities are marked `ontology_valid=True` and inherit parent classes and relationships.

**When to use:**
- Standardize entity representation (e.g., "aspirin" → "medication")
- Add domain relationships from existing schemas
- Validate entities against medical/enterprise vocabularies

**Format:** Any RDFLib-parsable format (.owl, .ttl, .rdf, JSON-LD)

**Best practice:** Use small, curated subsets (50-500 classes), not full DBpedia/Wikidata. Create domain-specific ontologies or extract relevant subsets from public sources.

#### Sessions & Caching

Maintains conversational context across searches using `(user_id, session_id)`:

```python
answers = await cognee.search(
    query_text=query,
    query_type=cognee.SearchType.GRAPH_COMPLETION,
    session_id="user_123"
)
```

**What it does:** Stores short-term conversation history (Q&A pairs, context) scoped to `(user_id, session_id)`. LLM receives both current query and previous interactions for context-aware responses.

**When to use:**
- Enable follow-up questions ("What about her other medications?")
- Maintain conversation context within a user session
- Support multi-turn caregiving chats

**Search types that use sessions:** `GRAPH_COMPLETION`, `RAG_COMPLETION`, `TRIPLET_COMPLETION` (batch queries do not)

**Setup:**
```bash
# .env
CACHING=true
CACHE_BACKEND=redis  # or filesystem
CACHE_HOST=localhost
CACHE_PORT=6379
```

**Note:** Without caching, searches work but forget previous context. If `session_id` is omitted, defaults to `default_session`.

### Reference

Check out the [cognee](https://github.com/topoteretes/cognee) repository for more information.