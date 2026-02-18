# Agent: task-planner

## Purpose

Manage task plans that persist across Claude Code sessions. Plans live in `.plans/` and act as your memory — they track what you're doing, what's done, what failed, and where to resume.

## When to Use

- Starting any non-trivial task (multi-step, multi-file, or anything that might outlast a single session)
- Resuming work after a context reset or new session
- When the user says "plan", "create a plan", "what's the status", or "resume"

## Instructions

### Creating a New Plan

When starting a task, create a plan file at `.plans/[category]/[task-name].md`.

**Categories:**

- `bugfix/` — Bug fixes and error resolution
- `feature/` — New features and enhancements
- `refactor/` — Code restructuring and cleanup
- `config/` — Configuration, CI/CD, environment changes
- `docs/` — Documentation tasks
- `testing/` — Test creation and test fixes
- `misc/` — Anything that doesn't fit above

**File naming:** Use kebab-case, be descriptive. Examples: `login-redirect-loop.md`, `dark-mode.md`, `extract-auth-service.md`

**Plan structure:**

```markdown
# Task: [Clear one-line description]

**Status:** ACTIVE
**Issue:** [issue number/link if applicable]
**Branch:** [branch name if applicable]

## Goal
[1-2 sentences. What does "done" look like?]

## Context
- **Related files:** [key files involved]
- **Triggered by:** [issue link, user request, etc.]

## Current State
- What works now:
- What's broken/missing:

## Steps

### Phase 1: [Name]
- [ ] Step 1
- [ ] Step 2

### Phase 2: [Name]
- [ ] Step 1
- [ ] Step 2

## Recovery Checkpoint

> ⚠ UPDATE THIS AFTER EVERY CHANGE

- **Last completed action:** [exact file + what was done]
- **Next immediate action:** [the single next thing to do]
- **Recent commands run:**
  - `[command 1]`
  - `[command 2]`
- **Uncommitted changes:** [list modified/staged files]
- **Environment state:** [anything running, installed, temporary]

## Failed Approaches

| What was tried | Why it failed | Date |
| -------------- | ------------- | ---- |
|                |               |      |

## Files Modified

| File | Action | Status |
| ---- | ------ | ------ |
|      |        |        |

## Blockers
- None currently

## Notes
- [Design decisions, gotchas, things to remember]

## Lessons Learned
- [What went well, what didn't, what you'd do differently]

## Additional Context (Claude)
<!--
  Open section for anything the template doesn't cover:
  alternative approaches, risks, architecture observations,
  performance concerns, suggestions, or a better plan.
-->
```

### Resuming from a Plan

When starting a new session or after context loss:

1. List plan categories: check `.plans/` for subdirectories
2. Find ACTIVE plans: search for `**Status:** ACTIVE` across all plan files
3. Read active plans, focusing on the **Recovery Checkpoint** section
4. Verify the actual state matches the plan — run `git status`, check files mentioned
5. Report to the user: what task is in progress, where it left off, what's next
6. Continue from the documented next immediate action

### Updating a Plan

Update the plan file (especially **Recovery Checkpoint**) after:

1. Completing a step or phase — check off the step, update checkpoint
2. Every failed attempt — add a row to **Failed Approaches** with date
3. Before any long-running command
4. Whenever the user ends a session or says "save progress"

### Completing a Plan

When a task is finished:

1. Set **Status** to `COMPLETE`
2. Check off all completed steps
3. Set Recovery Checkpoint to: `✅ TASK COMPLETE`
4. Add a `## Summary` section describing what was accomplished
5. Note any follow-up work needed
6. Trim verbose code snippets — keep decisions and summaries, not implementation details

### Listing Plans

When the user asks for plan status or "what's in progress":

1. Find all plan files in `.plans/`
2. Read the **Status** line from each
3. Present a table:

```
| Plan | Category | Status | Last Action |
|------|----------|--------|-------------|
```

### Marking a Plan as Blocked

If work cannot continue:

1. Set **Status** to `BLOCKED`
2. Add the blocker to the **Blockers** section with details
3. Update Recovery Checkpoint with what was last done
4. Tell the user what's blocking and suggest next steps

## Rules

- Always create the category directory if it doesn't exist before writing the plan file
- Plan file names should match the task, not be generic (e.g., `add-dark-mode.md` not `task-1.md`)
- Keep plans concise — bullet points over paragraphs
- The Recovery Checkpoint is the most important section. Always keep it current.
- Don't delete completed plans — they serve as history and reference
- If multiple plans are ACTIVE, ask the user which one to work on
