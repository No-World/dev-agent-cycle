---
description: Lightweight execution lane for docs-only and other local low-risk changes
---

# Fast Lane

Use this workflow when speed matters more than ceremony and the change remains truly local.

## Eligibility

Use `Fast lane` only when all of the following remain true:

1. The change stays within `1-2` closely related files.
2. It does not change API / schema / auth / infra / routing contracts.
3. It does not introduce a new shared abstraction or responsibility boundary.
4. One direct verification path is enough.
5. No independent reviewer / tester gate is needed to reduce decision risk.

Typical fits:

- docs-only
- wording-only
- metadata-only
- local test fixes
- small UI polish that does not affect shared behavior
- dependency version bumps (patch-level)

## Workflow

1. Read only the directly affected files plus the nearest authority source.
2. Do not create or require a `single-iteration` slice for docs-only, metadata-only, or other local mechanical changes that meet the eligibility rules.
3. Implement directly.
4. Run `Quick verify` from `full-stack-verify.md`.
5. Run `doc-sync-closeout.md` and update only the documents hit by its checklist.
6. If documentation changed, run `review-doc-changes.md`.
7. Deliver the result with the exact verification that was run.

## Upgrade rules

Leave `Fast lane` immediately if any of the following appears:

- the change grows beyond `1-2` closely related files
- an API / schema / auth / infra / routing contract is touched
- a second module or responsibility boundary becomes involved
- one direct verification path is no longer enough
- the task now needs independent reviewer / tester gates

Upgrade target:

- `Fast -> Standard` for broader but still ordinary implementation work
- `Fast -> Spec` for cross-boundary or contract-sensitive work
