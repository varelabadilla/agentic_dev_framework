# Database Conventions — PostgreSQL

> Portable rule file. Copy `.claude/rules/` to any new repository to apply these conventions.
> Applies to: any project using PostgreSQL as the database engine.

---

## Naming Conventions

- All table names use **snake_case** — enforced via `@@map("table_name")` on every Prisma model
- All column names use **snake_case** — enforced via `@map("column_name")` on every camelCase field
- Junction table names combine both entity names: `org_application`, `user_module_access`
- Index names follow the pattern: `idx_<table>_<column(s)>`

---

## DateTime and Timezone

- All `DateTime` fields must use `@db.Timestamptz(3)` — stores as UTC with explicit timezone declaration in PostgreSQL
- **Never use** `timestamp without time zone` for any date/time field
- The application server always operates in **UTC**
- Timezone conversion to local time is the responsibility of each client application — never stored server-side

---

## Data Integrity

- Use **UUID** as primary key type for all entities — never auto-increment integers for entities exposed via API
- Define explicit `UNIQUE` constraints for business uniqueness rules — do not rely on application-layer enforcement alone
- Use **soft deletion** via `isActive: false` — never physically delete records unless explicitly designed to do so
- Invalidation fields (`revokedAt`, `usedAt`) are preferred over deletion for time-limited or single-use records

---

## Query Rules

- Every query that returns tenant-scoped data must include all tenant filter columns — never query across tenant boundaries
- Never use `deleteMany` or `updateMany` without explicit scope filters

---

## What NOT to Do

- Never store derived or calculated values as columns — compute at query time
- Never store plaintext secrets, passwords, or tokens — always hash before storage
- Never use `timestamp without time zone` — always declare timezone explicitly
- Never rely on database-level default timezones — always use UTC at the application layer
