# secrets-scanner

Scans Git repositories for accidentally committed API keys, passwords, private keys, tokens, and sensitive config files.

## How It Works

This is a Claude Code agent. It has two scanning modes:

1. **gitleaks mode** (preferred) — If [gitleaks](https://github.com/gitleaks/gitleaks) is installed, the agent runs it against the target repo and analyzes the structured JSON output. This catches 800+ secret types.

2. **Built-in pattern mode** (fallback) — If gitleaks isn't available, the agent uses Grep with the regex patterns in `patterns.txt` to scan for common secrets. Covers AWS, GitHub, Google, Stripe, Slack, database connection strings, private keys, and generic password/token assignments.

Both modes produce a severity-classified report (CRITICAL / HIGH / MEDIUM / LOW) with false positive detection.

## Usage

### Option 1: Paste into Claude Code

Open Claude Code in your target repository, then paste the contents of `CLAUDE.md` from this folder and ask:

> Scan this repo for secrets

### Option 2: Add as project instructions

Copy `CLAUDE.md` into your project's `.claude/` directory or append it to your project's `CLAUDE.md`:

```
cp secrets-scanner/CLAUDE.md /path/to/your-project/.claude/commands/scan-secrets.md
```

Then in Claude Code, run the command.

### Option 3: Reference directly

If you have this agents repo cloned, tell Claude Code:

> Read the file at /path/to/agents/secrets-scanner/CLAUDE.md and follow those instructions to scan this repo

## Prerequisites

- **Claude Code** — Required
- **gitleaks** (optional, recommended) — Install from [github.com/gitleaks/gitleaks](https://github.com/gitleaks/gitleaks#installing)

## What to Expect

The agent will:
1. Detect which scanner to use
2. Scan all files in the repo
3. Present a structured report with findings by severity
4. Identify likely false positives
5. Offer to help fix issues (replace with env vars, update .gitignore, etc.)

The agent **never displays actual secret values** — it refers to findings by file, line number, and pattern type only.

## Covered Secret Types

See `patterns.txt` for the full list. Key categories:
- AWS access keys and secret keys
- GitHub tokens (PAT, OAuth, fine-grained)
- Google API keys
- Stripe live keys
- Private keys (RSA, EC, DSA, OPENSSH, PGP)
- Database connection strings with passwords
- Slack, Twilio, SendGrid, Mailgun tokens
- Generic password/secret/token assignments
- JWT tokens
- npm and PyPI tokens
- Azure and Heroku credentials
