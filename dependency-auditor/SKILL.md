---
name: audit-deps
description: Audit project dependencies for vulnerabilities, outdated packages, and unused imports. Use when the user asks to audit, check, or review dependencies.
allowed-tools: Read, Grep, Glob, Bash
---

Audit the dependencies in this project. $ARGUMENTS

# Agent: dependency-auditor

## Purpose

Audit project dependencies for known vulnerabilities, outdated packages, deprecated libraries, and unused imports.

## Instructions

### Step 1 — Detect Package Manager

Glob for: `package.json`, `requirements.txt`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `Gemfile`, `composer.json` (exclude node_modules, vendor, .venv, target, dist, build).

### Step 2 — Read Dependency Manifests

Read manifests and lock files. Build a list of direct dependencies with versions.

### Step 3 — Run Security Audit

| Manager | Command | Check |
|---------|---------|-------|
| npm | `npm audit --json` | `command -v npm` |
| yarn | `yarn audit --json` | `command -v yarn` |
| pnpm | `pnpm audit --json` | `command -v pnpm` |
| pip | `pip-audit --format=json` | `command -v pip-audit` |
| cargo | `cargo audit --json` | `command -v cargo-audit` |
| go | `govulncheck ./...` | `command -v govulncheck` |
| bundler | `bundle audit check` | `command -v bundle-audit` |
| composer | `composer audit --format=json` | `command -v composer` |

If not available, tell user and skip.

### Step 4 — Check for Outdated Packages

npm: `npm outdated --json`. pip: `pip list --outdated --format=json`. Similar for others.

### Step 5 — Check for Unused Dependencies

Grep source code for imports of each declared dependency. Flag packages with no imports as potentially unused. Note implicit packages (plugins, types, CLI tools).

### Step 6 — Classify and Report

- **CRITICAL**: Known CVE
- **HIGH**: 2+ major versions behind or deprecated
- **MEDIUM**: 1 major version behind or deprecated with replacement
- **LOW**: Minor/patch updates or potentially unused

```
## Dependency Audit Report

**Project**: <path>
**Package Manager**: <detected>
**Date**: <date>

### Summary
| Severity | Count |

### Vulnerabilities
| # | Severity | Package | Version | Issue | Fix Version |

### Outdated Packages
| # | Package | Current | Latest | Bump | Severity |

### Potentially Unused
| # | Package | Confidence | Note |

### Recommendations
1. [Prioritized upgrade list]
```

### Step 7 — Help Upgrade (if asked)

Only when explicitly asked. One package at a time. Warn about breaking changes. Run tests after.

## Rules

- Detect package manager first — never assume
- Don't run install/upgrade unless asked
- Note when audit tools are missing
- Flag implicit packages as "verify manually"
- Never run `npm audit fix --force` without consent
