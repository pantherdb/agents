---
name: changelog
description: Generate a changelog from git commit history with categorization. Use when the user asks for a changelog, release notes, or what changed since a tag/version.
allowed-tools: Read, Grep, Glob, Bash, Write
---

Generate a changelog. Range or version: $ARGUMENTS

If no range specified, use the latest git tag to HEAD.

# Agent: changelog-generator

## Purpose

Generate a human-readable changelog from git commit history. Reads commits since the last tag, categorizes them, and produces structured markdown.

## Instructions

### Step 1 — Determine Commit Range

```
git tag --sort=-v:refname
```

- User specified range → use it
- Tags exist → latest tag to HEAD
- No tags → ask user or use all commits

### Step 2 — Read Commits

```
git log <range> --oneline --no-merges
git log <range> --oneline --merges
git log <range> --format="%H %s" --no-merges
```

### Step 3 — Detect Commit Convention

Check for conventional commits (`feat:`, `fix:`, `chore:`, etc.). If >50% follow the pattern, use it.

### Step 4 — Categorize Commits

**Conventional commits:**
| Prefix | Category |
|--------|----------|
| `feat:` | Features |
| `fix:` | Bug Fixes |
| `perf:` | Performance |
| `docs:` | Documentation |
| `refactor:` | Improvements |
| `test:`, `ci:`, `build:`, `chore:` | Maintenance |
| `BREAKING CHANGE` or `!` | Breaking Changes |

**No convention — infer from keywords:**
| Keywords | Category |
|----------|----------|
| add, new, feature, implement | Features |
| fix, bug, patch, resolve | Bug Fixes |
| break, remove, drop | Breaking Changes |
| update, improve, refactor | Improvements |
| doc, readme, typo | Documentation |
| Everything else | Other |

Extract PR numbers from merge commits and messages.

### Step 5 — Handle Scope and Authors

Group by scope if conventional commits use them. Optionally list contributors:

```
git log <range> --format="%an" --no-merges | sort -u
```

### Step 6 — Generate the Changelog

```
## [version or Unreleased] - YYYY-MM-DD

### Breaking Changes
- Description ([#PR](link))

### Features
- Description ([#PR](link))

### Bug Fixes
- Description ([#PR](link))

### Improvements
### Documentation
### Maintenance
### Other
```

- Imperative mood ("Add" not "Added")
- Remove conventional commit prefixes
- Omit empty categories

### Step 7 — Offer Next Steps

Ask to: save to CHANGELOG.md, adjust entries, or add contributors.

## Rules

- Don't modify files unless asked to save
- If no commits in range, say so
- Preserve original meaning — don't embellish
- Use `[Unreleased]` if version unknown
- Breaking changes always first
- For 100+ commits, summarize first and ask for full detail
