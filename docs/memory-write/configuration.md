# Configuration

Mirix uses a YAML configuration to define LLM settings, embeddings, and the meta-agent layout. This guide covers all configuration options and provides examples for different LLM providers.

## Quick Start: Using Provider Shortcuts

The easiest way to initialize Mirix is using provider shortcuts:

!!! info "Get Your API Key"
    Sign up at [app.mirix.io](https://app.mirix.io) to get your API key.

```python
from mirix import MirixClient

client = MirixClient(api_key="your_api_key_here")

# Simple initialization - uses default config for the provider
client.initialize_meta_agent(provider="openai")  # or "anthropic", "google_ai"
```

This automatically configures the appropriate model, embeddings, and memory agents for the provider.

## Advanced: Custom YAML Configuration

For full control, create a YAML configuration file. Mirix includes examples for multiple providers in `mirix/configs/examples/`.

=== "OpenAI (Recommended)"

    ```yaml
    llm_config:
      model: "gpt-4o-mini"
      model_endpoint_type: "openai"
      api_key: "sk-proj-xxx"  # Or use OPENAI_API_KEY env var
      model_endpoint: "https://api.openai.com/v1"
      context_window: 128000
    
    # Optional: separate model for topic extraction
    topic_extraction_llm_config:
      model: "gpt-4o-nano"
      model_endpoint_type: "openai"
      api_key: "sk-proj-xxx"
      model_endpoint: "https://api.openai.com/v1"
      context_window: 128000
    
    build_embeddings_for_memory: true
    
    embedding_config:
      embedding_model: "text-embedding-3-small"
      embedding_endpoint: "https://api.openai.com/v1"
      api_key: "sk-proj-xxx"
      embedding_endpoint_type: "openai"
      embedding_dim: 1536
    
    meta_agent_config:
      system_prompts_folder: mirix\prompts\system\base
      agents:
        - core_memory_agent
        - resource_memory_agent
        - semantic_memory_agent
        - episodic_memory_agent
        - procedural_memory_agent
        - knowledge_memory_agent
        - reflexion_agent
        - background_agent
      memory:
        core:
          - label: "human"
            value: ""
          - label: "persona"
            value: "I am a helpful assistant."
        decay:
          fade_after_days: 30      # Memories become inactive (excluded from retrieval)
          expire_after_days: 90    # Memories are permanently deleted
    ```

=== "Google Gemini"

    ```yaml
    llm_config:
      model: "gemini-2.0-flash"
      model_endpoint_type: "google_ai"
      model_endpoint: "https://generativelanguage.googleapis.com"
      context_window: 1000000  # 1M context window!
    
    topic_extraction_llm_config:
      model: "gemini-2.0-flash-lite"
      model_endpoint_type: "google_ai"
      model_endpoint: "https://generativelanguage.googleapis.com"
      context_window: 1000000
    
    build_embeddings_for_memory: true
    
    embedding_config:
      embedding_model: "text-embedding-004"
      embedding_endpoint_type: "google_ai"
      embedding_endpoint: "https://generativelanguage.googleapis.com"
      embedding_dim: 768
    
    meta_agent_config:
      agents:
        - core_memory_agent
        - resource_memory_agent
        - semantic_memory_agent
        - episodic_memory_agent
        - procedural_memory_agent
        - knowledge_memory_agent
        - reflexion_agent
        - background_agent
      memory:
        core:
          - label: "human"
            value: ""
          - label: "persona"
            value: "I am a helpful assistant."
        decay:
          fade_after_days: 30
          expire_after_days: 90
    ```

=== "Anthropic Claude"

    ```yaml
    llm_config:
      model: "claude-3-5-sonnet-20241022"
      model_endpoint_type: "anthropic"
      model_endpoint: "https://api.anthropic.com/v1"
      context_window: 200000
    
    topic_extraction_llm_config:
      model: "claude-3-5-sonnet-20241022"
      model_endpoint_type: "anthropic"
      model_endpoint: "https://api.anthropic.com/v1"
      context_window: 200000
    
    build_embeddings_for_memory: true
    
    # Note: Anthropic doesn't provide embeddings, use Google or OpenAI
    embedding_config:
      embedding_model: "text-embedding-004"
      embedding_endpoint_type: "google_ai"
      embedding_endpoint: "https://generativelanguage.googleapis.com"
      embedding_dim: 768
    
    meta_agent_config:
      agents:
        - core_memory_agent
        - resource_memory_agent
        - semantic_memory_agent
        - episodic_memory_agent
        - procedural_memory_agent
        - knowledge_memory_agent
        - reflexion_agent
        - background_agent
      memory:
        core:
          - label: "human"
            value: ""
          - label: "persona"
            value: "I am a helpful assistant."
        decay:
          fade_after_days: 30
          expire_after_days: 90
    ```

## Configuration Fields Explained

### LLM Configuration

| Field | Description | Required |
|-------|-------------|----------|
| `model` | Model name (e.g., "gpt-4o-mini", "claude-3-5-sonnet") | Yes |
| `model_endpoint_type` | Provider type: `openai`, `anthropic`, `google_ai` | Yes |
| `model_endpoint` | API endpoint URL | Yes |
| `api_key` | API key (can use environment variable instead) | Depends on provider |
| `context_window` | Maximum context window size | Yes |

### Topic Extraction LLM (Optional)

Configure a separate (often cheaper/faster) model for topic extraction during retrieval. If omitted, uses the main LLM.

### Embedding Configuration

| Field | Description | Required |
|-------|-------------|----------|
| `embedding_model` | Embedding model name | Yes |
| `embedding_endpoint_type` | Provider type for embeddings | Yes |
| `embedding_endpoint` | Embedding API endpoint | Yes |
| `api_key` | API key for embedding service | Depends on provider |
| `embedding_dim` | Embedding dimension (e.g., 1536, 768) | Yes |
| `build_embeddings_for_memory` | Enable embedding-based search | Yes |

!!! tip "Embedding Dimensions"
    - OpenAI `text-embedding-3-small`: 1536
    - Google `text-embedding-004`: 768
    - Match the dimension to your embedding model

### Meta Agent Configuration

| Field | Description |
|-------|-------------|
| `agents` | List of memory agents to enable. Standard set includes all 8 agents. |
| `memory.core` | Initial core memory blocks (human info, persona) |
| `memory.decay.fade_after_days` | Days until memories become inactive (excluded from retrieval) |
| `memory.decay.expire_after_days` | Days until memories are permanently deleted |
| `system_prompts_folder` | Optional: custom system prompts directory |

### Memory Agents

The standard agent configuration includes:

- **core_memory_agent**: Stores persistent facts about the user
- **episodic_memory_agent**: Records events and interactions
- **semantic_memory_agent**: Stores concepts and entities
- **procedural_memory_agent**: Remembers processes and workflows
- **resource_memory_agent**: Manages documents and files
- **knowledge_memory_agent**: Stores sensitive information (passwords, keys)
- **reflexion_agent**: Analyzes and improves memory quality
- **background_agent**: Handles asynchronous memory processing

## Loading Configuration

### From YAML File

```python
import yaml
from mirix import MirixClient

# Load config from file
with open("mirix/configs/examples/mirix_openai.yaml", "r") as f:
    config = yaml.safe_load(f)

client = MirixClient(api_key="your_api_key_here")
client.initialize_meta_agent(config=config)
```

### Programmatic Configuration

```python
from mirix import MirixClient

client = MirixClient(api_key="your_api_key_here")

config = {
    "llm_config": {
        "model": "gpt-4o-mini",
        "model_endpoint_type": "openai",
        "model_endpoint": "https://api.openai.com/v1",
        "context_window": 128000,
    },
    "build_embeddings_for_memory": True,
    "embedding_config": {
        "embedding_model": "text-embedding-3-small",
        "embedding_endpoint": "https://api.openai.com/v1",
        "embedding_endpoint_type": "openai",
        "embedding_dim": 1536,
    },
    "meta_agent_config": {
        "agents": [
            "core_memory_agent",
            "resource_memory_agent",
            "semantic_memory_agent",
            "episodic_memory_agent",
            "procedural_memory_agent",
            "knowledge_memory_agent",
            "reflexion_agent",
            "background_agent",
        ],
        "memory": {
            "core": [
                {"label": "human", "value": ""},
                {"label": "persona", "value": "I am a helpful assistant."},
            ],
            "decay": {
                "fade_after_days": 30,
                "expire_after_days": 90,
            },
        },
    }
}

client.initialize_meta_agent(config=config)
```

### Environment Variables

API keys can be set via environment variables instead of hardcoding them:

```bash
# OpenAI
export OPENAI_API_KEY=sk-proj-xxx

# Anthropic
export ANTHROPIC_API_KEY=sk-ant-xxx

# Google Gemini
export GEMINI_API_KEY=your-gemini-key
```

Then omit the `api_key` field from your YAML configuration.

## Configuration Examples

All example configurations are available in the Mirix repository at `mirix/configs/examples/`:

- `mirix_openai.yaml` - OpenAI GPT models
- `mirix_gemini.yaml` - Google Gemini models
- `mirix_claude.yaml` - Anthropic Claude models
- `mirix_openai_single_agent.yaml` - Simplified single-agent setup
- `mirix_gemini_single_agent.yaml` - Simplified Gemini setup

---

[:octicons-arrow-left-24: Quickstart](../getting-started/quickstart.md){ .md-button }
[Next: Memory Search :octicons-arrow-right-24:](../memory-search/search.md){ .md-button .md-button--primary }
