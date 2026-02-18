# changelog-generator

Generates a human-readable changelog from git commit history. Reads commits since the last tag, categorizes them, and produces structured markdown release notes.

## How It Works

This agent reads your git history between two points (usually last tag to HEAD), detects whether you use conventional commits, categorizes each commit (Features, Bug Fixes, Breaking Changes, etc.), and generates a formatted changelog. It can also extract PR numbers and contributor names.

## Usage

### Option 1: Paste into Claude Code

Open Claude Code in your repository, paste the contents of `CLAUDE.md`, then ask:

> Generate a changelog since the last release

or:

> Create release notes for v1.2.0..v1.3.0

or:

> What changed since v2.0.0?

### Option 2: Add as project instructions

```
cp changelog-generator/CLAUDE.md /path/to/your-project/.claude/commands/changelog.md
```

Then: `/changelog`

### Option 3: Reference directly

> Read the file at /path/to/agents/changelog-generator/CLAUDE.md and follow those instructions

## What You Get

A structured markdown changelog with sections for:

- Breaking Changes
- Features
- Bug Fixes
- Improvements
- Documentation
- Maintenance

Each entry includes a description and PR link (when available). The agent can save the result to `CHANGELOG.md`.

## Prerequisites

- **Claude Code** — Required
- **git** — Required (for reading commit history and tags)
