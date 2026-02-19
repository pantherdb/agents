# Panther Agents

A collection of reusable Claude Code agents for the team. Each agent is available in two formats: **CLAUDE.md** (copy/paste) and **SKILL.md** (auto-registering slash commands).

## Agent Catalog

| Agent | Slash Command | Description |
|-------|--------------|-------------|
| [secrets-scanner](secrets-scanner/) | `/scan-secrets` | Scans repos for accidentally committed API keys, passwords, and sensitive config files |
| [task-planner](task-planner/) | `/plan` | Manages persistent task plans that survive across Claude Code sessions |
| [codebase-onboarding](codebase-onboarding/) | `/onboard` | Explores a repo and generates a structured onboarding walkthrough |
| [pr-reviewer](pr-reviewer/) | `/review` | Reviews PRs or staged changes for bugs, security, style, and missing tests |
| [dependency-auditor](dependency-auditor/) | `/audit-deps` | Audits dependencies for vulnerabilities, outdated packages, and unused imports |
| [refactor-advisor](refactor-advisor/) | `/refactor` | Analyzes code for refactoring opportunities, prioritized by impact and effort |
| [changelog-generator](changelog-generator/) | `/changelog` | Generates a changelog from git commits with categorization and PR links |
| [dead-code-finder](dead-code-finder/) | `/find-dead-code` | Finds unused functions, variables, imports, exports, types, and unreachable code |

## How to Use

Each agent folder contains:
- **CLAUDE.md** — Full agent instructions. Copy/paste into any Claude Code session.
- **SKILL.md** — Same instructions with YAML frontmatter. Drop into `.claude/skills/` for auto-registered slash commands.
- **README.md** — Human-readable docs.

### Method 1: Copy/Paste (works anywhere)

1. Open the agent's folder
2. Copy the `CLAUDE.md` content
3. Paste into your Claude Code session

### Method 2: Install as Skills (slash commands)

Copy an agent folder into your project's `.claude/skills/` directory:

```
cp -r secrets-scanner /path/to/your-project/.claude/skills/
```

Now `/scan-secrets` is available as a slash command in that project.

To install for all your projects, copy to `~/.claude/skills/` instead.

### Method 3: Install All Agents

```
cp -r secrets-scanner task-planner codebase-onboarding pr-reviewer dependency-auditor refactor-advisor changelog-generator dead-code-finder /path/to/your-project/.claude/skills/
```

### Slash Commands with Arguments

Skills support arguments via `$ARGUMENTS`:

```
/review 42                    → Reviews PR #42
/refactor src/api/auth.ts     → Analyzes that file
/changelog v1.0.0..v2.0.0    → Changelog for that range
/scan-secrets /path/to/repo   → Scans that repo
/plan add dark mode feature   → Creates a plan for that task
```

## Adding a New Agent

1. Copy the `_template/` folder and rename it (use kebab-case)
2. Edit `CLAUDE.md` with your agent's instructions (portable format)
3. Edit `SKILL.md` with the same instructions + YAML frontmatter (skill format)
4. Write a `README.md` with usage docs
5. Add a row to the catalog table above
