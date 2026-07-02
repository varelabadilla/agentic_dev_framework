# agentic-dev-framework

A structured methodology for solo developers building software with Claude Code and Claude.ai.

Extracted from real project experience. Not a theory — a working method.

---

## What This Is

A framework that brings structure to AI-assisted development by separating two things that most developers conflate:

- **Planning** (Claude.ai or Claude Code) — architecture, proposals, decisions, prompt generation
- **Execution** (Claude Code) — reading the repo, implementing, testing, staging

Everything moves through a repeatable phase cycle with explicit approval gates. Nothing gets implemented without a plan. Nothing gets merged without documentation.

---

## Quick Start

1. Fork this repository
2. Open Claude Code in the forked repo
3. Run `/init-project`
4. Follow the prompts — Claude Code will set up your project structure, personalize your `CLAUDE.md`, and tell you what to complete manually

---

## Repository Structure

```
agentic-dev-framework/
├── README.md                  # This file
├── how-this-works.md          # Full methodology explanation
│
├── .claude/
│   ├── commands/
│   │   ├── init-project.md        # Bootstraps a new project — self-deletes after running
│   │   └── update-structure.md    # Regenerates STRUCTURE.md — stays after init
│   ├── rules/                     # Empty until /init-project populates it
│   └── agents/                    # Empty until /init-project populates it
│
├── docs/
│   └── planner/
│       └── claude-planner-rules.md    # Upload to Claude.ai project
│
└── bootstrap/                 # Consumed by /init-project, then deleted
    ├── rules/                     # Portable rule files → copied to .claude/rules/
    │   └── *.md
    ├── commands/                  # Slash commands → copied to .claude/commands/
    │   └── *.md
    ├── agents/                    # Specialized review agents → copied to .claude/agents/
    │   └── *.md
    └── templates/                 # Project-root templates (CLAUDE.md, decision-ai-log, etc.)
        └── *.md
```

---

## The Phase Cycle

```
/plan → /plan-adjust → /plan-approve → /plan-execute → /plan-docs → /plan-clean-up
```

For new ideas before they are planned:

```
/proposal → /proposal-to-phase → /proposal-approve
```

---

## Read More

See [`how-this-works.md`](how-this-works.md) for the full methodology, file reference, and layer-by-layer explanation.

---

## Origin

Extracted from the development of a standalone server built with NestJS, TypeScript, Prisma, and PostgreSQL. The conventions and workflow documented here emerged from that project and were formalized into this reusable framework.
