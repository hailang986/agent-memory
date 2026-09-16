# AgentMemory — Agent Rules

This directory is a shared long-term knowledge vault for AI coding agents.

Markdown files are the current facts. Git stores history. Agent-private memory is not a second source of truth.

## 1. Source of Truth

- Markdown files in this vault are the only long-term source of truth.
- Do not treat any agent's built-in memory, chat context, hidden state, cache, or inference as final fact.
- If an agent's private memory conflicts with this vault, check the latest explicit records here first.
- A user's current correction or a freshly verified observation can invalidate older records. State the difference, then update only within the authorized scope.
- Unconfirmed inference must never replace a recorded fact.

## 2. Write Principles

- Write only information that will remain useful in the future.
- Do not write something merely because it "might be useful."
- Do not record unconfirmed agent inference as a user fact.
- Do not store one-off chat details, temporary ideas, or information with no lasting value.
- When unsure whether to write, default to not writing.

## 3. File Responsibilities

### `AGENTS.md`

Stores agent behavior rules and vault governance only.

Do not store:

- user profile facts
- project progress
- project decisions
- temporary status

### `INDEX.md`

Navigation entry for the vault.

Do not dump large bodies of text, project status, or user facts into INDEX.

### `USER.md`

Stores only:

- long-term facts the user has explicitly confirmed
- verified, stable preferences
- information that remains useful across multiple future projects

Do not:

- write agent inference directly into `USER.md`
- write short-lived status into `USER.md`
- write project-specific information into `USER.md`
- store emails, phone numbers, legal names, government IDs, financial account numbers, credentials, or other unnecessary identifying information

### `projects/<project>/PROJECT.md`

Stores the project's stable definition:

- goal
- scope
- core principles
- long-term constraints
- confirmed overall architecture

Record only relatively stable information.

Do not store current status, next steps, or blockers here. Those belong in `CURRENT.md`.

### `projects/<project>/CURRENT.md`

Stores the project's single current state:

- current phase
- completed items
- in-progress items
- next steps
- current blockers

Update by maintaining the latest state in place. If a fact depends on verification, note the date it was actually checked. An edit date is not a verification date; do not refresh a verification date without rechecking.

A missing later record means the state has not been rechecked. It does not mean the work is complete or overdue.

Do not keep old versions of status in the file. History belongs in Git.

### `projects/<project>/DECISIONS.md`

Stores important decisions that have been confirmed and will continue to affect later work.

Each decision should usually include:

- what was decided
- why it was decided
- necessary background or constraints

Ordinary operating steps are not decisions.

Do not record every choice. Only decisions that remain useful over time belong here.

### `projects/<project>/PITFALLS.md`

Stores problems that actually happened and are likely to recur, including:

- what was observed
- the cause, once it is reasonably confirmed
- a verified fix
- practices to avoid

Unverified guesses must not be written as settled conclusions.

Do not record every issue. Only problems that actually occurred, have a reasonably confirmed cause, have a verified remedy, and may recur belong here.

## 4. Project Workflow

When working on a project in this vault:

1. Use `INDEX.md` to locate the project, and read relevant preferences in `USER.md`.
2. Read that project's `PROJECT.md` and `CURRENT.md`.
3. Read `DECISIONS.md` when historical decisions matter.
4. Read `PITFALLS.md` when dealing with known failures or environment issues.
5. Analysis, review, and diagnosis requests should deliver judgment by default. Do not automatically modify files or memory.
6. After authorized work that actually changes project state, update the corresponding Markdown files only as needed.

Do not mechanically edit every file just to leave a trace.

## 5. Git Principles

- Git stores historical versions of the Markdown files.
- Markdown holds current truth. Git holds history.
- Do not maintain a long hand-written history inside `CURRENT.md`.
- Important changes may be committed after they are complete. When handing off work, say whether the edit, commit, and push each happened. Push only with project decisions and current task authorization. Do not claim a remote backup exists if nothing was pushed.
- Do not rewrite, delete, or otherwise damage existing Git history without explicit authorization.

Git is the history layer for this repository. These Git principles apply whenever changes are version-controlled.

## 6. Agent Memory Isolation

- An agent's built-in memory cannot replace this vault.
- Avoid creating a second long-term truth source beside these Markdown files.
- Agents that read and write this vault should prefer the Markdown files here. Actual read/write ability depends on the permissions and verification of the current session, not on a product name or self-description.
- Do not introduce another long-term memory framework as a core dependency unless the user explicitly approves it.

Claude Code and Codex adapters in this repository exist only to keep agent-private memory from competing with the vault. They are not a second knowledge base.

## 7. Secrets and Sensitive Data

- Anything that enters a Git commit should be treated as able to enter a configured remote Git repository. Do not invent a fuzzy boundary such as "local Git is fine, but GitHub is not."
- Passwords, PINs, CVV/CVC, OTP / SMS codes / TOTP, sessions, cookies, API tokens, access tokens, SSH private keys, and recovery codes must never enter the vault or Git.
- Full government ID numbers, passport numbers, full payment-card numbers, and full bank account numbers must not enter the long-term vault.
- Raw bank statements, bank export files, identity documents, and verification screenshots must not enter Git by default.
- Sensitive facts that truly must be retained should follow data minimization and aliasing.
- Truly local-only sensitive material must live outside the Git repository. `.gitignore` is a safeguard against accidental commits, not a real security boundary.
- If a secret was ever committed, deleting the current file is not enough. Revoke or rotate it first, then separately evaluate history cleanup.

## 8. Operational Safety

- Before changing an existing file, read its actual contents and any current uncommitted changes. Avoid having multiple agents write the same file at once. If the file changed, re-check before writing. Do not overwrite another agent's work.
- Do not overwrite a file based on assumption.
- If the current state is unclear, inspect first.
- Be especially careful before deletion, bulk overwrite, migration, or large-scale refactoring.
- Prefer simple, transparent, reversible approaches.

## 9. Keep It Simple

This version aims to stay small, stable, and inspectable.

Do not add complex directories, databases, automatic memory systems, or extra frameworks on your own.

Unless the user explicitly approves it, do not introduce:

- a second long-term memory product as a core dependency
- a separate `HANDOFF.md` as a required file
- a separate decisions directory
- a separate workflow directory
- a separate agent directory
- a separate archive directory

Prefer getting the simple system working before expanding it.
