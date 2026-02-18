# pr-reviewer

Reviews pull requests, staged changes, or branch diffs for bugs, security issues, style problems, performance concerns, and missing tests. Produces a structured review with a clear verdict.

## How It Works

This agent takes a PR number, staged changes, or branch diff and performs a thorough review:

1. Fetches the diff and reads all changed files in full context
2. Gathers project conventions (linting, testing, CI)
3. Analyzes every change across: BUG, SECURITY, STYLE, PERFORMANCE, SUGGESTION
4. Checks for missing tests and breaking changes
5. Produces a prioritized report with Blockers, Warnings, and Suggestions
6. Renders a verdict: APPROVE, REQUEST CHANGES, or COMMENT

Can post the review to GitHub via `gh` CLI if asked.

## Usage

### Option 1: Paste into Claude Code

Open Claude Code in your repo, paste `CLAUDE.md` contents, then ask:

> Review PR #42

or:

> Review my staged changes

or:

> Review this branch against main

### Option 2: Add as project instructions

```
cp pr-reviewer/CLAUDE.md /path/to/your-project/.claude/commands/review.md
```

Then: `/review 42` or `/review staged`

### Option 3: Reference directly

> Read the file at /path/to/agents/pr-reviewer/CLAUDE.md and follow those instructions for PR #42

## Verdicts

| Verdict | Meaning |
|---------|---------|
| **APPROVE** | No bugs or security issues. Minor notes only. |
| **REQUEST CHANGES** | Has bugs, security issues, or serious performance problems. |
| **COMMENT** | No blockers but enough concerns to discuss first. |

## Prerequisites

- **Claude Code** — Required
- **git** — Required
- **gh CLI** (optional) — Needed for reviewing PRs by number and posting reviews. Install from [cli.github.com](https://cli.github.com)
