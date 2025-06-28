# Installation

This guide will walk you through setting up MIRIX on your system with PostgreSQL for optimal performance.

## Prerequisites

- **Python 3.11** or later
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

## Step 5: Start MIRIX

```bash
python main.py
```

MIRIX will automatically create all necessary database tables on first startup and begin processing on-screen activities immediately.

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

### Verification

Once installed, verify MIRIX is working:

1. **Check the logs** - MIRIX should start capturing screenshots
2. **Test the chat** - Send a simple message to the agent
3. **Check memory storage** - Verify data is being stored in your database

## Next Steps

Now that MIRIX is installed, learn how to use it:

[**Quick Start Guide →**](quick-start.md){ .md-button .md-button--primary } 