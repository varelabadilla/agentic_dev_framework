# /define — Guide the User Through Project Definition

## Usage

`/define`

Run this command once after `/init-project`, before `/define-generate`.
This command does not generate documentation files — it conducts a structured
conversation to extract enough information to generate them accurately.

---

## Purpose

This command acts as a **requirements elicitation session**. It guides the user
through a series of strategic questions organized in thematic blocks, validates
completeness of the answers, and produces a temporary draft document at
`.claude/tmp/project-definition-draft.md` that will be used by `/define-generate`.

The goal is not to collect a wish list — it is to discover the real needs,
constraints, and structure of the project before a single line of code is written.

---

## Satisfaction Protocol

After each block, evaluate the responses against four dimensions before moving on:

| Dimension | Question to ask internally |
|---|---|
| **Correctness** | Does what was documented reflect what the user actually needs, or is it vague/assumed? |
| **Completeness** | Are all key aspects of this block covered with non-empty, non-vague answers? |
| **Consistency** | Does this block contradict anything said in a previous block? |
| **Validity** | Is what the user described technically feasible with the chosen stack? |

If any dimension fails:
- Ask one targeted follow-up question addressing the specific gap
- Apply the four-dimension check again after the response
- If the dimension still fails after two follow-up rounds, mark the field as
  `[PENDING — needs clarification]` in the draft and move on
- Do not loop indefinitely on a single point

---

## Process

### Step 1 — Introduction

Inform the user:

"I'm going to guide you through a structured definition session for your project.
We'll cover 8 topics in order. For each one I'll ask you some questions —
answer as much or as little as you know right now. If something is unclear or
not yet decided, say so and we'll mark it as pending.

At the end I'll generate a definition draft that we can review and adjust
before creating your project documentation.

Let's start."

---

### Step 2 — Execute the blocks in order

Work through each block sequentially. Present the block name and its purpose
before asking the questions. Do not present all blocks at once — one block at a
time, waiting for the user's response before moving on.

After completing all blocks, proceed to Step 3.

---

## Block Definitions

---

### Block 1 — Product Identity

**Purpose:** Establish the core identity of the product. Everything else depends
on having a clear, honest answer to "what is this and why does it exist."
Without this, scope creep starts on day one.

**Key questions:**
1. What is the name of the product?
2. In one or two sentences — what does it do?
3. What specific problem does it solve, and for whom?
4. What does it explicitly NOT do? (boundaries are as important as capabilities)
5. Is this a greenfield project or does it replace or extend something existing?

**Sufficiency criteria:**
- Name is defined
- Problem statement is specific (not "it helps users manage things")
- At least one explicit "not in scope" boundary is stated

---

### Block 2 — Actors and Users

**Purpose:** Identify who interacts with the system and what they need from it.
This shapes the role model, the access control design, and which flows exist.

**Key questions:**
1. Who are the different types of users of this system?
2. For each type: what is their primary goal when using the product?
3. Are there system actors (other services, automated processes) that interact
   with this product?
4. Is there a hierarchy or permission difference between user types?
5. Who configures or administers the system?

**Sufficiency criteria:**
- At least one human actor identified with a clear goal
- Any admin or configuration role is explicitly named
- System actors (APIs, services) are identified if applicable

---

### Block 3 — Core Entities

**Purpose:** Identify the main data objects the system manages. These become the
foundation of the database schema and the domain model.

**Key questions:**
1. What are the main "things" the system keeps track of? (e.g. users, orders, projects)
2. For each entity: what are its most important attributes?
3. How do the entities relate to each other? (e.g. a User belongs to an Organization)
4. Are there any uniqueness constraints? (e.g. email must be unique per organization)
5. Are there entities that should never be deleted — only deactivated?

**Sufficiency criteria:**
- At least two entities identified with attributes
- At least one relationship between entities described
- Deletion vs. deactivation strategy mentioned

---

### Block 4 — Key Flows

**Purpose:** Understand how actors interact with the system to accomplish their
goals. Flows reveal hidden requirements, edge cases, and the real complexity
of the product. This block feeds directly into `user_flows.md`.

**Key questions:**
1. What is the most important flow a user goes through? Walk me through it step by step.
2. What happens when something goes wrong in that flow? (error cases)
3. Are there any flows that involve multiple actors? (e.g. an admin approves
   something a member requested)
4. Are there time-sensitive flows? (e.g. a token that expires, a session timeout)
5. Are there any flows that external systems or services are part of?

**Sufficiency criteria:**
- At least one complete flow described with a happy path and one error case
- Time-sensitive or expiry-based behavior identified if applicable
- External system interactions identified if applicable

---

### Block 5 — Technical Stack

**Purpose:** Establish the technical constraints that shape every implementation
decision. Some of these may have already been captured in `/init-project` —
if so, confirm them rather than asking again.

