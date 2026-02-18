# refactor-advisor

Analyzes code for refactoring opportunities. Identifies code smells, classifies them by impact and effort, and presents a prioritized list of suggestions.

## How It Works

This agent reads your code and looks for: long functions, deep nesting, code duplication, unclear naming, tight coupling, dead code, missing abstractions, and more. Each finding gets an impact score (HIGH/MEDIUM/LOW) and an effort estimate, then they're sorted so you tackle quick wins first.

## Usage

### Option 1: Paste into Claude Code

Open Claude Code in your project, paste the contents of `CLAUDE.md`, then ask:

> Analyze src/services/auth.ts for refactoring opportunities

or:

> Review the entire src/api/ folder for code smells

or:

> Scan the whole repo for refactoring opportunities

### Option 2: Add as project instructions

```
cp refactor-advisor/CLAUDE.md /path/to/your-project/.claude/commands/refactor.md
```

Then: `/refactor src/services/auth.ts`

### Option 3: Reference directly

> Read the file at /path/to/agents/refactor-advisor/CLAUDE.md and follow those instructions for src/api/

## What You Get

A prioritized report organized as:

1. **Quick wins** — HIGH impact, LOW effort
2. **Worth doing** — HIGH impact, MEDIUM effort
3. **Nice to have** — MEDIUM impact, LOW effort
4. **Consider later** — everything else

Each suggestion includes what, why, how, and the tradeoff.

The agent can also implement specific refactors if you ask.

## Prerequisites

- **Claude Code** — Required
- No other dependencies
