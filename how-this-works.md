# How This Works — agentic-dev-framework

> A structured methodology for solo developers building software with Claude Code and Claude.ai.
> Extracted from real project experience building NestAuth — a multi-tenant authentication server.

---

## The Core Idea

Most AI-assisted development fails at scale because it is unstructured: ad-hoc prompts, no shared context, no approval gates, and no separation between thinking and doing. This framework addresses that by splitting development into two deliberate roles and a repeatable phase cycle.

**Two instances of Claude, two roles:**

| Instance | Role | What it does |
|---|---|---|
| Claude.ai | Planner / Architect | Discusses, decides, generates prompts |
| Claude Code | Executor | Reads the repo, implements, tests, stages |

**One repeatable cycle:**

```
/plan → /adjust → /approve → /execute → /docs → /clean-up
```

Nothing gets implemented without a plan. Nothing gets merged without documentation. Everything is explicit and approved by the developer.

---

## The Four Layers

### Layer 1 — Persistent Context

What both instances always know. Lives in the repository and in the Claude.ai project.

**Repository files (Claude Code reads these):**
- `CLAUDE.md` — what this project is, how it is built, all conventions and rules
- `.claude/rules/*.md` — portable standards (code, security, database, git, workflow)
- `docs/development_plan.md` — master index of all phases
- `docs/phases/*.md` — task detail for each phase
- `docs/decision_log.md` — every architectural decision with D-XXX identifier
- `docs/technical_decisions.md` — stack decisions and pending decisions
- `docs/information_architecture.md` — entity relationships and hierarchy
- `docs/product_definition.md` — what the product is and is not
- `docs/user_flows.md` — key flows for users and systems
- `docs/dev_status.md` — current phase, known issues, completion status
- `decision_ai_log.md` — planning decisions from Claude.ai sessions

**Claude.ai project files (the planner reads these):**
- `project-setup-template.md` (configured and uploaded) — product context, architecture rules, workflow
- `claude-planner-rules.md` — how this instance operates
- `decision_ai_log.md` — uploaded by the developer after planning sessions
- All key `docs/` files — uploaded to project knowledge

### Layer 2 — Role Separation

The planner (Claude.ai) and the executor (Claude Code) are deliberately separate. This separation means:

- Architecture decisions happen before implementation, not during
- The executor never makes architectural choices — it follows the plan
- The developer is always the approval gate between planning and execution
- Session history and project context accumulate in Claude.ai, not in Claude Code

Claude Code is the same underlying model as Claude.ai. The difference is context: Claude Code sees only what is in the repository and the current prompt. Claude.ai sees the accumulated history of all planning conversations in the project.

**The bridge between them is the prompt.** Everything Claude Code needs to know must either be in the prompt or in a file it can read in the repository.

### Layer 3 — Phase Cycle

Every feature, fix, or capability is delivered through a phase. A phase is a unit of work with a dedicated plan document, explicit approval, and a documentation step at the end.

**The cycle:**

```
/plan phase-N
  → reads docs/phases/phase-NN-slug.md and docs/development_plan.md
  → generates implementation plan with test coverage analysis
  → records session start in activity.log
  → writes .runbook/plan.md
  → waits for /approve or /adjust

/adjust {text or path}
  → modifies .runbook/plan.md
  → presents updated plan for review

/approve
  → locks the plan
  → does NOT execute anything

/execute phase-N
  → reads .runbook/plan.md
  → implements all steps in order
  → runs build verification
  → runs QA checklist
  → stages files with git add
  → writes .runbook/implementation-results.md

/docs phase-N
  → updates dev_status.md, development_plan.md, decision_log.md, technical_decisions.md
  → never touches source code

/clean-up
  → confirms deletion of .runbook/ contents (except activity.log)
  → records session end and duration in activity.log
  → leaves the project in a clean state for the next phase
```

**Definition of done — every phase must satisfy all of these before it is marked complete:**

- All automated tests pass
- No `- [ ]` items remain without justification (use `- [-]` for intentionally skipped)
- `docs/dev_status.md` updated
- `docs/decision_log.md` updated with D-XXX entries
- `docs/technical_decisions.md` reviewed
- `docs/development_plan.md` marks the phase `✅ Complete`
- Files staged, commit message provided

### Layer 4 — Prompt Structure

When the planner generates a prompt for Claude Code, every prompt follows this structure:

```markdown
## Context
{Relevant technical context: entities involved, endpoints affected, dependencies}

## Files Involved
{Listed as reference — Claude Code reads the actual files}

## What to Do
{Numbered steps in execution order}

## What NOT to Touch
{Explicit restrictions for this task}

## How to Verify
{Orientative verification steps}

## Commit Message
{Only when outside the normal phase cycle}
```

One logical unit per prompt. Backend and frontend are always separate prompts (separate repositories).

---

## Pre-Phase: Proposals

Before an idea becomes a phase, it goes through a proposal step. This keeps the `development_plan.md` clean — only validated, actionable ideas become phases.

