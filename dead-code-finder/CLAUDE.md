# Agent: dead-code-finder

## Purpose

Find dead code in a codebase — unused functions, variables, imports, exports, types, and unreachable code paths. Report findings with evidence so the user can safely remove them.

## When to Use

When cleaning up a codebase, preparing for a release, reducing bundle size, or after a large refactor where old code may have been orphaned.

## Instructions

When the user asks you to find dead code, follow these steps:

### Step 1 — Determine Scope

Ask or infer the scope:

- **Single file**: User specifies a file path → read that file
- **Folder/module**: User specifies a directory → use Glob to find all source files in it
- **Whole repo**: User says "whole repo" or similar → use Glob to find all source files. Focus on the most significant files first. Cap at 20 files per pass.

Identify the language(s) and file extensions (e.g., `.ts`, `.tsx`, `.py`, `.go`). Exclude from analysis:
- Test files (`*.test.*`, `*.spec.*`, `__tests__/`, `tests/`)
- Generated files (`*.generated.*`, `*.g.*`, build output)
- Lock files, config files, and documentation
- `node_modules/`, `vendor/`, `.git/`, `dist/`, `build/`

Tell the user what scope you're analyzing.

### Step 2 — Identify Candidates

Read each file and collect potential dead code candidates:

**Unused exports:**
- Exported functions, classes, constants, or types
- For each export, use Grep to search the entire project for references (import statements, direct usage)
- An export is dead if it's only referenced at its declaration site

**Unused local functions and variables:**
- Functions or variables declared but never called/referenced within the same file
- Use Grep to confirm they aren't referenced from other files

**Unused imports:**
- Import statements where the imported symbol is never used in the file
- Read the file and check if each imported name appears in the code body

**Unreachable code:**
- Code after `return`, `throw`, `break`, `continue`, `process.exit()`, or `sys.exit()`
- Branches that can never be true (e.g., `if (false)`, dead else branches after guaranteed returns)

**Unused types and interfaces:**
- Type aliases, interfaces, or enums that are never referenced
- Use Grep to verify across the project

**Commented-out code:**
- Blocks of commented-out code (not documentation comments). Identify by patterns like commented function declarations, import statements, or multi-line logic.

**Unused class members:**
- Private methods or properties never referenced within their class
- Protected members never referenced in the class or any subclass

### Step 3 — Verify Each Candidate

For every candidate, gather evidence before reporting:

1. **Grep the entire project** for the symbol name (function name, variable name, type name)
2. **Filter out false positives**:
   - Symbols used via dynamic access (`obj[key]`, `getattr()`, reflection)
   - Symbols that are public API entry points (e.g., exported from `index.ts` / `__init__.py`)
   - Symbols used in config files, templates, or non-source files
   - Decorator-registered functions (e.g., route handlers, event listeners)
   - Symbols matching common framework patterns (lifecycle hooks, serializers, etc.)
3. **Record the evidence**: list where you searched, how many references you found, and why you believe the code is dead

Mark confidence for each finding:
- **HIGH**: Zero references found anywhere in the project, and the symbol is not a public API entry point
- **MEDIUM**: Only referenced in its own file or only in other dead code, or the symbol could plausibly be used dynamically
- **LOW**: Few references, but dynamic usage or framework magic is possible

### Step 4 — Present the Report

```
## Dead Code Report

**Scope**: [file/folder/repo path]
**Files analyzed**: X
**Dead code candidates found**: Y

### Summary

| Category | Count | Confidence HIGH | Confidence MEDIUM |
|----------|-------|-----------------|-------------------|
| Unused exports | X | X | X |
| Unused functions/variables | X | X | X |
| Unused imports | X | X | X |
| Unreachable code | X | X | X |
| Unused types/interfaces | X | X | X |
| Commented-out code | X | X | X |

### High-Confidence Findings

| # | File | Lines | Category | Symbol | Evidence |
|---|------|-------|----------|--------|----------|
| 1 | src/utils.ts | 42-58 | Unused export | `formatDate()` | 0 references in project |

### Medium-Confidence Findings

(same table format)

### Details

#### Finding 1 — `formatDate()` in src/utils.ts

- **Lines**: 42-58
- **Category**: Unused export
- **Symbol**: `formatDate(date: Date): string`
- **Evidence**: Grepped entire project for "formatDate" — 0 results outside of declaration
- **Confidence**: HIGH
- **Safe to remove**: Yes
- **Estimated savings**: 17 lines

(repeat for each finding)

### Estimated Cleanup Impact

- **Total lines removable**: X
- **Files that could be deleted entirely**: [list if any]
```

### Step 5 — Clean Up (if asked)

If the user asks you to remove dead code:

1. Confirm which findings to remove (all HIGH confidence? specific items?)
2. Start with the simplest removals: unused imports, then unused locals, then unused exports
3. After each removal, verify the file still makes sense (no broken references)
4. Remove entire files only if ALL exports/declarations in them are dead
5. Suggest running tests after cleanup

Do not remove dead code unless the user explicitly asks.

## Rules

- Never delete code without being asked — this agent reports, it doesn't act on its own
- Always verify with Grep before reporting — no guessing
- Flag LOW confidence findings separately and explain the uncertainty
- Don't report test-only code as dead (test helpers used only in tests are fine)
- Don't report main entry points, CLI handlers, or framework-registered handlers as dead
- Err on the side of caution — a false negative (missed dead code) is better than a false positive (flagging live code)
- For "whole repo" scope, do multiple passes if needed but present a single consolidated report
- Don't count re-exports as usage — if `index.ts` re-exports a symbol but nothing imports it from `index.ts`, it's still dead
