# task-planner

Manages persistent task plans that survive across Claude Code sessions. Plans track progress, failed approaches, and recovery checkpoints so work can resume after context resets.

## How It Works

This agent creates and maintains markdown plan files in a `.plans/` directory in your project. Each plan tracks:

- Task status (ACTIVE / COMPLETE / BLOCKED)
- Step-by-step phases with checkboxes
- A **Recovery Checkpoint** — the most important part — that records exactly where work left off
- Failed approaches (so mistakes aren't repeated after context loss)
- Files modified and uncommitted changes

## Usage

### Option 1: Add to your project

Copy `CLAUDE.md` into your project's Claude Code commands:

```
cp task-planner/CLAUDE.md /path/to/your-project/.claude/commands/plan.md
```

Then in Claude Code: `/plan create a plan for adding user authentication`

### Option 2: Add to project CLAUDE.md

Add these lines to your project's `CLAUDE.md`:

```markdown
## Task Plans

- Always create and maintain task plans using the .plans/ system.
- On context resume, check `.plans/` for ACTIVE plans before doing anything else.
```

Then copy the agent's `CLAUDE.md` content into your project's instructions.

### Option 3: Paste directly

Paste the `CLAUDE.md` content into a Claude Code conversation and ask:

> Create a plan for [your task]

## Plan Categories

| Category | Use for |
|----------|---------|
| `bugfix/` | Bug fixes and error resolution |
| `feature/` | New features and enhancements |
| `refactor/` | Code restructuring and cleanup |
| `config/` | Configuration, CI/CD, environment changes |
| `docs/` | Documentation tasks |
| `testing/` | Test creation and test fixes |
| `misc/` | Anything else |

## Key Commands

- **"Create a plan for X"** — Creates a new plan file
- **"Resume"** — Finds ACTIVE plans and picks up where it left off
- **"What's the status?"** — Lists all plans and their states
- **"Mark plan as done"** — Completes the current plan

## Prerequisites

- **Claude Code** — Required
- No other dependencies
