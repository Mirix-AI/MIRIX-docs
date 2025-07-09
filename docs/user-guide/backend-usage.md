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

## Basic Operations

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

## Understanding Parameters

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

## Complete Workflow Examples

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

Learn about performance optimization and advanced topics:

[**Performance →**](../advanced/performance.md){ .md-button .md-button--primary }