# dead-code-finder

Finds dead code in a codebase — unused functions, variables, imports, exports, types, and unreachable code paths. Reports findings with evidence and confidence levels so you can safely remove them.

## How It Works

This agent reads your code and searches the entire project (via Grep) for references to each symbol it finds. If a function, variable, type, or import has zero references outside its declaration, it's flagged as dead. Each finding gets a confidence level (HIGH/MEDIUM/LOW) based on the likelihood of dynamic usage or framework magic.

## Usage

### Option 1: Paste into Claude Code

Open Claude Code in your project, paste the contents of `CLAUDE.md`, then ask:

> Find dead code in src/services/

or:

> Scan src/utils.ts for unused exports

or:

> Find all dead code in the whole repo

### Option 2: Install as a skill

```
cp -r dead-code-finder /path/to/your-project/.claude/skills/
```

Then: `/find-dead-code src/services/`

### Option 3: Reference directly

> Read the file at /path/to/agents/dead-code-finder/CLAUDE.md and follow those instructions for src/

## What You Get

A structured report with:

1. **Summary table** — counts by category (unused exports, imports, locals, etc.)
2. **High-confidence findings** — safe to remove, zero references found
3. **Medium-confidence findings** — likely dead but could have dynamic usage
4. **Detailed evidence** — for each finding: file, lines, symbol, grep results, confidence
5. **Cleanup impact estimate** — total removable lines and fully-deletable files

The agent can also remove the dead code for you if you ask.

## Prerequisites

- **Claude Code** — Required
- No other dependencies
