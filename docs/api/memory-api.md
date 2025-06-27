# Memory API Reference

Advanced API reference for direct memory management and operations in Mirix.

!!! info "Advanced API"
    
    This API is for advanced users who need direct memory access. For most use cases, the [Agent API](agent-api.md) is sufficient.

## Overview

The Memory API provides direct access to Mirix's six memory components, allowing for advanced memory operations, custom search implementations, and fine-grained control over memory management.

## Memory Manager Classes

### Core Memory Manager

```python
from mirix.memory import CoreMemoryManager

core_manager = CoreMemoryManager(agent_state)
```

#### Methods

```python
# Get current core memory
core_memory = core_manager.get_core_memory()

# Update persona block
core_manager.update_persona("I am a helpful AI assistant...")

# Update human understanding
core_manager.update_human("User prefers Python for development")

# Check memory capacity
if core_manager.needs_rewrite():
    core_manager.rewrite_memory()
```

### Episodic Memory Manager

```python
from mirix.memory import EpisodicMemoryManager

episodic_manager = EpisodicMemoryManager(agent_state)
```

#### Methods

```python
# Add new episodic entry
episodic_manager.add_entry({
    "event_type": "user_message",
    "summary": "User worked on documentation",
    "details": "Created MkDocs site with Material theme...",
    "actor": "user",
    "timestamp": "2024-03-15T14:30:00Z"
})

# Search episodic memories
results = episodic_manager.list_items(
    query="documentation work",
    search_method="bm25",
    limit=20
)

# Get memories by time range
today_memories = episodic_manager.get_memories_by_date_range(
    start_date="2024-03-15T00:00:00Z",
    end_date="2024-03-15T23:59:59Z"
)
```

### Semantic Memory Manager

```python
from mirix.memory import SemanticMemoryManager

semantic_manager = SemanticMemoryManager(agent_state)
```

#### Methods

```python
# Add semantic concept
semantic_manager.add_entry({
    "name": "MkDocs Material",
    "summary": "Documentation framework based on MkDocs",
    "details": "Static site generator with beautiful theming...",
    "source": "user_interaction"
})

# Find related concepts
related = semantic_manager.find_related_concepts("documentation", limit=10)

# Merge duplicate concepts
duplicates = semantic_manager.find_duplicates(similarity_threshold=0.95)
semantic_manager.merge_concepts(duplicates)
```

### Procedural Memory Manager

```python
from mirix.memory import ProceduralMemoryManager

procedural_manager = ProceduralMemoryManager(agent_state)
```

#### Methods

```python
# Add workflow
procedural_manager.add_entry({
    "entry_type": "workflow",
    "description": "Deploy documentation to GitHub Pages",
    "steps": [
        "1. Build documentation with mkdocs build",
        "2. Push to main branch",
        "3. GitHub Actions deploys automatically"
    ]
})

# Search procedures
workflows = procedural_manager.search_workflows("deployment")

# Validate workflow
is_valid = procedural_manager.validate_workflow(workflow_id)
```

### Resource Memory Manager

```python
from mirix.memory import ResourceMemoryManager

resource_manager = ResourceMemoryManager(agent_state)
```

#### Methods

```python
# Add resource
resource_manager.add_entry({
    "title": "Mirix Documentation Site",
    "summary": "Complete documentation website built with MkDocs",
    "resource_type": "website",
    "content": "Full HTML content of the documentation..."
})

# Get resources by type
docs = resource_manager.get_resources_by_type("markdown")

# Compress large resources
resource_manager.compress_large_resources(threshold_mb=5)
```

### Knowledge Vault Manager

```python
from mirix.memory import KnowledgeVaultManager

vault_manager = KnowledgeVaultManager(agent_state)
```

#### Methods

```python
# Store sensitive data
vault_manager.add_entry({
    "entry_type": "api_key",
    "source": "github",
    "sensitivity": "high",
    "secret_value": "ghp_xxxxxxxxxxxx",
    "caption": "GitHub Personal Access Token"
})

# Retrieve with security check
api_key = vault_manager.get_secure_value(
    entry_id="github_token_1",
    require_auth=True
)

# List by sensitivity
high_security = vault_manager.list_by_sensitivity("high")
```

## Search Operations

### Advanced Search

```python
from mirix.search import AdvancedSearchEngine

search_engine = AdvancedSearchEngine(agent_state)

# Multi-memory search with custom weights
results = search_engine.search_across_memories(
    query="machine learning project",
    memory_weights={
        "episodic": 1.0,
        "semantic": 0.8,
        "resource": 0.6,
        "procedural": 0.4
    },
    search_method="bm25",
    limit=50
)

# Semantic similarity search
similar = search_engine.find_semantically_similar(
    reference_text="deep learning neural networks",
    memory_types=["semantic", "resource"],
    similarity_threshold=0.7
)
```

