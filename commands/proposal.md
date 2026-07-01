# /proposal — Generate a Structured Project Proposal

## Usage

`/proposal {brief description of the idea}`

Example: `/proposal Add rate limiting to auth endpoints`

Use this command when exploring a new feature, capability, or change before committing to a formal phase. The proposal is informed by the existing codebase and documentation — it is not a blank template.

## Process

1. Parse `$ARGUMENTS` to extract the idea description.
   If no description is provided, ask the user for one before proceeding.

2. Read the following files in full:
   - `docs/development_plan.md` — understand existing phases and what is already planned
   - `docs/decision_log.md` — identify decisions that constrain or inform this idea
   - `docs/information_architecture.md` — understand entity relationships affected
   - `docs/technical_decisions.md` — identify relevant pending or resolved decisions
   - `CLAUDE.md` — apply project conventions

3. Search the `src/` directory for files relevant to the idea.
   Read any directly relevant service, module, or controller files.

4. Assess proposal readiness:
   - Is the idea specific enough to define concrete tasks?
   - Are there unresolved dependencies or conflicts with existing decisions?
   - Are there missing details the user should clarify before this becomes a phase?

   If the proposal lacks sufficient detail to generate useful tasks:
   - List specifically what is missing
   - Ask the user to provide the missing information before continuing

5. Determine the next available proposal number by reading `docs/proposals/` if it exists.
   Use zero-padded three-digit numbering: `001`, `002`, etc.

6. Generate the proposal document:

   ```markdown
   # Proposal — {Title}

   > proposal-{NNN} | {date}

   ## Summary

   {2-3 sentences describing what this proposal adds or changes and why.}

   ## Motivation

   {What problem does this solve? What value does it add?}

   ## Affected Areas

   {List of files, modules, entities, or flows likely affected.}

   ## Proposed Approach

   {High-level description of the implementation approach.
   Not a full phase plan — enough to validate the direction.}

   ## Open Questions

   {Unresolved decisions or clarifications needed before this becomes a phase.
   If none, write "None".}

   ## Risks and Constraints

   {Known risks, dependencies on other phases, or technical constraints.}

   ## Related Decisions

   {Links to relevant D-XXX entries in decision_log.md, or "None".}

   ## Status

   Draft — pending review
   ```

7. Create `docs/proposals/` if it does not exist.

8. Write the proposal to `docs/proposals/proposal-{NNN}.md`.

9. Append to `activity.log`:
   `[{timestamp}] /proposal: proposal-{NNN}.md created — {brief description}`

10. Present the proposal to the user and inform them:
    "Proposal saved to docs/proposals/proposal-{NNN}.md

    Next steps:
    - Review and refine the proposal in Claude.ai for architectural discussion
    - When ready to formalize as a phase, run: /proposal-to-phase docs/proposals/proposal-{NNN}.md"
