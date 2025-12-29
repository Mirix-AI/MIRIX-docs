# Backend Usage

Learn how to use the MIRIX backend directly through Python code for maximum flexibility and control over your personal assistant.

## Getting Started

### Initialize the Agent

First, create and initialize your MIRIX agent:

```python
from mirix.agent import AgentWrapper

# Initialize agent with configuration
agent = AgentWrapper("./configs/mirix.yaml")
```

!!! tip "Configuration File"
    
    Make sure your `mirix.yaml` configuration file and `.env` file are properly set up before initializing the agent.

!!! info "Custom Models"
    
    Want to use your own models? Check out our [Custom Models Guide](custom-models.md) to learn how to serve models with vllm and integrate them with MIRIX.

## Python SDK (Simplified Interface)

For a simpler, more streamlined experience, you can use the MIRIX Python SDK. This approach is perfect for quick prototyping and straightforward use cases.

### Quick Start with SDK

```python
from mirix import Mirix

# Initialize memory agent (defaults to Google Gemini 2.0 Flash)
memory_agent = Mirix(api_key="your-google-api-key")

# Add memories
memory_agent.add("The moon now has a president")
memory_agent.add("John loves Italian food and is allergic to peanuts")

# Chat with memory context
response = memory_agent.chat("Does the moon have a president?")
print(response)  # "Yes, according to my memory, the moon has a president."

response = memory_agent.chat("What does John like to eat?") 
print(response)  # "John loves Italian food. However, he's allergic to peanuts."
```

### SDK Initialization Options

The SDK supports various initialization parameters for different use cases:

```python
from mirix import Mirix

# Basic initialization (Google Gemini)
agent = Mirix(api_key="your-google-api-key")

# Use OpenAI models
agent = Mirix(
    api_key="your-openai-api-key",
    model_provider="openai",
    model="gpt-4o"
)

# Use Anthropic Claude
agent = Mirix(
    api_key="your-anthropic-api-key", 
    model_provider="anthropic",
    model="claude-3-sonnet"
)

# Use custom configuration file (default to `gemini-flash`)
agent = Mirix(
    api_key="your-gemini-api-key",
    config_path="./configs/mirix_custom_model.yaml"
)

# Initialize with existing backup
agent = Mirix(
    api_key="your-api-key",
    load_from="./my_backup_directory"
)

# Override model in default config
agent = Mirix(
    api_key="your-gemini-api-key",
    model="gemini-2.0-flash"  # Override default model
)
```

#### Supported Model Providers

| Provider | `model_provider` Value | Environment Variable Set |
|----------|----------------------|-------------------------|
| Google Gemini | `"google_ai"`, `"google"`, `"gemini"` | `GEMINI_API_KEY` |
| OpenAI | `"openai"`, `"gpt"` | `OPENAI_API_KEY` |
| Anthropic | `"anthropic"`, `"claude"` | `ANTHROPIC_API_KEY` |
| Custom | Any custom string | `{PROVIDER}_API_KEY` |

### SDK Usage Examples

#### Building a Personal Assistant

```python
from mirix import Mirix

# Initialize with your API key
assistant = Mirix(api_key="your-google-api-key")

# Store personal information
assistant.add("I prefer coffee over tea")
assistant.add("My work hours are 9 AM to 5 PM")
assistant.add("Important meeting with client on Friday at 2 PM")

# Query your assistant
response = assistant.chat("What's my schedule like this week?")
print(response)

response = assistant.chat("What drink do I prefer?")
print(response)
```

#### Knowledge Base Building

```python
# Create a knowledge base about a project
project_memory = Mirix(api_key="your-google-api-key")

# Add project information
project_memory.add("Project name: MIRIX Documentation")
project_memory.add("Tech stack: Python, MkDocs, Material theme")
project_memory.add("Team members: Alice (PM), Bob (Dev), Carol (Designer)")
project_memory.add("Deadline: End of December 2024")

# Query the knowledge base
response = project_memory.chat("Who are the team members?")
print(response)

response = project_memory.chat("What technology are we using?")
print(response)
```

### SDK Memory Management

#### Clearing Memory and Conversation History

```python
# Clear conversation history while keeping memories
result = agent.clear_conversation_history()
if result['success']:
    print(f"Cleared {result['messages_deleted']} conversation messages")
    print("Memories are still preserved!")
else:
    print(f"Failed: {result['error']}")

# Clear all memories (requires manual database reset)
result = agent.clear()
if not result['success']:
    print(result['warning'])
    for instruction in result['instructions']:
        print(instruction)
    print(f"Manual command: {result['manual_command']}")
```

