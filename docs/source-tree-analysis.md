# Source Tree Analysis - opcode

**Project:** opcode
**Type:** Desktop Application (Tauri 2)
**Generated:** 2025-12-03
**Scan Level:** Exhaustive

## Project Structure Overview

opcode is a monolithic Tauri 2 desktop application with a React frontend and Rust backend. The project follows a clear separation between frontend (`src/`) and backend (`src-tauri/`) concerns.

## Annotated Directory Tree

```
opcode-pm/
├── src/                          # React Frontend Application
│   ├── components/               # UI Components (88 files)
│   │   ├── ui/                   # UI Primitives (19 Radix UI wrappers)
│   │   │   ├── button.tsx
│   │   │   ├── dialog.tsx
│   │   │   ├── dropdown-menu.tsx
│   │   │   ├── card.tsx
│   │   │   ├── tabs.tsx
│   │   │   └── ... (14 more primitives)
│   │   ├── widgets/              # Specialized Widgets
│   │   │   ├── BashWidget.tsx
│   │   │   ├── LSWidget.tsx
│   │   │   └── TodoWidget.tsx
│   │   ├── claude-code-session/  # Session Sub-components
│   │   │   ├── MessageList.tsx
│   │   │   ├── PromptQueue.tsx
│   │   │   └── SessionHeader.tsx
│   │   ├── Agents.tsx            # Agent Management
│   │   ├── AgentExecution.tsx
│   │   ├── AgentRunsList.tsx
│   │   ├── ClaudeCodeSession.tsx # Main Session Component
│   │   ├── SessionList.tsx
│   │   ├── ProjectList.tsx       # Project Browser
│   │   ├── MCPManager.tsx        # MCP Server Management
│   │   ├── UsageDashboard.tsx    # Analytics Dashboard
│   │   ├── Settings.tsx          # Application Settings
│   │   └── ... (~60 more components)
│   ├── stores/                   # State Management (Zustand)
│   │   ├── sessionStore.ts       # Projects & Sessions State
│   │   ├── agentStore.ts         # Agent Execution State
│   │   └── README.md             # Store Implementation Notes
│   ├── hooks/                    # Custom React Hooks
│   │   ├── useAnalytics.ts
│   │   ├── useApiCall.ts
│   │   ├── useDebounce.ts
│   │   └── ... (more hooks)
│   ├── lib/                      # Utility Libraries
│   │   ├── api.ts                # API Client Interface
│   │   └── ... (utilities)
│   ├── services/                 # Frontend Services
│   ├── types/                    # TypeScript Type Definitions
│   ├── contexts/                 # React Contexts
│   │   ├── TabContext.tsx
│   │   └── ThemeContext.tsx
│   ├── assets/                   # Frontend Assets
│   │   ├── fonts/inter/
│   │   │   └── Inter.ttf
│   │   ├── nfo/
│   │   │   ├── asterisk-logo.png
│   │   │   └── opcode-nfo.ogg
│   │   └── shimmer.css
│   ├── App.tsx                   # ⚡ Main Application Entry Point
│   └── main.tsx                  # ⚡ React Entry Point
│
├── src-tauri/                    # Rust Backend (Tauri)
│   ├── src/                      # Rust Source Code
│   │   ├── main.rs               # ⚡ Desktop App Entry Point
│   │   ├── web_main.rs           # ⚡ Web Server Entry Point (Axum)
│   │   ├── lib.rs                # Library Exports
│   │   └── ... (Rust modules)
│   ├── capabilities/             # Tauri Capabilities & Permissions
│   ├── icons/                    # Application Icons (50+ files)
│   │   ├── icon.png              # Main Icon
│   │   ├── icon.icns             # macOS Icon
│   │   ├── icon.ico              # Windows Icon
│   │   ├── ios/                  # iOS Icon Set
│   │   └── android/              # Android Icon Set
│   ├── tests/                    # Backend Tests
│   ├── Cargo.toml                # 📦 Rust Dependencies & Config
│   ├── tauri.conf.json           # 📦 Tauri Configuration
│   └── build.rs                  # Build Script
│
├── .github/                      # GitHub Configuration
│   └── workflows/                # CI/CD Pipelines (7 workflows)
│       ├── release.yml           # Automated Releases
│       ├── build-macos.yml       # macOS Builds
│       ├── build-linux.yml       # Linux Builds
│       ├── build-test.yml        # Build Testing
│       ├── pr-check.yml          # PR Validation
│       ├── claude-code-review.yml # AI Code Review
│       └── claude.yml            # Claude Integration
│
├── cc_agents/                    # Custom Claude Code Agents
│   └── README.md
│
├── scripts/                      # Build & Development Scripts
│   └── fetch-and-build.js        # Claude Binary Build Script
│
├── docs/                         # Project Documentation
│   ├── sprint-artifacts/         # Sprint Planning Artifacts
│   ├── bmm-workflow-status.yaml  # BMM Workflow Tracking
│   └── project-scan-report.json  # This Scan's State File
│
├── .bmad/                        # BMad Method Framework
│   ├── core/                     # Core Workflows & Tasks
│   ├── bmm/                      # BMad Method Module
│   ├── cis/                      # Creative Innovation Suite
│   └── ...
│
├── .claude/                      # Claude Code Configuration
│   └── commands/                 # Custom Slash Commands
│
├── package.json                  # 📦 Frontend Dependencies (npm/bun)
├── tsconfig.json                 # TypeScript Configuration
├── vite.config.ts                # Vite Build Configuration
├── tailwind.config.ts            # Tailwind CSS Configuration
├── index.html                    # HTML Entry Point
├── README.md                     # 📖 Project Documentation
├── CONTRIBUTING.md               # 📖 Contribution Guidelines
└── web_server.design.md          # 📖 Web Server Architecture

```

