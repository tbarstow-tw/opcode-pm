# Deployment Guide - opcode

**Project:** opcode - GUI app and Toolkit for Claude Code
**Type:** Desktop Application (Tauri 2)
**Generated:** 2025-12-03

## Deployment Overview

opcode uses **GitHub Actions** for automated CI/CD across multiple platforms. The release process builds native installers for macOS, Linux, and Windows.

## Deployment Platforms

### Supported Platforms

| Platform | Formats | Architecture |
|----------|---------|--------------|
| **macOS** | `.dmg`, `.app.tar.gz` | Universal (x86_64 + ARM64) |
| **Linux** | `.deb`, `.rpm`, `.AppImage` | x86_64 |
| **Windows** | `.msi`, `.exe` | x86_64 |

## CI/CD Pipeline

### GitHub Actions Workflows

**Location:** `.github/workflows/`

#### 1. Release Workflow (`release.yml`)

**Trigger:**
- Push tags matching `v*` (e.g., `v0.2.1`)
- Manual dispatch with version input

**Process:**
1. Triggers platform-specific build workflows
2. Collects artifacts from all builds
3. Creates GitHub release with binaries
4. Uploads installers as release assets

**Jobs:**
- `build-linux` - Builds Linux packages
- `build-macos` - Builds macOS universal binaries
- `create-release` - Creates GitHub release

#### 2. Build Workflows

**macOS Build (`build-macos.yml`):**
- Builds universal binary (Intel + Apple Silicon)
- Creates `.dmg` installer
- Code signs with Apple Developer certificate (if configured)
- Notarizes app for macOS Gatekeeper
- Uploads artifacts

**Linux Build (`build-linux.yml`):**
- Builds for x86_64 Linux
- Creates `.deb`, `.rpm`, and `.AppImage`
- Uploads artifacts

**Build Test (`build-test.yml`):**
- Runs on PRs and commits
- Validates builds without creating releases
- Ensures code compiles on all platforms

#### 3. Code Quality Workflows

**PR Check (`pr-check.yml`):**
- Runs linting and type checks
- Validates code formatting
- Ensures tests pass

**Claude Code Review (`claude-code-review.yml`):**
- AI-assisted code review
- Automated feedback on PRs

**Claude Integration (`claude.yml`):**
- Integrates with Claude Code for enhanced workflows

## Release Process

### Creating a New Release

#### Method 1: Git Tag (Recommended)

```bash
# 1. Update version in files
# - package.json
# - src-tauri/Cargo.toml
# - src-tauri/tauri.conf.json

# 2. Commit version bump
git add .
git commit -m "chore: bump version to v0.3.0"

# 3. Create and push tag
git tag v0.3.0
git push origin main --tags
```

This triggers the release workflow automatically.

#### Method 2: Manual Dispatch

1. Go to GitHub Actions → Release workflow
2. Click "Run workflow"
3. Enter version (e.g., `v0.3.0`)
4. Click "Run workflow"

### Release Workflow Steps

**1. Build Phase:**
- Parallel builds on macOS and Linux runners
- Compiles Rust backend with `--release` flag
- Builds frontend with Vite production mode
- Creates platform-specific installers

**2. Artifact Collection:**
- Downloads build artifacts from each job
- Organizes by platform and architecture
- Renames files with version suffix

**3. Release Creation:**
- Creates GitHub release with tag
- Uploads all installer files
- Generates changelog from commits
- Publishes release

### Build Output

**Release Artifacts:**
```
opcode_v0.3.0_macos_universal.dmg
opcode_v0.3.0_macos_universal.app.tar.gz
opcode_v0.3.0_linux_x86_64.deb
opcode_v0.3.0_linux_x86_64.AppImage
opcode-0.3.0.tar.gz (source code)
opcode-0.3.0.zip (source code)
```

## Build Configuration

### Tauri Bundle Settings

**File:** `src-tauri/tauri.conf.json`

```json
{
  "bundle": {
    "active": true,
    "targets": ["deb", "rpm", "appimage", "app", "dmg"],
    "icon": ["icons/..."],
    "category": "DeveloperTool",
    "shortDescription": "GUI app and Toolkit for Claude Code",
    "copyright": "© 2025 Asterisk. All rights reserved."
  }
}
```

### Platform-Specific Configuration

**macOS:**
```json
{
  "macOS": {
    "minimumSystemVersion": "10.15",
    "dmg": {
      "windowSize": { "width": 540, "height": 380 },
      "appPosition": { "x": 140, "y": 200 },
      "applicationFolderPosition": { "x": 400, "y": 200 }
    }
  }
}
```

**Linux:**
```json
{
  "linux": {
    "deb": {
      "depends": ["libwebkit2gtk-4.1-0", "libgtk-3-0"]
    },
    "appimage": {
      "bundleMediaFramework": true
    }
  }
}
```

## Code Signing

### macOS Code Signing

**Requirements:**
- Apple Developer Account
- Developer ID Application certificate
- App-specific password for notarization

**Setup:**
1. Add secrets to GitHub repository:
   - `APPLE_CERTIFICATE`
   - `APPLE_CERTIFICATE_PASSWORD`
   - `APPLE_ID`
   - `APPLE_ID_PASSWORD`
   - `APPLE_TEAM_ID`

