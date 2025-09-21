# Universal DevContainer Template

A ready-to-use development container with Node.js, Python, and AI-powered tools.

## Quick Start

### Method 1: Download from Releases (Recommended)
```bash
# One command setup
mkdir -p .devcontainer && curl -L https://github.com/ichuan/devcontainer/releases/latest/download/devcontainer.json -o .devcontainer/devcontainer.json
```
Then open in VS Code → "Reopen in Container"

### Method 2: Copy from Repository
```bash
# Download the template
curl -o devcontainer.json https://raw.githubusercontent.com/ichuan/devcontainer/main/.devcontainer/devcontainer.json

# Create directory and move file
mkdir -p .devcontainer
mv devcontainer.json .devcontainer/
```

## What's Included

- **Node.js LTS** + pnpm
- **Python 3.12+** + Poetry + pyenv
- **TypeScript**, **Claude Code**, **GitHub CLI**
- **VS Code Extensions**: TypeScript, Python, Prettier, Ruff, GitHub Copilot
- **Pre-built Docker Image**: `ghcr.io/ichuan/devcontainer:latest`

## Features

- ⚡ **Instant Setup**: Uses pre-built Docker image (no build time)
- 🔧 **Auto-configured**: Works out of the box
- 🤖 **AI-Ready**: Claude Code + GitHub Copilot integrated
- 🔄 **Auto-Updates**: New releases when template changes
- 🚀 **Universal**: Supports most Node.js/Python projects

## Port Forwarding
- **3000**: Frontend dev server
- **5000**: Backend dev server
- **8000**: Python dev server

## Automatic Updates

When `devcontainer.json` changes are pushed to this repository:
- ✅ New GitHub release is created automatically
- ✅ Docker image is rebuilt and published
- ✅ Download latest `devcontainer.json` from releases

## Requirements

- [Docker](https://www.docker.com/get-started)
- [VS Code](https://code.visualstudio.com/) + [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)