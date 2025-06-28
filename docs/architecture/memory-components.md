# Memory Components

MIRIX organizes information into six distinct memory components, each designed to handle specific types of data and provide optimal retrieval performance.

## 1. Core Memory

**Purpose**: Persistent information that should always be visible to the agent when interacting with the user.

**Inspired by**: MemGPT's core memory architecture

### Structure

Core Memory is organized in multiple blocks (`human` and `persona`)

```
<human 117/500 characters>
User's name is David
User prefers coffee over tea
User works as a software engineer
User enjoys reading sci-fi novels
</human>

<persona 24/5000 characters>
I am a helpful assistant
</persona>
```

## 2. Episodic Memory

**Purpose**: Captures context-specific events and temporal activities, serving as a summarization or calendar of user behaviors.

### Structure

Each episodic entry contains:

```json
{
  "event_type": "user_message",
  "summary": "User reviewed quarterly sales report",
  "details": "Detailed analysis of Q3 sales performance, identified key growth areas in mobile segment, discussed strategies with marketing team",
  "actor": "user",
  "timestamp": "2025-03-05 10:15"
}
```

## 3. Semantic Memory

**Purpose**: Maintains general knowledge, concepts, and abstracted information independent of temporal context.

### Structure

Each semantic entry includes:

```json
{
  "name": "PostgreSQL",
  "summary": "Open-source relational database management system",
  "details": "Powerful, enterprise-grade database with advanced features like JSONB support, full-text search, and vector extensions. Preferred by user for its performance and reliability.",
  "source": "user_interaction"
}
```

### Content Types

- **Factual Knowledge**: "Harry Potter is written by J.K. Rowling"
- **Facts and Understandings about Other People**: "John is user's good friend who likes jogging"
- **Concepts**: "Machine learning algorithms and their applications"

### Example Entries

```json
[
  {
    "name": "MkDocs Material",
    "summary": "Documentation framework based on MkDocs",
    "details": "Static site generator that creates beautiful documentation sites from Markdown files. Features include responsive design, search functionality, and extensive customization options.",
    "source": "documentation_project"
  },
  {
    "name": "Team Standup Meeting",
    "summary": "Daily team synchronization meeting",
    "details": "Occurs every weekday at 9 AM, attended by development team to discuss progress, blockers, and daily goals. Usually lasts 15-20 minutes.",
    "source": "recurring_activity"
  }
]
```

## 4. Procedural Memory

**Purpose**: Records process workflows and step-by-step instructions for accomplishing specific tasks.

### Structure

Each procedural entry contains:

```json
{
  "entry_type": "workflow",
  "description": "Deploy application to production",
  "steps": [
    "1. Run test suite to ensure all tests pass",
    "2. Create production build with 'npm run build'",
    "3. Review build artifacts for any issues",
    "4. Deploy to staging environment first",
    "5. Perform smoke tests on staging",
    "6. Deploy to production using CI/CD pipeline",
    "7. Monitor application metrics post-deployment"
  ]
}
```

### Example Entries

```json
[
  {
    "entry_type": "workflow",
    "description": "Setting up new development environment",
    "steps": [
      "1. Install Python 3.11 or later",
      "2. Set up virtual environment with 'python -m venv venv'",
      "3. Activate virtual environment",
      "4. Install dependencies with 'pip install -r requirements.txt'",
      "5. Configure environment variables in .env file",
      "6. Initialize database with 'python manage.py migrate'",
      "7. Run development server with 'python manage.py runserver'"
    ]
  },
  {
    "entry_type": "guide",
    "description": "Troubleshooting PostgreSQL connection issues",
    "steps": [
      "1. Check if PostgreSQL service is running",
      "2. Verify database exists with 'psql -l'",
      "3. Test connection with 'psql -U username -d database'",
      "4. Check firewall settings if connecting remotely",
      "5. Verify authentication configuration in pg_hba.conf"
    ]
  }
]
```

## 5. Resource Memory

**Purpose**: Manages active documents and project-related files that the user interacts with.

### Structure

Each resource entry includes:

```json
{
  "title": "Project Proposal - Q4 2024",
  "summary": "Comprehensive proposal for new mobile application development project including timeline, budget, and technical specifications",
  "resource_type": "pdf_text",
  "content": "# Project Proposal\n\n## Executive Summary\nThis proposal outlines the development of a new mobile application...\n\n## Technical Requirements\n- React Native framework\n- PostgreSQL database\n- AWS cloud infrastructure..."
}
```

### Example Entries

```json
[
  {
    "title": "API Documentation Draft",
    "summary": "Initial draft of REST API documentation for the customer management system, includes endpoint specifications and example requests",
    "resource_type": "markdown",
    "content": "# Customer Management API\n\n## Overview\nThis API provides endpoints for managing customer data...\n\n## Endpoints\n\n### GET /api/customers\nRetrieve list of customers..."
  },
  {
    "title": "Meeting Recording - Sprint Planning",
    "summary": "Voice recording from sprint planning meeting discussing user stories and development priorities for next iteration",
    "resource_type": "voice_transcript",
    "content": "Transcript: 'Let's start with the user authentication story. Based on our previous discussion, we need to implement OAuth 2.0 integration...'"
  }
]
```

## 6. Knowledge Vault

**Purpose**: Securely stores structured personal data such as addresses, phone numbers, contacts, and credentials.

### Structure

Each vault entry contains:

```json
{
  "entry_type": "credential",
  "source": "github",
  "sensitivity": "high",
  "secret_value": "ghp_xxxxxxxxxxxxxxxxxxxx",
  "caption": "GitHub Personal Access Token for API access"
}
```

### Sensitivity Levels

- `low`: General bookmarks and public information
- `medium`: Contact information and non-critical data
- `high`: Passwords, API keys, and sensitive credentials

### Security Features

- **Encryption**: Sensitive data encrypted at rest
- **Access Control**: Restricted access based on sensitivity level
- **Audit Trail**: All access to sensitive data is logged
- **Automatic Expiration**: Credentials can have expiration dates

### Example Entries

```json
[
  {
    "entry_type": "api_key",
    "source": "openai",
    "sensitivity": "high",
    "secret_value": "sk-proj-xxxxxxxxxxxxxxxxxxxx",
    "caption": "OpenAI API key for ChatGPT integration"
  },
  {
    "entry_type": "bookmark",
    "source": "user_provided",
    "sensitivity": "low",
    "secret_value": "https://docs.mirix.ai/",
    "caption": "MIRIX documentation website"
  },
  {
    "entry_type": "contact_info",
    "source": "user_profile",
    "sensitivity": "medium",
    "secret_value": "john.doe@example.com",
    "caption": "Primary email address"
  }
]
```

## Memory Interaction Patterns

### Search Integration

All memory components support unified search:

```python
# Search across all memory types
results = search_in_memory(
    query="machine learning project",
    memory_type='episodic', # can be chosen from ['all', 'episodic', 'semantic', 'resource', 'procedural', 'knowledge_vault']
    limit=20
)
```

## Memory Optimization

### Automatic Cleanup

- **Core Memory**: Rewrites blocks when approaching capacity
- **Episodic Memory**: Archives old entries based on relevance
- **Semantic Memory**: Merges duplicate concepts
- **Procedural Memory**: Updates workflows based on usage patterns
- **Resource Memory**: Compresses or removes unused resources
- **Knowledge Vault**: Expires outdated credentials

<!-- 
### Performance Tuning
- **Indexing**: Optimized database indexes for fast retrieval
- **Caching**: Frequently accessed data cached in memory
- **Compression**: Large content compressed to save space
- **Partitioning**: Data partitioned by date and type for efficient queries -->
<!-- 
## Best Practices

### Memory Organization

1. **Keep Core Memory Concise**: Only essential, persistent information
2. **Detailed Episodic Entries**: Rich context for better retrieval
3. **Abstract Semantic Concepts**: Focus on reusable knowledge
4. **Actionable Procedures**: Clear, step-by-step instructions
5. **Comprehensive Resources**: Full content for better context
6. **Secure Vault Management**: Proper sensitivity classification

### Search Optimization

- Use specific queries for better results
- Combine memory types for comprehensive answers
- Leverage field-specific search when needed
- Regular memory cleanup for optimal performance -->

## What's Next?

Learn about MIRIX's advanced search capabilities:

[**Search Capabilities →**](search-capabilities.md){ .md-button .md-button--primary } 