### SDK Backup and Restore

#### Save Current State

```python
# Save with auto-generated timestamp directory
result = agent.save()
if result['success']:
    print(f"Backup saved to: {result['path']}")

# Save to specific directory
result = agent.save("./my_project_backup")
if result['success']:
    print(f"Backup completed: {result['message']}")
else:
    print(f"Backup failed: {result['error']}")
```

#### Load from Backup

```python
# Load state from backup
result = agent.load("./my_project_backup")
if result['success']:
    print("Memory state restored successfully!")
    # All previous memories are now available
else:
    print(f"Restore failed: {result['error']}")
```

### Advanced SDK Features

#### Using Agent as a Callable

```python
# You can call the agent directly like a function
agent = Mirix(api_key="your-api-key")
agent.add("Python is my favorite programming language")

# These are equivalent:
response1 = agent.chat("What's my favorite language?")
response2 = agent("What's my favorite language?")  # Callable interface

print(response1)  # Same as response2
```

#### Chaining Operations

```python
# Initialize and set up memories in one flow
agent = Mirix(api_key="your-api-key")

# Add multiple memories
memories = [
    "Team standup every Monday at 9 AM",
    "Sprint planning on first Tuesday of each month", 
    "Code review sessions on Wednesday afternoons",
    "Demo day every other Friday"
]

for memory in memories:
    agent.add(memory)

# Query the knowledge
schedule = agent("What's our team meeting schedule?")
print(schedule)
```

## Basic Operations (AgentWrapper)

### Sending Messages

#### Simple Text Messages

```python
# Send basic text information
agent.send_message(
    message="The moon now has a president.",
    memorizing=True,
    force_absorb_content=True
)
```

#### Multi-Modal Content

MIRIX can process text, images, and voice recordings together:

```python
# Send information with images and voice
agent.send_message(
    message="I'm working on a new project about machine learning.",
    image_uris=["/path/to/screenshot1.png", "/path/to/screenshot2.png"],
    memorizing=True,
    force_absorb_content=True
)
```
<!-- voice_files=["base64_encoded_audio_1", "base64_encoded_audio_2"], -->


#### Structured Multi-Modal Messages

For complex multi-modal conversations:

```python
# Multi-modal message format
agent.send_message(
    message=[
        {'type': 'text', 'text': "The moon now has a president. This is how she looks like:"},
        {'type': 'image', 'image_url': "base64_encoded_image"}
    ],
    image_uris=["/path/to/image_1", "/path/to/image_2"],
    memorizing=True,
    force_absorb_content=True
)
```

<!-- voice_files=["base64_encoded_audio"], -->

### Conversational Queries

```python
# Ask questions about your activities
response = agent.send_message("What was I working on yesterday?")
print("MIRIX:", response)

# Get specific information
response = agent.send_message("Show me documents about PostgreSQL")
print("MIRIX:", response)
```

## Understanding Parameters (AgentWrapper)

### Memory Management Control

The `memorizing` parameter controls where messages are routed:

| Value | Destination | Purpose |
|-------|-------------|---------|
| `True` | Meta-memory-manager | Store information for long-term recall and knowledge building |
| `False` | Chat agent | Direct conversation without memory storage |

```python
# Store information for future reference
agent.send_message(
    message="Project meeting notes - new feature requirements discussed",
    memorizing=True,
    force_absorb_content=True
)

# Direct chat without memory storage
agent.send_message(
    message="What's the weather like today?",
    memorizing=False
)
```

### Content Absorption Control

The `force_absorb_content` parameter controls when information is processed:

| Value | Behavior | Use Case |
|-------|----------|----------|
| `True` | Process immediately | Important information, real-time updates |
| `False` | Batch processing (after 20 images) | Regular browsing, background activities |

```python
# Immediate processing for important content
agent.send_message(
    message="Critical meeting notes from client call",
    memorizing=True,
    force_absorb_content=True
)

# Batch processing for regular activities
agent.send_message(
    message="Browsing documentation",
    memorizing=True,
    force_absorb_content=False
)
```

### Input Types and Formats

#### Message Parameter

Can be either a string or a structured list:

```python
# Simple string
message = "Working on the project documentation"

# Structured multi-modal format
message = [
    {'type': 'text', 'text': "Here's the latest design mockup:"},
    {'type': 'image', 'image_url': "data:image/png;base64,iVBORw0KGgoAAAANS..."}
]
```
The images included here are in the user query. When the agent is generating responses, it will respond to the multi-modal query.  

