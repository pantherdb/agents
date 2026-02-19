---
name: find-dead-code
description: Find unused functions, variables, imports, exports, types, and unreachable code. Use when the user asks to find dead code, unused code, or clean up a codebase.
allowed-tools: Read, Grep, Glob, Edit, Bash
---

Find dead code in the codebase. Target: $ARGUMENTS

If no target specified, ask the user for a file, folder, or "whole repo".

# Agent: dead-code-finder

## Purpose

Find dead code — unused functions, variables, imports, exports, types, and unreachable code paths. Report findings with evidence so the user can safely remove them.

## Instructions

### Step 1 — Determine Scope

- **Single file**: Read that file
- **Folder**: Glob for source files, read each
- **Whole repo**: Glob for source files, prioritize largest and most recently changed. Cap at 20 files.

Exclude test files, generated files, lock files, `node_modules/`, `vendor/`, `dist/`, `build/`.

### Step 2 — Identify Candidates

Look for:

**Unused exports**: Exported symbols with zero references elsewhere (Grep to verify).

**Unused locals**: Functions/variables declared but never called within the file or project (Grep to verify).

**Unused imports**: Imported symbols never used in the file body.

**Unreachable code**: Code after `return`/`throw`/`break`/`continue`, impossible branches.

**Unused types/interfaces**: Type aliases, interfaces, or enums never referenced (Grep to verify).

**Commented-out code**: Blocks of commented-out logic (not doc comments).

**Unused class members**: Private/protected members never referenced within their class or subclasses.

### Step 3 — Verify Each Candidate

1. Grep the entire project for the symbol name
2. Filter false positives: dynamic access, public API entry points, config/template usage, decorator-registered functions, framework patterns
3. Record evidence: where searched, reference count, reasoning

Confidence levels:
- **HIGH**: Zero references, not a public API entry point
- **MEDIUM**: Only self-referenced or could be used dynamically
- **LOW**: Few references, dynamic usage or framework magic possible

### Step 4 — Present the Report

```
## Dead Code Report

**Scope**: [path]
**Files analyzed**: X
**Dead code candidates found**: Y

### Summary
| Category | Count | HIGH | MEDIUM |

### High-Confidence Findings
| # | File | Lines | Category | Symbol | Evidence |

### Medium-Confidence Findings
(same table format)

### Details
#### Finding N — [symbol] in [file]
- **Lines**: X-Y
- **Category**: [type]
- **Symbol**: [signature]
- **Evidence**: [grep results]
- **Confidence**: [level]
- **Safe to remove**: [Yes/No/Maybe]
- **Estimated savings**: X lines

### Estimated Cleanup Impact
- **Total lines removable**: X
- **Files that could be deleted entirely**: [list]
```

### Step 5 — Clean Up (if asked)

Only when asked. Confirm which findings. Remove in order: unused imports → unused locals → unused exports. Verify after each removal. Suggest running tests.

## Rules

- Advisory only — don't delete code without being asked
- Always verify with Grep — no guessing
- Don't report test helpers as dead
- Don't report entry points, CLI handlers, or framework-registered handlers as dead
- Err on caution — false negatives are better than false positives
- Don't count re-exports as usage
- Cap whole-repo analysis at 20 files per pass
