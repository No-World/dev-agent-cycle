---
description: Minimal documentation sync checklist to keep docs accurate without broad rewrites
---

# Doc Sync Closeout

Run this workflow before claiming a task is complete.

The goal is to update only the documents that the change actually touched, instead of broad rewriting by habit.

## Checklist

1. Did implemented behavior, capability boundary, or current reality change?
   - If yes, update `docs/IMPLEMENTED_BASELINE.md`.

2. Did feature ownership, main entry points, or file-level navigation change?
   - If yes, update `docs/CODE_MAP.md`.

3. Did this task uncover a reusable trap, invariant, or recovery pattern?
   - If yes, update `docs/PITFALLS.md`.

4. Did backlog status, active slice status, or parent feature-cluster status change?
   - If yes, update the relevant todo document in `docs/todos/`.

5. Did workflow rules, process authority, or collaboration policy change?
   - If yes, update the project's primary instruction file (`CLAUDE.md` or `AGENTS.md`) or the relevant `.agents/workflows/*.md` file.

6. (Project-specific) Did any project-specific contracts, interfaces, or critical boundaries change?
   - If yes, update the corresponding authority doc.

## Rules

7. Update only the documents hit by `yes` answers above.
8. If none of the checklist items are hit, state explicitly that no project-doc update was required.
9. Prefer concise delta edits over broad rewrites.
10. If any documentation changed, run `review-doc-changes.md` before close-out.
