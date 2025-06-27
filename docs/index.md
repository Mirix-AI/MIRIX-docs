# Welcome to Mirix

**Mirix** is a multi-agent personal assistant designed to track on-screen activities and answer user questions intelligently. By capturing real-time visual data and consolidating it into structured memories, Mirix transforms raw inputs into a rich knowledge base that adapts to your digital experiences.

---

## :material-rocket-launch: Quick Start

Get up and running with Mirix in minutes:

!!! tip "New to Mirix?"
    
    Start with our [**Installation Guide**](getting-started/installation.md) to set up Mirix on your system.

```bash
# Clone the repository
git clone https://github.com/Mirix-AI/Mirix.git
cd Mirix

# Install dependencies
pip install -r requirements.txt

# Start Mirix
python main.py
```

## :material-brain: Key Features

<div class="grid cards" markdown>

-   :material-account-group:{ .lg .middle } **Multi-Agent System**

    ---

    Eight specialized agents working collaboratively to process and manage your digital experiences.

    [:octicons-arrow-right-24: Learn more](architecture/multi-agent-system.md)

-   :material-database:{ .lg .middle } **Six Memory Components**

    ---

    Core, Episodic, Semantic, Procedural, Resource, and Knowledge Vault memories for comprehensive data organization.

    [:octicons-arrow-right-24: Explore Memory](architecture/memory-components.md)

-   :material-magnify:{ .lg .middle } **Advanced Search**

    ---

    PostgreSQL-native BM25 search with vector similarity and fuzzy matching capabilities.

    [:octicons-arrow-right-24: Search Features](architecture/search-capabilities.md)

-   :material-shield-lock:{ .lg .middle } **Privacy First**

    ---

    All long-term data stored locally with user-controlled privacy settings and secure handling.

    [:octicons-arrow-right-24: Security Details](advanced/security-privacy.md)

</div>

## :material-chart-line: Performance

Mirix delivers exceptional performance with intelligent memory consolidation:

| Dataset | Duration | Images | Accuracy | Storage Size |
|---------|----------|--------|----------|--------------|
| Dataset 1 | 1.5 hours | 700 | **50.0%** | Optimized |
| Dataset 2 | 24 hours | 5,886 | **50.0%** | **20.57 MB** |

*Significantly outperforming traditional approaches like Gemini (0-8.33% accuracy) with 1000x+ storage efficiency.*

## :material-account-voice: How It Works

```mermaid
graph TB
    A[User Input] --> B[Meta Agent]
    B --> C[Memory Managers]
    C --> D[Memory Base]
    
    E[User Query] --> F[Chat Agent]
    F --> G[search_memory()]
    G --> D
    D --> H[Contextual Response]
    
    subgraph "Memory Components"
        D1[Core Memory]
        D2[Episodic Memory]
        D3[Semantic Memory]
        D4[Procedural Memory]
        D5[Resource Memory]
        D6[Knowledge Vault]
    end
    
    D --> D1
    D --> D2
    D --> D3
    D --> D4
    D --> D5
    D --> D6
```

## :material-book-open-page-variant: Getting Started

<div class="grid" markdown>

!!! note "Installation"
    
    Set up Mirix with PostgreSQL for optimal performance.
    
    [**Installation Guide →**](getting-started/installation.md)

!!! example "Quick Start"
    
    Learn the basic usage patterns and start tracking activities.
    
    [**Quick Start →**](getting-started/quick-start.md)

!!! info "Architecture"
    
    Understand the multi-agent system and memory components.
    
    [**Architecture →**](architecture/multi-agent-system.md)

!!! tip "User Guide"
    
    Comprehensive guides for both desktop app and backend usage.
    
    [**User Guide →**](user-guide/desktop-app.md)

</div>

## :material-github: Open Source

Mirix is open source and actively developed. Join our community:

- [:fontawesome-brands-github: **GitHub Repository**](https://github.com/Mirix-AI/Mirix) - Source code and issue tracking
- [:material-email: **Contact**](mailto:yuw164@ucsd.edu) - Questions and support

---

*Ready to transform your digital experience with intelligent memory?* **[Get Started →](getting-started/overview.md)** 