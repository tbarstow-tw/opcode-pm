# Architecture Documentation - opcode

**Project:** opcode - GUI app and Toolkit for Claude Code
**Type:** Desktop Application (Tauri 2)
**Architecture Pattern:** Event-Driven Desktop Application
**Generated:** 2025-12-03
**Scan Level:** Exhaustive

---

## Executive Summary

opcode is a sophisticated desktop application built with **Tauri 2** that provides a comprehensive GUI interface for **Claude Code**. The application bridges command-line AI-assisted development with an intuitive visual experience, enabling both technical and non-technical users to leverage Claude Code's capabilities effectively.

**Key Architectural Decisions:**
- **Tauri 2 Framework:** Native desktop performance with web technologies
- **React + TypeScript Frontend:** Modern, type-safe UI development
- **Rust Backend:** System-level performance and security
- **Event-Driven IPC:** Seamless frontend-backend communication
- **Zustand State Management:** Lightweight, performant global state
- **Multi-Platform Support:** macOS, Linux, Windows with native installers

**Project Scope:**
- Session and project management
- Custom agent creation and execution
- Real-time usage analytics and cost tracking
- MCP (Model Context Protocol) server management
- Timeline navigation with checkpoints
- Optional web server deployment mode

---

## Technology Stack

### Frontend Technologies

| Technology | Version | Purpose | Rationale |
|------------|---------|---------|-----------|
| **React** | 18.3.1 | UI Framework | Industry-standard, component-based architecture |
| **TypeScript** | 5.6.2 | Language | Type safety, better DX, reduced runtime errors |
| **Vite** | 6.0.3 | Build Tool | Fast HMR, optimized production builds |
| **Tailwind CSS** | 4.1.8 | Styling | Utility-first, rapid UI development, small bundle |
| **Radix UI** | Various | UI Primitives | Accessible, unstyled components for custom design |
| **Zustand** | 5.0.6 | State Management | Lightweight, minimal boilerplate, excellent TypeScript support |
| **TanStack Virtual** | 3.13.10 | List Virtualization | Performance for large lists |
| **Framer Motion** | 12.0.0-alpha.1 | Animations | Smooth, declarative animations |
| **React Hook Form** | 7.54.2 | Form Management | Performance, validation, less re-renders |
| **Zod** | 3.24.1 | Schema Validation | Type-safe runtime validation |
| **PostHog** | Latest | Analytics | Product analytics and usage tracking |

### Backend Technologies

| Technology | Version | Purpose | Rationale |
|------------|---------|---------|-----------|
| **Tauri** | 2.7.1 | Desktop Framework | Small binaries, native performance, security |
| **Rust** | 2021 | Language | Memory safety, performance, concurrency |
| **rusqlite** | 0.32 | Database | Embedded SQLite for local data storage |
| **Axum** | 0.8 | Web Server | Modern Rust web framework (optional web mode) |
| **Tokio** | 1.x | Async Runtime | Async/await for I/O operations |
| **reqwest** | 0.12 | HTTP Client | HTTP requests with native-tls |
| **serde** | 1.x | Serialization | JSON serialization/deserialization |
| **Clap** | 4.0 | CLI Parsing | Command-line argument parsing |

### Development Tools

| Tool | Purpose |
|------|---------|
| **Bun** | Package manager and JavaScript runtime |
| **cargo** | Rust package manager and build tool |
| **GitHub Actions** | CI/CD for automated builds and releases |

---

## Architecture Pattern

### Overall Pattern: Event-Driven Desktop Application

opcode follows an **event-driven architecture** with clear separation between frontend (React) and backend (Rust) layers, communicating via Tauri's IPC (Inter-Process Communication) mechanism.

