# PROJECT.md

Fictional example: a small command-line weather tool used to demonstrate AgentMemory. Not a real product.

## Goal

Build a local CLI that prints a short weather summary for one city name.

## Scope

In scope:

- a single command, `weather <city>`
- fetch current conditions from a public weather HTTP API
- print temperature, wind, and a one-line condition
- keep a short on-disk cache so repeated queries for the same city are cheap

Out of scope:

- accounts, billing, or maps
- a graphical interface
- storing user identity or location history beyond the current process arguments
- becoming a general AgentMemory feature

## Principles

- Keep the command obvious: one argument in, one summary out.
- Fail in a way a person can read. Do not dump raw stack traces as the normal error path.
- Treat the upstream weather API as untrusted input.
- Use AgentMemory files for project facts; do not hide status in agent-private memory.

## Stable constraints

- The example dataset and city names are fictional.
- No credentials belong in this project. If an API later requires a key, it stays in an untracked local environment file.
- The CLI must remain a small library-plus-binary layout, not a service.
- Markdown in this folder is the current project truth.

## Confirmed architecture

- A CLI entrypoint parses the city argument and flags.
- A fetcher calls the weather HTTP API and decodes JSON.
- A cache layer stores the last successful payload per city for a short TTL.
- A renderer turns the decoded payload into one or two lines of text.
- AgentMemory files in this directory describe the project; they are not runtime config for the CLI.
