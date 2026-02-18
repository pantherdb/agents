---
name: onboard
description: Explore a repository and generate a structured onboarding walkthrough. Use when a user asks to understand a codebase, explain a project, or onboard someone.
allowed-tools: Read, Grep, Glob, Bash
---

Explore this codebase and generate an onboarding walkthrough. $ARGUMENTS

# Agent: codebase-onboarding

## Purpose

Explore a repository and generate a structured onboarding walkthrough covering the tech stack, project structure, key files, architecture, and how to build/run/test.

## Instructions

### Step 1 — Detect Project Type

Scan for manifest files to identify the tech stack:

- `package.json`, `tsconfig.json`, `next.config.*`, `vite.config.*` → Node.js / TypeScript
- `pyproject.toml`, `requirements.txt`, `manage.py` → Python
- `Cargo.toml` → Rust
- `go.mod` → Go
- `Gemfile` → Ruby
- `composer.json` → PHP
- `pom.xml`, `build.gradle*` → Java / Kotlin
- `docker-compose.yml`, `Dockerfile` → Containerized
- `.github/workflows/*.yml` → CI/CD

Read the primary manifest to extract: name, description, dependencies, scripts.

Check for monorepo indicators: `workspaces`, `lerna.json`, `pnpm-workspace.yaml`, `turbo.json`.

### Step 2 — Map Directory Structure

List top-level structure with `ls -la`. Explore key directories. Look for: `src/`, `lib/`, `app/`, `api/`, `components/`, `pages/`, `models/`, `services/`, `utils/`, `tests/`, `config/`, `scripts/`, `docs/`, `migrations/`, `public/`.

Skip: node_modules, vendor, dist, build, .git, __pycache__, .next, venv.

### Step 3 — Find Entry Points

Use Grep for: `"main"` or `"start"` in package.json, `if __name__ == "__main__"`, `func main()`, `fn main()`, `public static void main`.

Look for: `index.ts`, `main.py`, `app.py`, `main.go`, `main.rs`, `App.tsx`.

Read each entry point.

### Step 4 — Read Config and Documentation

Read: README.md, CONTRIBUTING.md, .env.example, docker-compose.yml, Dockerfile, CI config, CLAUDE.md.

### Step 5 — Identify Architecture

Identify: pattern (MVC, microservices, monolith), data layer (DB, ORM, migrations), API layer (REST, GraphQL, gRPC), frontend (SSR, SPA), auth (JWT, sessions, OAuth), key integrations.

Use Grep for: `mongoose`, `prisma`, `sqlalchemy`, `express`, `fastapi`, `passport`, `jwt`.

### Step 6 — Generate Onboarding Document

Present in this format:

```
# Codebase Onboarding: [Project Name]

## Tech Stack
- **Language**: [e.g., TypeScript 5.x]
- **Framework**: [e.g., Next.js 14]
- **Database**: [e.g., PostgreSQL via Prisma]
- **Testing**: [e.g., Jest + React Testing Library]
- **CI/CD**: [e.g., GitHub Actions]

## Project Structure
[Annotated directory tree]

## Key Files
| File | Purpose |
|------|---------|

## How to Get Started
### Prerequisites
### Setup
### Common Commands

## Architecture Overview
[Pattern, data flow, key modules]

## Where to Find Things
- **API routes**: [path]
- **Database models**: [path]
- **Business logic**: [path]
- **Tests**: [path]
```

## Rules

- Do not modify any project files — this skill is read-only
- Reference existing docs rather than duplicating content
- Say "unknown" rather than guessing
- For monorepos, offer to deep-dive into a specific package
