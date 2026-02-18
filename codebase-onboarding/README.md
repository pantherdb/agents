# codebase-onboarding

Explores a repository and generates a structured onboarding walkthrough for new team members. Covers tech stack, project structure, key files, architecture, and how to build/run/test.

## How It Works

This agent scans the repository for manifest files, config, entry points, and documentation. It reads key files to understand the architecture and generates a structured onboarding guide.

The agent is **read-only** — it never modifies project files.

## Usage

### Option 1: Paste into Claude Code

Open Claude Code in the target repository, paste the contents of `CLAUDE.md` from this folder, then ask:

> Explore this codebase and give me an onboarding walkthrough

### Option 2: Add as project instructions

```
cp codebase-onboarding/CLAUDE.md /path/to/your-project/.claude/commands/onboard.md
```

Then in Claude Code: `/onboard`

### Option 3: Reference directly

> Read the file at /path/to/agents/codebase-onboarding/CLAUDE.md and follow those instructions for this repo

## What You Get

A structured markdown document covering:

- Tech stack (language, framework, database, testing, CI/CD)
- Project structure (annotated directory tree)
- Key files table
- Setup instructions (prerequisites, install, run, test)
- Architecture overview
- Where to find things (routes, models, logic, tests, config)

## Prerequisites

- **Claude Code** — Required
- No other dependencies
