# Code Standards — Node.js + TypeScript

> Portable rule file. Copy `.claude/rules/` to any new repository to apply these conventions.
> Applies to: any Node.js project using TypeScript.

---

## Language

- All code, comments, and documentation must be written in **English**
- **TypeScript always** — never plain JavaScript
- Never use `any` as a type unless there is an explicit, documented justification

---

## General Code Rules

- Never hardcode secrets, credentials, URLs, or environment-specific values — always use environment variables
- Never store derived values in the database — calculate at query time
- No background jobs or cron tasks unless explicitly planned in the phase — all derived values are computed on request
- Methods and functions must have a single, focused responsibility
- Error handling must occur at the correct layer with meaningful, actionable messages
- No manual instantiation of services or classes — use dependency injection

---

## NestJS Patterns

- Business logic lives in **services** — controllers only route and delegate
- **Guards** handle authentication and role enforcement — services do not check roles
- **DTOs** are used for all request bodies and response shapes
- Domain entities (Prisma models) are never returned directly from controllers — always map to a response DTO that excludes sensitive fields
- Dependency injection via constructor — never `new SomeService()` manually
- New endpoints must be documented with Swagger decorators (`@ApiTags`, `@ApiOperation`, `@ApiResponse`)

---

## API Design

- When a feature requires a different query shape than an existing endpoint, **create a new endpoint** rather than modifying the existing one — backward compatibility must be preserved
- Endpoint prefixes must be consistent and defined in `CLAUDE.md` for the project

---

## What NOT to Do

- Do not use `HS256` — use asymmetric signing only (RS256 or equivalent)
- Do not couple this service to any consumer application
- Do not return sensitive fields (password hashes, raw tokens, private keys) in any API response
- Do not switch Git branches during a task — work on the currently checked-out branch
- Do not hardcode UUIDs or credentials in service logic
