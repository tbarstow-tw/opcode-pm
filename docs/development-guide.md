# Development Guide - opcode

**Project:** opcode - GUI app and Toolkit for Claude Code
**Type:** Desktop Application (Tauri 2)
**Generated:** 2025-12-03

## Prerequisites

### Required Software

**Frontend Development:**
- **Bun** (latest) - Fast JavaScript runtime and package manager
  Install: `curl -fsSL https://bun.sh/install | bash`
- **Node.js** 18+ (alternative to Bun)
- **TypeScript** 5.6.2 (installed via dependencies)

**Backend Development:**
- **Rust** (2021 edition or later)
  Install: `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`
- **Tauri CLI** 2.7.1+
  Installed via npm: `bun install @tauri-apps/cli`

**Platform-Specific:**

*macOS:*
- Xcode Command Line Tools: `xcode-select --install`
- Minimum macOS 10.15+

*Linux:*
- WebKit2GTK 4.1+
- GTK 3.0+
- Development libraries: `libwebkit2gtk-4.1-dev libgtk-3-dev`

*Windows:*
- Microsoft Visual C++ Build Tools
- WebView2 Runtime (usually pre-installed on Windows 10/11)

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/getAsterisk/opcode.git
cd opcode
```

### 2. Install Dependencies

**Frontend:**
```bash
bun install
# or with npm
npm install
```

**Backend** (Rust dependencies are managed automatically):
```bash
cd src-tauri
cargo check
```

## Environment Setup

### Configuration Files

**Frontend Configuration:**
- `tsconfig.json` - TypeScript compiler options
- `vite.config.ts` - Vite bundler settings
- `tailwind.config.ts` - Tailwind CSS customization

**Backend Configuration:**
- `src-tauri/Cargo.toml` - Rust dependencies and binaries
- `src-tauri/tauri.conf.json` - Tauri application settings

### Environment Variables

Create a `.env` file in the project root if needed:

```env
# Optional: Override default settings
TAURI_DEV_HOST=localhost
```

## Local Development

### Development Server

**Start Frontend Dev Server:**
```bash
bun run dev
```
- Runs Vite dev server on `http://localhost:1420`
- Hot module replacement enabled
- Strict port mode (fails if port unavailable)

**Start Tauri Desktop App (Recommended):**
```bash
bun run tauri dev
```
- Launches desktop window with frontend
- Hot reload for both frontend and backend
- Rust compiler errors displayed in terminal
- Frontend served from dev server

### Build Commands

**Check TypeScript:**
```bash
bun run check
# Equivalent to: tsc --noEmit && cd src-tauri && cargo check
```

**Build Frontend Only:**
```bash
bun run build
# Equivalent to: tsc && vite build
```

**Build Desktop App:**
```bash
bun run tauri build
```
- Builds optimized production binary
- Creates platform-specific installers
- Output: `src-tauri/target/release/`

**Build DMG (macOS only):**
```bash
bun run build:dmg
```

### Development Workflow

**1. Frontend Development (React/TypeScript)**

```bash
# Start dev server
bun run dev

# In another terminal, run TypeScript checks
bun run check
```

**2. Backend Development (Rust/Tauri)**

```bash
# Run Tauri dev mode
bun run tauri dev

# In another terminal, run Rust checks
cd src-tauri
cargo clippy
cargo fmt --check
```

**3. Full Stack Development**

```bash
# Single command for full dev experience
bun run tauri dev
```

## Testing

### Frontend Tests

**Run Tests:**
```bash
# Test pattern: *.test.ts, *.spec.ts, *.test.tsx
# Currently: Manual testing workflow
# TODO: Add automated test suite
```

**Manual Testing:**
- Test UI components in dev mode
- Verify all features in desktop app
- Check responsiveness and accessibility

### Backend Tests

**Run Rust Tests:**
```bash
cd src-tauri
cargo test
```

**Run Specific Test:**
```bash
cargo test test_name
```

**Test Coverage:**
```bash
cargo test --verbose
```

## Code Quality Tools

### TypeScript/JavaScript

**Linting:**
```typescript
// Use ESLint (add configuration as needed)
// Currently: Manual code review
```

**Formatting:**
- Prettier (recommended, not configured)
- Follow existing code style

**Type Checking:**
```bash
tsc --noEmit
```

### Rust

**Formatting:**
```bash
cd src-tauri
cargo fmt
```

**Linting:**
```bash
cargo clippy
```

**All Checks:**
```bash
cargo fmt && cargo clippy && cargo test
```

