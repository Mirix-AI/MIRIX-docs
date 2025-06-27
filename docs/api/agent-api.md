# Agent API Reference

Complete API reference for the Mirix `AgentWrapper` class and related functionality.

## AgentWrapper Class

The main interface for interacting with Mirix agents.

### Constructor

```python
AgentWrapper(config_path: str, load_from: str = None)
```

Initialize a new Mirix agent.

**Parameters:**
- `config_path` (str): Path to the configuration YAML file
- `load_from` (str, optional): Path to backup directory to restore from

**Example:**
```python
from mirix.agent import AgentWrapper

# Basic initialization
agent = AgentWrapper("./configs/mirix.yaml")

# Initialize with backup restore
agent = AgentWrapper("./configs/mirix.yaml", load_from="./backup")
```

### Core Methods

#### send_message()

```python
send_message(
    message: Union[str, List[Dict]], 
    image_uris: List[str] = None,
    voice_files: List[str] = None, 
    force_absorb_content: bool = False
) -> str
```

Send a message to the agent for processing or conversation.

**Parameters:**
- `message`: Text message or multi-modal message structure
- `image_uris`: List of local image file paths
- `voice_files`: List of base64-encoded audio data
- `force_absorb_content`: Whether to process immediately (True) or batch (False)

**Returns:**
- `str`: Agent's response

**Examples:**
```python
# Simple text message
response = agent.send_message("What did I work on today?")

# Multi-modal message
response = agent.send_message(
    message="Working on documentation",
    image_uris=["/screenshots/vscode.png"],
    force_absorb_content=True
)

# Structured multi-modal
response = agent.send_message(
    message=[
        {'type': 'text', 'text': "Here's the design:"},
        {'type': 'image', 'image_url': "data:image/png;base64,..."}
    ]
)
```

#### search_memory()

```python
search_memory(
    query: str,
    memory_types: List[str] = None,
    limit: int = 50,
    search_method: str = "bm25"
) -> List[Dict]
```

Search across memory components.

**Parameters:**
- `query`: Search query string
- `memory_types`: List of memory types to search (default: all)
- `limit`: Maximum number of results
- `search_method`: Search method ("bm25", "embedding", "string_match", "fuzzy_match")

**Returns:**
- `List[Dict]`: Search results with metadata

**Example:**
```python
# Search all memories
results = agent.search_memory("machine learning")

# Search specific memory types
results = agent.search_memory(
    query="project documentation",
    memory_types=["resource", "procedural"],
    limit=20
)
```

### Backup and Restore

#### save_agent()

```python
save_agent(
    backup_path: str,
    include_logs: bool = False,
    compress: bool = False,
    verify_integrity: bool = False
) -> Dict
```

Create a backup of the agent state.

**Parameters:**
- `backup_path`: Directory to save backup
- `include_logs`: Include log files in backup
- `compress`: Compress backup files
- `verify_integrity`: Verify backup after creation

**Returns:**
- `Dict`: Backup result with status and metadata

**Example:**
```python
result = agent.save_agent(
    "./backup_20240315",
    include_logs=True,
    verify_integrity=True
)

if result['success']:
    print(f"Backup created: {result['backup_path']}")
    print(f"Size: {result['size_mb']} MB")
```

#### load_agent()

```python
load_agent(
    backup_path: str,
    restore_components: Dict = None
) -> Dict
```

Restore agent state from backup.

**Parameters:**
- `backup_path`: Path to backup directory
- `restore_components`: Dictionary specifying which components to restore

**Returns:**
- `Dict`: Restore result with status and statistics

**Example:**
```python
result = agent.load_agent(
    "./backup_20240315",
    restore_components={
        "memories": True,
        "conversations": True,
        "config": False
    }
)

if result['success']:
    print(f"Restored {result['memory_count']} memories")
```

### Utility Methods

#### get_memory_statistics()

```python
get_memory_statistics() -> Dict
```

Get statistics about stored memories.

**Returns:**
- `Dict`: Memory statistics including counts and sizes

**Example:**
```python
stats = agent.get_memory_statistics()
print(f"Total memories: {stats['total_count']}")
print(f"Database size: {stats['total_size_mb']} MB")
```

#### get_agent_status()

```python
get_agent_status() -> Dict
```

Get current agent status and health information.

**Returns:**
- `Dict`: Agent status including processing state and performance metrics

**Example:**
```python
status = agent.get_agent_status()
print(f"Agent state: {status['state']}")
print(f"Processing queue: {status['queue_length']} items")
```

## Data Structures

### Message Structure

For multi-modal messages:

```python
message = [
    {
        'type': 'text',
        'text': 'Your text content here'
    },
    {
        'type': 'image', 
        'image_url': 'data:image/png;base64,iVBORw0KGgo...'
    }
]
```

### Search Result Structure

```python
{
    'id': 'memory_123',
    'memory_type': 'semantic',
    'content': 'Memory content...',
    'metadata': {
        'timestamp': '2024-03-15T10:30:00Z',
        'source': 'user_interaction',
        'confidence': 0.95
    },
    'score': 0.87
}
```

### Backup Result Structure

```python
{
    'success': True,
    'message': 'Agent state saved successfully',
    'backup_path': './backup_20240315',
    'size_mb': 45.2,
    'file_count': 12,
    'verification_passed': True
}
```

## Error Handling

### Common Exceptions

```python
from mirix.exceptions import (
    MirixConfigError,
    MirixDatabaseError,
    MirixBackupError,
    MirixSearchError
)

try:
    agent = AgentWrapper("./configs/mirix.yaml")
    response = agent.send_message("Hello")
except MirixConfigError as e:
    print(f"Configuration error: {e}")
except MirixDatabaseError as e:
    print(f"Database error: {e}")
except Exception as e:
    print(f"Unexpected error: {e}")
```

### Error Response Format

```python
{
    'success': False,
    'error': 'Error description',
    'error_code': 'MIRIX_001',
    'details': 'Additional error details'
}
```

## Configuration

### Agent Configuration (mirix.yaml)

```yaml
agent:
  name: "mirix_assistant"
  version: "1.0.0"
  
database:
  type: "postgresql"  # or "sqlite"
  connection_string: "postgresql://user@localhost/mirix"
  
processing:
  batch_size: 20
  timeout_seconds: 300
  
search:
  default_method: "bm25"
  default_limit: 50
  
memory:
  auto_cleanup: true
  max_size_mb: 1000
```

## Best Practices

### Performance Optimization

```python
# Use batch processing for multiple operations
agent.send_message("Activity 1", force_absorb_content=False)
agent.send_message("Activity 2", force_absorb_content=False)  
agent.send_message("Force processing", force_absorb_content=True)

# Cache search results for repeated queries
search_cache = {}
def cached_search(query):
    if query not in search_cache:
        search_cache[query] = agent.search_memory(query)
    return search_cache[query]
```

### Error Handling

```python
# Robust error handling
def safe_agent_operation(operation, *args, **kwargs):
    max_retries = 3
    for attempt in range(max_retries):
        try:
            return operation(*args, **kwargs)
        except Exception as e:
            if attempt == max_retries - 1:
                raise
            print(f"Attempt {attempt + 1} failed: {e}")
            time.sleep(2 ** attempt)  # Exponential backoff
```

## What's Next?

Explore the Memory API for more advanced memory management:

[**Memory API →**](memory-api.md){ .md-button .md-button--primary } 