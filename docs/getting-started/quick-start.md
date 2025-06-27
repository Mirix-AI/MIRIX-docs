# Quick Start

This guide will help you get Mirix up and running quickly on your system.

## Prerequisites

Before starting, ensure you have:

- **Python 3.11+** installed
- **PostgreSQL 17** or **SQLite** for database
- **GEMINI API key** from Google AI Studio
- **4GB RAM** minimum (8GB recommended)
- **10GB free disk space**

!!! tip "New Users"
    
    If you haven't installed the prerequisites yet, check our [**Installation Guide**](installation.md) first.

## Step 1: Clone and Setup

```bash
# Clone the Mirix repository
git clone https://github.com/Mirix-AI/Mirix.git
cd Mirix

# Create and activate virtual environment
python -m venv mirix-env
source mirix-env/bin/activate  # On Windows: mirix-env\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

## Step 2: Configure Environment

Create a `.env` file in the project root:

```bash
# Database Configuration (choose one)
# For PostgreSQL (recommended):
DATABASE_URL=postgresql://username:password@localhost:5432/mirix_db

# For SQLite (simpler setup):
# DATABASE_URL=sqlite:///mirix.db

# API Configuration
GEMINI_API_KEY=your_gemini_api_key_here

# Optional: Customize screenshot settings
SCREENSHOT_INTERVAL=1  # seconds between screenshots
MAX_SCREENSHOTS=1000   # maximum screenshots to store
```

!!! warning "API Key Required"
    
    You must obtain a GEMINI API key from [Google AI Studio](https://makersuite.google.com/app/apikey) to use Mirix.

## Step 3: Initialize Database

If using PostgreSQL:

```bash
# Create database
createdb mirix_db

# Initialize tables
python scripts/init_db.py
```

If using SQLite, the database will be created automatically on first run.

## Step 4: Start Mirix

```bash
# Start the main application
python main.py
```

You should see output similar to:

```
[INFO] Initializing Mirix Multi-Agent System...
[INFO] Memory system initialized with 6 components
[INFO] 8 agents loaded and ready
[INFO] Screenshot capture started (interval: 1s)
[INFO] Mirix is now running and tracking your activities
```

## Step 5: Test the System

### Basic Usage

Open a new terminal and test the chat functionality:

```bash
# In a new terminal, activate the same environment
source mirix-env/bin/activate

# Start interactive chat
python chat.py
```

Try some example queries:

```
> What have I been doing today?
> Show me recent documents I've worked on
> What websites did I visit in the last hour?
```

### Backend API Usage

You can also interact with Mirix programmatically:

```python
from mirix import MirixAgent

# Initialize the agent
agent = MirixAgent()

# Search memories
results = agent.search_memory("meeting notes", memory_type="episodic")

# Get activity summary
summary = agent.get_activity_summary(hours=2)
print(summary)
```

## Next Steps

!!! success "You're Ready!"
    
    Mirix is now tracking your screen activities and building your personal memory base.

### Explore Further

- **[Architecture Overview](../architecture/multi-agent-system.md)** - Understand how Mirix works
- **[User Guide](../user-guide/backend-usage.md)** - Learn advanced usage patterns
- **[Desktop App](../user-guide/desktop-app.md)** - Try the GUI interface
- **[API Reference](../api/agent-api.md)** - Build custom integrations

### Common Tasks

- **View Memory Components**: Learn about the [6 memory types](../architecture/memory-components.md)
- **Search Capabilities**: Explore [advanced search features](../architecture/search-capabilities.md)
- **Performance Tuning**: Check [optimization tips](../advanced/performance.md)
- **Privacy Settings**: Configure [security options](../advanced/security-privacy.md)

## Troubleshooting

### Common Issues

!!! warning "Screenshot Permissions"
    
    On macOS, you may need to grant screen recording permissions:
    
    1. Go to **System Preferences > Security & Privacy > Privacy**
    2. Select **Screen Recording**
    3. Add your terminal application or Python executable

!!! error "Database Connection"
    
    If you encounter database connection errors:
    
    ```bash
    # Check PostgreSQL status
    pg_ctl status
    
    # Restart if needed
    brew services restart postgresql
    ```

!!! tip "Memory Usage"
    
    If Mirix uses too much memory, adjust settings:
    
    ```python
    # In config.py
    SCREENSHOT_INTERVAL = 2  # Reduce frequency
    MAX_MEMORY_SIZE = "500MB"  # Limit memory usage
    ```

### Getting Help

- **Documentation**: Browse our comprehensive [user guide](../user-guide/backend-usage.md)
- **Issues**: Report bugs on [GitHub Issues](https://github.com/Mirix-AI/Mirix/issues)
- **Contact**: Email us at [yuw164@ucsd.edu](mailto:yuw164@ucsd.edu)

---

**Ready to dive deeper?** Check out our [**User Guide**](../user-guide/backend-usage.md) for advanced features and usage patterns. 