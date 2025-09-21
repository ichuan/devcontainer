# Universal Development Container

A comprehensive, ready-to-use development container template that provides a standardized setup for modern web development with Node.js, Python, and AI-powered development tools.

## How to Use This Template

1. **Copy to your project**: Copy the `.devcontainer/` directory to your project root
2. **Customize as needed**: Modify `devcontainer.json` and `Dockerfile` for your specific requirements
3. **Follow the Getting Started guide below**

This template is designed to be flexible and can be adapted for various project types requiring Node.js and/or Python environments.

## What's Included

### Core Runtime Environment
- **Node.js LTS** with pnpm package manager (npm alternative)
- **Python 3.12+** managed via pyenv with Poetry for dependency management
- **TypeScript** globally installed for type-safe JavaScript development

### Development Tools
- **Git LFS** and **GitHub CLI** for version control and collaboration
- **Claude Code** - Anthropic's official AI coding assistant
- **Essential CLI tools**: vim, curl, wget, jq, ag (silver searcher), htop, tmux
- **Build tools**: make, build-essential, and Python compilation dependencies

### VS Code Integration
Pre-configured with essential extensions:
- TypeScript, Prettier, Tailwind CSS
- Python development (debugpy, black formatter, ruff linter)
- GitHub Copilot and Claude Code AI assistants
- JSON/YAML support and hex editor

### Port Forwarding
Automatically forwards common development ports:
- **3000**: Frontend dev server
- **5000**: Backend dev server
- **8000**: Python dev server
- **8080, 9000**: Additional services

## Prerequisites

- [Docker](https://www.docker.com/get-started) installed on your machine
- [VS Code](https://code.visualstudio.com/) with the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

## Getting Started

1. **Copy the devcontainer to your project**:
   ```bash
   # Copy .devcontainer directory to your project root
   cp -r /path/to/this/template/.devcontainer /path/to/your/project/
   ```
2. **Open your project in VS Code**:
   ```bash
   code /path/to/your/project
   ```
3. **Reopen in Container**: When prompted, click "Reopen in Container" or use Command Palette (`Ctrl+Shift+P`) → "Dev Containers: Reopen in Container"
4. **Wait for setup**: The container will build and configure automatically (first-time setup takes 5-10 minutes)
5. **Customize**: Modify `.devcontainer/devcontainer.json` and `.devcontainer/Dockerfile` as needed for your project

## Key Features

### Persistent Configuration
- **SSH keys, Git config**: Automatically mounted from your host system
- **Claude Code settings**: Persisted across container rebuilds via named volume
- **Custom dotfiles**: Includes pre-configured shell environment

### Development Workflow Support
- **TDD-ready**: Configured for test-driven development with immediate test execution
- **Code quality**: Automatic formatting on save, import organization, and linting
- **AI assistance**: Integrated Claude Code and GitHub Copilot for enhanced productivity

### Multi-language Support
- **JavaScript/TypeScript**: pnpm-first workflow with modern tooling
- **Python**: Poetry + pyenv setup with ruff formatting and comprehensive linting
- **Universal**: Supports any project requiring Node.js and/or Python environments

## Directory Structure

```
.devcontainer/
├── devcontainer.json           # Main container configuration
├── Dockerfile                  # Container image definition
├── .claude/
│   ├── CLAUDE.md              # Development workflow and coding standards
│   ├── settings.local.json    # Claude Code permissions configuration
│   └── agents/                # AI agent configurations
│       ├── code-reviewer.md
│       ├── documentation-writer.md
│       └── test-engineer.md
└── .claude.json               # MCP server configurations for enhanced AI capabilities
```

## Environment Details

- **Base OS**: Debian Bookworm (stable)
- **User**: Non-root `dev` user with sudo access
- **Shell**: Bash with enhanced dotfiles configuration
- **Timezone**: UTC
- **Locale**: en_US.UTF-8

## Post-Setup Verification

After the container starts, verify your environment:

```bash
# Check versions
node --version    # Node.js LTS
pnpm --version    # Package manager
python --version  # Python 3.12+
poetry --version  # Dependency management
claude --version  # AI coding assistant
```

## Customization

### Common Modifications
- **Ports**: Update `forwardPorts` in `devcontainer.json` for your application
- **Extensions**: Add or remove VS Code extensions in the `extensions` array
- **Environment variables**: Modify `containerEnv` section as needed
- **System packages**: Add packages to the `apt-get install` command in `Dockerfile`
- **Claude Code settings**: Customize `.claude/CLAUDE.md` for your development workflow

### Project-Specific Setup
After copying this template, consider:
- Adding your project's specific runtime requirements
- Configuring database containers if needed
- Setting up project-specific environment variables
- Customizing the development workflow in `.claude/CLAUDE.md`

This devcontainer template provides a consistent, feature-rich development environment that eliminates "works on my machine" issues and accelerates project setup for both individual developers and teams.