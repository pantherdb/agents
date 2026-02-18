# Task: Add SKILL.md format to all agents

**Status:** COMPLETE
**Branch:** main

## Goal

Add SKILL.md files alongside existing CLAUDE.md files in every agent. SKILL.md = YAML frontmatter + same instructions. This gives users two install methods: copy/paste (CLAUDE.md) or drop into `.claude/skills/` (SKILL.md).

## Context

- **Related files:** All 7 agent folders, root README.md, _template/
- **Triggered by:** Learning about Claude Code skills system — better distribution than manual copy/paste

## Steps

### Phase 1: Create SKILL.md for each agent

- [x] secrets-scanner/SKILL.md
- [x] task-planner/SKILL.md
- [x] codebase-onboarding/SKILL.md
- [x] pr-reviewer/SKILL.md
- [x] dependency-auditor/SKILL.md
- [x] refactor-advisor/SKILL.md
- [x] changelog-generator/SKILL.md

### Phase 2: Update docs

- [x] Update _template/ with SKILL.md template
- [x] Update root README.md with skills installation method
- [x] Update root CLAUDE.md conventions
- [ ] Commit

## Recovery Checkpoint

- **Last completed action:** All SKILL.md files created, docs updated
- **Next immediate action:** Commit
- **Uncommitted changes:** All SKILL.md files, updated README.md, updated CLAUDE.md

## Files Modified

| File | Action | Status |
| ---- | ------ | ------ |
| secrets-scanner/SKILL.md | Create | Done |
| task-planner/SKILL.md | Create | Done |
| codebase-onboarding/SKILL.md | Create | Done |
| pr-reviewer/SKILL.md | Create | Done |
| dependency-auditor/SKILL.md | Create | Done |
| refactor-advisor/SKILL.md | Create | Done |
| changelog-generator/SKILL.md | Create | Done |
| _template/SKILL.md | Create | Done |
| README.md | Edit | Done |
| CLAUDE.md | Edit | Done |

## Notes

- SKILL.md = YAML frontmatter + same content as CLAUDE.md
- Use $ARGUMENTS for dynamic input (e.g., /review 42, /refactor src/file.ts)
- Use allowed-tools to restrict what each skill can access
- Keep CLAUDE.md untouched — it's the portable format
