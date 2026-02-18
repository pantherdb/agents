# Task: Add 5 new Claude Code agents to the repository

**Status:** COMPLETE
**Branch:** main

## Goal

Create 5 new agents: codebase-onboarding, refactor-advisor, changelog-generator, dependency-auditor, pr-reviewer. Each gets a CLAUDE.md + README.md. Update root catalog.

## Context

- **Related files:** README.md (catalog table), _template/ (structure reference), secrets-scanner/ (pattern reference)
- **Triggered by:** User request to expand the agents collection

## Steps

### Phase 1: Create agents

- [x] codebase-onboarding/CLAUDE.md + README.md
- [x] refactor-advisor/CLAUDE.md + README.md
- [x] changelog-generator/CLAUDE.md + README.md
- [x] dependency-auditor/CLAUDE.md + README.md
- [x] pr-reviewer/CLAUDE.md + README.md

### Phase 2: Finalize

- [x] Update root README.md catalog table with all 5 new agents
- [ ] Commit locally

## Agent Designs

### 1. codebase-onboarding

Steps: detect project type → map directory structure → find entry points → read config files → identify architecture patterns → generate onboarding document (tech stack, structure, key files, build/run/test, architecture)

### 2. refactor-advisor

Steps: determine scope (file/folder/repo) → read and analyze for code smells (long functions, deep nesting, duplication, unclear naming, tight coupling, dead code) → classify by impact + effort → present prioritized report → implement if asked

### 3. changelog-generator

Steps: find latest git tag → read commits in range → detect conventional commits → categorize (Features, Bug Fixes, Breaking Changes, Improvements, Docs, Other) → parse merge commits for PR numbers → generate markdown changelog → offer to write CHANGELOG.md

### 4. dependency-auditor

Steps: detect package manager (npm/pip/poetry/cargo/go/bundler/composer) → read manifests + lock files → run security audit if tool available → check outdated → check deprecated → check unused (Grep for imports) → classify CRITICAL/HIGH/MEDIUM/LOW → help upgrade if asked

### 5. pr-reviewer

Steps: determine target (PR number via gh, staged changes, branch diff) → read full diff + changed files in context → gather project context (lint/test/CI config) → review for BUG/SECURITY/STYLE/PERFORMANCE/SUGGESTION → check missing tests → check breaking changes → present report with verdict (APPROVE/REQUEST CHANGES/COMMENT) → post to GitHub if asked

## Recovery Checkpoint

> ⚠ UPDATE THIS AFTER EVERY CHANGE

- **Last completed action:** Updated root README.md catalog with all 5 agents
- **Next immediate action:** Commit locally (user will review and push)
- **Uncommitted changes:** 10 new agent files + README.md edit + this plan file

## Failed Approaches

| What was tried | Why it failed | Date |
| -------------- | ------------- | ---- |
|                |               |      |

## Files Modified

| File | Action | Status |
| ---- | ------ | ------ |
| codebase-onboarding/CLAUDE.md | Create | Done |
| codebase-onboarding/README.md | Create | Done |
| refactor-advisor/CLAUDE.md | Create | Done |
| refactor-advisor/README.md | Create | Done |
| changelog-generator/CLAUDE.md | Create | Done |
| changelog-generator/README.md | Create | Done |
| dependency-auditor/CLAUDE.md | Create | Done |
| dependency-auditor/README.md | Create | Done |
| pr-reviewer/CLAUDE.md | Create | Done |
| pr-reviewer/README.md | Create | Done |
| README.md | Edit | Done |

## Blockers

- None currently

## Notes

- All agents follow the established pattern: Purpose, When to Use, Instructions (### Steps), Rules
- No supporting files needed — all logic in CLAUDE.md
- Agents detect tool availability and fall back gracefully (same pattern as secrets-scanner with gitleaks)
