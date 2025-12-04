# Project Documentation Index - opcode

**Project:** opcode - GUI app and Toolkit for Claude Code
**Type:** Desktop Application (Tauri 2)
**Architecture:** Monolithic Event-Driven Desktop Application
**Generated:** 2025-12-03
**Scan Level:** Exhaustive

---

## 🎯 Quick Start

**For AI-Assisted Development:**
- Start with [Project Overview](./project-overview.md) for a high-level understanding
- Review [Architecture](./architecture.md) for technical details
- Check [Component Inventory](./component-inventory.md) for UI components
- Refer to [Source Tree Analysis](./source-tree-analysis.md) for codebase structure

**For Developers:**
- Read [Development Guide](./development-guide.md) to set up your environment
- Follow [Contribution Guide](./contribution-guide.md) for contributing
- See [Deployment Guide](./deployment-guide.md) for release processes

---

## 📊 Project Overview

**Repository Type:** Monolith
**Primary Languages:** TypeScript (Frontend), Rust (Backend)
**Framework:** Tauri 2 + React 18
**Architecture Pattern:** Event-Driven Desktop Application with IPC

### Quick Reference

- **Tech Stack:** Tauri 2, React 18, TypeScript 5.6, Rust 2021
- **Entry Points:**
  - Frontend: `src/main.tsx`, `src/App.tsx`
  - Backend: `src-tauri/src/main.rs` (desktop), `src-tauri/src/web_main.rs` (web)
- **Build Tool:** Vite 6.0.3
- **State Management:** Zustand 5.0.6
- **UI Library:** Radix UI + Tailwind CSS 4.1.8
- **Database:** SQLite (rusqlite 0.32)

### Project Statistics

- **Components:** 88 files (19 UI primitives, 65+ feature components, 3 widgets)
- **State Stores:** 2 (sessionStore, agentStore)
- **CI/CD Workflows:** 7 GitHub Actions pipelines
- **Application Icons:** 50+ multi-platform icons
- **Documentation:** 8 generated files + 5 existing files

---

## 📁 Generated Documentation

### Core Documentation

**[Project Overview](./project-overview.md)**
Comprehensive project summary including purpose, technology stack, features, and quick reference. Ideal starting point for understanding the project scope.

**[Architecture](./architecture.md)**
Complete architecture documentation covering:
- Technology stack details
- Architecture patterns and design decisions
- Data architecture (SQLite schema, Zustand stores)
- API design (IPC communication, event system)
- Component overview and patterns
- Performance characteristics
- Security considerations

**[Source Tree Analysis](./source-tree-analysis.md)**
Annotated directory structure with:
- Complete file tree with descriptions
- Critical directories explained
- Entry points highlighted
- Integration points documented
- Configuration files reference

**[Component Inventory](./component-inventory.md)**
Comprehensive catalog of all 88 UI components:
- UI primitives (Radix UI wrappers)
- Feature components by category
- Specialized widgets
- Component design patterns
- Performance optimizations
- Accessibility features

### Development & Operations

**[Development Guide](./development-guide.md)**
Complete developer reference including:
- Prerequisites and installation
- Local development workflow
- Build commands and processes
- Testing strategies
- Code quality tools
- Common development tasks
- Debugging techniques
- Troubleshooting

**[Deployment Guide](./deployment-guide.md)**
CI/CD and release management:
- GitHub Actions workflows
- Multi-platform builds (macOS, Linux, Windows)
- Release process and versioning
- Code signing procedures
- Environment variables
- Deployment checklist
- Rollback procedures
- Web server deployment (optional mode)

**[Contribution Guide](./contribution-guide.md)**
Guidelines for contributors:
- Contribution workflow (fork, branch, PR)
- Coding standards (TypeScript, Rust)
- Security requirements
- Testing requirements
- Code review process
- Git commit guidelines

---

## 📖 Existing Documentation

### Project Documentation (Root)

**[README.md](../README.md)**
Main project documentation with features, installation instructions, usage guide, and getting started information.

