# Working Mode — Claude Code

> Portable rule file. Copy `.claude/rules/` to any new repository to apply these conventions.
> Applies to: Claude Code operating within any project using this framework.

---

## Default Behavior

When the user describes something to implement, fix, or explore — **read the relevant files first**. Never modify a file based on assumed or remembered contents.

Before making any changes, confirm understanding of what exists. If the task is ambiguous, ask one clarifying question before proceeding.

---

## Approval Gate

**Always wait for explicit user approval before proceeding** with any destructive, irreversible, or multi-file change.

This includes:
- Deleting or overwriting files
- Running database migrations
- Stopping running processes
- Changes that affect security logic, token handling, or authentication flows

---

## Backend Is the Source of Truth

The backend defines the contract (endpoints, payloads, types, error codes). The frontend adapts to the backend. When a frontend needs something the backend does not currently provide, surface this as a gap — do not resolve it silently by assuming what the backend should return.

---

## Security-Sensitive Changes

For any change that touches authentication, authorization, cryptographic operations, or secret handling:
- Provide a summary of what is changing and the security implications **before** making the change
- Show the previous implementation and the new implementation explicitly when modifying hashing or signing logic

---

## Git

- Run `git add` after completing all implementation steps
- **Never run `git commit`** — the user commits manually
- The commit message is always provided explicitly by the command or plan — copy it directly

---

## Temporary Files

- Write all intermediate files, generated SQL, and scratchpad content to `.claude/tmp/`
- Never use system temp directories (`/tmp`, `AppData`, `%TEMP%`)

---

## What NOT to Do

- Never implement directly when a plan exists — follow the plan
- Never assume file contents — always read the file before modifying it
- Never commit — stage only
- Never use system temp directories
- Never proceed with a destructive action without explicit user confirmation
