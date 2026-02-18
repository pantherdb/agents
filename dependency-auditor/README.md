# dependency-auditor

Audits project dependencies for known vulnerabilities, outdated packages, deprecated libraries, and unused imports. Produces a severity-classified report with upgrade recommendations.

## How It Works

This agent detects your package manager, then runs a multi-layered audit:

1. **Security audit** — Runs built-in audit commands (npm audit, pip-audit, cargo audit, etc.) for known CVEs
2. **Outdatedness check** — Identifies packages behind their latest version
3. **Unused dependency check** — Searches source code for actual imports of each declared dependency

Findings are classified as CRITICAL, HIGH, MEDIUM, or LOW.

## Supported Ecosystems

| Ecosystem | Manifest | Audit Tool | Outdated Check |
|-----------|----------|------------|----------------|
| Node.js (npm/yarn/pnpm) | package.json | npm/yarn/pnpm audit | npm/yarn/pnpm outdated |
| Python (pip/poetry/uv) | requirements.txt / pyproject.toml | pip-audit | pip list --outdated |
| Rust | Cargo.toml | cargo-audit | cargo-outdated |
| Go | go.mod | govulncheck | go list -m -u |
| Ruby | Gemfile | bundle-audit | bundle outdated |
| PHP | composer.json | composer audit | composer outdated |

## Usage

### Option 1: Paste into Claude Code

Open Claude Code in your project, paste `CLAUDE.md` contents, then ask:

> Audit the dependencies in this project

### Option 2: Add as project instructions

```
cp dependency-auditor/CLAUDE.md /path/to/your-project/.claude/commands/audit-deps.md
```

Then: `/audit-deps`

### Option 3: Reference directly

> Read the file at /path/to/agents/dependency-auditor/CLAUDE.md and follow those instructions

## Prerequisites

- **Claude Code** — Required
- **Package manager CLI** — The project's package manager should be installed
- **Audit tools** (optional, recommended):
  - Python: `pip install pip-audit`
  - Rust: `cargo install cargo-audit`
  - Go: `go install golang.org/x/vuln/cmd/govulncheck@latest`
  - Ruby: `gem install bundler-audit`
