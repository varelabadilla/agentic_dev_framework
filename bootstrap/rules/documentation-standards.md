# Documentation Standards

> Portable rule file. Copy `.claude/rules/` to any new repository to apply these conventions.
> Applies to: any project using the agentic-dev-framework documentation workflow.

---

## Core Principle

Documentation prompts and implementation prompts are always separate. A prompt that modifies source code never updates documentation, and a prompt that updates documentation never modifies source code.

---

## File Editing Rules

- **Always read each target file in full before editing it** — never modify a file based on assumed contents
- After all edits, verify: no broken markdown tables, no unclosed code blocks, no duplicate section headings
- Checkbox syntax must be exactly `- [x]` or `- [ ]` (space inside brackets, no variation)
- D-XXX entries in `decision_log.md` are always **appended** — never inserted between existing entries

---

## Documentation Prompt Rules

- Never modify `.ts` or any source code files in a documentation prompt
- Never run `git add` or `git commit` in a documentation prompt
- Never mark the **next** phase as "In Progress" when completing the current phase — only update the phase that was just completed

---

## Contract Boundary — Backend and Frontend

The backend exposes **contracts**: endpoints, request/response payloads, HTTP status codes, and error shapes. These are the source of truth.

The backend does not dictate:
- How the frontend renders or presents data
- UI logic, component structure, or state management
- Frontend routing or navigation decisions

When a frontend needs something the backend does not currently provide, this is a documented gap — resolved by either extending the backend contract or by a frontend-side adaptation. The decision must be explicit, not assumed.

---

## Decision Log (D-XXX)

Every significant architectural or technical decision made during a phase must receive a D-XXX entry in `decision_log.md`:

```markdown
## D-XXX — Title

| | |
|---|---|
| **Decision** | What was decided |
| **Alternatives considered** | What else was evaluated |
| **Rationale** | Why this option was chosen |
| **Consequences** | What this decision implies going forward |
| **Phase** | Phase N |
```

Entries are always appended at the end of the file. Never renumber or reorder existing entries.

---

## Files Updated on Phase Completion

| File | What to update |
|---|---|
| `docs/dev_status.md` | Current phase narrative + completion status table + known issues |
| `docs/development_plan.md` | Phase status → `✅ Complete`; add BF-XXX bugfix entries if applicable |
| `docs/decision_log.md` | Append D-XXX entries for every significant decision |
| `docs/technical_decisions.md` | Update pending decisions table if any item was resolved or added |

---

## What NOT to Do

- Never modify source code in a documentation step
- Never insert D-XXX entries between existing entries
- Never skip the documentation step at the end of a phase
- Never assume the current contents of a documentation file — always read it first
