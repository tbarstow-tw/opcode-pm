# Component Inventory - opcode

**Project:** opcode
**Type:** Desktop Application (Tauri 2)
**UI Framework:** React 18.3.1 + TypeScript 5.6.2
**Component Library:** Radix UI + Custom Components
**Generated:** 2025-12-03
**Total Components:** 88 files

---

## Component Organization

opcode's component architecture is organized into four main categories:

1. **UI Primitives** (19) - Radix UI wrappers
2. **Feature Components** (65+) - Application-specific components
3. **Specialized Widgets** (3) - Tool-specific widgets
4. **Sub-component Modules** (3) - Session-related sub-components

---

## UI Primitives (`src/components/ui/`)

**Purpose:** Accessible, reusable UI building blocks based on Radix UI

**Design System:** Custom-styled Radix UI primitives with Tailwind CSS

| Component | File | Purpose | Radix Primitive |
|-----------|------|---------|-----------------|
| **Badge** | `badge.tsx` | Status indicators, labels | N/A (custom) |
| **Button** | `button.tsx` | Interactive buttons with variants | N/A (custom) |
| **Card** | `card.tsx` | Content containers | N/A (custom) |
| **Dialog** | `dialog.tsx` | Modal dialogs | `@radix-ui/react-dialog` |
| **Dropdown Menu** | `dropdown-menu.tsx` | Context menus, dropdowns | `@radix-ui/react-dropdown-menu` |
| **Input** | `input.tsx` | Text input fields | N/A (custom) |
| **Label** | `label.tsx` | Form labels | `@radix-ui/react-label` |
| **Pagination** | `pagination.tsx` | Page navigation | N/A (custom) |
| **Popover** | `popover.tsx` | Floating content panels | `@radix-ui/react-popover` |
| **Radio Group** | `radio-group.tsx` | Radio button groups | `@radix-ui/react-radio-group` |
| **Scroll Area** | `scroll-area.tsx` | Scrollable containers | `@radix-ui/react-scroll-area` |
| **Select** | `select.tsx` | Dropdown selects | `@radix-ui/react-select` |
| **Split Pane** | `split-pane.tsx` | Resizable split panels | N/A (custom) |
| **Switch** | `switch.tsx` | Toggle switches | `@radix-ui/react-switch` |
| **Tabs** | `tabs.tsx` | Tabbed interfaces | `@radix-ui/react-tabs` |
| **Textarea** | `textarea.tsx` | Multi-line text input | N/A (custom) |
| **Toast** | `toast.tsx` | Notifications | `@radix-ui/react-toast` |
| **Tooltip (Modern)** | `tooltip-modern.tsx` | Modern tooltip variant | `@radix-ui/react-tooltip` |
| **Tooltip** | `tooltip.tsx` | Hover tooltips | `@radix-ui/react-tooltip` |

**Characteristics:**
- Fully accessible (WCAG 2.1)
- Keyboard navigation support
- Customizable via Tailwind CSS
- Type-safe with TypeScript
- Consistent API across all components

---

## Feature Components (`src/components/`)

### Agent Management (7 components)

Components for creating, managing, and executing custom AI agents.

| Component | File | Purpose |
|-----------|------|---------|
| **Agents** | `Agents.tsx` | Main agent management interface |
| **AgentExecution** | `AgentExecution.tsx` | Execute agent with live progress tracking |
| **AgentExecutionDemo** | `AgentExecutionDemo.tsx` | Demo/example for agent execution |
| **AgentRunOutputViewer** | `AgentRunOutputViewer.tsx` | View agent execution output |
| **AgentRunView** | `AgentRunView.tsx` | Detailed view of single agent run |
| **AgentRunsList** | `AgentRunsList.tsx` | List of all agent execution history |
| **CCAgents** | `CCAgents.tsx` | Claude Code agents browser |
| **CreateAgent** | `CreateAgent.tsx` | Agent creation form |
| **AgentsModal** | `AgentsModal.tsx` | Modal for agent selection/management |

**Key Features:**
- Real-time execution monitoring
- Background process management
- Execution history tracking
- Agent library browser (GitHub integration)

### Session Management (6 components)

Components for managing Claude Code sessions.

