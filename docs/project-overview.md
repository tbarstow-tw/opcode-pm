# Project Overview - opcode

**Project Name:** opcode
**Description:** GUI app and Toolkit for Claude Code
**Type:** Desktop Application (Tauri 2)
**License:** AGPL-3.0
**Version:** 0.2.1
**Generated:** 2025-12-03

---

## Executive Summary

**opcode** is a comprehensive desktop application that transforms how developers interact with Claude Code. Built with Tauri 2, it provides an intuitive graphical interface for managing Claude Code sessions, creating custom AI agents, tracking usage metrics, and integrating with the broader Claude Code ecosystem.

The application bridges the gap between command-line AI-assisted development and visual user experience, making Claude Code accessible to both technical developers and non-technical users such as designers and product managers.

---

## Project Purpose

### Problem Statement

While Claude Code is powerful for AI-assisted development, its command-line interface presents barriers for:
- Non-technical users (designers, product managers, content creators)
- Users who prefer visual interfaces
- Teams needing centralized session and agent management
- Organizations requiring usage tracking and cost monitoring

### Solution

opcode provides:
1. **Visual Interface:** Intuitive GUI for all Claude Code operations
2. **Session Management:** Browse, search, and resume coding sessions
3. **Custom Agents:** Create and execute specialized AI agents
4. **Usage Analytics:** Real-time token usage and cost tracking
5. **MCP Integration:** Manage Model Context Protocol servers
6. **Multi-Platform Support:** Native apps for macOS, Linux, Windows

---

## Technology Stack Summary

### Frontend

| Technology | Version | Role |
|------------|---------|------|
| **React** | 18.3.1 | UI Framework |
| **TypeScript** | 5.6.2 | Type-safe Development |
| **Tailwind CSS** | 4.1.8 | Styling |
| **Radix UI** | Various | UI Components |
| **Zustand** | 5.0.6 | State Management |
| **Vite** | 6.0.3 | Build Tool |

### Backend

| Technology | Version | Role |
|------------|---------|------|
| **Tauri** | 2.7.1 | Desktop Framework |
| **Rust** | 2021 | Backend Language |
| **SQLite** (rusqlite) | 0.32 | Local Database |
| **Axum** | 0.8 | Web Server (optional) |
| **Tokio** | 1.x | Async Runtime |

### Development & Deployment

- **Package Manager:** Bun
- **CI/CD:** GitHub Actions (7 workflows)
- **Platforms:** macOS (Universal), Linux (x86_64), Windows (x86_64)

---

## Architecture Classification

**Architecture Type:** Event-Driven Desktop Application

**Pattern:** Tauri-based hybrid architecture with:
- **Frontend Layer:** React + TypeScript (UI)
- **Backend Layer:** Rust (Business Logic, System Integration)
- **Communication:** IPC (Inter-Process Communication)
- **State Management:** Zustand (Frontend), SQLite (Persistent)

**Deployment Model:** Native desktop application with optional web server mode

---

## Repository Structure

**Repository Type:** Monolith
**Primary Language:** TypeScript (Frontend), Rust (Backend)
**Build System:** Tauri + Vite

### Key Directories

```
opcode-pm/
├── src/                    # React Frontend
│   ├── components/         # UI Components (88 files)
│   ├── stores/             # State Management (2 Zustand stores)
│   ├── hooks/              # Custom React Hooks
│   ├── lib/                # Utilities & API Client
│   └── types/              # TypeScript Definitions
│
├── src-tauri/              # Rust Backend
│   ├── src/                # Rust Source Code
│   ├── Cargo.toml          # Rust Dependencies
│   └── tauri.conf.json     # Tauri Configuration
│
├── .github/workflows/      # CI/CD Pipelines
├── docs/                   # Project Documentation
└── cc_agents/              # Custom Agent Examples
```

---

## Core Features

### 1. **Project & Session Management**
- Browse all Claude Code projects in `~/.claude/projects/`
- View session history with search and filtering
- Resume past sessions with full context
- Session insights (timestamps, first messages, metadata)

### 2. **Custom AI Agents**
- Create specialized agents with custom system prompts
- Build agent library for different tasks
- Execute agents in background processes
- Track execution history with detailed logs

### 3. **Usage Analytics Dashboard**
- Real-time Claude API usage and cost tracking
- Token analytics by model, project, and time period
- Visual charts for usage trends
- Data export for accounting

### 4. **MCP Server Management**
- Centralized Model Context Protocol server registry
- UI-based server configuration
- Connection testing
- Import from Claude Desktop

### 5. **Timeline & Checkpoints**
- Session versioning with checkpoints
- Visual timeline navigation
- Instant checkpoint restoration
- Fork sessions from any point

### 6. **Developer Tools**
- Integrated file editor
- Custom hooks configuration
- Slash command management
- Claude version selection

