# Contributing to Mirix

Thank you for your interest in contributing to Mirix! This guide will help you get started with contributing to our multi-agent personal assistant project.

## 🚀 Quick Start for Contributors

### Development Setup

1. **Fork and Clone**
   ```bash
   git clone https://github.com/YOUR_USERNAME/Mirix.git
   cd Mirix
   ```

2. **Set up Development Environment**
   ```bash
   # Create virtual environment
   python -m venv mirix-dev
   source mirix-dev/bin/activate  # On Windows: mirix-dev\Scripts\activate
   
   # Install development dependencies
   pip install -r requirements-dev.txt
   ```

3. **Configure Pre-commit Hooks**
   ```bash
   pre-commit install
   ```

## 🛠️ Development Workflow

### Branch Structure

- `main` - Production-ready code
- `develop` - Integration branch for features
- `feature/*` - New features
- `bugfix/*` - Bug fixes
- `hotfix/*` - Critical production fixes

### Making Changes

1. **Create a Feature Branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make Your Changes**
   - Write clean, documented code
   - Follow our coding standards
   - Add tests for new functionality

3. **Test Your Changes**
   ```bash
   # Run unit tests
   pytest tests/
   
   # Run integration tests
   pytest tests/integration/
   
   # Check code quality
   flake8 src/
   black src/
   ```

4. **Submit a Pull Request**
   - Use our PR template
   - Include clear description of changes
   - Reference related issues

## 🧪 Testing Guidelines

### Test Structure

```
tests/
├── unit/           # Unit tests for individual components
├── integration/    # Integration tests for agent interactions
├── memory/         # Tests for memory components
├── agents/         # Tests for individual agents
└── fixtures/       # Test data and fixtures
```

### Running Tests

```bash
# All tests
pytest

# Specific test categories
pytest tests/unit/
pytest tests/integration/
pytest tests/memory/

# With coverage
pytest --cov=src tests/
```

### Writing Tests

```python
import pytest
from mirix.agents import MetaAgent
from mirix.memory import CoreMemory

class TestMetaAgent:
    def test_agent_initialization(self):
        agent = MetaAgent()
        assert agent.name == "MetaAgent"
        assert agent.is_active == True
    
    def test_memory_interaction(self):
        agent = MetaAgent()
        memory = CoreMemory()
        
        result = agent.process_screenshot("test_screenshot.png")
        assert result is not None
```

## 🏗️ Architecture Guidelines

### Agent Development

When creating new agents:

1. **Inherit from BaseAgent**
   ```python
   from mirix.core import BaseAgent
   
   class YourAgent(BaseAgent):
       def __init__(self):
           super().__init__(name="YourAgent")
       
       def process(self, data):
           # Your agent logic here
           pass
   ```

2. **Follow Agent Patterns**
   - Single responsibility principle
   - Clear input/output interfaces
   - Error handling and logging
   - Memory interaction protocols

### Memory Component Guidelines

For new memory types:

1. **Implement Memory Interface**
   ```python
   from mirix.memory import BaseMemory
   
   class YourMemory(BaseMemory):
       def store(self, data):
           # Storage logic
           pass
       
       def retrieve(self, query):
           # Retrieval logic
           pass
   ```

2. **Database Schema**
   - Use Alembic for migrations
   - Follow naming conventions
   - Add proper indexes

## 📋 Coding Standards

### Python Style Guide

We follow PEP 8 with these additions:

- **Line Length**: 88 characters (Black default)
- **Import Order**: isort with Black compatibility
- **Docstrings**: Google style docstrings
- **Type Hints**: Use for all public functions

### Code Quality Tools

```bash
# Formatting
black src/
isort src/

# Linting
flake8 src/
pylint src/

# Type checking
mypy src/
```

### Documentation

- **Docstrings**: Required for all public methods
- **Type Hints**: Required for function signatures
- **Comments**: Explain complex logic, not obvious code
- **README**: Update if adding new features

Example:

```python
def search_memory(
    self, 
    query: str, 
    memory_type: Optional[str] = None,
    limit: int = 10
) -> List[MemoryResult]:
    """Search across memory components for relevant information.
    
    Args:
        query: Search query text
        memory_type: Optional memory type filter
        limit: Maximum number of results to return
        
    Returns:
        List of memory results sorted by relevance
        
    Raises:
        ValueError: If query is empty or invalid
        MemoryError: If memory system is unavailable
    """
    pass
```

## 🐛 Bug Reports

### Before Reporting

1. Check existing issues
2. Reproduce with latest version
3. Test with minimal configuration

### Bug Report Template

```markdown
**Bug Description**
Clear description of the bug

**Steps to Reproduce**
1. Step one
2. Step two
3. See error

**Expected Behavior**
What should happen

**Environment**
- OS: [e.g., macOS 14.0]
- Python: [e.g., 3.11.5]
- Mirix Version: [e.g., 1.2.0]
- Database: [PostgreSQL 17 / SQLite]

**Additional Context**
Logs, screenshots, etc.
```

## 💡 Feature Requests

### Proposal Process

1. **Check Roadmap**: Review our [project roadmap](https://github.com/Mirix-AI/Mirix/projects)
2. **Open Discussion**: Create a GitHub Discussion
3. **RFC Process**: For major features, write an RFC
4. **Implementation**: After approval, implement the feature

### Feature Request Template

```markdown
**Feature Description**
Clear description of the proposed feature

**Use Case**
Why is this feature needed?

**Proposed Solution**
How should it work?

**Alternatives Considered**
Other approaches you've considered

**Additional Context**
Mockups, examples, etc.
```

## 🔄 Release Process

### Versioning

We use Semantic Versioning (SemVer):
- `MAJOR.MINOR.PATCH`
- Major: Breaking changes
- Minor: New features (backward compatible)
- Patch: Bug fixes

### Release Checklist

- [ ] All tests passing
- [ ] Documentation updated
- [ ] CHANGELOG.md updated
- [ ] Version bumped
- [ ] Tag created
- [ ] Release notes written

## 🤝 Community Guidelines

### Code of Conduct

- Be respectful and inclusive
- Provide constructive feedback
- Help newcomers learn
- Keep discussions on-topic

### Communication Channels

- **GitHub Issues**: Bug reports and feature requests
- **GitHub Discussions**: General questions and ideas
- **Email**: [yuw164@ucsd.edu](mailto:yuw164@ucsd.edu) for sensitive matters

## 📚 Additional Resources

### Documentation

- [Architecture Overview](architecture/multi-agent-system.md)
- [Memory System Design](architecture/memory-components.md)
- [API Reference](api/agent-api.md)
- [Development Setup](getting-started/installation.md)

### External Resources

- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [Python Async/Await Guide](https://docs.python.org/3/library/asyncio.html)
- [Pytest Documentation](https://docs.pytest.org/)

---

## 🙏 Recognition

Contributors are recognized in:
- GitHub contributor graphs
- Release notes
- CONTRIBUTORS.md file
- Special mentions for significant contributions

Thank you for helping make Mirix better! 🚀 