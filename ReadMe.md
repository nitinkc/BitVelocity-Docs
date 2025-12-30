
# BitVelocity Documentation

This repository contains the documentation for the BitVelocity platform built with MkDocs.

## 🚀 Quick Start

### Prerequisites
- Python 3.9+ installed and accessible in PATH
- MkDocs and plugins installed (see setup below)

### Setup (First Time Only)

**On macOS/Linux:**

```bash
# Create virtual environment in your home directory
python3 -m venv ~/mkdocs-venv

# Activate virtual environment
source ~/mkdocs-venv/bin/activate

# Install dependencies
pip install mkdocs mkdocs-material mkdocs-mermaid2-plugin
```


### Build Documentation

**On macOS/Linux:**

```bash
~/mkdocs-venv/bin/mkdocs build
```

This generates static site files in the `site/` directory.

### Preview Locally

**On macOS/Linux:**

```bash
~/mkdocs-venv/bin/mkdocs serve
```

Open your browser to: **http://127.0.0.1:8000/BitVelocity-Docs/**

### Deploy to GitHub Pages

**On macOS/Linux:**

```bash
~/mkdocs-venv/bin/mkdocs gh-deploy
```

## 📝 Notes

- Virtual environment is installed in your home/user directory
- Site is deployed to: https://nitinkc.github.io/BitVelocity-Docs/

## 🐛 Troubleshooting

**Python not found:**
- Ensure Python is installed and in your PATH
- Try `python3` (macOS/Linux) or `python` (Windows)

**Permission denied:**
- Use virtual environment in your home directory (no sudo/admin needed)