2. Configure in `build-macos.yml`:
   - Certificate installed during workflow
   - Signing happens during Tauri build
   - Notarization step after build

**Manual Signing:**
```bash
# Sign the app
codesign --force --deep --sign "Developer ID Application: ..." opcode.app

# Create DMG
hdiutil create -volname "opcode" -srcfolder opcode.app -ov -format UDZO opcode.dmg

# Notarize
xcrun notarytool submit opcode.dmg --apple-id "..." --password "..." --team-id "..."
```

### Windows Code Signing

**Requirements:**
- Code signing certificate
- SignTool from Windows SDK

**Setup:**
- Add certificate to GitHub secrets
- Configure in build workflow

## Environment Variables

### Build-Time Variables

**GitHub Actions:**
```yaml
env:
  RUST_LOG: info
  CARGO_TERM_COLOR: always
  TAURI_PRIVATE_KEY: ${{ secrets.TAURI_PRIVATE_KEY }}
  TAURI_KEY_PASSWORD: ${{ secrets.TAURI_KEY_PASSWORD }}
```

**Local Builds:**
```bash
export RUST_LOG=debug
cargo build --release
```

## Deployment Checklist

### Pre-Release

- [ ] Update version numbers in all files
- [ ] Test build on all platforms locally
- [ ] Run full test suite
- [ ] Update CHANGELOG.md
- [ ] Review and close related issues
- [ ] Create release notes draft

### Release

- [ ] Create and push version tag
- [ ] Monitor GitHub Actions workflow
- [ ] Verify all platform builds succeed
- [ ] Test downloaded installers on each platform
- [ ] Publish release notes
- [ ] Announce release (Discord, social media)

### Post-Release

- [ ] Monitor for crash reports
- [ ] Respond to installation issues
- [ ] Update documentation if needed
- [ ] Plan next release cycle

## Rollback Procedure

If a release has critical issues:

1. **Mark release as pre-release:**
   - Edit GitHub release
   - Check "This is a pre-release"

2. **Prepare hotfix:**
   ```bash
   git checkout v0.3.0
   git checkout -b hotfix/v0.3.1
   # Fix the issue
   git commit -m "fix: critical issue"
   git tag v0.3.1
   git push origin hotfix/v0.3.1 --tags
   ```

3. **Create new release:**
   - Hotfix tag triggers new build
   - New version replaces problematic release

## Monitoring & Analytics

### Build Metrics

- **GitHub Actions:** View workflow run times and success rates
- **Download Stats:** Track release download counts
- **Crash Reports:** Monitor PostHog for crash analytics

### Performance Tracking

**Binary Size:**
- Monitor installer sizes across releases
- Track bundle optimization

**Build Times:**
- Optimize CI/CD pipeline
- Parallelize jobs where possible

## Troubleshooting Deployment

### Failed Builds

**Rust Compilation Error:**
```bash
# Check locally
cd src-tauri
cargo build --release
```

**Frontend Build Error:**
```bash
# Check locally
bun run build
```

**Platform-Specific Issues:**
- Review platform-specific logs in GitHub Actions
- Test on actual hardware for the platform
- Check dependency versions

### Release Upload Failures

**Large File Issues:**
- GitHub has size limits for release assets
- Split large files or use external hosting

**Duplicate Release:**
- Delete existing release
- Delete tag: `git push --delete origin v0.3.0`
- Re-create release

## Web Server Deployment (Optional)

### Building Web Server Binary

opcode includes an optional web server mode:

```bash
cd src-tauri
cargo build --bin opcode-web --release
```

**Binary Location:** `target/release/opcode-web`

### Deploying Web Server

**1. Prepare Server:**
```bash
# Copy binary
scp target/release/opcode-web user@server:/opt/opcode/

# Create systemd service
sudo nano /etc/systemd/system/opcode-web.service
```

**2. Systemd Service File:**
```ini
[Unit]
Description=opcode Web Server
After=network.target

[Service]
Type=simple
User=opcode
WorkingDirectory=/opt/opcode
ExecStart=/opt/opcode/opcode-web
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

**3. Start Service:**
```bash
sudo systemctl daemon-reload
sudo systemctl enable opcode-web
sudo systemctl start opcode-web
```

### Reverse Proxy (Nginx)

```nginx
server {
    listen 80;
    server_name opcode.example.com;

    location / {
        proxy_pass http://localhost:8080;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

## Security Considerations

### Secure Releases

- **Code Signing:** Required for macOS and recommended for Windows
- **Checksum Verification:** Provide SHA256 checksums for installers
- **HTTPS Only:** Serve downloads over HTTPS
- **Update Mechanism:** Use Tauri updater with signature verification

### Secrets Management

**GitHub Secrets:**
- Never commit secrets to repository
- Rotate certificates and keys regularly
- Use environment-specific secrets

**Runtime Secrets:**
- Store API keys securely
- Use system keychain where possible
- Validate all inputs

---

For development instructions, see `development-guide.md`.
For contribution guidelines, see `CONTRIBUTING.md`.
