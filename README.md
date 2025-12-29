# MIRIX Documentation

Professional documentation website for [MIRIX](https://github.com/Mirix-AI/MIRIX) - a memory system for agents.

## About MIRIX

Starting with version 0.1.6, MIRIX is a pure memory system that can be plugged into any existing agents:

- **[Main Branch (v0.1.6+)](https://github.com/Mirix-AI/MIRIX/tree/main)**: Pure memory system for integration with any agent
- **[Desktop-Agent Branch (v0.1.3)](https://github.com/Mirix-AI/MIRIX/tree/desktop-agent)**: Legacy desktop personal assistant with built-in agent (deprecated)

## 📖 View Documentation

**Live Site**: [https://mirix-ai.github.io/MIRIX-docs/](https://mirix-ai.github.io/MIRIX-docs/)

## 🚀 Local Development

### Prerequisites

- Python 3.11 or later
- Git

### Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/Mirix-AI/MIRIX-docs.git
   cd MIRIX-docs
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Serve locally**
   ```bash
   mkdocs serve
   ```

4. **Open in browser**
   - Visit [http://localhost:8000](http://localhost:8000)
   - Documentation will auto-reload when you make changes

### Building

To build the static site:

```bash
mkdocs build
```

The built site will be in the `site/` directory.

## 📁 Documentation Structure

```
docs/
├── index.md                    # Homepage
├── getting-started/
│   └── installation.md       # Installation and getting started guide
├── architecture/
│   ├── multi-agent-system.md # Multi-agent architecture
│   ├── memory-components.md   # Six memory types
│   └── search-capabilities.md # Advanced search
├── user-guide/
│   ├── desktop-app.md         # Desktop application (coming soon)
│   └── backend-usage.md       # Python backend usage
├── advanced/
│   ├── performance.md         # Performance optimization
│   ├── security-privacy.md    # Security and privacy
│   └── backup-restore.md      # Backup and restore
└── assets/                    # Images and media files
```

## 🛠 Technology Stack

- **[MkDocs](https://www.mkdocs.org/)** - Static site generator
- **[Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)** - Beautiful documentation theme
- **[GitHub Pages](https://pages.github.com/)** - Hosting platform
- **[GitHub Actions](https://github.com/features/actions)** - Automated deployment

## 📝 Contributing

### Content Guidelines

1. **Use clear, concise language**
2. **Include code examples** for technical concepts
3. **Add diagrams** using Mermaid for complex architectures
4. **Test all code examples** before submitting
5. **Follow the existing structure** and formatting

### Making Changes

1. **Fork the repository**
2. **Create a feature branch**
   ```bash
   git checkout -b improve-installation-docs
   ```
3. **Make your changes**
4. **Test locally**
   ```bash
   mkdocs serve
   ```
5. **Submit a pull request**

### Writing Style

- Use **bold** for important terms and UI elements
- Use `inline code` for file names, commands, and code snippets
- Use admonitions (info, warning, tip) for important notes
- Include cross-references to related sections

## 🚀 Deployment

The documentation is automatically deployed using GitHub Actions:

1. **Push to main branch** triggers the deployment workflow
2. **GitHub Actions** builds the documentation using MkDocs
3. **GitHub Pages** serves the built site

### Manual Deployment

If needed, you can deploy manually:

```bash
mkdocs gh-deploy
```

## 📋 Configuration

Key configuration files:

- `mkdocs.yml` - Main MkDocs configuration
- `requirements.txt` - Python dependencies
- `.github/workflows/docs.yml` - GitHub Actions workflow

### Customization

To customize the documentation:

1. **Theme settings** - Edit `mkdocs.yml` under `theme:`
2. **Navigation** - Update the `nav:` section in `mkdocs.yml`
3. **Extensions** - Add Markdown extensions in `markdown_extensions:`
4. **Styling** - Add custom CSS in `docs/assets/`

## 🔧 Troubleshooting

### Common Issues

**MkDocs command not found**
```bash
pip install mkdocs mkdocs-material
```

**Build errors**
```bash
mkdocs build --strict --verbose
```

**Broken links**
```bash
mkdocs serve --strict
```

### Getting Help

- **Documentation Issues**: [Open an issue](https://github.com/Mirix-AI/MIRIX-docs/issues)
- **MIRIX Project**: [Main repository](https://github.com/Mirix-AI/MIRIX)
- **Contact**: yuw164@ucsd.edu

## 📄 License

This documentation is licensed under the same terms as the MIRIX project - see the [MIT License](https://github.com/Mirix-AI/MIRIX/blob/main/LICENSE).

---

**[View Live Documentation →](https://mirix-ai.github.io/MIRIX-docs/)** 