**Key questions:**
1. What is the primary programming language and framework?
2. What database engine will be used? Is it hosted or self-managed?
3. What ORM or data access layer will be used?
4. What is the API style? (REST, GraphQL, gRPC, etc.)
5. How will authentication work? (JWT, sessions, OAuth, etc.)
6. Are there any technology choices that are already decided and non-negotiable?

**Sufficiency criteria:**
- Language and framework confirmed
- Database and ORM confirmed
- Authentication mechanism identified
- Any non-negotiable tech constraints recorded

---

### Block 6 — Restrictions and Risks

**Purpose:** Identify anything that limits what can be built or how it can be
built. These constraints shape architecture decisions and must be captured
before design begins — not discovered mid-implementation.

**Key questions:**
1. Are there any legal or compliance requirements? (data privacy, audit logs, GDPR, etc.)
2. Are there security requirements beyond standard authentication?
   (e.g. encrypted at rest, key rotation, rate limiting)
3. Are there hosting or infrastructure constraints?
   (e.g. must run on-premise, specific cloud provider required)
4. Are there performance or scalability expectations?
5. Are there any third-party dependencies or integrations that are required?
6. What is the biggest technical risk you see in this project right now?

**Sufficiency criteria:**
- At least one constraint or risk identified (even if "none known yet")
- Security requirements beyond basic auth are explicitly addressed
- Any mandatory integrations are named

---

### Block 7 — What This Product Is NOT

**Purpose:** Explicitly drawing boundaries prevents the most common failure mode
in solo projects — scope creep driven by "while we're at it" thinking.
What the product will NOT do is as important as what it will do.

**Key questions:**
1. What features or capabilities might seem natural but are explicitly out of scope?
2. Is there a related problem this product is NOT trying to solve?
3. Are there user types this product is NOT serving?
4. Are there things this product will delegate to another system?
   (e.g. "email sending is handled by a separate service")

**Sufficiency criteria:**
- At least two explicit "not in scope" boundaries stated
- At least one delegation to an external system identified (or explicitly none)

---

### Block 8 — Initial Phases

**Purpose:** Translate the product definition into a prioritized build sequence.
This block produces the first version of `development_plan.md`.
Prioritization uses **MoSCoW**: Must Have, Should Have, Could Have, Won't Have (for now).

**Key questions:**
1. If you could only ship one thing first, what would it be and why?
2. What are the next 3-5 most important capabilities after that?
3. Are there any technical foundations that must exist before any product
   feature can be built? (e.g. database schema, auth layer, API structure)
4. Are there any capabilities that are desirable but not critical for the
   first version?
5. Is there a natural sequence where one thing must exist before another?

**Sufficiency criteria:**
- At least one "must have first" item identified
- Technical foundation phases separated from feature phases
- At least one item explicitly deferred to a later phase

---

### Step 3 — Generate the draft

After all blocks are complete:

1. Consolidate all answers into `.claude/tmp/project-definition-draft.md`
   using this structure:

```markdown
# Project Definition Draft — {PROJECT_NAME}

> Generated by /define on {date}
> Status: Draft — pending /define-approve

---

## Block 1 — Product Identity

**Name:** {value}
**Summary:** {value}
**Problem statement:** {value}
**Out of scope:** {value}
**Type:** {greenfield / replacement / extension}

---

## Block 2 — Actors and Users

{Table or list of actors with goals}

---

## Block 3 — Core Entities

{Entities with attributes and relationships}

---

## Block 4 — Key Flows

{Flows with happy path and error cases}

---

## Block 5 — Technical Stack

| Layer | Technology |
|---|---|
| Language | {value} |
| Framework | {value} |
| Database | {value} |
| ORM | {value} |
| API style | {value} |
| Auth | {value} |

---

## Block 6 — Restrictions and Risks

{List of constraints, compliance requirements, risks}

---

## Block 7 — What This Product Is NOT

{List of explicit out-of-scope boundaries and delegations}

---

## Block 8 — Initial Phases

### Must Have (Phase 1 candidates)
{List}

### Should Have (Phase 2+ candidates)
{List}

### Could Have (Deferred)
{List}

### Technical Foundation Required First
{List}

---

## Pending Items

{Any fields marked [PENDING — needs clarification] during the session}

---

## Proposed Phase Sequence

{Ordered list of proposed phases derived from Block 8, with one-line goal each}
```

2. Append to `activity.log`:
   `[{timestamp}] /define: Definition session complete. Draft saved to .claude/tmp/project-definition-draft.md`

3. Inform the user:
   "Definition session complete. Draft saved to .claude/tmp/project-definition-draft.md

   Next steps:
   - Run **/define-adjust** to refine any part of the draft
   - Run **/define-approve** to confirm the draft and prepare for documentation generation"

---

## Restrictions

- Do not generate any `docs/` files during this command
- Do not modify `CLAUDE.md`, `README.md`, or any project file
- Do not proceed to `/define-generate` automatically — wait for `/define-approve`
- All output in English
