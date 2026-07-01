# commands/

Slash commands for Claude Code. Copy the entire `commands/` folder to `.claude/commands/` in any new project.

Commands are invoked directly in Claude Code: `/plan phase-5`, `/approve`, etc.

---

## Phase Cycle Commands

These are the core commands used in every phase:

| Command | When to use | What it does |
|---|---|---|
| `/plan phase-N` | Start of every phase | Reads phase file and docs, generates implementation plan, records session start |
| `/adjust {text or path}` | After reviewing the plan | Modifies the plan — accepts inline text or a path to a Markdown file |
| `/approve` | After the plan looks correct | Locks the plan for execution — does NOT implement anything |
| `/execute phase-N` | After approval | Implements all steps, runs build and QA, stages files |
| `/docs phase-N` | After execution is committed | Updates all project documentation files |
| `/clean-up` | After docs are committed | Clears `.runbook/`, records session end and duration |

---

## Utility Commands

| Command | When to use | What it does |
|---|---|---|
| `/plan-discard` | When the current plan is wrong | Deletes `.runbook/plan.md` so a new plan can be generated |
| `/time-report` | Anytime | Shows total time logged across all sessions from `activity.log` |

---

## Proposal Commands

Used before a phase exists — for evaluating new ideas:

| Command | When to use | What it does |
|---|---|---|
| `/proposal {idea}` | When exploring a new feature | Generates a structured `docs/proposals/proposal-NNN.md` informed by the codebase |
| `/proposal-to-phase {path}` | When a proposal is ready to formalize | Validates the proposal and generates a phase file draft |
| `/proposal-approve` | After reviewing the phase draft | Writes the phase file and updates `development_plan.md` |

---

## Definition Cycle Commands

Used once, between `/init-project` and the first `/plan`. Guides the user through
a structured requirements elicitation session and generates all project documentation.

| Command | When to use | What it does |
|---|---|---|
| `/define` | After `/init-project` | Conversational session — asks strategic questions across 8 blocks, generates `.claude/tmp/project-definition-draft.md` |
| `/define-adjust {text or path}` | After reviewing the draft | Refines the definition draft — accepts inline text or path to a file |
| `/define-approve` | When the draft is correct | Validates and locks the draft for documentation generation |
| `/define-generate` | After approval | Generates all `docs/` files, updates `CLAUDE.md` and `README.md` |

---

## Structure Command

| Command | When to use | What it does |
|---|---|---|
| `/update-structure` | After init, after any phase that changes files | Scans the repo and writes/overwrites `STRUCTURE.md` at the root |

---

## Initialization Command

| Command | When to use | What it does |
|---|---|---|
| `/init-project` | Once, after forking the framework | Sets up project structure, personalizes files, removes `bootstrap/` |

This command detects whether it has already run by checking for `bootstrap/`. If `bootstrap/` does not exist, it stops safely.

---

## Agent State

All commands communicate via `.runbook/` (git-ignored):

| File | Written by | Purpose |
|---|---|---|
| `plan.md` | `/plan` | The implementation plan |
| `implementation-results.md` | `/execute` | Build result, QA results, files changed |
| `pm-status.md` | `/docs` | Summary of documentation updates |
| `arch-status.md` | `architect` agent | Architecture review findings |
| `sec-status.md` | `security-reviewer` agent | Security review findings |
| `activity.log` (project root) | All commands | Permanent history — never deleted |
