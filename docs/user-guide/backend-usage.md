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
    voice_files=["base64_encoded_audio_1", "base64_encoded_audio_2"],
    force_absorb_content=True
)
```

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
    voice_files=["base64_encoded_audio"],
    force_absorb_content=True
)
```

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
    force_absorb_content=True
)

# Batch processing for regular activities
agent.send_message(
    message="Browsing documentation",
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

#### Image Handling

```python
# Use local file paths (recommended)
image_uris = [
    "/Users/username/Screenshots/screenshot1.png",
    "/Users/username/Documents/diagram.jpg"
]

# Avoid base64 encoding for image_uris to prevent memory issues
```

#### Voice Files

```python
# Base64-encoded audio data
voice_files = [
    "UklGRnoGAABXQVZFZm10IBAAAAABAAEA...",  # base64 audio
    "UklGRnoGAABXQVZFZm10IBAAAAABAAEA..."   # base64 audio
]
```

## Complete Workflow Examples

### Example 1: Daily Work Session

```python
from mirix.agent import AgentWrapper

# Initialize
agent = AgentWrapper("./configs/mirix.yaml")

# Morning briefing
agent.send_message(
    message="Starting work day. Today's priorities: finish documentation, review code, team meeting at 2 PM",
    force_absorb_content=True
)

# Work activities (batch processing)
agent.send_message(
    message="Working on API documentation",
    image_uris=["/screenshots/vscode_api_docs.png"],
    force_absorb_content=False
)

agent.send_message(
    message="Code review session with team",
    image_uris=["/screenshots/github_pr.png"],
    force_absorb_content=False
)

# Important meeting (immediate processing)
agent.send_message(
    message="Team meeting - discussed Q4 roadmap, new feature priorities",
    voice_files=["base64_meeting_recording"],
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
    force_absorb_content=False
)

# Taking notes
agent.send_message(
    message=[
        {'type': 'text', 'text': "Key insight from the research:"},
        {'type': 'text', 'text': "Adam optimizer performs better than SGD for sparse gradients"}
    ],
    force_absorb_content=True
)

# Query research findings
findings = agent.send_message("What are the key points about optimization techniques I discovered?")
print("Research Findings:", findings)
```

### Example 3: Document Processing

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

## Advanced Features

### Memory Search

```python
# Search across all memory types
results = agent.search_memory("machine learning algorithms")

# Search specific memory types
results = agent.search_memory(
    query="project documentation",
    memory_types=["resource", "procedural"],
    limit=20
)
```

### Backup and Restore

```python
# Create backup
result = agent.save_agent("./backup_folder")
print(result['message'])

# Restore from backup
agent = AgentWrapper("./configs/mirix.yaml", load_from="./backup_folder")
```

## Performance Optimization

### Batch Processing Strategy

```python
# Collect multiple activities before processing
activities = []

# Throughout the day
activities.append({
    "message": "Email review session",
    "images": ["/screenshots/gmail.png"]
})

activities.append({
    "message": "Code development", 
    "images": ["/screenshots/vscode.png"]
})

# Process batch at end of day
for activity in activities:
    agent.send_message(
        message=activity["message"],
        image_uris=activity["images"],
        force_absorb_content=False  # Batch processing
    )

# Force final processing
agent.send_message(
    message="End of day summary",
    force_absorb_content=True
)
```

### Memory Management

```python
# Monitor memory usage
memory_stats = agent.get_memory_statistics()
print(f"Total memories: {memory_stats['total_count']}")
print(f"Memory size: {memory_stats['total_size_mb']} MB")

# Clean up old memories (if supported)
agent.cleanup_old_memories(days_old=30)
```

## Error Handling

```python
try:
    response = agent.send_message(
        message="Test message",
        image_uris=["/invalid/path.png"],
        force_absorb_content=True
    )
except FileNotFoundError as e:
    print(f"Image file not found: {e}")
except Exception as e:
    print(f"Error processing message: {e}")
```

## Integration Patterns

### Automated Screenshot Processing

```python
import os
import time
from pathlib import Path

def process_screenshots_directory(screenshot_dir, agent):
    """Process all new screenshots in a directory"""
    screenshot_path = Path(screenshot_dir)
    processed_file = screenshot_path / ".processed"
    
    # Load previously processed files
    processed = set()
    if processed_file.exists():
        processed = set(processed_file.read_text().splitlines())
    
    # Find new screenshots
    new_screenshots = []
    for img_file in screenshot_path.glob("*.png"):
        if str(img_file) not in processed:
            new_screenshots.append(str(img_file))
    
    if new_screenshots:
        # Process new screenshots
        agent.send_message(
            message=f"Processing {len(new_screenshots)} new screenshots",
            image_uris=new_screenshots,
            force_absorb_content=len(new_screenshots) > 10  # Force if many screenshots
        )
        
        # Update processed list
        processed.update(new_screenshots)
        processed_file.write_text("\n".join(processed))

# Use the function
agent = AgentWrapper("./configs/mirix.yaml")
process_screenshots_directory("/Users/username/Screenshots", agent)
```

### Scheduled Processing

```python
import schedule
import time

def daily_summary():
    """Generate daily summary"""
    agent = AgentWrapper("./configs/mirix.yaml")
    summary = agent.send_message("Provide a summary of today's activities")
    
    # Save summary to file or send notification
    with open(f"daily_summary_{time.strftime('%Y%m%d')}.txt", "w") as f:
        f.write(summary)

# Schedule daily summary
schedule.every().day.at("18:00").do(daily_summary)

# Keep the script running
while True:
    schedule.run_pending()
    time.sleep(60)
```

## Best Practices

### 1. Efficient Image Handling
- Use local file paths instead of base64 encoding for `image_uris`
- Batch multiple images when possible
- Clean up temporary screenshots regularly

### 2. Strategic Force Processing
- Use `force_absorb_content=True` for important or time-sensitive information
- Use `force_absorb_content=False` for routine activities to enable batch processing

### 3. Structured Messaging
- Provide context in your messages
- Use descriptive messages that help with memory organization
- Include relevant metadata when available

### 4. Regular Maintenance
- Create regular backups of your agent state
- Monitor memory usage and performance
- Clean up old or irrelevant memories periodically

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
    ```

## What's Next?

Learn about advanced memory management techniques:

[**Memory Management →**](memory-management.md){ .md-button .md-button--primary }

Or explore the API reference:

[**Agent API →**](../api/agent-api.md){ .md-button } 