# Privacy

AgentMemory files are meant to be read by humans, agents, and later by Git. Treat anything written here as potentially shareable.

## Do not put these in the vault or in Git

- API keys
- tokens
- passwords
- cookies
- sessions
- private keys
- OTP / TOTP / SMS codes
- recovery codes
- government IDs
- full financial account numbers
- raw financial exports
- private evidence or screenshots
- real home directory paths, hostnames, private IP addresses, or private endpoints

A documentation example of a forbidden path should stay generic, for example `/Users/example-user/...`.

## Allowed in the vault

- non-sensitive working preferences
- project goals, scope, and architecture
- current status that does not include secrets
- decisions and verified pitfalls written with generic names
- explanations of the privacy rules themselves, including words such as password, token, or secret

## USER.md boundary

`USER.md` may contain an optional alias and durable working preferences.

It must not contain:

- legal names
- email addresses
- phone numbers
- government IDs
- financial account numbers
- credentials
- other unnecessary identifying information

## Local-only material

If something must exist on disk but must not be shared, keep it outside the vault, or in a directory that is ignored and never committed.

`.gitignore` is only a safeguard against accidental adds. It is not a security boundary.

Typical ignored names in this candidate:

- `secrets/`
- `private/`
- `raw/`
- `attachments/`
- `.env` files, except `.env.example`

## If a secret entered Git history

Deleting the current file is not enough.

1. Revoke or rotate the credential first.
2. Then decide how to clean Git history.
3. Do not assume a later public release is safe until history has been reviewed.

## Review before sharing

Before this folder is copied, archived, or published:

- search for real usernames, emails, phone numbers, IPs, hostnames, and private paths
- search for private project names and financial data
- confirm examples are fictional
- confirm adapters do not contain personal config
