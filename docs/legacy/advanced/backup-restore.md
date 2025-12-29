# Backup & Restore

MIRIX provides simple backup and restore functionality for your agent data, ensuring your memories and configurations are safe and portable.

## Overview

The backup system automatically handles both PostgreSQL and SQLite databases, preserving all your conversations, memories, and agent configurations in a portable format.

## Prerequisites

### For PostgreSQL Backups

Since backups use `pg_dump` to save the database, ensure PostgreSQL tools are accessible:

```bash
# macOS with Homebrew
export PATH="$(brew --prefix postgresql@17)/bin:$PATH"

# Verify pg_dump is available
which pg_dump
```

!!! warning "PATH Configuration Required"
    
    Without proper PATH configuration, PostgreSQL backup operations will fail with "pg_dump: No such file or directory" error.

### For SQLite Backups

No additional setup required - SQLite backups work out of the box.

## Creating Backups

### Basic Backup

```python
from mirix.agent import AgentWrapper

# Initialize agent
agent = AgentWrapper("./configs/mirix.yaml")

# Create backup
result = agent.save_agent("./my_backup")
print(result['message'])  # "Agent state saved successfully..."
```

That's it! The backup is created in the specified directory.

## Backup Structure

The backup structure depends on your database type:

### PostgreSQL Backup Structure

```
my_backup/
├── agent_config.json      # Agent configuration
├── mirix_database.dump    # Binary database dump
└── mirix_database.sql     # SQL database dump
```

### SQLite Backup Structure

```
my_backup/
├── agent_config.json      # Agent configuration
└── sqlite.db             # Complete SQLite database file
```

## Restoring from Backups

### Method 1: Restore During Initialization (Recommended)

```python
# Initialize agent with backup data
agent = AgentWrapper("./configs/mirix.yaml", "./my_backup")

print("Agent restored from backup")
```

### Method 2: Load Backup After Creation

```python
# Create agent normally
agent = AgentWrapper("./configs/mirix.yaml")

# Then load backup data
config_path = "./my_backup/mirix_config.yaml"
self._agent = AgentWrapper(str(config_path), load_from="./my_backup")

if result['success']:
    print("Backup restored successfully")
else:
    print(f"Restore failed: {result['error']}")
```


## Troubleshooting

??? failure "\"pg_dump: No such file or directory\" error"
    
    PostgreSQL tools are not in your PATH:
    ```bash
    # Add PostgreSQL to PATH
    export PATH="$(brew --prefix postgresql@17)/bin:$PATH"
    
    # Add to your shell profile for persistence
    echo 'export PATH="$(brew --prefix postgresql@17)/bin:$PATH"' >> ~/.zshrc
    ```

??? failure "\"Permission denied\" during backup"
    
    Check database permissions:
    ```sql
    -- Connect to your database
    psql -U $(whoami) -d mirix
    
    -- Check permissions
    \du
    ```

??? failure "Backup directory already exists"
    
    Choose a different backup path or remove the existing directory:
    ```python
    import shutil
    import os
    
    backup_path = "./my_backup"
    if os.path.exists(backup_path):
        shutil.rmtree(backup_path)
    
    # Now create backup
    result = agent.save_agent(backup_path)
    ```

??? failure "Restore fails"
    
    Verify the backup directory contains the required files:
    ```python
    import os
    
    backup_path = "./my_backup"
    
    # Check for required files
    config_exists = os.path.exists(f"{backup_path}/agent_config.json")
    
    # For PostgreSQL
    pg_dump_exists = os.path.exists(f"{backup_path}/mirix_database.dump")
    pg_sql_exists = os.path.exists(f"{backup_path}/mirix_database.sql")
    
    # For SQLite
    sqlite_exists = os.path.exists(f"{backup_path}/sqlite.db")
    
    print(f"Config file: {config_exists}")
    print(f"PostgreSQL dumps: {pg_dump_exists and pg_sql_exists}")
    print(f"SQLite database: {sqlite_exists}")
    ```

## Migration Between Systems

### Export for Migration

To move your MIRIX agent to a new system:

1. **Create a backup** on the source system:
   ```python
   agent = AgentWrapper("./configs/mirix.yaml")
   result = agent.save_agent("./migration_backup")
   ```

2. **Copy the backup** to the target system:
   ```bash
   # Copy backup directory to new system
   scp -r ./migration_backup user@new-system:/path/to/mirix/
   ```

3. **Restore on the target** system:
   ```python
   # On the new system
   agent = AgentWrapper("./configs/mirix.yaml", "./migration_backup")
   ```

## What's Next?

Learn about contributing:

[**Contributing →**](../contributing.md){ .md-button .md-button--primary }

 