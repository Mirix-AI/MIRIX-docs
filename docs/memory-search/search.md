# Memory Search

Mirix provides powerful search capabilities across all memory types. Choose between direct search with `search()`, context-aware retrieval with `retrieve_with_conversation()`, or topic-based retrieval with `retrieve_with_topic()`.

## Search Methods Overview

| Method | Use Case | Key Features |
|--------|----------|-------------|
| `search()` | Direct keyword/semantic search | BM25 or embedding search, field-specific, temporal filters |
| `retrieve_with_conversation()` | Context-aware memory retrieval | Analyzes conversation, extracts topics, retrieves relevant memories |
| `retrieve_with_topic()` | Topic-based memory lookup | Simple topic/keyword search across all memory types |
| `search_all_users()` | Organization-wide search | Search across all users in an organization |

## Direct Search

The `search()` method provides fine-grained control over memory queries.

### Basic Search

```python
from mirix import MirixClient

client = MirixClient(api_key="your_api_key_here")
client.initialize_meta_agent(provider="openai")

# Search across all memory types
results = client.search(
    user_id="demo-user",
    query="restaurants",
    limit=5,
)

print(f"Found {results['count']} results")
for result in results['results']:
    print(f"- [{result['memory_type']}] {result.get('summary', result.get('caption', 'N/A'))}")
```

### Search Specific Memory Types

```python
# Search only episodic memories
episodic_results = client.search(
    user_id="demo-user",
    query="meeting",
    memory_type="episodic",
    limit=10,
)

# Search procedural memories
procedural_results = client.search(
    user_id="demo-user",
    query="deployment process",
    memory_type="procedural",
    limit=5,
)
```

### Search Specific Fields

Different memory types have different searchable fields:

```python
# Search in episodic memory details
episodic_results = client.search(
    user_id="demo-user",
    query="database migration",
    memory_type="episodic",
    search_field="details",  # Options: "summary", "details"
    limit=10,
)

# Search in resource memory content
resource_results = client.search(
    user_id="demo-user",
    query="API documentation",
    memory_type="resource",
    search_field="content",  # Options: "summary", "content"
    limit=5,
)

# Search in semantic memory
semantic_results = client.search(
    user_id="demo-user",
    query="Python libraries",
    memory_type="semantic",
    search_field="details",  # Options: "name", "summary", "details"
    limit=5,
)
```

### Searchable Fields by Memory Type

| Memory Type | Available Fields |
|-------------|------------------|
| `episodic` | `summary`, `details` |
| `resource` | `summary`, `content` |
| `procedural` | `summary`, `steps` |
| `knowledge` | `caption`, `secret_value` |
| `semantic` | `name`, `summary`, `details` |
| `all` | Use `"null"` (searches all default fields) |

## Search Methods: BM25 vs Embedding

### BM25 Search (Keyword-based)

BM25 is a keyword-based search algorithm that works well for exact matches and specific terms:

```python
# Fast keyword search
results = client.search(
    user_id="demo-user",
    query="PostgreSQL performance optimization",
    search_method="bm25",  # Default
    limit=10,
)
```

**Best for:**
- Exact keyword matches
- Technical terms and identifiers
- Fast retrieval
- No embedding model required

### Embedding Search (Semantic)

Embedding search finds semantically similar memories, even if they don't share exact keywords:

```python
# Semantic similarity search
results = client.search(
    user_id="demo-user",
    query="How do I improve database speed?",
    search_method="embedding",
    similarity_threshold=0.7,  # Optional: filter by similarity
    limit=10,
)
```

**Best for:**
- Conceptual similarity
- Natural language queries
- Finding related topics
- Understanding context

!!! tip "Similarity Thresholds"
    When using `search_method="embedding"`, you can filter results by similarity:
    
    - `0.5` - Strict: Only highly relevant results
    - `0.7` - Moderate: Reasonably relevant results (recommended)
    - `0.9` - Loose: Loosely related results
    - `None` - No filtering: Returns top N results regardless of similarity

## Temporal Filtering

Filter episodic memories by time range:

```python
# Find memories from a specific date range
temporal_results = client.search(
    user_id="demo-user",
    query="project updates",
    memory_type="episodic",  # Temporal filters only apply to episodic
    start_date="2025-12-01T00:00:00",
    end_date="2025-12-05T23:59:59",
    limit=10,
)
```

Date format: ISO 8601 (`YYYY-MM-DDTHH:MM:SS` or `YYYY-MM-DDTHH:MM:SSZ`)

## Tag Filtering

Use custom tags to organize and filter memories:

```python
# Filter by tags
tagged_results = client.search(
    user_id="demo-user",
    query="customer support",
    filter_tags={
        "project": "alpha",
        "department": "engineering",
        "priority": "high"
    },
    limit=10,
)
```

Tags are set when adding memories:

```python
client.add(
    user_id="demo-user",
    messages=[
        {"role": "user", "content": [{"type": "text", "text": "Fix the login bug"}]},
        {"role": "assistant", "content": [{"type": "text", "text": "I'll prioritize that."}]},
    ],
    filter_tags={
        "project": "alpha",
        "department": "engineering",
        "priority": "high"
    }
)
```

## Context-Aware Retrieval

The `retrieve_with_conversation()` method analyzes conversation context to retrieve relevant memories intelligently:

