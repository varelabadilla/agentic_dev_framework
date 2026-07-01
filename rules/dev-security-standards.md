# Security Standards

> Portable rule file. Copy `.claude/rules/` to any new repository to apply these conventions.
> Applies to: any project handling authentication, credentials, or sensitive data.

---

## Secrets and Credentials

- **Never hardcode** secrets, API keys, passwords, or connection strings — always use environment variables
- **Never expose private keys** in logs, API responses, error messages, or console output
- **Never commit `.env` files** or any file containing secrets to the repository
- Keep `.env.example` up to date when adding new environment variables — use placeholder values only

---

## Password and Token Hashing

- Always use **bcrypt** for password and secret hashing — never MD5, SHA-1, or plain SHA-256 alone
- Minimum bcrypt rounds: **10** for secrets, **12** for passwords
- **Raw tokens are never stored** — always hash before persisting (`RefreshToken`, `PasswordResetToken`, etc.)
- Use SHA-256 to hash tokens before storage; compare hashed values on lookup

---

## API Responses

- Sensitive fields (password hashes, raw tokens, private keys, internal secrets) must **never appear in any API response**
- Use response DTOs or Prisma `select` to explicitly exclude sensitive fields
- Apply exclusion at the service layer — do not rely on the controller or serializer to catch it

---

## Input Validation

- All controller parameters and request bodies must be validated via DTOs before use
- UUIDs must be validated before being used as database filter values
- User-controlled data must never be used in raw queries without sanitization

---

## Cryptography

- Use **asymmetric signing** (RS256 or equivalent) for JWTs — symmetric algorithms (HS256) are forbidden
- Minimum RSA key length: **2048 bits**
- The private key is used only for signing — never shared with consumer services

---

## Changes Affecting Security

- Before modifying any file that handles authentication, authorization, token issuance, or secret storage: **provide a summary of the changes and their security implications** before proceeding
- Show the previous implementation and the new implementation side by side when modifying cryptographic or hashing logic
- Any change that weakens an existing security control is a **BLOCKER** — do not proceed without explicit user confirmation

---

## What NOT to Do

- Never store passwords, tokens, or secrets in plaintext
- Never log sensitive values — passwords, tokens, hashes, or key material
- Never use symmetric JWT signing
- Never skip input validation on public endpoints
- Never return internal error details (stack traces, SQL errors) to the client