| Component | File | Purpose |
|-----------|------|---------|
| **ClaudeCodeSession** | `ClaudeCodeSession.tsx` | Main session interface (primary view) |
| **ClaudeCodeSession.refactored** | `ClaudeCodeSession.refactored.tsx` | Refactored version (WIP) |
| **SessionList** | `SessionList.tsx` | Browse and filter all sessions |
| **SessionList.optimized** | `SessionList.optimized.tsx` | Optimized variant with virtualization |
| **SessionOutputViewer** | `SessionOutputViewer.tsx` | View session output/logs |
| **RunningClaudeSessions** | `RunningClaudeSessions.tsx` | Monitor active sessions |

**Key Features:**
- Virtual scrolling for large session lists
- Search and filter capabilities
- Real-time session status
- Session output streaming

### Session Sub-components (`src/components/claude-code-session/`)

Modular components for the ClaudeCodeSession view.

| Component | File | Purpose |
|-----------|------|---------|
| **MessageList** | `MessageList.tsx` | Virtualized message display |
| **PromptQueue** | `PromptQueue.tsx` | Manage queued prompts |
| **SessionHeader** | `SessionHeader.tsx` | Session metadata and controls |

**Hooks:**
| Hook | File | Purpose |
|------|------|---------|
| **useCheckpoints** | `useCheckpoints.ts` | Checkpoint management logic |
| **useClaudeMessages** | `useClaudeMessages.ts` | Message handling and updates |

### Project Management (3 components)

Components for browsing and managing Claude Code projects.

| Component | File | Purpose |
|-----------|------|---------|
| **ProjectList** | `ProjectList.tsx` | Browse all Claude Code projects |
| **ProjectSettings** | `ProjectSettings.tsx` | Configure project-specific settings |
| **FilePicker** | `FilePicker.tsx` | File/directory picker dialog |
| **FilePicker.optimized** | `FilePicker.optimized.tsx` | Optimized picker with caching |

**Key Features:**
- Project discovery from `~/.claude/projects/`
- Quick search and filtering
- Project-specific configurations

### MCP Server Management (4 components)

Components for managing Model Context Protocol servers.

| Component | File | Purpose |
|-----------|------|---------|
| **MCPManager** | `MCPManager.tsx` | Main MCP server management UI |
| **MCPAddServer** | `MCPAddServer.tsx` | Add new MCP server form |
| **MCPImportExport** | `MCPImportExport.tsx` | Import/export server configurations |
| **MCPServerList** | `MCPServerList.tsx` | List of configured MCP servers |

**Key Features:**
- Server connection testing
- Configuration import from Claude Desktop
- Enable/disable servers
- Environment variable management

### Analytics & Usage (3 components)

Components for tracking usage metrics and costs.

| Component | File | Purpose |
|-----------|------|---------|
| **UsageDashboard** | `UsageDashboard.tsx` | Main analytics dashboard |
| **UsageDashboard.original** | `UsageDashboard.original.tsx` | Original implementation (backup) |
| **TokenCounter** | `TokenCounter.tsx` | Real-time token counting |
| **AnalyticsConsent** | `AnalyticsConsent.tsx` | Analytics opt-in/opt-out dialog |
| **AnalyticsErrorBoundary** | `AnalyticsErrorBoundary.tsx` | Error boundary with analytics |

**Key Features:**
- Cost tracking by model and time period
- Visual charts (Recharts)
- Token usage breakdown
- Data export capabilities

### Settings & Configuration (8 components)

Components for application and feature configuration.

| Component | File | Purpose |
|-----------|------|---------|
| **Settings** | `Settings.tsx` | Main settings interface |
| **CheckpointSettings** | `CheckpointSettings.tsx` | Checkpoint preferences |
| **ProxySettings** | `ProxySettings.tsx` | HTTP proxy configuration |
| **StorageTab** | `StorageTab.tsx` | Storage management and cleanup |
| **HooksEditor** | `HooksEditor.tsx` | Edit custom hooks |
| **SlashCommandsManager** | `SlashCommandsManager.tsx` | Manage slash commands |
| **SlashCommandPicker** | `SlashCommandPicker.tsx` | Slash command selector |
| **ClaudeVersionSelector** | `ClaudeVersionSelector.tsx` | Select Claude model version |

**Key Features:**
- Tabbed settings organization
- Real-time validation
- Import/export configurations
- Custom hooks and commands

### UI & Interaction Components (10 components)

General UI and interaction components.

