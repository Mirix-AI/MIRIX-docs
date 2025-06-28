# Installation & Getting Started

This guide will walk you through setting up MIRIX on your system and getting started with your first use.

## Prerequisites

- **Python 3.11** or later
- **Node.js 14** or later (for the desktop app)
- A valid [**GEMINI API key**](https://aistudio.google.com/app/apikey)
- **PostgreSQL 17** (recommended for best performance)

!!! warning "API Key Required"
    
    You'll need a GEMINI API key to use MIRIX. Get one free from [Google AI Studio](https://aistudio.google.com/app/apikey).

## Step 1: Clone the Repository

```bash
git clone https://github.com/Mirix-AI/MIRIX.git
cd MIRIX
```

## Step 2: Set up Environment Variables

Create a `.env` file in the project root:

```bash
# Create .env file
touch .env
```

Add your GEMINI API key to the `.env` file:

```dotenv
GEMINI_API_KEY=your_api_key_here
```

## Step 3: Install Python Dependencies

```bash
pip install -r requirements.txt
```

## Step 4: Database Setup

### Option A: PostgreSQL (Recommended)

PostgreSQL provides better performance, scalability, and vector search capabilities.

#### Install PostgreSQL and pgvector

=== "macOS (Homebrew)"

    ```bash
    # Install PostgreSQL and pgvector
    brew install postgresql@17 pgvector
    
    # Start PostgreSQL service
    brew services start postgresql@17
    
    # Add PostgreSQL to your PATH
    export PATH="$(brew --prefix postgresql@17)/bin:$PATH"
    ```

=== "Ubuntu/Debian"

    ```bash
    # Install PostgreSQL
    sudo apt-get update
    sudo apt-get install postgresql postgresql-contrib
    
    # Install pgvector (see https://github.com/pgvector/pgvector for latest instructions)
    # Start PostgreSQL
    sudo systemctl start postgresql
    ```

=== "Windows"

    1. Download PostgreSQL from [postgresql.org](https://www.postgresql.org/download/windows/)
    2. Install pgvector following the [official guide](https://github.com/pgvector/pgvector)
    3. Start PostgreSQL service

#### Create Database and Enable Extensions

```bash
# Create the mirix database
createdb mirix

# Enable pgvector extension
psql -U $(whoami) -d mirix -c "CREATE EXTENSION IF NOT EXISTS vector;"
```

#### Configure Environment Variables

Add PostgreSQL configuration to your `.env` file:

```dotenv
GEMINI_API_KEY=your_api_key_here

# PostgreSQL Configuration
# Replace 'your_username' with your system username (find it with: echo $(whoami))
MIRIX_PG_URI=postgresql+pg8000://your_username@localhost:5432/mirix
```

!!! note "Username Setup"
    
    This setup uses your system user for simplicity in development. For production, consider creating a dedicated PostgreSQL user with limited privileges.

### Option B: SQLite (Fallback)

If PostgreSQL setup fails, MIRIX will automatically use SQLite. Simply omit the PostgreSQL environment variables from your `.env` file.

!!! info "SQLite Limitations"
    
    SQLite works but has limitations in concurrent access and advanced search capabilities compared to PostgreSQL.

## Step 5: Start MIRIX Backend

```bash
python main.py
```

MIRIX will automatically create all necessary database tables on first startup and begin processing on-screen activities immediately.

## Step 6: Launch the Desktop App

To use the graphical interface, you'll need to set up and run the frontend application.

### Prerequisites

Ensure you have **Node.js** (version 14 or later) installed:

- **macOS**: `brew install node`
- **Ubuntu/Debian**: `sudo apt install nodejs npm`
- **Windows**: Download from [nodejs.org](https://nodejs.org/)

### Setup and Launch

```bash
# Navigate to the frontend directory
cd frontend

# Install frontend dependencies
npm install

# Launch the desktop application
npm run electron-dev
```

The MIRIX desktop application will open, providing a user-friendly interface to interact with your personal assistant.

!!! tip "Keep Both Running"
    
    Keep both the backend (`python main.py`) and frontend (`npm run electron-dev`) running simultaneously for the full MIRIX experience.



## Getting Started

Now that MIRIX is installed, you can start using it:

1. **Launch the desktop app** - Run both the backend and frontend as described in Steps 5 and 6
2. **Start with chat** - Initially, MIRIX functions as a chatbot where you can send messages
3. **Enable screen monitoring** - Click the **"Screenshots"** tab on the right side of the app, then click **"Start Monitoring"** to begin screen capture
4. **Let it learn** - Give MIRIX a few minutes to build your initial memory base from your screen activities
5. **Ask questions** - Once monitoring is active, interact with MIRIX about your recent activities and documents
6. **Explore features** - Check out the [User Guide](../user-guide/) for advanced usage

!!! success "You're Ready!"
    
    MIRIX is now tracking your screen activities and building your personal memory base.

### What Happens Next?

- **Automatic Screenshot Capture**: MIRIX takes and processes screenshots every 1.5 seconds
- **Memory Building**: Your activities are automatically categorized into 6 memory types
- **Real-time Processing**: 8 specialized agents work together to understand and organize your data
- **Privacy First**: All data is stored locally on your machine

### Explore Further

- **[Architecture Overview](../architecture/multi-agent-system.md)** - Understand how MIRIX works
- **[User Guide](../user-guide/backend-usage.md)** - Learn advanced usage patterns
- **[Desktop App](../user-guide/desktop-app.md)** - Try the GUI interface
- **[API Reference](../api/agent-api.md)** - Build custom integrations

## Troubleshooting

### Common PostgreSQL Issues

??? failure "\"extension 'vector' is not available\" error"

    Install pgvector first:
    ```bash
    # macOS
    brew install pgvector
    
    # Then enable the extension
    psql -U $(whoami) -d mirix -c "CREATE EXTENSION IF NOT EXISTS vector;"
    ```

??? failure "\"permission denied to create extension\" error"

    The pgvector extension requires superuser privileges:
    ```bash
    # Try connecting as postgres superuser
    psql -U postgres -d mirix -c "CREATE EXTENSION IF NOT EXISTS vector;"
    ```

??? failure "Connection refused or timeout"

    Make sure PostgreSQL service is running:
    ```bash
    # macOS
    brew services start postgresql@17
    
    # Linux
    sudo systemctl start postgresql
    ```

??? failure "\"database 'mirix' does not exist\" error"

    Create the database manually:
    ```bash
    createdb mirix
    
    # If createdb is not in PATH, use full path:
    # /opt/homebrew/opt/postgresql@17/bin/createdb mirix  # macOS with Homebrew
    ```

??? failure "\"pg_dump: No such file or directory\" error (during backup)"

    Set the correct PATH for PostgreSQL tools:
    ```bash
    export PATH="$(brew --prefix postgresql@17)/bin:$PATH"
    ```

### Frontend Issues

??? failure "\"node: command not found\" error"

    Install Node.js first:
    ```bash
    # macOS
    brew install node
    
    # Ubuntu/Debian  
    sudo apt install nodejs npm
    
    # Verify installation
    node --version
    npm --version
    ```

??? failure "\"npm install\" fails with permission errors"

    Fix npm permissions or use a Node version manager:
    ```bash
    # Option 1: Fix npm permissions (Linux/macOS)
    sudo chown -R $(whoami) ~/.npm
    
    # Option 2: Use nvm (recommended)
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
    nvm install node
    ```

??? failure "\"Module not found\" errors when running electron"

    Ensure all dependencies are installed:
    ```bash
    cd frontend
    rm -rf node_modules package-lock.json
    npm install
    npm run electron-dev
    ``` 