### Search Filters

```python
# Complex search with multiple filters
filtered_results = search_engine.advanced_search(
    query="project documentation",
    filters={
        "memory_type": ["resource", "procedural"],
        "date_range": {
            "start": "2024-01-01T00:00:00Z",
            "end": "2024-12-31T23:59:59Z"
        },
        "sensitivity": ["low", "medium"],
        "content_type": ["markdown", "text"],
        "min_relevance": 0.5
    },
    sort_by="relevance",
    limit=100
)
```

## Memory Analytics

### Statistics and Insights

```python
from mirix.analytics import MemoryAnalytics

analytics = MemoryAnalytics(agent_state)

# Get comprehensive statistics
stats = analytics.get_memory_statistics()
print(f"Total memories: {stats['total_count']}")
print(f"Memory distribution: {stats['type_distribution']}")
print(f"Growth rate: {stats['daily_growth_rate']}")

# Analyze memory patterns
patterns = analytics.analyze_patterns()
print(f"Most active hours: {patterns['peak_hours']}")
print(f"Common topics: {patterns['frequent_topics']}")

# Memory health check
health = analytics.memory_health_check()
if health['issues']:
    print("Memory issues found:")
    for issue in health['issues']:
        print(f"  - {issue}")
```

### Memory Optimization

```python
from mirix.optimization import MemoryOptimizer

optimizer = MemoryOptimizer(agent_state)

# Find optimization opportunities
recommendations = optimizer.analyze_optimization_opportunities()

# Apply automatic optimizations
results = optimizer.optimize_memories(
    compress_old_data=True,
    merge_duplicates=True,
    archive_unused=True,
    rebuild_indexes=False
)

print(f"Optimization saved {results['space_saved_mb']} MB")
```

## Memory Synchronization

### Multi-Agent Coordination

```python
from mirix.sync import MemorySynchronizer

sync = MemorySynchronizer(agent_state)

# Sync with another agent
sync_result = sync.synchronize_with_agent(other_agent_id)

# Resolve conflicts
conflicts = sync.detect_conflicts()
for conflict in conflicts:
    resolution = sync.resolve_conflict(
        conflict_id=conflict['id'],
        strategy="latest_wins"  # or "manual", "merge"
    )
```

## Custom Memory Types

### Extending Memory System

```python
from mirix.memory.base import BaseMemoryManager

class CustomMemoryManager(BaseMemoryManager):
    def __init__(self, agent_state):
        super().__init__(agent_state, "custom_memory")
    
    def add_custom_entry(self, data):
        # Custom logic for your memory type
        entry = self.process_custom_data(data)
        return self.store_entry(entry)
    
    def search_custom(self, query, custom_params):
        # Custom search logic
        return self.search_with_custom_logic(query, custom_params)

# Register custom memory type
custom_manager = CustomMemoryManager(agent_state)
```

## Configuration

### Memory Configuration

```yaml
# mirix.yaml memory settings
memory:
  managers:
    core:
      max_blocks: 10
      rewrite_threshold: 0.9
    
    episodic:
      max_entries: 10000
      archive_after_days: 90
    
    semantic:
      duplicate_threshold: 0.95
      auto_merge: true
    
    procedural:
      validate_workflows: true
      auto_cleanup: false
    
    resource:
      compression_threshold_mb: 5
      max_content_size_mb: 100
    
    vault:
      encryption_enabled: true
      audit_access: true
      
  search:
    default_method: "bm25"
    cache_results: true
    max_cache_size: 1000
```

## Error Handling

### Memory-Specific Exceptions

```python
from mirix.exceptions import (
    MemoryCapacityError,
    MemoryCorruptionError,
    MemoryAccessDeniedError,
    MemoryNotFoundException
)

try:
    core_manager.add_entry(large_data)
except MemoryCapacityError:
    # Handle memory full
    core_manager.rewrite_memory()
    
except MemoryCorruptionError:
    # Handle corrupted memory
    core_manager.repair_memory()
    
except MemoryAccessDeniedError:
    # Handle permission issue
    authenticate_user()
```

## Performance Considerations

### Batch Operations

```python
# Efficient batch processing
entries_to_add = [entry1, entry2, entry3, ...]

# Use batch operations instead of individual adds
semantic_manager.add_entries_batch(entries_to_add)

# Batch search across multiple queries
queries = ["query1", "query2", "query3"]
results = semantic_manager.search_batch(queries)
```

### Memory Pooling

```python
# Reuse manager instances
from mirix.memory import MemoryManagerPool

pool = MemoryManagerPool(agent_state)

# Get manager from pool
semantic_manager = pool.get_manager("semantic")

# Operations...

# Return to pool when done
pool.return_manager(semantic_manager)
```

## What's Next?

Return to explore other API features:

[**Agent API →**](agent-api.md){ .md-button } 