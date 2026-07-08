---
description: Todo-driven planning workflow covering decomposition, approval gates, live maintenance, and completion cleanup
---

# Todo-Driven Planning

Use this workflow before any project modification that is supposed to be driven by repository todo documents.

## Phase 1: Read the todo layers in order

1. Read these files in order:
   - `docs/todos/single-iteration-todolist.md`
   - `docs/todos/mid-term-feature-todolist.md`
   - `docs/todos/project-todolist.md`

2. Apply this stop rule while reading:
   - If `single-iteration-todolist.md` already contains unfinished executable items, stop there.
   - If it does not, continue to `mid-term-feature-todolist.md`.
   - If `mid-term-feature-todolist.md` contains unfinished items, stop there and decompose the relevant cluster downward before using project-level todo items.
   - Only use `project-todolist.md` as the next planning source if the mid-term file has no executable unfinished items.

## Phase 2: Decompose to the active execution slice

3. If the relevant work only exists in `mid-term-feature-todolist.md`, decompose it into `docs/todos/single-iteration-todolist.md` before implementation.

4. If the relevant work only exists in `project-todolist.md`, decompose it into `docs/todos/mid-term-feature-todolist.md` first, then into `docs/todos/single-iteration-todolist.md` when the active execution slice is clear.

5. Enforce the context coherence rule:
   - one `single-iteration` slice must only contain one problem category
   - do not mix unrelated frontend, backend, infra, or documentation categories into the same active slice

## Phase 3: Approval gate

6. Present the current `docs/todos/single-iteration-todolist.md` to the user before implementation.

7. Do not start code or project-document changes until the user explicitly approves the current single-iteration slice.

8. Exception: editing `docs/todos/single-iteration-todolist.md` itself to draft, refine, or maintain the active slice is allowed before the slice is approved; that todo change must be reviewed together with the slice it describes.

9. If unfinished executable items already exist in `single-iteration-todolist.md` but the user has not approved them in the current task, treat implementation as blocked until approval is given.

## Phase 4: Maintain the active slice during execution

10. Update `docs/todos/single-iteration-todolist.md` in real time while work is in progress.

11. Do not wait until the task is complete to backfill status changes.

12. If the active slice belongs to an existing mid-term feature cluster, keep the relationship visible so completion can be written back correctly.

## Phase 5: Complete and clean up

13. Complete the required documentation updates for the finished slice, including any mid-term write-back that keeps the parent cluster accurate.

14. If documentation changed, run the mandatory documentation review before close-out.

15. Clear finished items from `docs/todos/single-iteration-todolist.md` so the file returns to active work only or an empty skeleton.

16. After the slice has been implemented, verified, documented, reviewed, and the active `single-iteration` file has been cleaned, create one git commit as the closing action unless the user explicitly says not to commit.

17. Only clear a mid-term cluster after the whole cluster is finished, canceled, or intentionally dropped.