```
┌─────────────────────────────────────────────────────────────┐
│                    DESKTOP APPLICATION                       │
│  ┌──────────────────────┐          ┌──────────────────────┐ │
│  │   Frontend Layer     │          │    Backend Layer      │ │
│  │   (React/TS)         │◄────────►│    (Rust/Tauri)      │ │
│  │                      │   IPC    │                       │ │
│  │  - Components        │          │  - Commands          │ │
│  │  - State (Zustand)   │          │  - Events            │ │
│  │  - Hooks             │          │  - Database          │ │
│  │  - Services          │          │  - File System       │ │
│  └──────────────────────┘          └──────────────────────┘ │
│           │                                    │             │
│           │                                    │             │
│           ▼                                    ▼             │
│  ┌──────────────────────┐          ┌──────────────────────┐ │
│  │   Tauri WebView      │          │   System APIs        │ │
│  │   (Browser Engine)   │          │   (OS Integration)   │ │
│  └──────────────────────┘          └──────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

### Component Hierarchy

**Frontend Component Architecture:**
```
App.tsx (Root)
├── Theme & Context Providers
├── TabManager (Multi-tab interface)
│   ├── ProjectList (Browse projects)
│   ├── SessionList (Browse sessions)
│   ├── ClaudeCodeSession (Main session view)
│   │   ├── SessionHeader
│   │   ├── MessageList (Virtual scrolling)
│   │   ├── PromptQueue
│   │   └── ExecutionControlBar
│   ├── Agents (Agent management)
│   │   ├── AgentExecution
│   │   ├── AgentRunsList
│   │   └── CreateAgent
│   ├── MCPManager (MCP servers)
│   ├── UsageDashboard (Analytics)
│   └── Settings
├── Widgets (Bash, LS, Todo)
└── UI Primitives (Radix UI wrappers)
```

### Backend Module Architecture

**Rust Backend Structure:**
```
src-tauri/src/
├── main.rs              # Desktop app entry point
├── web_main.rs          # Web server entry point
├── lib.rs               # Shared library exports
└── modules/
    ├── db/              # SQLite database operations
    ├── claude/          # Claude Code integration
    ├── agents/          # Agent execution logic
    ├── sessions/        # Session management
    ├── projects/        # Project operations
    └── mcp/             # MCP server management
```

---

## Data Architecture

### Local Database (SQLite)

**Database:** rusqlite (embedded SQLite)
**Location:** User's application data directory (`~/.claude` or platform equivalent)

**Key Tables:**

```sql
-- Projects (Claude Code projects)
CREATE TABLE projects (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    path TEXT NOT NULL UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    last_accessed TIMESTAMP
);

-- Sessions (Claude Code sessions)
CREATE TABLE sessions (
    id TEXT PRIMARY KEY,
    project_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP,
    status TEXT,
    first_message TEXT,
    FOREIGN KEY (project_id) REFERENCES projects(id)
);

