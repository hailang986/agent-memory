# AgentMemory

AgentMemory is a lightweight shared memory layer for AI coding agents.

It is a small set of Markdown files that different agents can read and update. The files hold current project facts. Git holds history.

**Markdown = current truth**
**Git = history**

This repository is a public, generalized implementation derived from a private personal workflow. It does not contain the original private vault or its Git history.

## Problem

Coding agents such as Claude Code and Codex each keep their own short-lived context. That context is not a shared, inspectable, version-controlled record of a project.

Without an external fact layer:

- two agents can remember different "current" states
- agent-private memory can silently diverge from files on disk
- users cannot easily audit, revert, or hand off long-term project knowledge

AgentMemory keeps the durable facts in ordinary Markdown, so humans and agents share the same source of truth.

## Architecture

The design is intentionally small.

- Markdown files are the only long-term source of truth.
- Git stores historical versions of those files.
- An agent's built-in memory is not a second knowledge base.
- Thin tool adapters exist only so Claude Code and Codex do not grow a competing private memory store.

There is no database, vector index, or hidden runtime state in this version.

## Directory structure

```text
.
├── README.md
├── README.zh-CN.md
├── LICENSE
├── AGENTS.md
├── INDEX.md
├── USER.md
├── CLAUDE.md
├── .gitignore
├── .claude/
│   └── settings.json
├── .codex/
│   └── config.toml
├── templates/
│   ├── USER.md
│   └── project/
│       ├── PROJECT.md
│       ├── CURRENT.md
│       ├── DECISIONS.md
│       └── PITFALLS.md
├── examples/
│   └── weather-cli/
│       ├── PROJECT.md
│       ├── CURRENT.md
│       ├── DECISIONS.md
│       └── PITFALLS.md
└── docs/
    ├── architecture.md
    ├── privacy.md
    ├── codex.md
    └── claude-code.md
```

In a working vault, real projects live under `projects/<project>/` using the same four files as the example.

## Quick start

1. Copy this folder, or copy `AGENTS.md`, `INDEX.md`, `USER.md`, and the project templates into a new directory.
2. Fill `USER.md` with non-sensitive long-term preferences only.
3. Create `projects/<project>/` from `templates/project/`.
4. Point Claude Code or Codex at that directory.
5. Treat the Markdown files as current truth. Use Git for history once you are ready to version the vault.

Do not store credentials, identity documents, or private evidence in these files.

## File responsibilities

| File | Role |
| --- | --- |
| `AGENTS.md` | Agent behavior and vault governance. |
| `INDEX.md` | Navigation only. |
| `USER.md` | Confirmed, stable, non-sensitive user preferences. |
| `PROJECT.md` | Stable project definition: goal, scope, principles, constraints, architecture. |
| `CURRENT.md` | The single current state: phase, completed, in progress, next steps, blockers. |
| `DECISIONS.md` | Important decisions that will keep mattering. |
| `PITFALLS.md` | Real, verified problems that are likely to recur. |

`CURRENT.md` is overwritten in place as the project moves. Old states belong in Git, not in a growing status file.

## Codex integration

Codex should treat `AGENTS.md` as the main governance file.

`.codex/config.toml` is a thin project-level adapter. In this candidate it only disables Codex private memories so they do not compete with the Markdown vault.

It does not scan files, block secrets, or enforce privacy automatically. Privacy remains an agent-behavior rule plus human review. See `docs/codex.md`.

## Claude Code integration

Claude Code should treat `CLAUDE.md` as the project instruction entry.

`CLAUDE.md` contains only `@AGENTS.md`, which imports the shared governance rules. `.claude/settings.json` is a thin project-level adapter that disables Claude Code auto-memory.

Do not copy `AGENTS.md` into `CLAUDE.md`. See `docs/claude-code.md`.

## Privacy boundary

This vault is meant to be readable by humans, agents, and later by Git.

Do not put the following in these files:

- passwords, tokens, cookies, sessions, private keys, OTP, or recovery codes
- government IDs or full financial account numbers
- raw financial exports, identity documents, or private screenshots
- real home paths, hostnames, private IPs, or private endpoints

If a secret ever entered Git history, deleting the current file is not enough. Revoke or rotate it first, then decide how to clean history.

See `docs/privacy.md`.

## Example project

`examples/weather-cli/` is a fictional Weather CLI used only to show the four project files working together.

It is not a real product and does not contain personal projects, devices, or financial data.

## Limitations

- This is a file convention, not a hosted service.
- It does not automatically detect secrets.
- It does not sync agent-private memory for you; the adapters only try to keep that memory from becoming a second source of truth.
- Agents still need permission and discipline to read the files before writing.
- Git is the history layer. Use normal Git commits to preserve history, review changes, and roll back when needed.

No user counts, download numbers, benchmarks, or production-adoption claims are made here.