## Common Development Tasks

### Adding a New Frontend Component

1. Create component file: `src/components/MyComponent.tsx`
2. Define TypeScript types: `src/types/my-component.ts`
3. Add to component index: `src/components/index.ts`
4. Import and use in parent component

```typescript
// src/components/MyComponent.tsx
import { FC } from 'react';

interface MyComponentProps {
  title: string;
}

export const MyComponent: FC<MyComponentProps> = ({ title }) => {
  return <div className="p-4">{title}</div>;
};
```

### Adding a New Zustand Store

1. Create store file: `src/stores/myStore.ts`
2. Define state interface and actions
3. Export hook for components

```typescript
// src/stores/myStore.ts
import { create } from 'zustand';

interface MyState {
  count: number;
  increment: () => void;
}

export const useMyStore = create<MyState>((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
}));
```

### Adding a New Tauri Command

1. Define command in `src-tauri/src/main.rs` or module
2. Add to command list in Tauri builder
3. Call from frontend via `invoke()`

```rust
// src-tauri/src/main.rs
#[tauri::command]
fn my_command(arg: String) -> Result<String, String> {
    Ok(format!("Received: {}", arg))
}

// In builder
fn main() {
    tauri::Builder::default()
        .invoke_handler(tauri::generate_handler![my_command])
        .run(tauri::generate_context!())
        .expect("error while running tauri application");
}
```

```typescript
// Frontend usage
import { invoke } from '@tauri-apps/api/core';

const result = await invoke<string>('my_command', { arg: 'Hello' });
```

### Building Claude Binaries

```bash
# Build all platforms
bun run build:executables

# Build current platform only
bun run build:executables:current

# Build specific platform
bun run build:executables:linux
bun run build:executables:macos
bun run build:executables:windows
```

## Debugging

### Frontend Debugging

**Browser DevTools:**
- Open DevTools in Tauri window: Right-click → Inspect Element
- Console logs appear in browser console
- React DevTools available

**Vite Dev Server:**
- View at `http://localhost:1420`
- Network requests in DevTools
- HMR error overlay

### Backend Debugging

**Rust Logging:**
```rust
use log::{info, warn, error, debug};

info!("Application started");
debug!("Debug information: {:?}", some_value);
```

**View Logs:**
```bash
# Run with debug output
RUST_LOG=debug bun run tauri dev
```

**GDB/LLDB Debugging:**
```bash
cd src-tauri
cargo build
rust-gdb target/debug/opcode
# or on macOS
rust-lldb target/debug/opcode
```

## Project Structure Notes

**Frontend Architecture:**
- `/src/components` - All React components
- `/src/stores` - Zustand state management
- `/src/hooks` - Custom React hooks
- `/src/lib` - Shared utilities and API client
- `/src/types` - TypeScript type definitions

**Backend Architecture:**
- `/src-tauri/src/main.rs` - Desktop app entry
- `/src-tauri/src/web_main.rs` - Web server mode
- `/src-tauri/src/lib.rs` - Shared library code

**Build Output:**
- `/dist` - Vite frontend build output
- `/src-tauri/target` - Rust build artifacts

## Performance Tips

**Frontend:**
- Use React.memo() for expensive components
- Implement virtualization for long lists (TanStack Virtual)
- Lazy load routes and heavy components
- Use Zustand selectors to prevent unnecessary re-renders

**Backend:**
- Use async/await for I/O operations
- Profile with `cargo flamegraph`
- Optimize database queries
- Use channels for concurrent operations

## Troubleshooting

**Issue: Tauri fails to build**
- Solution: Ensure Rust toolchain is up to date: `rustup update`
- Check platform-specific dependencies are installed

**Issue: Port 1420 already in use**
- Solution: Kill process using port or change port in `vite.config.ts`

**Issue: Frontend and backend out of sync**
- Solution: Restart dev server with `bun run tauri dev`

**Issue: TypeScript errors in IDE**
- Solution: Reload TypeScript server, check `tsconfig.json`

**Issue: Bun lock file conflicts**
- Solution: Delete `bun.lockb` and run `bun install`

## Additional Resources

- **Tauri Docs:** https://tauri.app/
- **React Docs:** https://react.dev/
- **Zustand Docs:** https://zustand-demo.pmnd.rs/
- **Vite Docs:** https://vitejs.dev/
- **Rust Book:** https://doc.rust-lang.org/book/

---

For contribution guidelines, see `CONTRIBUTING.md`.
For deployment instructions, see `deployment-guide.md`.