-- Agents (Custom agents)
CREATE TABLE agents (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    description TEXT,
    system_prompt TEXT,
    model TEXT DEFAULT 'claude-sonnet-3-5',
    icon TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Agent Runs (Execution history)
CREATE TABLE agent_runs (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    agent_id INTEGER NOT NULL,
    project_path TEXT NOT NULL,
    task TEXT NOT NULL,
    status TEXT DEFAULT 'pending',
    started_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    completed_at TIMESTAMP,
    output TEXT,
    metrics_json TEXT,
    FOREIGN KEY (agent_id) REFERENCES agents(id)
);

-- Usage Tracking (Token and cost metrics)
CREATE TABLE usage_metrics (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    session_id TEXT,
    model TEXT NOT NULL,
    input_tokens INTEGER DEFAULT 0,
    output_tokens INTEGER DEFAULT 0,
    cost_usd REAL DEFAULT 0,
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- MCP Servers (Model Context Protocol servers)
CREATE TABLE mcp_servers (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL UNIQUE,
    command TEXT NOT NULL,
    args_json TEXT,
    env_json TEXT,
    enabled BOOLEAN DEFAULT 1
);
```

### State Management (Frontend)

**Pattern:** Zustand with subscribeWithSelector middleware

**Store Architecture:**

**sessionStore** (Projects & Sessions):
```typescript
interface SessionState {
  // Data
  projects: Project[];
  sessions: Record<string, Session[]>; // Keyed by projectId
  currentSession: Session | null;
  sessionOutputs: Record<string, string>;

  // UI State
  isLoading: boolean;
  error: string | null;

  // Actions
  fetchProjects(): Promise<void>;
  fetchSessions(projectId): Promise<void>;
  setCurrentSession(id): void;
  deleteSession(id): Promise<void>;

  // Real-time
  handleSessionUpdate(session): void;
  handleOutputUpdate(id, output): void;
}
```

**agentStore** (Agent Execution):
```typescript
interface AgentState {
  // Data
  agentRuns: AgentRunWithMetrics[];
  runningAgents: Set<string>;
  sessionOutputs: Record<string, string>;

  // UI State
  isLoading: boolean;
  lastFetchTime: number;

  // Actions
  fetchAgentRuns(forceRefresh?): Promise<void>;
  createAgentRun(data): Promise<AgentRunWithMetrics>;
  cancelAgentRun(id): Promise<void>;
  deleteAgentRun(id): Promise<void>;

  // Polling
  startPolling(interval?): void;
  stopPolling(): void;
  pollingInterval: NodeJS.Timeout | null;
}
```

**Design Patterns:**
- Optimistic updates for better UX
- Caching with time-based invalidation
- Polling for real-time updates when needed
- Selective subscriptions to prevent unnecessary re-renders

---

## API Design

### IPC Communication (Frontend ↔ Backend)

**Pattern:** Tauri Command-based IPC with type-safe interfaces

**Communication Flow:**
```
Frontend (TypeScript)  ──invoke()──►  Backend (Rust)
      │                                   │
      │  Request: { cmd, args }           │
      │                                   │
      │                       Process Command
      │                       ├─ Database Query
      │                       ├─ File System Op
      │                       └─ External Process
      │                                   │
      │  Response: Result<T, E> ◄─────────┤
      │                                   │
      ▼                                   ▼
Update State                        Emit Events (optional)
```

**Frontend API Client** (`src/lib/api.ts`):

```typescript
// Example API methods
export const api = {
  // Projects
  listProjects: () => invoke<Project[]>('list_projects'),
  getProject: (id: number) => invoke<Project>('get_project', { id }),

  // Sessions
  getProjectSessions: (projectId: string) =>
    invoke<Session[]>('get_project_sessions', { projectId }),
  getClaudeSessionOutput: (sessionId: string) =>
    invoke<string>('get_claude_session_output', { sessionId }),

  // Agents
  listAgents: () => invoke<Agent[]>('list_agents'),
  executeAgent: (agentId: number, projectPath: string, task: string, model?: string) =>
    invoke<number>('execute_agent', { agentId, projectPath, task, model }),
  killAgentSession: (runId: number) =>
    invoke<void>('kill_agent_session', { runId }),

  // MCP
  listMcpServers: () => invoke<McpServer[]>('list_mcp_servers'),
  addMcpServer: (server: McpServerConfig) =>
    invoke<number>('add_mcp_server', { server }),

  // Usage
  getUsageMetrics: (filters?: UsageFilters) =>
    invoke<UsageMetrics[]>('get_usage_metrics', { filters }),
};
```

**Backend Commands** (Rust):

```rust
// Tauri command handlers
#[tauri::command]
async fn list_projects(state: State<'_, AppState>) -> Result<Vec<Project>, String> {
    db::projects::list_all(&state.db)
        .map_err(|e| e.to_string())
}

#[tauri::command]
async fn execute_agent(
    agent_id: i64,
    project_path: String,
    task: String,
    model: Option<String>,
    state: State<'_, AppState>,
) -> Result<i64, String> {
    agents::execute(agent_id, &project_path, &task, model, &state)
        .await
        .map_err(|e| e.to_string())
}
```

### Event System

**Backend → Frontend Events:**

```rust
// Emit events from Rust
app.emit_all("agent-run-update", AgentRunUpdate {
    run_id: 123,
    status: "running",
    progress: 50,
});
```

```typescript
// Listen for events in React
import { listen } from '@tauri-apps/api/event';

useEffect(() => {
  const unlisten = listen<AgentRunUpdate>('agent-run-update', (event) => {
    agentStore.handleAgentRunUpdate(event.payload);
  });

  return () => { unlisten.then(f => f()); };
}, []);
```

---

## Component Overview

### UI Component Library

**Design System:** Custom components built on Radix UI primitives

**Component Categories:**

**1. UI Primitives (19 components):**
- `button`, `dialog`, `dropdown-menu`, `card`, `tabs`
- `input`, `textarea`, `select`, `switch`, `checkbox`
- `tooltip`, `popover`, `scroll-area`, `badge`, `label`
- `toast`, `pagination`, `radio-group`, `split-pane`

**2. Feature Components (60+ components):**

*Agent Management:*
- `Agents` - Main agent management interface
- `AgentExecution` - Execute agent with real-time progress
- `AgentRunsList` - List of agent execution history
- `AgentRunView` - Detailed view of agent run
- `CreateAgent` - Agent creation form
- `CCAgents` - Custom Claude Code agents browser

*Session Management:*
- `ClaudeCodeSession` - Main session interface
- `SessionList` - Browse and filter sessions
- `SessionHeader` - Session controls and metadata
- `MessageList` - Virtualized message display
- `PromptQueue` - Queued prompts management
- `ExecutionControlBar` - Session execution controls

*Project Management:*
- `ProjectList` - Browse Claude Code projects
- `ProjectSettings` - Project configuration
- `FilePicker` - File/directory picker dialog

*MCP Servers:*
- `MCPManager` - MCP server management
- `MCPAddServer` - Add new MCP server
- `MCPImportExport` - Import/export configs
- `MCPServerList` - List of configured servers

*Analytics & Usage:*
- `UsageDashboard` - Token usage and cost tracking
- `TokenCounter` - Real-time token counting
- `AnalyticsConsent` - Analytics opt-in/out

*Settings & Configuration:*
- `Settings` - Application settings
- `ProxySettings` - Proxy configuration
- `CheckpointSettings` - Checkpoint preferences
- `StorageTab` - Storage management
- `HooksEditor` - Custom hooks editor

**3. Specialized Widgets (3 components):**
- `BashWidget` - Bash command output viewer
- `LSWidget` - File listing widget
- `TodoWidget` - Todo list display

**4. Utility Components:**
- `CustomTitlebar` - Custom window titlebar (frameless)
- `ErrorBoundary` - React error boundary
- `AnalyticsErrorBoundary` - Analytics-aware error boundary
- `ImagePreview` - Image preview dialog
- `GitHubAgentBrowser` - Browse GitHub agents
- `WebviewPreview` - Embedded webview preview

### Component Patterns

**Composition:**
- Components composed from UI primitives
- Compound components for complex interactions
- Slot-based composition for flexibility

**Data Flow:**
- Props for component configuration
- Zustand stores for shared state
- Custom hooks for reusable logic
- Context for tree-wide data

**Performance:**
- Virtual scrolling for large lists (TanStack Virtual)
- React.memo() for expensive components
- Lazy loading for route components
- Code splitting via dynamic imports

---

## Source Tree

Refer to `source-tree-analysis.md` for complete annotated directory structure.

**Critical Paths:**

```
src/
├── components/       # All UI components (88 files)
├── stores/           # Zustand state management (2 stores)
├── hooks/            # Custom React hooks
├── lib/              # API client and utilities
├── types/            # TypeScript definitions
└── App.tsx           # Application entry point

src-tauri/
├── src/              # Rust backend source
├── Cargo.toml        # Rust dependencies
└── tauri.conf.json   # Tauri configuration
```

---

## Development Workflow

### Local Development

```bash
# Install dependencies
bun install

# Start development mode
bun run tauri dev
```

**Hot Reload:**
- Frontend: Vite HMR (instant)
- Backend: Rust recompiles on save (slower)

### Build Process

**Development Build:**
1. Vite bundles React app
2. Tauri creates debug binary
3. App launches in dev window

**Production Build:**
1. TypeScript compilation (`tsc`)
2. Vite production build (optimized, minified)
3. Rust release build (`cargo build --release`)
4. Platform-specific installer creation

**Build Optimization:**
- Code splitting (manual chunks in vite.config.ts)
- Tree shaking (automatic)
- Minification (Vite + Rust)
- Compression (installer-specific)

### Testing Strategy

**Frontend Testing:**
- Manual testing in dev mode
- Component testing (to be added)
- E2E testing (future consideration)

**Backend Testing:**
- Unit tests: `cargo test`
- Integration tests for database operations
- IPC command testing

**CI/CD Testing:**
- Build validation on all platforms
- Automated type checking
- Linting (cargo clippy)

---

## Deployment Architecture

### Multi-Platform Distribution

**Build Matrix:**

| Platform | Runner | Outputs |
|----------|--------|---------|
| macOS | `macos-latest` | `.dmg`, `.app.tar.gz` (Universal) |
| Linux | `ubuntu-latest` | `.deb`, `.rpm`, `.AppImage` |
| Windows | `windows-latest` | `.msi`, `.exe` (future) |

**CI/CD Pipeline:**
- GitHub Actions workflows (`.github/workflows/`)
- Parallel builds for each platform
- Automated releases on git tags (`v*`)
- Artifact collection and publishing

### Deployment Modes

**1. Desktop Application (Primary):**
- Native installers for each platform
- Auto-updates via Tauri updater
- Platform-specific integrations

**2. Web Server Mode (Optional):**
- Binary: `opcode-web`
- Framework: Axum web server
- Deployment: Standard web server hosting

**Architecture for Web Mode:**
```
┌─────────────┐
│   Browser   │
└──────┬──────┘
       │ HTTP/WebSocket
       ▼
┌─────────────────────┐
│  opcode-web Server  │
│  (Axum + Rust)      │
└──────┬──────────────┘
       │
       ▼
┌─────────────────────┐
│   SQLite Database   │
│   File System       │
└─────────────────────┘
```

### Security Considerations

**Tauri Security:**
- CSP (Content Security Policy) configured
- IPC commands explicitly allowed
- File system access scoped to user home
- No arbitrary code execution

**Data Security:**
- Local SQLite database
- No cloud storage of sensitive data
- API keys stored in system keychain (future)
- Secure defaults for all configurations

---

## Integration Points

### Claude Code Integration

**Integration Method:** Process execution
**Communication:** Filesystem + standard I/O

```
opcode → Spawns claude process
       → Monitors output
       → Captures logs
       → Tracks usage
       → Manages sessions
```

### MCP (Model Context Protocol)

**Integration:** MCP server management
**Servers:** User-configured external processes
**Protocol:** JSON-RPC over stdio

### PostHog Analytics

**Integration:** JavaScript SDK
**Data:** Usage metrics, feature tracking
**Privacy:** User consent required

---

## Performance Characteristics

### Desktop App Performance

**Startup Time:** < 2 seconds (typical)
**Memory Usage:** ~100-200 MB (varies with active sessions)
**Binary Size:** 10-15 MB (compressed installer)

**Optimization Techniques:**
- Lazy loading of components
- Virtual scrolling for lists
- Efficient state updates (Zustand)
- Rust performance for I/O operations

### Scalability

**Session Handling:**
- Supports hundreds of sessions
- Virtualized rendering
- On-demand data loading

**Agent Execution:**
- Concurrent agent runs
- Background process management
- Resource monitoring

---

## Future Architectural Considerations

**Potential Enhancements:**
- Plugin system for extensibility
- Cloud sync for settings (optional)
- Collaborative features
- Advanced agent orchestration
- Integrated terminal emulator

**Technical Debt:**
- Add comprehensive test suite
- Implement proper error boundaries
- Add logging framework
- Improve type coverage

---

## References

- Tauri Documentation: https://tauri.app/
- React Documentation: https://react.dev/
- Zustand: https://zustand-demo.pmnd.rs/
- Rust Book: https://doc.rust-lang.org/book/

For detailed guides, see:
- `development-guide.md` - Development setup and workflow
- `deployment-guide.md` - Deployment and release process
- `contribution-guide.md` - Contribution guidelines
- `source-tree-analysis.md` - Complete directory structure
