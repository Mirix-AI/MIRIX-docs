# Memory Management

Learn how to effectively manage and organize your MIRIX memories for optimal performance and retrieval.

!!! info "Documentation in Progress"
    
    This section is being developed. For now, refer to the [Backend Usage Guide](backend-usage.md) for memory-related operations.

## Overview

MIRIX's memory system is designed to automatically organize your digital activities into six distinct memory components. While most memory management happens automatically, you can optimize and tune the system for your specific needs.

## Memory Types Quick Reference

| Memory Type | Purpose | Auto-Managed | User Control |
|-------------|---------|--------------|--------------|
| **Core Memory** | Personal preferences, identity | ✅ | Manual updates |
| **Episodic Memory** | Activities and events | ✅ | Cleanup policies |
| **Semantic Memory** | Knowledge and concepts | ✅ | Concept merging |
| **Procedural Memory** | Workflows and processes | ✅ | Workflow validation |
| **Resource Memory** | Documents and files | ✅ | Archive policies |
| **Knowledge Vault** | Credentials and sensitive data | ⚠️ | Manual classification |

## Coming Soon

This guide will cover:

- **Memory Inspection**: Browse and search your stored memories
- **Manual Organization**: Organize memories into custom categories
- **Cleanup Strategies**: Automatic and manual memory cleanup
- **Performance Tuning**: Optimize memory storage and retrieval
- **Export/Import**: Backup and migrate your memories
- **Privacy Controls**: Manage sensitive information
- **Memory Analytics**: Understand your digital patterns

## Current Operations

For now, you can manage memories through the Python API:

```python
from mirix.agent import AgentWrapper

# Initialize agent
agent = AgentWrapper("./configs/mirix.yaml")

# Search memories
results = agent.search_memory("machine learning", limit=20)

# Get memory statistics
stats = agent.get_memory_statistics()
print(f"Total memories: {stats['total_count']}")

# Create backup
backup_result = agent.save_agent("./backup")
```

## What's Next?

While this guide is being developed, explore other areas:

[**Backend Usage →**](backend-usage.md){ .md-button .md-button--primary }
[**Architecture Deep Dive →**](../architecture/memory-components.md){ .md-button } 