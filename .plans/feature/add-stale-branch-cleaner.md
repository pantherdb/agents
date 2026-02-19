# Task: Add stale-branch-cleaner agent

**Status:** COMPLETE
**Branch:** main

## Goal

Create a stale-branch-cleaner agent that identifies old/merged git branches and helps the user safely clean them up. Must be extra cautious since branch deletion is destructive and can affect the whole team.

## Context

- **Related files:** stale-branch-cleaner/CLAUDE.md, stale-branch-cleaner/SKILL.md, stale-branch-cleaner/README.md, README.md
- **Triggered by:** User request

## Steps

### Phase 1: Create agent files

- [x] stale-branch-cleaner/CLAUDE.md
- [x] stale-branch-cleaner/SKILL.md
- [x] stale-branch-cleaner/README.md

### Phase 2: Update docs and commit

- [x] Add row to root README.md catalog table
- [x] Commit all changes

## Recovery Checkpoint

✅ TASK COMPLETE

## Files Modified

| File | Action | Status |
| ---- | ------ | ------ |
| stale-branch-cleaner/CLAUDE.md | Create | Done |
| stale-branch-cleaner/SKILL.md | Create | Done |
| stale-branch-cleaner/README.md | Create | Done |
| README.md | Edit | Done |

## Notes

- This agent is unique in the collection because it takes destructive actions (branch deletion)
- Safety model: report-first, protected branch list, local-before-remote, safe delete by default, per-branch confirmation for unmerged
- Uses `git branch -d` (safe) not `-D` (force) unless user explicitly approves per-branch
