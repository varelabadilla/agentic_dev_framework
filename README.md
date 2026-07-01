# agentic-dev-framework

A structured methodology for solo developers building software with Claude Code and Claude.ai.

Extracted from real project experience. Not a theory — a working method.

---

## What This Is

A framework that brings structure to AI-assisted development by separating two things that most developers conflate:

- **Planning** (Claude.ai) — architecture, proposals, decisions, prompt generation
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
├── planner/
│   └── claude-planner-rules.md    # Upload to Claude.ai project
│
├── bootstrap/                 # Templates — consumed by /init-project, then deleted
│   ├── CLAUDE.md
│   ├── decision_ai_log.md
│   ├── phase-template.md
│   ├── project-setup-template.md
│   ├── project-security-standards.md
│   └── project-tech-standards-nodejs.md
│
├── rules/                     # Portable rule files for Claude Code
│   └── *.md
│
├── commands/                  # Slash commands for Claude Code
│   └── *.md
│
└── agents/                    # Specialized review agents
    └── *.md
```

---

## The Phase Cycle

```
/plan → /adjust → /approve → /execute → /docs → /clean-up
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

Extracted from the development of **NestAuth** — a standalone multi-tenant RS256 JWT authentication server built with NestJS, TypeScript, Prisma, and PostgreSQL. The conventions and workflow documented here emerged from that project and were formalized into this reusable framework.
