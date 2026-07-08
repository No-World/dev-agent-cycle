---
description: Full-stack verification checklist — fill in your project's actual commands
---

# Full-Stack Verify

> **Project owners**: replace the placeholders below with your actual test/lint/type-check commands.
> Remove sections that don't apply to your tech stack.

Run this workflow after any non-trivial code change to ensure nothing is broken.
Use `verification-failure-recovery.md` when repeated failures, regressions, or rollback-and-restart decisions need a separate handling path.

Default rule: match verification depth to the execution lane.

- `Fast lane` -> `Quick verify`
- `Standard lane` -> `Standard verify`
- `Spec lane` -> `Full verify`

If a narrower lane stops matching reality, upgrade the verification depth immediately.

## Quick verify (Fast lane, targeted, ~1 min)

Use the narrowest relevant command that directly exercises the touched surface.

1. Backend targeted test:
   ```
   # {{BACKEND_TEST_RUNNER}} path/to/test_file
   # Example: cd backend && pytest tests/path/to/test.py -v
   # Example: cd backend && go test ./path/to/package
   ```

2. Frontend targeted test:
   ```
   # {{FRONTEND_TEST_RUNNER}} path/to/spec
   # Example: cd frontend && npm test -- --run path/to/spec
   ```

3. Targeted lint:
   ```
   # {{LINT_RUNNER}} path/to/file
   # Example: cd frontend && npx eslint path/to/file
   ```

4. Docs-only / workflow-only changes:
   - no build or test command is required by default
   - run `review-doc-changes.md` instead

## Standard verify (Standard lane, host-first, ~2-4 min)

Use this for ordinary single-module or medium-complexity changes.

5. Backend unit tests:
   ```
   # {{BACKEND_FULL_TEST_CMD}}
   # Example: cd backend && pytest
   # Example: cd backend && go test ./...
   ```

6. Backend type check (if applicable):
   ```
   # {{BACKEND_TYPE_CHECK_CMD}}
   # Example: cd backend && mypy .
   ```

7. Frontend lint:
   ```
   # {{FRONTEND_LINT_CMD}}
   # Example: cd frontend && npm run lint
   ```

8. Frontend type-check:
   ```
   # {{FRONTEND_TYPE_CHECK_CMD}}
   # Example: cd frontend && npm run type-check
   ```

9. Frontend unit tests:
   ```
   # {{FRONTEND_TEST_CMD}}
   # Example: cd frontend && npm test -- --run
   ```

Choose the relevant subset when the change is clearly frontend-only or backend-only. If runtime, contract, or infra risk appears, upgrade to `Full verify`.

## Full verify (Spec lane or elevated risk, container-first, ~5-8 min)

Prefer container-based commands for build/test/debug verification. Use host commands only when the container path is unavailable or when you are intentionally iterating faster before the final run.

10. Backend unit tests in container:
    ```
    # {{BACKEND_CONTAINER_TEST_CMD}}
    # Example: docker compose -f deploy/docker-compose.yml -f deploy/docker-compose.test.yml --profile unit run --rm backend-unit
    ```

11. Frontend unit tests in container:
    ```
    # {{FRONTEND_CONTAINER_TEST_CMD}}
    # Example: docker compose -f deploy/docker-compose.yml -f deploy/docker-compose.test.yml --profile unit run --rm frontend-unit
    ```

12. Lint in container:
    ```
    # {{CONTAINER_LINT_CMD}}
    ```

13. Type-check in container:
    ```
    # {{CONTAINER_TYPE_CHECK_CMD}}
    ```

14. Integration tests (requires running services):
    ```
    # {{INTEGRATION_TEST_CMD}}
    # Example: docker compose -f deploy/docker-compose.yml -f deploy/docker-compose.test.yml --profile integration run --rm backend-integration
    ```

## Post-verify checklist

15. If the selected verification depth passes: proceed.
16. If a targeted `Quick verify` fails and the root cause is broader than the local change, upgrade to `Standard verify`.
17. If `Standard verify` exposes runtime, contract, or integration risk, upgrade to `Full verify`.
18. If backend fails: fix and re-run the relevant backend step.
19. If frontend fails: fix and re-run the relevant frontend step.
20. If integration fails: check infrastructure status first, then investigate service connectivity.