```
/proposal {idea description}
  → reads existing docs and codebase for context
  → generates docs/proposals/proposal-NNN.md
  → asks clarifying questions if the idea lacks detail

/proposal-to-phase docs/proposals/proposal-NNN.md
  → validates the proposal has sufficient detail
  → reads development_plan.md to determine next phase number
  → generates a phase file draft
  → waits for /proposal-approve

/proposal-approve
  → writes docs/phases/phase-NN-slug.md
  → updates development_plan.md
  → marks the proposal as converted
```

---

## Repository Structure

After initialization, a project using this framework has:

```
{project}/
├── CLAUDE.md                          # Project-specific instructions for Claude Code
├── README.md                          # Human-readable project overview
├── decision_ai_log.md                 # Planning session decisions
├── activity.log                       # Permanent session and phase history
│
├── docs/
│   ├── development_plan.md            # Master index of all phases
│   ├── dev_status.md                  # Current state and known issues
│   ├── decision_log.md                # D-XXX architectural decisions
│   ├── technical_decisions.md         # Stack decisions and pending items
│   ├── information_architecture.md    # Entity relationships
│   ├── product_definition.md          # What the product is
│   ├── user_flows.md                  # Key flows
│   ├── phases/                        # One file per phase
│   ├── proposals/                     # Pre-phase proposals
│   └── diagrams/                      # Flow diagrams
│
└── .claude/
    ├── rules/                         # Portable rule files
    ├── commands/                      # Slash commands
    ├── agents/                        # Specialized review agents
    └── tmp/                           # Git-ignored scratchpad
```

---

## File Reference

### Rules (`.claude/rules/`)

| File | Purpose | Portable to |
|---|---|---|
| `code-standards-nodejs-typescript.md` | Language, NestJS patterns, API design | Any Node.js + TypeScript project |
| `git-conventions.md` | Commit format, staging rules | Any Git project |
| `database-conventions-postgresql.md` | Naming, datetime, data integrity | Any PostgreSQL project |
| `orm-conventions-prisma.md` | Schema mapping, transactions | Any Prisma project |
| `dev-environment.md` | Server lifecycle, temp files | Any local dev project |
| `dev-security-standards.md` | Secrets, hashing, cryptography | Any project handling auth |
| `phase-workflow.md` | Phase cycle, task states, done criteria | Any project using this framework |
| `documentation-standards.md` | Doc updates, decision log, contracts | Any project using this framework |
| `working-mode.md` | Claude Code operating principles | Any project using this framework |
| `diagram-standards.md` | When and how to create diagrams | Any project with flow diagrams |
| `project-security-standards.md` | Project-specific security rules | This project only |
| `project-tech-standards-nodejs.md` | Project-specific tech conventions | This project only |

### Commands (`.claude/commands/`)

| Command | When to use |
|---|---|
| `/plan phase-N` | Start of every phase — generates the plan |
| `/adjust` | After reviewing the plan — refine before approving |
| `/approve` | After reviewing the plan — locks it for execution |
| `/execute phase-N` | After approval — implements the plan |
| `/docs phase-N` | After execution — updates all documentation |
| `/clean-up` | After docs are committed — closes the session |
| `/plan-discard` | Discard the current plan and start over |
| `/time-report` | View accumulated time across all sessions |
| `/update-structure` | Scan the repo and write/overwrite `STRUCTURE.md` at the root |
| `/proposal` | Explore a new idea before it becomes a phase |
| `/proposal-to-phase` | Convert a validated proposal into a phase |
| `/proposal-approve` | Write the approved phase files |
| `/init-project` | One-time initialization of a new project from the framework |

### Agents (`.claude/agents/`)

| Agent | When to invoke |
|---|---|
| `architect` | After implementation — review for SOLID, Clean Architecture, NestJS patterns |
| `security-reviewer` | After implementation — review for secrets, auth, input validation |

Invoke from Claude Code: "Run the architect agent to review my changes"

### Bootstrap (`bootstrap/` — exists only in the framework repo)

| File | Purpose |
|---|---|
| `CLAUDE.md` | Template for the project's CLAUDE.md |
| `decision_ai_log.md` | Empty template ready to use |
| `phase-template.md` | Structure for new phase files |
| `project-setup-template.md` | Template for the Claude.ai Project Setup |
| `project-security-standards.md` | Template for project-specific security rules |
| `project-tech-standards-nodejs.md` | Template for project-specific tech conventions |

---

## Decision Log Convention

Every significant architectural or technical decision gets a D-XXX entry in `docs/decision_log.md`:

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

Entries are always appended. Never renumber or insert between existing entries.

Planning-level decisions (made in Claude.ai before a D-XXX exists) go in `decision_ai_log.md`.

---

## Time Tracking

Every session is tracked in `activity.log`:

- `/plan phase-N` records session start
- `/clean-up` records session end and duration
- `/time-report` shows total time across all sessions

`activity.log` is permanent — it is never deleted by any command.

---

## Scalability Note

This framework is designed for a **solo developer**. It can scale to small teams with adaptations (branch strategy, PR review gates, shared project context), but that is not the current goal. Team scalability is a future direction without a fixed timeline.

---

## Origin

This framework was extracted from the methodology that emerged organically while building **NestAuth** — a standalone multi-tenant RS256 JWT authentication server. What started as ad-hoc conventions became a repeatable, documented method. This repository formalizes that method so it can be applied to any new project from day one.
