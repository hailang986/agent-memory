# Codex

Codex should use this vault as an ordinary project directory.

`AGENTS.md` is the main governance file. Codex reads project rules from that Markdown file, not from a parallel knowledge store.

## Thin adapter

`.codex/config.toml` is a project-level adapter. The verified purpose in this repository is to keep Codex private memories from competing with the Markdown vault:

```toml
[features]
memories = false

[memories]
generate_memories = false
use_memories = false
```

That is isolation, not a knowledge base.

Do not copy `AGENTS.md`, project status, user preferences, or secrets into `config.toml`.

## What config.toml does not do

Privacy rules belong to agent behavior and human review.

`.codex/config.toml` does not:

- scan Markdown for secrets
- block real paths
- block personal data
- block financial data
- replace `AGENTS.md`
- prove that a future Git commit is safe

Do not invent configuration keys that claim those capabilities.

## Expected workflow

1. Open the vault directory in Codex.
2. Read `INDEX.md`, `AGENTS.md`, and the relevant project files.
3. Change Markdown only after checking the files on disk.
4. Keep Codex private memories disabled so session notes do not become a second long-term fact source.

If Codex still offers to save private memories, decline them for facts that belong in this vault.
