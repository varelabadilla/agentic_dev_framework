# Project Security Standards — {PROJECT_NAME}

> Project-specific security rules for Claude Code and the security-reviewer agent.
> These rules extend `.claude/rules/dev-security-standards.md` — do not duplicate what is already there.
> Complete all {placeholders} and remove sections that do not apply to this project.

---

## Authentication

- {Signing algorithm} only — {alternative} is explicitly forbidden
- {Token type}: {lifetime}, stored as {mechanism}, revoked via {strategy}
- {Other token type}: {lifetime}, {storage}, {invalidation strategy}
- {Auth endpoint} requires {validation rule}

---

## Sensitive Fields

The following fields must **never** appear in any API response:

- `{fieldName}` — {why it is sensitive}
- `{fieldName}` — {why it is sensitive}

Use response DTOs or Prisma `select` to exclude these fields at the service layer.

---

## Multi-Tenancy (if applicable)

- Every query that returns {entity} data must filter by {tenant field(s)} — never query across tenant boundaries
- {Role} bypasses tenant filters explicitly — all other roles do not
- {Entity} uniqueness is scoped to ({field1}, {field2}) — not just {field1} alone

---

## Audit Logging (if applicable)

- {AuditService or equivalent} must be called on every {auth / admin / security} operation
- {AuditService}.log() must never throw — errors are caught internally and never interrupt the main flow
- Both success and failure paths must log an audit event
- Sensitive values ({passwords, tokens, hashes}) must never appear in audit metadata

---

## Hashing

- Passwords: bcrypt with minimum {N} rounds
- Secrets: bcrypt with minimum {N} rounds
- Tokens ({RefreshToken, PasswordResetToken}): SHA-256 before storage — raw value never stored

---

## What NOT to Do

- Never return `{sensitiveField}` in any response
- Never store {token type} in plaintext
- Never use {forbidden algorithm}
- {Add project-specific prohibitions}