**[CONTRIBUTING.md](../CONTRIBUTING.md)**
Contribution guidelines, PR requirements, coding standards, and testing expectations.

**[web_server.design.md](../web_server.design.md)**
Architecture design for the optional web server deployment mode (opcode-web binary).

### Module Documentation

**[cc_agents/README.md](../cc_agents/README.md)**
Guide for creating and managing custom Claude Code agents.

**[src/stores/README.md](../src/stores/README.md)**
Implementation notes for Zustand state management stores (sessionStore, agentStore).

---

## 🏗️ Architecture Summary

### System Architecture

```
┌─────────────────────────────────────────┐
│      Desktop Application (Tauri)        │
│  ┌────────────┐      ┌───────────────┐ │
│  │  Frontend  │◄────►│    Backend    │ │
│  │ React + TS │ IPC  │  Rust + Tauri │ │
│  └────────────┘      └───────────────┘ │
│       │                      │          │
│       ▼                      ▼          │
│  WebView Engine      System APIs       │
└─────────────────────────────────────────┘
```

### Technology Layers

| Layer | Technologies |
|-------|-------------|
| **Frontend** | React 18, TypeScript 5.6, Tailwind CSS 4, Radix UI, Zustand |
| **Backend** | Rust 2021, Tauri 2, rusqlite, Axum (web mode) |
| **Communication** | Tauri IPC (commands + events) |
| **Database** | SQLite (embedded, local storage) |
| **Build** | Vite 6, cargo, GitHub Actions |
| **Deployment** | Native installers (DMG, DEB, AppImage, MSI) |

### Key Directories

```
/
├── src/                    # React frontend
│   ├── components/         # 88 UI components
│   ├── stores/             # Zustand state stores
│   ├── hooks/              # React custom hooks
│   └── lib/                # API client, utilities
│
├── src-tauri/              # Rust backend
│   ├── src/                # Rust source code
│   ├── Cargo.toml          # Rust dependencies
│   └── tauri.conf.json     # Tauri configuration
│
├── .github/workflows/      # CI/CD pipelines
└── docs/                   # This documentation
```

---

## 🚀 Getting Started

### For AI-Assisted Development

**Planning New Features:**
1. Review [Architecture](./architecture.md) to understand system design
2. Check [Component Inventory](./component-inventory.md) for reusable components
3. Consult [Source Tree](./source-tree-analysis.md) to locate relevant code
4. Follow patterns in existing feature components

**Understanding the Codebase:**
1. Start with [Project Overview](./project-overview.md)
2. Deep dive into specific areas via linked documentation
3. Use [Source Tree Analysis](./source-tree-analysis.md) for navigation
4. Reference [Architecture](./architecture.md) for design decisions

### For Developers

**First-Time Setup:**
1. Follow [Development Guide](./development-guide.md) prerequisites section
2. Clone repository and install dependencies
3. Run `bun run tauri dev` to start development
4. Read [Contribution Guide](./contribution-guide.md) before making changes

**Common Tasks:**
- **Add Component:** See Component Inventory and Development Guide
- **Add Tauri Command:** Reference Architecture (API Design section)
- **Create Agent:** Check cc_agents/README.md
- **Build Release:** Follow Deployment Guide

---

## 🔍 Finding Information

### By Topic

| Topic | Document |
|-------|----------|
| **Project Purpose & Features** | [Project Overview](./project-overview.md) |
| **System Design & Patterns** | [Architecture](./architecture.md) |
| **UI Components** | [Component Inventory](./component-inventory.md) |
| **Directory Structure** | [Source Tree Analysis](./source-tree-analysis.md) |
| **Setup & Development** | [Development Guide](./development-guide.md) |
| **Builds & Releases** | [Deployment Guide](./deployment-guide.md) |
| **Contributing** | [Contribution Guide](./contribution-guide.md) |

### By Role

**Product Manager / Planner:**
- [Project Overview](./project-overview.md) - Features and scope
- [Architecture](./architecture.md) - Technical capabilities
- Existing docs for current functionality

