# Panther Agents

A collection of Claude Code agents for the team. Each agent lives in its own folder.

## Structure

Every agent folder contains at minimum:
- `CLAUDE.md` — The agent instructions (this is what Claude Code reads)
- `README.md` — Human-readable docs: what it does, how to use it

Some agents include extra files (pattern lists, configs, etc.).

## How to Use an Agent

1. **Copy method**: Copy the agent's `CLAUDE.md` content into your project's `.claude/` commands or CLAUDE.md
2. **Reference method**: Point Claude Code at the agent folder and ask it to follow the instructions
3. **Direct method**: Open a conversation in Claude Code and paste the CLAUDE.md content

## How to Add a New Agent

1. Copy `_template/` to a new folder named after your agent
2. Edit the `CLAUDE.md` with your agent's instructions
3. Write a `README.md` explaining usage
4. Add the agent to the catalog table in the root `README.md`

## Conventions

- Agents must be self-contained — no cross-agent dependencies
- Each agent folder should work if copied out of this repo independently
- Agent names use kebab-case (e.g., `secrets-scanner`)
- CLAUDE.md files should include clear step-by-step instructions
- Agents should never output sensitive data (secrets, passwords, tokens) in plain text

## Git Commits

- Do NOT include the `Co-Authored-By` line in commit messages.

## Task Plans

- Always create and maintain task plans using the [.plans/template.md](.plans/template.md) system.
- On context resume, check `.plans/` for ACTIVE plans before doing anything else.