| Component | File | Purpose |
|-----------|------|---------|
| **App** | `App.tsx` | Root application component |
| **App.cleaned** | `App.cleaned.tsx` | Cleaned/refactored variant |
| **CustomTitlebar** | `CustomTitlebar.tsx` | Custom window titlebar (frameless) |
| **Topbar** | `Topbar.tsx` | Application top bar |
| **TabManager** | `TabManager.tsx` | Multi-tab interface manager |
| **TabContent** | `TabContent.tsx` | Individual tab content wrapper |
| **ExecutionControlBar** | `ExecutionControlBar.tsx` | Session execution controls |
| **FloatingPromptInput** | `FloatingPromptInput.tsx` | Floating prompt input field |
| **TimelineNavigator** | `TimelineNavigator.tsx` | Session timeline navigation |
| **StreamMessage** | `StreamMessage.tsx` | Streaming message display |

**Key Features:**
- Multi-tab navigation
- Custom window chrome
- Timeline visualization
- Real-time streaming

### Content Editors & Viewers (5 components)

Components for editing and viewing content.

| Component | File | Purpose |
|-----------|------|---------|
| **ClaudeFileEditor** | `ClaudeFileEditor.tsx` | Integrated file editor |
| **MarkdownEditor** | `MarkdownEditor.tsx` | Markdown editing component |
| **ImagePreview** | `ImagePreview.tsx` | Image preview dialog |
| **WebviewPreview** | `WebviewPreview.tsx` | Embedded webview preview |
| **PreviewPromptDialog** | `PreviewPromptDialog.tsx` | Preview generated prompts |

**Key Features:**
- Syntax highlighting
- Markdown preview
- Image viewing
- HTML preview

### Utility & Helper Components (6 components)

Supporting components for various functions.

| Component | File | Purpose |
|-----------|------|---------|
| **ErrorBoundary** | `ErrorBoundary.tsx` | React error boundary |
| **ClaudeBinaryDialog** | `ClaudeBinaryDialog.tsx` | Claude binary detection/setup |
| **ClaudeMemoriesDropdown** | `ClaudeMemoriesDropdown.tsx` | Memory/context selection |
| **GitHubAgentBrowser** | `GitHubAgentBrowser.tsx` | Browse GitHub agent repository |
| **IconPicker** | `IconPicker.tsx` | Icon selection dialog |
| **StartupIntro** | `StartupIntro.tsx` | First-time user onboarding |
| **NFOCredits** | `NFOCredits.tsx` | Credits and attribution screen |

**Key Features:**
- Error handling and recovery
- Onboarding experience
- External resource browsing
- User guidance

---

## Specialized Widgets (`src/components/widgets/`)

**Purpose:** Tool-specific widgets for displaying command outputs

| Widget | File | Purpose | Data Source |
|--------|------|---------|-------------|
| **BashWidget** | `BashWidget.tsx` | Display Bash command output | Claude Code tool output |
| **LSWidget** | `LSWidget.tsx` | File listing widget | Directory listings |
| **TodoWidget** | `TodoWidget.tsx` | Todo list display | Claude Code todos |

**Characteristics:**
- Tailored for specific tool outputs
- Syntax highlighting
- Collapsible/expandable
- Copy to clipboard support

**Integration:**
- Used within ClaudeCodeSession messages
- Dynamically rendered based on tool type
- Supports streaming updates

---

## Tool Widgets (`src/components/widgets/`)

Specialized widgets for displaying tool-specific outputs.

| Widget | File | Purpose |
|--------|------|---------|
| **BashWidget** | `BashWidget.tsx` | Bash command output viewer |
| **LSWidget** | `LSWidget.tsx` | File listing viewer |
| **TodoWidget** | `TodoWidget.tsx` | Todo list display |

---

## Component Design Patterns

### Composition Pattern

Components are highly composable:

```typescript
<Dialog>
  <DialogTrigger asChild>
    <Button>Open</Button>
  </DialogTrigger>
  <DialogContent>
    <DialogHeader>
      <DialogTitle>Title</DialogTitle>
    </DialogHeader>
    <DialogBody>Content</DialogBody>
  </DialogContent>
</Dialog>
```

### Hook-Based Logic

Custom hooks extract reusable logic:

```typescript
// useClaudeMessages hook
const { messages, sendMessage, isStreaming } = useClaudeMessages(sessionId);

// useCheckpoints hook
const { checkpoints, createCheckpoint, restoreCheckpoint } = useCheckpoints(sessionId);
```

### State Management Integration

Components connect to Zustand stores:

