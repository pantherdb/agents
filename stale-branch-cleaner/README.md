# stale-branch-cleaner

Finds stale git branches — merged, abandoned, or long-inactive — and helps you safely clean them up. Always shows a report before touching anything.

## How It Works

The agent fetches the latest remote state, then classifies every branch into categories: merged & stale, merged & recent, unmerged & stale, unmerged & active, gone remotes, and protected. It presents a report with last commit dates, authors, and merge status. Deletion only happens when you explicitly ask, with extra confirmation required for remote branches (since those affect the whole team).

## Safety Model

This agent is intentionally cautious:

- **Protected branches are untouchable** — main, master, develop, staging, production, release/*, hotfix/*
- **Report-first** — never deletes unless you ask
- **Local before remote** — local deletions are confirmed first, remote deletions need separate approval
- **Safe delete by default** — uses `git branch -d` (refuses if not merged) rather than `git branch -D`
- **Per-branch confirmation for unmerged** — won't bulk-delete branches with unmerged work
- **Shows authors** — so you can check with teammates before deleting their branches

## Usage

### Option 1: Paste into Claude Code

Open Claude Code in your project, paste the contents of `CLAUDE.md`, then ask:

> Find stale branches in this repo

or:

> Which branches are safe to delete?

or:

> Clean up all merged branches older than a month

### Option 2: Install as a skill

```
cp -r stale-branch-cleaner /path/to/your-project/.claude/skills/
```

Then: `/clean-branches`

### Option 3: Reference directly

> Read the file at /path/to/agents/stale-branch-cleaner/CLAUDE.md and follow those instructions

## What You Get

A categorized report:

1. **Merged & stale** — fully merged, >2 weeks old, safe to delete
2. **Gone remotes** — local tracking branches whose remote was already deleted
3. **Unmerged & stale** — not merged, >3 months old, shows unique commit count so you know what would be lost
4. **Active branches** — kept, not suggested for deletion

When you ask to clean up, you get a summary of what was deleted, skipped, and any errors. Or you can ask for a copy-paste script to run yourself.

## Prerequisites

- **Claude Code** — Required
- **Git** — Must be in a git repository
- No other dependencies
