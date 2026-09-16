# Claude Code

Claude Code should use this vault as an ordinary project directory.

## Instruction entry

`CLAUDE.md` is the Claude Code project instruction file.

It contains only:

```text
@AGENTS.md
```

That import pulls in the shared governance rules. It is not a second knowledge base.

Do not put the following in `CLAUDE.md`:

- a copy of `AGENTS.md`
- project status
- user profile facts
- model, temperature, maxTokens, or systemPrompt API settings
- personal endpoints or credentials

## Thin adapter

`.claude/settings.json` is a project-level adapter. The verified purpose in this repository is to disable Claude Code auto-memory:

```json
{
  "autoMemoryEnabled": false
}
```

That setting is tool wiring. It does not store facts and it does not enforce privacy by itself.

## Expected workflow

1. Open the vault directory in Claude Code.
2. Let `CLAUDE.md` import `AGENTS.md`.
3. Use `INDEX.md` to find the project, then read `PROJECT.md` and `CURRENT.md`.
4. Update Markdown files only when authorized work actually changes durable state.

If Claude Code still offers private memory features, do not use them as a long-term replacement for this vault.