---

## Key Metrics

**Codebase Size:**
- **Frontend:** 88 component files + hooks, services, utilities
- **Backend:** Rust modules for database, agents, sessions, MCP
- **Total Lines:** ~50,000+ lines (estimated)

**Component Inventory:**
- **UI Primitives:** 19 Radix UI wrappers
- **Feature Components:** 65+ specialized components
- **Widgets:** 3 (Bash, LS, Todo)

**State Management:**
- **Stores:** 2 Zustand stores (sessionStore, agentStore)
- **Real-time Updates:** Polling + event-driven
- **Caching:** Time-based with 5-second cache

**Assets:**
- **Application Icons:** 50+ multi-platform icons
- **Fonts:** Inter.ttf
- **Images/Audio:** Logo, branding assets

---

## Development Status

**Current Version:** 0.2.1
**Maturity:** Active Development
**Release Cycle:** Tag-based releases (`v*`)
**CI/CD:** Automated multi-platform builds

**Recent Activity:**
- ✅ Windows support for Claude binary detection
- ✅ Web server mode with WebSocket support
- ✅ Project path display improvements
- ✅ cargo fmt compliance

---

## Links to Detailed Documentation

### Technical Documentation
- **[Architecture](./architecture.md)** - Comprehensive architecture documentation
- **[Source Tree Analysis](./source-tree-analysis.md)** - Annotated directory structure
- **[Component Inventory](./component-inventory.md)** - Complete component catalog

### Development Guides
- **[Development Guide](./development-guide.md)** - Setup, workflow, and best practices
- **[Deployment Guide](./deployment-guide.md)** - CI/CD, releases, and deployment
- **[Contribution Guide](./contribution-guide.md)** - How to contribute

### Existing Documentation
- **[README.md](../README.md)** - Main project documentation
- **[CONTRIBUTING.md](../CONTRIBUTING.md)** - Contribution guidelines
- **[web_server.design.md](../web_server.design.md)** - Web server architecture
- **[cc_agents/README.md](../cc_agents/README.md)** - Custom agents guide
- **[src/stores/README.md](../src/stores/README.md)** - State management notes

---

## Getting Started

### For Users
1. Download installer for your platform from [Releases](https://github.com/getAsterisk/opcode/releases)
2. Install the application
3. Launch opcode
4. Connect to your Claude Code projects

### For Developers
1. Clone the repository
2. Install dependencies: `bun install`
3. Run development mode: `bun run tauri dev`
4. See [Development Guide](./development-guide.md) for details

---

## Use Cases

**Primary Audiences:**
- **Developers:** Visual interface for Claude Code sessions
- **Designers:** Access AI assistance without terminal
- **Product Managers:** Use Claude Code for documentation, planning
- **Teams:** Centralized agent and session management

**Typical Workflows:**
- Browse and resume past coding sessions
- Create custom agents for repetitive tasks
- Track API usage and costs
- Manage MCP servers for extended functionality
- Navigate session timelines and checkpoints

---

## Unique Value Propositions

1. **UI/UX Focus:** Makes Claude Code accessible to non-developers
2. **Agent System:** Extensible custom agent framework
3. **Usage Tracking:** Built-in analytics and cost monitoring
4. **MCP Integration:** Seamless Model Context Protocol server management
5. **Multi-Platform:** Native apps for all major desktop platforms
6. **Open Source:** AGPL-3.0 licensed, community-driven

---

## Future Roadmap

**Potential Enhancements:**
- Plugin system for third-party extensions
- Cloud sync for settings (optional, privacy-focused)
- Collaborative features for team workflows
- Enhanced agent orchestration
- Integrated terminal emulator
- Mobile companion apps

---

## Community & Support

**GitHub:** [getAsterisk/opcode](https://github.com/getAsterisk/opcode)
**Discord:** [Join Community](https://discord.com/invite/KYwhHVzUsY)
**Issues:** [Report Bugs](https://github.com/getAsterisk/opcode/issues)
**Twitter:** [@getAsterisk](https://x.com/getAsterisk)

---

## License & Attribution

**License:** AGPL-3.0
**Copyright:** © 2025 Asterisk. All rights reserved.
**Authors:** mufeedvh, 123vviekr, and contributors

**Important Note:**
This project is not affiliated with, endorsed by, or sponsored by Anthropic. Claude is a trademark of Anthropic, PBC. opcode is an independent developer project that provides a GUI interface for Claude Code.

---

**For AI-Assisted Development:**

This project-overview.md provides a comprehensive snapshot of the opcode project suitable for:
- Onboarding new developers
- Planning feature additions
- Understanding project scope and architecture
- Brownfield PRD development
- Quick reference for AI assistants

Refer to linked documentation for detailed technical information.