```python
# Retrieve memories based on conversation context
memories = client.retrieve_with_conversation(
    user_id="demo-user",
    messages=[
        {"role": "user", "content": [{"type": "text", "text": "What did we discuss about the database migration last week?"}]},
    ],
    limit=5,
)

# Access memories by type
if memories.get("memories"):
    episodic = memories["memories"].get("episodic", {})
    semantic = memories["memories"].get("semantic", {})
    
    print(f"Found {episodic.get('total_count', 0)} episodic memories")
    print(f"Found {semantic.get('total_count', 0)} semantic memories")
```

### Automatic Temporal Extraction

`retrieve_with_conversation()` automatically extracts time references from queries:

```python
# Natural language time references
memories = client.retrieve_with_conversation(
    user_id="demo-user",
    messages=[
        {"role": "user", "content": [{"type": "text", "text": "What happened yesterday?"}]},
    ],
    limit=5,
)

# Check what time range was detected
if memories.get("temporal_expression"):
    print(f"Detected time: {memories['temporal_expression']}")
    print(f"Date range: {memories.get('date_range')}")
```

Supported expressions: "today", "yesterday", "last week", "last 3 days", "this month", etc.

### Explicit Date Ranges

You can also provide explicit date ranges:

```python
memories = client.retrieve_with_conversation(
    user_id="demo-user",
    messages=[
        {"role": "user", "content": [{"type": "text", "text": "What did we work on?"}]},
    ],
    start_date="2025-12-01T00:00:00",
    end_date="2025-12-05T23:59:59",
    limit=5,
)
```

## Topic-Based Retrieval

Search by topic or keyword across all memory types:

```python
# Simple topic search
memories = client.retrieve_with_topic(
    user_id="demo-user",
    topic="machine learning",
    limit=5,
    filter_tags={"project": "ai-research"}
)

# Check results
for memory_type, data in memories.get("memories", {}).items():
    if data and data.get("total_count", 0) > 0:
        print(f"{memory_type}: {data['total_count']} results")
```

## Organization-Wide Search

Search across all users in your organization (requires appropriate permissions):

```python
# Search memories across all users
org_results = client.search_all_users(
    query="quarterly planning",
    memory_type="episodic",
    search_method="embedding",
    limit=20,
    client_id=client.client_id,  # Optional: scope to specific client
    similarity_threshold=0.7
)

print(f"Found {org_results['count']} results across all users")
```

!!! warning "Privacy & Permissions"
    Organization-wide search respects user privacy settings and client scopes. You can only search memories that your API key has permission to access.

## Complete Parameter Reference

### `search()` Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `user_id` | str | Required | User ID for the search |
| `query` | str | Required | Search query string |
| `memory_type` | str | `"all"` | Memory type: `episodic`, `resource`, `procedural`, `knowledge`, `semantic`, or `all` |
| `search_field` | str | `"null"` | Specific field to search (see table above) |
| `search_method` | str | `"bm25"` | Search method: `bm25` or `embedding` |
| `limit` | int | `10` | Maximum number of results per memory type |
| `filter_tags` | dict | `None` | Optional tags for filtering |
| `similarity_threshold` | float | `None` | Similarity threshold for embedding search (0.0-2.0) |
| `start_date` | str | `None` | Start date for temporal filtering (ISO 8601) |
| `end_date` | str | `None` | End date for temporal filtering (ISO 8601) |

### `retrieve_with_conversation()` Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `user_id` | str | Required | User ID for retrieval |
| `messages` | List | Required | Conversation messages (must end with user turn) |
| `limit` | int | `10` | Maximum items per memory type |
| `filter_tags` | dict | `None` | Optional tags for filtering |
| `use_cache` | bool | `True` | Enable Redis caching |
| `start_date` | str | `None` | Override automatic temporal extraction |
| `end_date` | str | `None` | Override automatic temporal extraction |

### `retrieve_with_topic()` Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `user_id` | str | Required | User ID for retrieval |
| `topic` | str | Required | Topic or keyword to search |
| `limit` | int | `10` | Maximum items per memory type |
| `filter_tags` | dict | `None` | Optional tags for filtering |
| `use_cache` | bool | `True` | Enable Redis caching |

## Best Practices

1. **Choose the right method:**
   - Use `search()` for direct, specific queries
   - Use `retrieve_with_conversation()` for conversational context
   - Use `retrieve_with_topic()` for simple keyword lookups

2. **Optimize search method:**
   - Use `bm25` for exact keyword matches (faster)
   - Use `embedding` for semantic similarity (more flexible)

3. **Leverage temporal filtering:**
   - Combine search with date ranges for time-specific queries
   - Use natural language time expressions with `retrieve_with_conversation()`

4. **Use tags effectively:**
   - Tag memories during `add()` operations
   - Filter searches by relevant tags
   - Use hierarchical tags (e.g., `project:module:feature`)

5. **Tune similarity thresholds:**
   - Start with `0.7` for embedding search
   - Lower for stricter, higher for broader results
   - Monitor result quality and adjust

---

[:octicons-arrow-left-24: Memory Write](../memory-write/configuration.md){ .md-button }
[Next: Architecture :octicons-arrow-right-24:](../architecture/multi-agent-system.md){ .md-button .md-button--primary }
