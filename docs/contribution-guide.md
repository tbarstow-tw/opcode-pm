# Contribution Guide - opcode

**Project:** opcode - GUI app and Toolkit for Claude Code
**Generated:** 2025-12-03
**Source:** CONTRIBUTING.md

## Welcome Contributors

We welcome contributions to enhance opcode's capabilities and improve its performance. Before contributing, read through existing issues and pull requests to avoid duplicating efforts.

**Report Bugs:** [GitHub Issues](https://github.com/getAsterisk/opcode/issues)

## Contribution Workflow

### 1. Fork & Clone

```bash
# Fork the repository on GitHub
# Then clone your fork
git clone https://github.com/YOUR_USERNAME/opcode.git
cd opcode
```

### 2. Create Feature Branch

```bash
# Create branch for your feature or fix
git checkout -b feature/my-new-feature
# or
git checkout -b fix/bug-description
```

### 3. Make Changes

- Write code following project conventions
- Add/update tests as needed
- Update documentation
- Ensure code passes all checks

### 4. Test Your Changes

```bash
# Run type checking
bun run check

# Run Rust tests
cd src-tauri && cargo test

# Test manually in development mode
bun run tauri dev
```

### 5. Submit Pull Request

```bash
# Commit your changes
git add .
git commit -m "Feature: add custom agent timeout configuration"

# Push to your fork
git push origin feature/my-new-feature
```

Then create a Pull Request on GitHub.

## Pull Request Guidelines

### Title Format

Use these prefixes for PR titles:

| Prefix | Use Case | Example |
|--------|----------|---------|
| `Feature:` | New features | `Feature: added custom agent timeout configuration` |
| `Fix:` | Bug fixes | `Fix: resolved session list scrolling issue` |
| `Docs:` | Documentation changes | `Docs: updated installation instructions` |
| `Refactor:` | Code refactoring | `Refactor: simplified state management logic` |
| `Improve:` | Performance improvements | `Improve: optimized component rendering` |
| `Other:` | Other changes | `Other: updated dependencies` |

### Description Requirements

Provide a clear and detailed description including:

1. **Problem:** What issue are you solving?
2. **Approach:** How did you solve it?
3. **Side Effects:** Any potential limitations or side effects?
4. **Testing:** How have you tested your changes?

### Documentation

Update relevant documentation:
- README.md
- Code comments (JSDoc/Rust doc comments)
- API documentation
- User-facing guides

### Dependencies

If your changes require new dependencies:
- Document why the dependency is needed
- Add to `package.json` (frontend) or `Cargo.toml` (backend)
- Verify license compatibility
- Keep dependency count minimal

### Review Process

- PRs not meeting guidelines may be closed without merging
- Ensure you have the latest code before creating PR
- Sync your fork with upstream if needed

## Coding Standards

### Frontend (React/TypeScript)

**Language:**
- Use TypeScript for all new code
- Strict type checking enabled
- No `any` types without justification

**Components:**
- Functional components with hooks (no class components)
- Use React.FC or explicit typing
- Extract reusable logic into custom hooks

**Styling:**
- Use Tailwind CSS for all styling
- Follow existing class naming patterns
- Use `className` utility (`clsx`, `tailwind-merge`)

**Documentation:**
- Add JSDoc comments for exported functions and components
- Document props with TypeScript interfaces
- Include usage examples for complex components

**Example:**
```typescript
/**
 * Displays an agent execution with real-time status updates
 * @param agentId - The ID of the agent to execute
 * @param projectPath - Path to the project directory
 */
export const AgentExecution: FC<AgentExecutionProps> = ({
  agentId,
  projectPath
}) => {
  // Implementation
};
```

### Backend (Rust)

**Conventions:**
- Follow Rust standard conventions
- Use `snake_case` for functions and variables
- Use `PascalCase` for types and traits

**Formatting:**
```bash
# Format code before committing
cargo fmt
```

**Linting:**
```bash
# Run clippy for linting
cargo clippy
```

**Error Handling:**
- Handle all `Result` types explicitly
- Use `?` operator for propagation
- Provide meaningful error messages
- Use `anyhow` or `thiserror` for custom errors

**Documentation:**
```rust
/// Executes a Claude Code agent with the given parameters
///
/// # Arguments
/// * `agent_id` - The ID of the agent to execute
/// * `project_path` - Path to the project directory
/// * `task` - The task description for the agent
///
/// # Returns
/// Result containing the agent run ID or an error
///
/// # Errors
/// Returns an error if:
/// - Agent not found
/// - Invalid project path
/// - Claude binary not available
pub fn execute_agent(
    agent_id: i64,
    project_path: &str,
    task: &str,
) -> Result<i64, Error> {
    // Implementation
}
```

### Security Requirements

**Critical - Always Follow:**

1. **Input Validation:**
   - Validate all inputs from the frontend
   - Sanitize user-provided data
   - Use type-safe parsing

2. **Database Operations:**
   - Use prepared statements (rusqlite handles this)
   - Never interpolate user input into SQL
   - Validate data before insertion

3. **Logging:**
   - Never log sensitive data (tokens, passwords, API keys)
   - Use appropriate log levels
   - Sanitize logs in production

4. **Configuration:**
   - Use secure defaults
   - Validate configuration on load
   - Document security implications

**Example - Input Validation:**
```rust
fn validate_project_path(path: &str) -> Result<PathBuf, Error> {
    let path_buf = PathBuf::from(path);

    // Ensure path is absolute
    if !path_buf.is_absolute() {
        return Err(Error::InvalidPath("Path must be absolute".into()));
    }

    // Ensure path exists
    if !path_buf.exists() {
        return Err(Error::InvalidPath("Path does not exist".into()));
    }

    Ok(path_buf)
}
```

## Testing Requirements

### Frontend Testing

**Unit Tests:**
- Test utility functions
- Test custom hooks in isolation
- Use React Testing Library for components

**Integration Tests:**
- Test component interactions
- Verify state management flows
- Test API integration

**Manual Testing:**
- Test UI in development mode
- Verify all features work end-to-end
- Check responsive design

### Backend Testing

**Unit Tests:**
```bash
cd src-tauri
cargo test
```

**Run Specific Tests:**
```bash
cargo test test_agent_execution
```

**Integration Tests:**
- Test database operations
- Test IPC commands
- Test file system operations

### Test Coverage

- Add tests for new functionality
- Ensure existing tests still pass
- Aim for high coverage on critical paths

## Code Review Process

### What Reviewers Look For

1. **Functionality:** Does it work as intended?
2. **Code Quality:** Is it readable and maintainable?
3. **Tests:** Are there adequate tests?
4. **Documentation:** Is it well-documented?
5. **Security:** Are there any security concerns?
6. **Performance:** Are there any performance implications?

### Responding to Feedback

- Address all reviewer comments
- Ask questions if feedback is unclear
- Make requested changes promptly
- Mark conversations as resolved when addressed

## Git Commit Guidelines

### Commit Message Format

```
<type>: <subject>

<body>

<footer>
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation only
- `style`: Formatting, missing semicolons, etc.
- `refactor`: Code restructuring
- `perf`: Performance improvement
- `test`: Adding tests
- `chore`: Maintain tasks, dependencies

**Example:**
```
feat: add custom agent timeout configuration

Added ability to configure agent execution timeout in settings.
Timeout can be set per-agent or globally. Defaults to 30 minutes.

Closes #123
```

### Commit Best Practices

- Keep commits focused and atomic
- Write clear, descriptive commit messages
- Reference issues in commit messages
- Squash fixup commits before merging

## Development Best Practices

### Code Organization

**Frontend:**
- Group related components together
- Keep components small and focused
- Extract shared logic into hooks/utilities
- Use barrel exports (`index.ts`) for cleaner imports

**Backend:**
- Organize by feature/module
- Keep functions small and single-purpose
- Use modules to group related functionality
- Separate business logic from I/O

### Performance Considerations

**Frontend:**
- Use React.memo() judiciously
- Implement virtualization for long lists
- Lazy load components when appropriate
- Optimize re-renders with proper state design

**Backend:**
- Use async/await for I/O operations
- Avoid blocking the main thread
- Profile performance-critical code
- Use appropriate data structures

### Accessibility

- Use semantic HTML elements
- Add ARIA labels where needed
- Ensure keyboard navigation works
- Test with screen readers
- Maintain sufficient color contrast

## Getting Help

**Questions:**
- Check existing documentation first
- Ask in GitHub Discussions
- Join Discord community

**Bug Reports:**
- Use GitHub Issues
- Provide reproduction steps
- Include environment details
- Attach relevant logs/screenshots

**Feature Requests:**
- Open GitHub Issue with "Feature:" prefix
- Explain use case and benefits
- Discuss implementation approach

## Recognition

Contributors are recognized in:
- Release notes
- GitHub contributors list
- Project README (for significant contributions)

Thank you for contributing to opcode! 🚀

---

For development setup, see `development-guide.md`.
For deployment information, see `deployment-guide.md`.
