---
description: Proposal/spec/tasks workflow for cross-boundary, high-risk, or multi-agent changes
---

# Spec-Driven Delivery

Use this workflow before implementing a task that crosses module boundaries or changes important contracts.

## When to use

Use `Spec lane` when any of the following is true:

- frontend and backend both change
- API / schema / auth / infra contracts change
- runtime-sensitive behavior changes
- multiple agents will work in parallel
- the change is acceptance-sensitive or hard to roll back

`Spec lane` adds design artifacts. It does not replace repository todo rules. Once the slice is clear, continue to use `todo-driven-planning.md` for active execution tracking.

## Artifact location

Create one directory per task:

`docs/specs/YYYY-MM-DD-<topic>/`

Required files:

- `proposal.md`
- `spec.md`
- `tasks.md`

## Phase 1: Proposal

1. State the problem, scope, and non-goals.
2. List the main constraints and risks.
3. Compare viable approaches briefly.
4. Record the recommended approach and approval status.

## Phase 2: Spec

5. Freeze the important boundaries before implementation:
   - architecture and ownership boundaries
   - data flow
   - API / schema / auth / infra contracts
   - migration, rollback, and observability expectations
   - acceptance and verification criteria
6. Reference stronger authority docs instead of copying large amounts of overlapping detail.
7. Get explicit user approval on the proposal/spec direction before implementation whenever the task is still decision-sensitive or changes external contracts.

## Phase 3: Tasks

8. Break implementation into coherent tasks with:
   - write scope
   - dependency order
   - verification path
   - integration owner
9. If multiple agents will be used, freeze their write scopes before dispatch.

## Phase 4: Implement

10. After the proposal/spec/tasks set is stable enough, create or update the active execution slice via `todo-driven-planning.md`.
11. Implement against the frozen boundaries. If the boundary itself must change, update the spec first.

## Phase 5: Verify and close out

12. Default to `Full verify` from `full-stack-verify.md`.
13. Run `doc-sync-closeout.md`.
14. If documentation changed, run `review-doc-changes.md`.
15. Deliver both the implementation result and the spec artifact location.
