# Panther Agents

A collection of reusable Claude Code agents for the team. Each agent is a set of instructions (CLAUDE.md) that Claude Code follows to perform a specific task.

## Agent Catalog

| Agent | Description | How to Use |
|-------|-------------|------------|
| [secrets-scanner](secrets-scanner/) | Scans repos for accidentally committed API keys, passwords, and sensitive config files | Copy `CLAUDE.md` into your project or paste into Claude Code |
| [task-planner](task-planner/) | Manages persistent task plans that survive across Claude Code sessions | Add to project CLAUDE.md or use as a slash command |

## How to Use

Each agent folder contains:
- **CLAUDE.md** — The agent instructions. Copy this into your project or paste it into a Claude Code conversation.
- **README.md** — Docs explaining what the agent does, prerequisites, and examples.

### Quick Start

1. Browse the catalog above
2. Open the agent's folder
3. Read its README for details
4. Copy its `CLAUDE.md` content into your Claude Code session

## Adding a New Agent

1. Copy the `_template/` folder and rename it (use kebab-case)
2. Edit `CLAUDE.md` with your agent's instructions
3. Write a `README.md` with usage docs
4. Add a row to the catalog table above
