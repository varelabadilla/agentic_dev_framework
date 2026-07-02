# /plan-discard — Discard the Current Plan

## Usage

`/plan-discard`

## Process

1. Check if `.runbook/plan.md` exists.

2. If it does not exist, inform the user:
   "No plan file found in .runbook/. Nothing to discard."
   Stop.

3. If it exists, delete `.runbook/plan.md`.

4. Append to `activity.log`:
   `[{timestamp}] /plan-discard: plan.md discarded by user`

5. Inform the user:
   "Plan discarded. You can now run /plan phase-N again with updated context or different instructions."