#### Image Handling
These are background images that will be treated as screenshots, the agent will not respond to these images, but rather extract information from them and save into memory.

```python
# Use local file paths
image_uris = [
    "/Users/username/Screenshots/screenshot1.png",
    "/Users/username/Screenshots/screenshot2.png",
]
```

#### Voice Files
Under Robustness Test, Coming Soon.

<!-- ```python
# Base64-encoded audio data
voice_files = [
    "UklGRnoGAABXQVZFZm10IBAAAAABAAEA...",  # base64 audio
    "UklGRnoGAABXQVZFZm10IBAAAAABAAEA..."   # base64 audio
]
``` -->

## Complete Workflow Examples (AgentWrapper)

### Example 1: Daily Work Session

```python
from mirix.agent import AgentWrapper

# Initialize
agent = AgentWrapper("./configs/mirix.yaml")

# Morning briefing
agent.send_message(
    message="Starting work day. Today's priorities: finish documentation, review code, team meeting at 2 PM",
    memorizing=True,
    force_absorb_content=True,
)

# Work activities (batch processing)
agent.send_message(
    message="Working on API documentation",
    image_uris=["/screenshots/vscode_api_docs.png"],
    memorizing=True,
    force_absorb_content=False
)

agent.send_message(
    message="Code review session with team",
    image_uris=["/screenshots/github_pr.png"],
    memorizing=True,
    force_absorb_content=False
)

# Important meeting (immediate processing all existing accumulated messages)
agent.send_message(
    message="Team meeting - discussed Q4 roadmap, new feature priorities",
    voice_files=["base64_meeting_recording"],
    memorizing=True,
    force_absorb_content=True
)

# End of day query
response = agent.send_message("Summarize what I accomplished today")
print("Today's Summary:", response)
```

### Example 2: Research Session

```python
# Research topic introduction
agent.send_message(
    message="Researching machine learning optimization techniques",
    memorizing=True,
    force_absorb_content=True
)

# Reading and capturing research materials
research_screenshots = [
    "/screenshots/arxiv_paper1.png",
    "/screenshots/arxiv_paper2.png",
    "/screenshots/github_implementation.png"
]

agent.send_message(
    message="Reading papers on gradient descent optimization",
    image_uris=research_screenshots,
    memorizing=True,
    force_absorb_content=False
)

# Taking notes
agent.send_message(
    message=[
        {'type': 'text', 'text': "Key insight from the research:"},
        {'type': 'text', 'text': "Adam optimizer performs better than SGD for sparse gradients"}
    ],
    memorizing=True,
    force_absorb_content=True
)

# Query research findings
findings = agent.send_message("What are the key points about optimization techniques I discovered?")
print("Research Findings:", findings)
```


<!-- ### Example 3: Document Processing

```python
# Processing multiple documents
documents = [
    "/documents/project_proposal.pdf",
    "/documents/technical_specs.docx", 
    "/documents/meeting_notes.md"
]

for doc_path in documents:
    agent.send_message(
        message=f"Processing document: {doc_path}",
        image_uris=[doc_path] if doc_path.endswith(('.png', '.jpg')) else [],
        force_absorb_content=True
    )

# Ask about document contents
response = agent.send_message("What are the main points from the project proposal?")
print("Document Summary:", response)
```
-->
<!-- 

## Troubleshooting

### Common Issues

!!! failure "Agent initialization fails"
    
    Check your configuration files:
    ```python
    # Verify config file exists
    import os
    print(os.path.exists("./configs/mirix.yaml"))
    
    # Check environment variables
    import os
    print("GEMINI_API_KEY" in os.environ)
    ```

!!! failure "Image processing errors"
    
    Verify image file paths:
    ```python
    # Check if image files exist
    image_paths = ["/path/to/image1.png", "/path/to/image2.png"]
    for path in image_paths:
        if not os.path.exists(path):
            print(f"Missing: {path}")
    ```

!!! failure "Memory or performance issues"
    
    Monitor system resources:
    ```python
    import psutil
    print(f"Memory usage: {psutil.virtual_memory().percent}%")
    print(f"CPU usage: {psutil.cpu_percent()}%")
    ``` -->

## What's Next?

Ready to explore more advanced topics?

[**Custom Models →**](custom-models.md){ .md-button .md-button--primary }