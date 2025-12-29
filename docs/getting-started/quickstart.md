# Quickstart

This quickstart uses the hosted API. If you are self-hosting, set `base_url` when creating the client.

## Install

```bash
pip install mirix==0.1.6
```

## Initialize the Client

```python
from mirix import MirixClient

client = MirixClient(api_key="your_api_key_here")

client.initialize_meta_agent(
    provider="openai"
)
```

## Add Memories

```python
client.add(
    user_id="demo-user",
    messages=[
        {"role": "user", "content": [{"type": "text", "text": "The moon now has a president."}]},
        {"role": "assistant", "content": [{"type": "text", "text": "Noted."}]},
    ],
)
```

## Retrieve with Conversation Context

```python
memories = client.retrieve_with_conversation(
    user_id="demo-user",
    messages=[
        {"role": "user", "content": [{"type": "text", "text": "What did we discuss recently?"}]},
    ],
    limit=5,
)
print(memories)
```

## Self-Hosted Setup

### Option A: Local Installation

#### Step 1: Install Dependencies

```bash
pip install -r requirements.txt
```

#### Step 2: Set Up Database

You have two database options:

=== "SQLite (Default - No Setup Required)"

    If you don't set up PostgreSQL, Mirix will automatically use SQLite. **No additional configuration needed!**
    
    Simply proceed to Step 3 and skip the PostgreSQL environment variables.
    
    !!! info "SQLite Limitations"
        SQLite works but has limitations in concurrent access and advanced search capabilities compared to PostgreSQL.

=== "PostgreSQL (Recommended for Production)"

    PostgreSQL provides better performance, scalability, and vector search capabilities.
    
    **Install PostgreSQL and pgvector:**
    
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
    
    **Create Database and Enable Extensions:**
    
    ```bash
    # Create the mirix database
    createdb mirix
    
    # Enable pgvector extension
    psql -U $(whoami) -d mirix -c "CREATE EXTENSION IF NOT EXISTS vector;"
    ```
    
    **Configure PostgreSQL in Step 3 below** by adding the database URI to your environment variables.

#### Step 3: Configure Environment

Create a `.env` file or set environment variables:

=== "Using SQLite (Default)"

    ```bash
    # Required: At least one LLM provider API key
    export OPENAI_API_KEY=sk-your-openai-api-key
    ```
    
    That's it! SQLite will be used automatically. Proceed to Step 4.

=== "Using PostgreSQL"

    ```bash
    # Required: At least one LLM provider API key
    export OPENAI_API_KEY=sk-your-openai-api-key
    
    # Required: PostgreSQL connection
    # Replace 'your_username' with your system username (find it with: echo $(whoami))
    export MIRIX_PG_URI=postgresql+pg8000://your_username@localhost:5432/mirix
    ```

=== "Optional: Add Redis"

    Add Redis configuration to either setup above:
    
    ```bash
    # Optional: Redis for caching (improves performance)
    export MIRIX_REDIS_ENABLED=true
    export MIRIX_REDIS_HOST=localhost
    export MIRIX_REDIS_PORT=6379
    ```

#### Step 4: Start the Backend

In terminal 1:
```bash
python scripts/start_server.py
# or for development with auto-reload:
python scripts/start_server.py --reload
```

The API server will start at `http://localhost:8531`

#### Step 5: Start the Dashboard (Optional)

In terminal 2:
```bash
cd dashboard
npm install
npm run dev
```

The dashboard will be available at `http://localhost:5173`

#### Step 6: Create an API Key

1. Open the dashboard at `http://localhost:5173`
2. Create an API key for authentication
3. Set it as an environment variable:
```bash
export MIRIX_API_KEY=your-generated-api-key
```

#### Step 7: Use the Client

```python
from mirix import MirixClient

client = MirixClient(
    api_key="your-api-key",  # or use MIRIX_API_KEY env var
    base_url="http://localhost:8531",
)

client.initialize_meta_agent(
    config={
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
        },
    }
)
```

### Option B: Docker Setup (PostgreSQL Included)

Docker automatically sets up PostgreSQL, Redis, and all services for you - no manual database configuration needed!

#### Step 1: Copy Environment File

```bash
cp docker/env.example .env
# Edit .env and set at least OPENAI_API_KEY
```

#### Step 2: Start All Services

```bash
docker-compose up -d
```

This will automatically start and configure:
- **PostgreSQL with pgvector** (port 5432) - automatically configured
- **Redis Stack** (port 6379) - for caching
- **Mirix API** (port 8531) - backend server
- **Dashboard** (port 5173) - web UI

!!! success "Database Included"
    PostgreSQL is automatically set up and configured in Docker - no manual database setup required!

#### Step 3: Access the Dashboard

Open `http://localhost:5173` and create an API key.

#### Step 4: Use the Client

Same as Option A above, using `base_url="http://localhost:8531"`.

### Helpful Resources

- **API Documentation**: `http://localhost:8531/docs` (Swagger UI)
- **Alternative API Docs**: `http://localhost:8531/redoc`
- **Full Examples**: See `samples/run_client.py` in the Mirix repo
- **Docker Documentation**: See `docker/README.md` in the Mirix repo

---

[:octicons-arrow-left-24: Overview](overview.md){ .md-button }
[Next: Memory Write :octicons-arrow-right-24:](../memory-write/configuration.md){ .md-button .md-button--primary }
