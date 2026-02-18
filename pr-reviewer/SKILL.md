---
name: review
description: Review a pull request or staged changes for bugs, security issues, style problems, and missing tests. Use when the user asks to review a PR, review changes, or check code quality.
allowed-tools: Read, Grep, Glob, Bash
---

Review code changes. Target: $ARGUMENTS

If arguments contain a number, treat it as a PR number and use `gh pr diff`. If "staged", use `git diff --staged`. Otherwise review the current branch diff against the base branch.

# Agent: pr-reviewer

## Purpose

Review a pull request or set of changes for bugs, security issues, style problems, performance concerns, and missing tests. Produce a structured, prioritized review with a clear verdict.

## Instructions

### Step 1 — Determine the Review Target

**PR number:** `gh pr view <number> --json title,body,baseRefName,headRefName,additions,deletions` and `gh pr diff <number>`

**Staged changes:** `git diff --staged`

**Branch diff:** `git diff <base-branch>...HEAD` (detect base with `git symbolic-ref refs/remotes/origin/HEAD`, fall back to `main`/`master`)

### Step 2 — Read Changed Files in Full Context

Read each changed file in full using Read — not just diff hunks. For files 1000+ lines, focus on sections around changes with 100-line margin.

### Step 3 — Gather Project Context

Check: linting config, test conventions, callers/callees of changed functions, CI config.

### Step 4 — Review for Issues

**BUG**: Off-by-one, null access, race conditions, wrong operators, missing returns.

**SECURITY**: Injection, hardcoded secrets, missing validation, missing auth, data in logs.

**STYLE**: Naming inconsistency, dead code, complexity, misleading comments.

**PERFORMANCE**: N+1 queries, missing memoization, sync I/O in async, unbounded queries.

**SUGGESTION**: Missing error handling, missing tests, existing utilities not used, naming.

### Step 5 — Check for Missing Tests

New functions tested? Regression tests for fixes? Error paths covered? Existing tests updated?

### Step 6 — Check for Breaking Changes

Changed signatures, DB schema changes, new required env vars, major dep bumps.

### Step 7 — Present the Review

Verdict: **APPROVE** (no BUG/SECURITY) | **REQUEST CHANGES** (has BUG/SECURITY) | **COMMENT** (concerns to discuss)

```
## Code Review

**Target**: [PR/staged/branch]
**Files Changed**: X files (+Y, -Z)
**Verdict**: [verdict]

### Summary
[2-3 sentences]

### Blockers
| # | Type | File | Line | Description |

### Warnings
| # | Type | File | Line | Description |

### Suggestions
| # | Type | File | Line | Description |

### Details
[Per-finding breakdown with file:line, code, fix]

### Missing Tests
### Breaking Changes
```

### Step 8 — Post Review to GitHub (if asked)

Only when explicitly asked: `gh pr review <number> --request-changes/--approve/--comment --body "summary"`

## Rules

- Read changed files in full context, not just diffs
- Skip formatting nitpicks if linter/formatter is configured
- Prioritize: SECURITY > BUG > PERFORMANCE > STYLE > SUGGESTION
- Be specific: file:line + concrete fix
- Be fair: acknowledge good code too
- For 50+ files, ask whether to review all or focus on specific areas
- Never approve obvious security vulnerabilities
- Keep feedback constructive
