---
description: Standard bug fix workflow with rollback decision points to prevent patch spirals
---

# Fix Bug Workflow

## Phase 1: Understand the bug

// turbo
1. Read `docs/PITFALLS.md` Quick Index to check if this is a known pitfall

2. Reproduce the bug:
   - Identify the exact reproduction steps
   - Capture the actual vs expected behavior
   - Identify the error message, stack trace, or incorrect output

3. Locate the root cause:
   - Use `docs/CODE_MAP.md` Fast Entry Points to find relevant files
   - Use `rg` or `grep` to search for error messages, function names, or related code
   - Read at most 2-3 files before forming a hypothesis

## Phase 2: Plan the fix

4. Before writing code, answer these questions:
   - What is the root cause (not just the symptom)?
   - Which files need to change?
   - Could this fix break anything else? (check Change Impact Matrix in `docs/CODE_MAP.md`)
   - Is there an existing test that should have caught this?

## Phase 3: Implement

5. Make the smallest change that fixes the root cause
   - Prefer fixing the cause over patching the symptom
   - Do not refactor unrelated code in the same change

6. Add or update a test that covers the bug scenario
   - The test should fail before the fix and pass after

## Phase 4: Verify

// turbo
7. Run the project's test suite and lint/type-check as defined in `full-stack-verify.md`

## Phase 5: Rollback decision point

> **CRITICAL**: Evaluate after each fix attempt. Do NOT skip this step.

8. Check the rollback criteria:
   - Has this fix introduced a NEW bug? -> Consider rollback
   - Have you patched 3+ unrelated locations to compensate? -> Rollback required
   - Has the same category of failure appeared 3 times? -> Rollback required

9. If rollback is needed:
   - Capture `git diff --stat` and record the failure pattern in `docs/PITFALLS.md` if it is a reusable lesson
   - End the current session
   - Restart from a fresh session, worktree, or branch based on a known-clean commit
   - If you must clean the current workspace, revert only the files owned by the failed attempt; ask the user before any destructive git command

10. If fix is clean and tests pass:
    - Cross-check `docs/CODE_MAP.md` Change Impact Matrix
    - Update `docs/PITFALLS.md` if this bug reveals a new pitfall
    - Proceed to commit

## Post-fix

11. Update documentation:
    - `docs/PITFALLS.md` if new pitfall discovered
    - `docs/IMPLEMENTED_BASELINE.md` if behavior boundary changed
    - `docs/todos/single-iteration-todolist.md` progress