```typescript
// From sessionStore
const { sessions, fetchSessions } = useSessionStore();

// From agentStore
const { agentRuns, createAgentRun } = useAgentStore();
```

### Variant-Based Styling

Components use class-variance-authority for variants:

```typescript
const buttonVariants = cva("base-classes", {
  variants: {
    variant: {
      default: "default-classes",
      outline: "outline-classes",
      ghost: "ghost-classes",
    },
    size: {
      sm: "small-classes",
      md: "medium-classes",
      lg: "large-classes",
    },
  },
});
```

---

## Component Performance Optimizations

### Virtualization

Components using TanStack Virtual for large lists:
- `SessionList` - Hundreds of sessions
- `MessageList` - Long conversation threads
- `AgentRunsList` - Execution history

### Memoization

Strategic use of `React.memo()`:
- Expensive rendering components
- Stable prop components
- Pure presentation components

### Code Splitting

Lazy loading for route components:
```typescript
const UsageDashboard = lazy(() => import('./components/UsageDashboard'));
```

### Selective Re-renders

Zustand selective subscriptions:
```typescript
const sessions = useSessionStore(state => state.sessions);
// Only re-renders when sessions change
```

---

## Accessibility Features

All components implement accessibility best practices:

- **Keyboard Navigation:** Full keyboard support
- **ARIA Labels:** Proper ARIA attributes
- **Focus Management:** Logical focus flow
- **Screen Reader Support:** Semantic HTML
- **Color Contrast:** WCAG 2.1 AA compliance

**Radix UI Benefits:**
- WAI-ARIA compliant out of the box
- Keyboard interactions included
- Focus management handled
- Screen reader tested

---

## Styling System

**Approach:** Tailwind CSS utility-first styling

**Utilities:**
- `clsx` - Conditional class names
- `tailwind-merge` - Merge conflicting classes
- `class-variance-authority` - Variant-based styling

**Theme:**
- Dark mode support (via ThemeContext)
- Custom color palette
- Consistent spacing scale
- Typography system (Inter font)

---

## Reusability Matrix

| Component Type | Reusability | Instances |
|----------------|-------------|-----------|
| UI Primitives | High | Used throughout app |
| Feature Components | Low-Medium | Specific to feature |
| Widgets | Medium | Multiple tool outputs |
| Sub-components | Low | Specific to parent |

**Highly Reusable:**
- All UI primitives (button, dialog, etc.)
- ErrorBoundary
- ImagePreview
- IconPicker

**Feature-Specific:**
- ClaudeCodeSession
- UsageDashboard
- MCPManager
- Agents

---

## Dependencies

### UI Component Dependencies

**Primary:**
- `@radix-ui/react-*` - 13 Radix UI packages
- `lucide-react` - Icon library
- `framer-motion` - Animations
- `react-hook-form` + `zod` - Forms

**Specialized:**
- `@uiw/react-md-editor` - Markdown editing
- `react-syntax-highlighter` - Code highlighting
- `recharts` - Data visualization
- `@tanstack/react-virtual` - List virtualization

---

## Future Component Development

**Planned Enhancements:**
- Additional UI primitives (data table, calendar)
- Enhanced accessibility testing
- Storybook documentation
- Component performance profiling
- Automated visual regression testing

**Potential New Components:**
- PluginManager (for extensibility)
- TerminalEmulator (integrated terminal)
- CollaborationPanel (team features)
- NotificationCenter (centralized notifications)

---

## Component Testing

**Current State:**
- Manual testing in development
- Integration testing via app usage

**Recommended:**
- Unit tests with React Testing Library
- Integration tests for complex workflows
- Visual regression tests (Percy, Chromatic)
- Accessibility audits (axe-core)

---

## Quick Reference

**Finding Components:**
- UI Primitives: `src/components/ui/`
- Feature Components: `src/components/`
- Widgets: `src/components/widgets/`
- Session Components: `src/components/claude-code-session/`

**Import Patterns:**
```typescript
// UI Primitives
import { Button } from '@/components/ui/button';

// Feature Components
import { Agents } from '@/components/Agents';

// Widgets
import { BashWidget } from '@/components/widgets/BashWidget';
```

---

**For AI-Assisted Development:**

This component inventory provides a complete catalog of all UI components in the opcode project. Use this reference to:
- Understand component organization
- Identify reusable components
- Plan new feature development
- Maintain consistency across the app
- Refactor and optimize components

Refer to individual component files for implementation details and prop interfaces.
