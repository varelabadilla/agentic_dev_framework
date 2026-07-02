# planner/

Files for the **Claude.ai planning instance** — not for Claude Code.

The planning instance is the Claude.ai conversation where you discuss architecture, evaluate proposals, and generate prompts for Claude Code. It is a separate role from Claude Code with different responsibilities and different context.

---

## Files

### `claude-planner-rules.md`

Upload this file to your Claude.ai Project alongside `project-setup-template.md` (from `bootstrap/`) and your key project documents.

It defines:
- The role of the planning instance vs. Claude Code
- Working mode — when to generate proposals, when to generate prompts, when to ask first
- Prompt quality rules — what every Claude Code prompt must include
- Decision logging — how to capture session decisions in `decision-ai-log.md`
- Approval gate — always wait for explicit developer confirmation

---

## What Goes in the Claude.ai Project

To configure the planning instance for a new project, upload these files to the Claude.ai Project:

| File | Source |
|---|---|
| `project-setup-template.md` (completed) | `bootstrap/` → complete all placeholders first |
| `claude-planner-rules.md` | This folder |
| `decision-ai-log.md` | Project root (after init, keep updated) |
| `information_architecture.md` | `docs/` |
| `product_definition.md` | `docs/` |
| `technical_decisions.md` | `docs/` |
| `user_flows.md` | `docs/` |

The planning instance does not read the repository directly. Everything it knows comes from these uploaded files and the conversation history within the project.
