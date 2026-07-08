---
description: Review new or changed documentation by weighing information value against context burden
---

# Review Document Changes

Use this workflow after documentation changes are completed, or when the user explicitly asks for review of specific documents.

## Scope

This workflow applies to all project knowledge files:

- `CLAUDE.md`, `AGENTS.md`, `README.md`
- `docs/CODE_MAP.md`, `docs/PITFALLS.md`, `docs/IMPLEMENTED_BASELINE.md`
- `docs/todos/` (all layers)
- ADRs, workflow docs, process docs, and other project knowledge files

## Review Standard

Judge every changed document by balancing information value against model context burden.

Each item must end in exactly one outcome:

- `pass`: value clearly exceeds context burden, and the content is accurate enough to retain.
- `fix`: value exceeds context burden, but there are clear omissions, drift, ambiguity, duplication, or incorrect guidance that should be corrected.
- `delete`: value does not justify the context burden, or the content is mostly noise, repetition, stale detail, editor residue, or unnecessary maintenance load.

## Phase 1: Establish document role

1. Identify what the document is supposed to do:
   - authority source
   - navigation aid
   - workflow guide
   - decision record
   - temporary process tracker

2. Identify the intended reader:
   - human maintainer
   - coding agent
   - reviewer
   - operator

3. Check whether the document belongs in long-lived project context at all.

## Phase 2: Evaluate value

4. Ask whether the document improves at least one of these:
   - executability
   - verifiability
   - discoverability
   - authority clarity
   - decision stability

5. Prefer content that helps future work happen faster or more safely:
   - clear entry points
   - stable decision boundaries
   - concise workflows
   - reusable pitfalls
   - explicit update triggers

## Phase 3: Evaluate context burden

6. Mark context-heavy content that does not earn its cost:
   - repeated instructions already stated elsewhere
   - stale timestamps or drifted claims
   - editor metadata or generated residue
   - low-signal prose that does not change execution
   - overlong checklists without clear trigger conditions
   - dangerous commands presented as defaults

7. Treat duplicated or weakly differentiated content as a deletion candidate unless it serves a distinct authority boundary.

## Phase 4: Cross-check against authority docs

8. Cross-check the reviewed doc against the nearest authority sources when relevant:
   - `docs/IMPLEMENTED_BASELINE.md`
   - `docs/CODE_MAP.md`
   - `docs/PITFALLS.md`
   - `CLAUDE.md` or `AGENTS.md`

9. If a document conflicts with implemented reality or a stronger authority document, classify it as `fix` or `delete`, not `pass`.

## Phase 5: Produce findings

10. Report findings first, ordered by severity.

11. For each finding:
    - identify the file and location
    - assign exactly one label: `pass`, `fix`, or `delete`
    - explain the reason in terms of value vs context burden

12. If the result is `fix`, state what should change, not just that it is imperfect.

13. If the result is `delete`, explain why maintenance cost or context burden is not justified.

14. If no meaningful issues are found, state that explicitly and note any remaining minor risks or follow-up checks.
