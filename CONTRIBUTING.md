# Contributing

## Workflow

1. Pick up an item from the Project board, move it to "In Progress",
   assign yourself.
2. Branch off `main`: `git checkout -b <sub-team>/<short-description>`
   (e.g. `autopilot/gps-integration`, `hull/pontoon-v1`).
3. Commit with clear messages. Reference the issue number
   (e.g. `Closes #7`) so it auto-closes on merge.
4. Open a pull request into `main`. At least your sub-team lead
   reviews before merge — for cross-team-affecting changes, tag the
   downstream sub-team lead too.
5. Move the board item to "Done" once merged.

## Before you start work that another sub-team depends on (or depends on)

Check `docs/decisions/` for an existing interface contract. If one
doesn't exist yet for the interface you're about to build against,
propose one using `docs/decisions/TEMPLATE.md` first.

## Governance / process questions

See `docs/GOVERNANCE.md` (leadership, decision-making, communication
cadence) and `docs/BUDGET.md` (expenses/reimbursement).