**Developer:**
- [Development Guide](./development-guide.md) - Setup and workflow
- [Architecture](./architecture.md) - Technical design
- [Component Inventory](./component-inventory.md) - Reusable components
- [Source Tree](./source-tree-analysis.md) - Code navigation

**DevOps Engineer:**
- [Deployment Guide](./deployment-guide.md) - CI/CD and releases
- [Architecture](./architecture.md) - Infrastructure needs
- [Development Guide](./development-guide.md) - Build processes

**Designer / UX:**
- [Component Inventory](./component-inventory.md) - UI components
- [Project Overview](./project-overview.md) - Features and flows
- [Architecture](./architecture.md) - Component patterns

---

## 📈 Documentation Quality

**Scan Details:**
- **Scan Type:** Exhaustive (reads all source files)
- **Scan Date:** 2025-12-03
- **Files Scanned:** All TypeScript, Rust, and configuration files
- **Analysis Depth:** Complete codebase analysis with detailed documentation

**Coverage:**
- ✅ Technology stack fully documented
- ✅ All 88 components cataloged
- ✅ Architecture patterns explained
- ✅ Development workflow documented
- ✅ Deployment process automated
- ✅ Contribution guidelines established

**Documentation Files Generated:**
1. project-overview.md
2. architecture.md
3. source-tree-analysis.md
4. component-inventory.md
5. development-guide.md
6. deployment-guide.md
7. contribution-guide.md
8. index.md (this file)

---

## 🤝 Contributing

This project welcomes contributions! Before contributing:

1. Read [Contribution Guide](./contribution-guide.md)
2. Review [Development Guide](./development-guide.md) for setup
3. Check [CONTRIBUTING.md](../CONTRIBUTING.md) for PR guidelines
4. Ensure you understand the [Architecture](./architecture.md)

**Quick Links:**
- Report Issues: [GitHub Issues](https://github.com/getAsterisk/opcode/issues)
- Join Discussion: [Discord](https://discord.com/invite/KYwhHVzUsY)
- Follow Updates: [@getAsterisk](https://x.com/getAsterisk)

---

## 📝 Notes for Brownfield PRD Development

When creating PRDs for new features in this brownfield project:

**Context Documents to Reference:**
- [Architecture](./architecture.md) - Understand existing system design
- [Component Inventory](./component-inventory.md) - Identify reusable components
- [Source Tree Analysis](./source-tree-analysis.md) - Locate relevant code
- [Project Overview](./project-overview.md) - Understand project scope

**Key Considerations:**
- **State Management:** Use existing Zustand stores or create new ones following patterns in `src/stores/README.md`
- **UI Components:** Reuse UI primitives from `src/components/ui/` and follow Radix UI patterns
- **Backend Integration:** Follow Tauri IPC command pattern documented in Architecture
- **Testing:** Add tests as per Contribution Guide requirements
- **Database:** SQLite schema changes require migrations

**Integration Points:**
- IPC communication between frontend/backend
- Zustand store updates and subscriptions
- Component composition patterns
- Tauri event system for real-time updates

---

## 🔗 External Resources

- **Tauri Documentation:** https://tauri.app/
- **React Documentation:** https://react.dev/
- **Zustand Documentation:** https://zustand-demo.pmnd.rs/
- **Radix UI Documentation:** https://www.radix-ui.com/
- **Tailwind CSS Documentation:** https://tailwindcss.com/
- **Rust Book:** https://doc.rust-lang.org/book/

---

## 📞 Support & Community

**Questions?**
- Check this documentation first
- Search [GitHub Issues](https://github.com/getAsterisk/opcode/issues)
- Ask in [Discord Community](https://discord.com/invite/KYwhHVzUsY)

**Found an Issue?**
- Documentation issues: Open PR with corrections
- Code bugs: Create GitHub issue
- Feature requests: Open GitHub issue with "Feature:" prefix

---

**Last Updated:** 2025-12-03
**Documentation Version:** 1.0.0
**Scan Type:** Exhaustive
**Project Version:** 0.2.1

---

*This index is your primary entry point for all opcode project documentation. All documents are optimized for both human developers and AI-assisted development workflows.*
