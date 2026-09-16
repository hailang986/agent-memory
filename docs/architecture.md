# Architecture

AgentMemory is a shared Markdown vault plus Git history.

It is a convention, not a runtime. Agents and humans read the same files. There is no hidden database and no required background service.

## Current truth and history

- Markdown files hold the current facts.
- Git holds previous versions of those files.
- `CURRENT.md` is updated in place. It is not a changelog.
- If a fact is no longer true, change the Markdown. Do not keep stale "current" sections for archaeology.

## Why a shared vault exists

Claude Code, Codex, and similar tools each have private context. That context is useful while a session is running, but it is not a shared project record.

The vault exists so that:

- different agents start from the same files
- a human can inspect the same facts without opening an agent transcript
- changes can be reviewed and reverted in Git

## Layers

```text
humans and agents
        │
        ▼
 Markdown files     <- current truth
        │
        ▼
      Git           <- history
```

Thin adapters sit beside the vault:

- `CLAUDE.md` imports `AGENTS.md` for Claude Code
- `.claude/settings.json` disables Claude Code auto-memory
- `.codex/config.toml` disables Codex private memories

Those files are tool wiring. They must not become a second copy of the knowledge base.

## File map

| Path | Layer |
| --- | --- |
| `AGENTS.md` | Governance |
| `INDEX.md` | Navigation |
| `USER.md` | Cross-project user preferences |
| `projects/<name>/PROJECT.md` | Stable project definition |
| `projects/<name>/CURRENT.md` | Single current state |
| `projects/<name>/DECISIONS.md` | Durable decisions |
| `projects/<name>/PITFALLS.md` | Recurring verified problems |
| `templates/` | Blank copies of the above |
| `examples/weather-cli/` | Fictional demonstration |

This repository uses `examples/` instead of a real `projects/` directory so it ships only fictional examples and no private work.

## Write path

1. Locate the project from `INDEX.md`.
2. Read `PROJECT.md` and `CURRENT.md` before changing anything.
3. Read `DECISIONS.md` or `PITFALLS.md` only when they are relevant.
4. Change the smallest file that actually owns the fact.
5. Leave unrelated files alone.

## What this version does not include

- vector search
- automatic summarization
- a required init script
- a claim that config files can block secrets
- a second knowledge base inside `CLAUDE.md` or `.codex/config.toml`