## Critical Directories Explained

### Frontend Architecture (`src/`)

**Components (`src/components/`)**
- **Purpose:** All React UI components organized by function
- **Structure:**
  - `ui/` - Reusable UI primitives (Radix UI wrappers)
  - `widgets/` - Specialized tool widgets
  - `claude-code-session/` - Session-specific components
  - Root level - Feature components (60+ files)
- **Design System:** Radix UI + Tailwind CSS
- **Patterns:** Composition, compound components, custom hooks

**State Management (`src/stores/`)**
- **Pattern:** Zustand with subscribeWithSelector middleware
- **Stores:**
  - `sessionStore` - Projects, sessions, outputs
  - `agentStore` - Agent runs, execution tracking, polling
- **Features:** Optimistic updates, real-time sync, caching

**Hooks (`src/hooks/`)**
- **Purpose:** Reusable React logic
- **Examples:** useAnalytics, useApiCall, useDebounce, useLoadingState

**Library (`src/lib/`)**
- **Key File:** `api.ts` - Frontend-to-backend API client
- **Purpose:** Shared utilities and helpers

### Backend Architecture (`src-tauri/`)

**Rust Source (`src-tauri/src/`)**
- **Entry Points:**
  - `main.rs` - Desktop application (Tauri)
  - `web_main.rs` - Web server mode (Axum)
  - `lib.rs` - Shared library exports
- **Architecture:** Event-driven IPC between frontend and Rust
- **Frameworks:** Tauri 2, Axum (web mode)

**Icons (`src-tauri/icons/`)**
- **Coverage:** macOS, Windows, Linux, iOS, Android
- **Formats:** PNG (various sizes), ICNS, ICO
- **Total:** 50+ icon files for all platforms

### CI/CD & DevOps (`.github/workflows/`)

**Build Pipelines:**
- Multi-platform builds (macOS, Linux, Windows)
- Automated releases with GitHub Actions
- PR validation and code review
- Claude Code integration for AI-assisted workflows

## Entry Points Summary

| Entry Point | Purpose | Technology |
|-------------|---------|------------|
| `src/main.tsx` | React app initialization | TypeScript |
| `src/App.tsx` | Main application component | React |
| `src-tauri/src/main.rs` | Desktop app backend | Rust/Tauri |
| `src-tauri/src/web_main.rs` | Web server mode | Rust/Axum |
| `index.html` | HTML entry point | HTML |

## Integration Points

**Frontend ↔ Backend Communication:**
- **Method:** Tauri IPC (Inter-Process Communication)
- **Transport:** JSON-serialized messages
- **Direction:** Bidirectional (frontend invokes Rust commands, Rust emits events to frontend)
- **API Surface:** Defined in `src/lib/api.ts` and Rust command handlers

**Optional Web Server Mode:**
- **Binary:** `opcode-web` (compiled from `web_main.rs`)
- **Framework:** Axum web framework
- **Purpose:** Standalone web server deployment option
- **Integration:** Shares core logic with desktop app

## Configuration Files

| File | Purpose |
|------|---------|
| `package.json` | Frontend dependencies (Bun/npm) |
| `Cargo.toml` | Rust dependencies and binaries |
| `tauri.conf.json` | Tauri app configuration |
| `tsconfig.json` | TypeScript compiler settings |
| `vite.config.ts` | Vite build configuration |
| `tailwind.config.ts` | Tailwind CSS customization |

## Build & Development

**Frontend Development:**
```bash
bun install          # Install dependencies
bun run dev          # Start Vite dev server
bun run build        # Build for production
```

**Desktop App Development:**
```bash
bun run tauri dev    # Start Tauri dev mode
bun run tauri build  # Build desktop app
```

**Web Server Mode:**
```bash
cd src-tauri
cargo build --bin opcode-web --release
```

## Testing Structure

- **Frontend Tests:** `*.test.ts`, `*.spec.ts`, `*.test.tsx`
- **Backend Tests:** `src-tauri/tests/`
- **CI Testing:** Automated via GitHub Actions (`build-test.yml`)

## Asset Management

**Frontend Assets:** `src/assets/`
- Fonts: Inter.ttf
- Images: Logo, branding
- Audio: NFO sound effects
- Styles: Custom CSS

**Application Icons:** `src-tauri/icons/`
- Multi-platform icon sets
- Adaptive icons for mobile
- Proper resolutions for all targets

---

**Notes:**
- ⚡ Indicates application entry points
- 📦 Indicates package/configuration files
- 📖 Indicates documentation files
- This is an exhaustive scan capturing the current state of the codebase
- For implementation details, refer to individual component/module documentation
