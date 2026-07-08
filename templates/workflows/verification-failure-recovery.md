---
description: Recovery workflow for repeated verification failures, patch spirals, and rollback-and-restart decisions
---

# Verification Failure Recovery

Use this workflow after running the relevant verification commands for a change. For the command matrix itself, use `full-stack-verify.md`.

## Phase 1: Classify the failure

1. Identify whether the failure is:
   - a direct bug in the current change
   - an environment or dependency problem
   - a regression introduced by the attempted fix
   - a broader structural issue that the current patch direction is not containing

2. Record the exact failing command, the observed failure, and the expected result before deciding what to do next.

## Phase 2: Decide whether to continue fixing or stop

3. Continue with the current attempt only if all of the following remain true:
   - the root cause is still understandable
   - the fix path is still local and coherent
   - the latest attempt did not introduce a new unrelated bug
   - the number of affected locations is still proportional to the original problem

4. Stop and re-evaluate immediately if any of the following happens:
   - the same category of failure appears 3 times in a row
   - the attempted fix introduces a new bug or regression
   - compensating for one bad change requires patching 3 or more unrelated locations
   - the current session is accumulating patches without a stable root-cause explanation

## Phase 3: Rollback-and-restart decision

5. If the stop conditions are met, do not keep layering patches in the same session.

6. Preserve the failure pattern first:
   - capture `git diff --stat`
   - note the root cause briefly in delivery notes
   - if the lesson is reusable, add it to `docs/PITFALLS.md`

7. Restart from a fresh session, worktree, or branch rooted at a known-clean commit instead of continuing the current patch spiral.

## Phase 4: Cleanup boundary

8. If workspace cleanup is needed, revert only the files owned by the failed attempt.

9. Any destructive rollback command must be explicitly confirmed by the user first.

10. Do not treat destructive cleanup as the default path just because the current attempt is messy.

## Phase 5: Recovery exit criteria

11. Resume normal implementation only after:
    - the root cause is restated clearly
    - the new attempt has a smaller and more coherent change boundary
    - the next verification path is known in advance
