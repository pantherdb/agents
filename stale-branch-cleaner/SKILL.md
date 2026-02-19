---
name: clean-branches
description: Find and clean up stale git branches — merged, abandoned, or inactive. Use when the user asks to clean up branches, find old branches, or do repo maintenance.
allowed-tools: Read, Grep, Glob, Bash
---

Find and report stale git branches. Target: $ARGUMENTS

If no target specified, analyze all branches in the current repo.

# Agent: stale-branch-cleaner

## Purpose

Identify stale git branches — merged, abandoned, or long-inactive — and help the user safely clean them up. Always reports before deleting. Never touches protected branches.

## Instructions

### Step 1 — Fetch and Inventory

```bash
git fetch --prune
```

Gather: local branches, remote branches, merged-into-main status, last commit date per branch.

Identify the primary branch (`main` or `master`) via `git symbolic-ref refs/remotes/origin/HEAD`.

### Step 2 — Classify Branches

**Protected (never touch):** `main`, `master`, `develop`, `development`, `staging`, `production`, `prod`, `release/*`, `hotfix/*`, current branch, anything the user protects.

**Merged & stale:** Merged into primary branch, last commit >2 weeks old. Safest to delete.

**Merged & recent:** Merged, last commit <2 weeks. Safe but might be referenced in recent PRs.

**Unmerged & stale:** Not merged, last commit >3 months old. Potentially abandoned — deletion loses commits.

**Unmerged & active:** Not merged, last commit <3 months. Likely active work — do not suggest deletion.

**Gone remotes:** Local tracking branches whose remote was deleted (`[gone]` in `git branch -vv`). Safe to clean up locally.

Record: name, last commit date, author, merge status, category.

### Step 3 — Present the Report

```
## Stale Branch Report

**Repository**: [name]
**Primary branch**: [main/master]
**Total branches**: X local, Y remote
**Protected**: Z (skipped)

### Summary
| Category | Local | Remote | Action |
|----------|-------|--------|--------|
| Merged & stale (>2 weeks) | X | X | Safe to delete |
| Merged & recent (<2 weeks) | X | X | Probably safe |
| Gone remotes | X | — | Safe to delete locally |
| Unmerged & stale (>3 months) | X | X | Review carefully |
| Unmerged & active (<3 months) | X | X | Keep |

### Merged & Stale — Safe to Delete
| # | Branch | Last Commit | Author | Age | Local | Remote |

### Gone Remotes — Safe to Delete Locally
| # | Branch | Tracked | Last Commit | Author |

### Unmerged & Stale — Review Carefully
| # | Branch | Last Commit | Author | Age | Unique Commits | Local | Remote |

(Show unique commit count: `git log main..branch --oneline | wc -l`)

### Active Branches (keeping)
| # | Branch | Last Commit | Author | Status |
```

### Step 4 — Clean Up (only when asked)

Do not delete unless explicitly asked. When asked:

1. **Confirm scope** — which categories to delete
2. **Local first** — `git branch -d` (safe). For unmerged: `git branch -D` only after per-branch confirmation.
3. **Remote separately** — list exact branches, warn "affects all team members", wait for confirmation, then `git push origin --delete`.
4. **Summary** — what was deleted, skipped, errored.

### Step 5 — Generate Commands (alternative)

If user prefers to run themselves, output a copy-paste script. Comment out risky commands by default.

## Rules

- Never delete protected branches (main, master, develop, staging, production, release/*, hotfix/*, current branch)
- Never delete without being asked — report only by default
- Never force-delete unmerged branches without per-branch confirmation
- Always separate local and remote deletion — remote needs its own confirmation
- Always fetch and prune first
- Show the author for each branch
- Err on the side of keeping
- Skip branches where merge status can't be verified
- Don't delete the currently checked-out branch
