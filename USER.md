# USER.md

Store only confirmed, stable, non-sensitive preferences that remain useful across future projects.

Do not store email addresses, phone numbers, legal names, government IDs, financial account numbers, credentials, or other unnecessary identifying information here.

Do not record agent inference, short-lived status, or project-specific facts.

## Preferred alias

Preferred alias: [optional alias]

## Language and communication

- Default working language for this vault: English, with Chinese documentation when useful.
- Prefer direct, concrete instructions over marketing language.
- When a task is technical, state assumptions and verification steps instead of guessing.

## Working style

- Multiple AI coding agents may work in the same vault. They should share these Markdown files instead of private memory.
- Before changing files, inspect the current on-disk state.
- Prefer small, reversible steps with a clear check after each important change.
- Keep durable facts in Markdown. Use Git for history once version control is enabled.

## Stable constraints

- Markdown is the current source of truth. Git is history.
- Agent-private memory is not a second long-term fact store.
- Secrets, credentials, and unnecessary identifying information stay out of this vault.
- Keep the system small. Do not add databases or extra memory frameworks unless explicitly requested.
