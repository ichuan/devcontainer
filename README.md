# Universal DevContainer Template

A ready-to-use development container with Node.js, Python, and AI-powered tools.

## Quick Start

```bash
mkdir -p .devcontainer .claude && curl -L https://raw.githubusercontent.com/ichuan/devcontainer/main/.devcontainer/devcontainer.json -o .devcontainer/devcontainer.json
```

If it's a frontend/typescript project:

```bash
curl -L https://raw.githubusercontent.com/ichuan/devcontainer/main/frontend_CLAUDE.md -o .claude/CLAUDE.md
```

Or if it's a backend/Python project:

```bash
curl -L https://raw.githubusercontent.com/ichuan/devcontainer/main/python_CLAUDE.md -o .claude/CLAUDE.md
```

Or if it's a hybrid project / you don't know:

```bash
curl -L https://raw.githubusercontent.com/ichuan/devcontainer/main/all_CLAUDE.md -o .claude/CLAUDE.md
```

Then open in VS Code → "Reopen in Container"

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
- 🔄 **Auto-Updates**: Always uses latest from main branch
- 🚀 **Universal**: Supports most Node.js/Python projects

## Port Forwarding
- **3000**: Frontend dev server
- **5173**: Vite dev server
- **8000**: Python dev server

## Requirements

- [Docker](https://www.docker.com/get-started)
- [VS Code](https://code.visualstudio.com/) + [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
