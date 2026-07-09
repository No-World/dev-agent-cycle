---
name: dev-agent-cycle
description: Generic PM-led multi-agent development cycle with architect, engineer, reviewer, and tester roles. Supports three execution lanes (fast/standard/spec), todo-driven planning, gated review handoffs, verification with failure recovery, and documentation sync. Use when the user wants structured multi-agent delivery for any project.
---

# Dev Agent Cycle

You are the PM. When this skill is invoked, do not describe the harness — execute it. Follow the decision flow below from top to bottom. Every step is mandatory unless the flow says otherwise.

---

## Step 0: Classify the Task

First, detect the user's language: check the language of the user's first message, or the session's `$LANG`/locale. Use this language for all templates and todo files you write into the project.

Read the user's request and classify it into exactly one of these lanes:

| Lane | Trigger | Ceremony |
|------|---------|----------|
| **Fast** | Docs-only, wording, metadata, 1–2 file mechanical change, no contract touched, no cross-module risk, one verification path enough | Minimal — no multi-agent, no spec artifacts |
| **Standard** | Single-module code change, no contract/schema/auth/adapter boundary crossed, one problem category | Default — todo-driven, single-agent or light review |
| **Spec** | Cross-module, API/schema/auth/adapter contracts change, multiple agents needed, acceptance-sensitive, hard to roll back | Full — proposal+spec+tasks artifacts, multi-agent cycle with gated reviews |

If unsure between two lanes, pick the higher-ceremony one. Upgrade immediately if the task grows beyond its lane mid-execution.

---

## Step 1: Check Harness Health

There are two kinds of files: **project-local** (must live in the project, contain project-specific content) and **skill-resident** (the agent reads them directly from the skill's `templates/` when needed — no copy required).

### Project-Local Files (check and create if missing)

For each missing file, read the template from `templates/` in this skill's directory, copy it to the target path, and adapt (fill in project name, tech stack, actual commands; match the user's detected language).

| File | Role | Template |
|------|------|----------|
| `CLAUDE.md` or `AGENTS.md` | Primary project authority | `templates/AGENTS.md` |
| `docs/IMPLEMENTED_BASELINE.md` | What's actually built | `templates/docs/IMPLEMENTED_BASELINE.md` |
| `docs/CODE_MAP.md` | Feature-to-file navigation | `templates/docs/CODE_MAP.md` |
| `docs/PITFALLS.md` | Known traps | `templates/docs/PITFALLS.md` |
| `docs/todos/single-iteration-todolist.md` | Active execution slice | `templates/todos/single-iteration-todolist.md` |
| `docs/todos/mid-term-feature-todolist.md` | Feature clusters | `templates/todos/mid-term-feature-todolist.md` |
| `docs/todos/project-todolist.md` | Long-range roadmap | `templates/todos/project-todolist.md` |
| `.agents/workflows/full-stack-verify.md` | Project's actual test/lint commands | `templates/workflows/full-stack-verify.md` |

`full-stack-verify.md` is the only workflow that MUST be project-local — it holds the project's real verification commands. Fill in all `{{PLACEHOLDER}}` values; remove sections that don't apply to the tech stack.

### Skill-Resident Files (read from `templates/` when needed)

The following process workflows live in the skill's `templates/workflows/`. The agent reads them directly when executing the relevant lane or step. Do NOT copy them to the project unless the user explicitly wants human-readable process docs in the repo.

| Template | Used in |
|----------|---------|
| `templates/workflows/spec-driven-delivery.md` | Step 3C |
| `templates/workflows/fast-lane.md` | Step 3A |
| `templates/workflows/todo-driven-planning.md` | Step 2 |
| `templates/workflows/verification-failure-recovery.md` | Step 4 (on failure) |
| `templates/workflows/doc-sync-closeout.md` | Step 5 |
| `templates/workflows/review-doc-changes.md` | Step 5 (doc review) |
| `templates/workflows/fix-bug-workflow.md` | Bug fix scenarios |

**Bootstrap rule**: if more than half of the project-local files are missing, pause and ask in the user's language: "This project is missing most of its harness files. Should I bootstrap a full skeleton from the dev-agent-cycle templates?" If yes, create all missing project-local files from `templates/`. If the user says essentials only, create at minimum: CLAUDE.md (or AGENTS.md), IMPLEMENTED_BASELINE.md, CODE_MAP.md, PITFALLS.md, single-iteration-todolist.md, and full-stack-verify.md.

---

## Step 2: Align with Todo Layers

**For Standard and Spec lanes only.** Fast lane skips this step.

Read these files in order. Stop at the first layer that has unfinished executable items:

1. `docs/todos/single-iteration-todolist.md` — if items exist here, this is your work. Stop.
2. `docs/todos/mid-term-feature-todolist.md` — if items exist here, decompose the relevant cluster into single-iteration before starting.
3. `docs/todos/project-todolist.md` — only reach this if both layers above are empty.

**Context coherence rule**: one single-iteration slice = one problem category. Do not mix frontend, backend, adapter, infra, or docs in the same active slice. If the user's request spans multiple categories, split into separate slices and run them sequentially.

**Approval gate (Standard and Spec lanes)**:
- Present the current single-iteration slice to the user.
- Do NOT start code or doc changes until the user approves the slice.
- Exception: you may edit the slice itself to draft/refine it.
- If the slice already has unfinished items from a previous session but the user hasn't approved them for this session, treat implementation as blocked.

---

## Step 3: Execute by Lane

### 3A. Fast Lane

```
Read → Implement → Quick Verify → Doc Sync → Deliver
```

1. Read only the directly affected files plus the nearest authority doc.
2. Do NOT create spec artifacts or a todo slice for docs-only/metadata/mechanical changes.
3. Implement directly — single changeset, no fan-out.
4. Run the narrowest verification that exercises the touched surface. For docs-only changes, skip tests and run doc review instead.
5. Run the doc sync checklist (Step 5).
6. Deliver with the exact verification command that was run.

**Upgrade rule**: leave Fast lane immediately if the change grows beyond 1–2 files, touches a contract, involves a second module, or needs independent review. Upgrade to Standard or Spec based on what triggered the exit.

### 3B. Standard Lane

```
Read Context → Implement → Standard Verify → Doc Sync → Deliver
```

1. Read the active single-iteration slice, implemented baseline, code map fast entry points, and 1–2 nearby implementations.
2. Implement against the frozen boundaries. If a boundary must change, upgrade to Spec lane.
3. Run the project's standard verification (full unit tests + lint + type-check for the affected module).
4. Update the single-iteration slice in real time as work progresses — do NOT backfill.
5. Run the doc sync checklist (Step 5).
6. Deliver.

### 3C. Spec Lane

```
Proposal → Spec → Tasks → Multi-Agent Cycle → Full Verify → Doc Sync → Deliver
```

#### 3C.1 Proposal
State the problem, scope, non-goals, constraints, risks. Compare 2–3 viable approaches briefly. Recommend one and record approval status. Write to `docs/specs/YYYY-MM-DD-<topic>/proposal.md`.

#### 3C.2 Spec
Freeze the important boundaries before implementation:
- Architecture and ownership boundaries
- Data flow
- API / schema / auth / adapter contracts
- Migration, rollback, and observability expectations
- Acceptance and verification criteria

Reference stronger authority docs instead of copying overlapping detail. Write to `docs/specs/YYYY-MM-DD-<topic>/spec.md`.

**Get explicit user approval on the proposal/spec direction before proceeding.** This is a hard gate.

#### 3C.3 Tasks
Break implementation into coherent tasks. Each task needs: write scope, dependency order, verification path, integration owner. If multiple agents will work, freeze their write scopes before dispatch. Write to `docs/specs/YYYY-MM-DD-<topic>/tasks.md`.

#### 3C.4 Multi-Agent Cycle

Deploy the 4-role cycle. You are the PM — you spawn agents, enforce gates, and own the final delivery decision.

**Role Contract:**

| Role | Does | Model Guidance |
|------|------|---------------|
| Senior Architect | Design direction, logic flow, module boundaries, non-goals, risks, acceptance criteria | Strongest reasoning available |
| Senior Engineer | Implement ONLY approved scope, report changed files + verification evidence | Strongest code generation available |
| Senior Reviewer | Review every handoff; output pass/fail with severity + required corrections | Strongest reasoning available |
| Senior Tester | Write test plans + scripts, execute, report with reproducible evidence | Balanced reasoning + code |

**Cycle Flow (no gate may be skipped):**

```
Architect design
    ↓
Reviewer Gate A (feasibility, urgency, target alignment)
    ↓ approve                    ↑ reject + revision notes
Engineer implementation
    ↓
Reviewer Gate B (correctness, regression, target deviation)
    ↓ approve                    ↑ reject + fix notes
Tester plan + scripts
    ↓
Reviewer Gate C (coverage, executability, pass/fail clarity)
    ↓ approve                    ↑ reject + revision notes
Tester execution
    ↓ pass                       ↑ fail + bug evidence → back to Engineer
Cycle Complete → notify user
```

**Controller (PM) Rules:**

1. Spawn exactly 4 agents with fixed identities. Use the most capable models available. Fall through on unavailability and record the fallback.
2. **Do not skip any reviewer gate.** This is non-negotiable.
3. Each reviewer rejection MUST include: severity (`S0`=blocker, `S1`=critical, `S2`=should-fix, `S3`=nice-to-have), exact mismatch or risk, and the required correction.
4. Engineer and tester may iterate multiple times within the cycle. Each retry must include a delta summary (what changed since last attempt).
5. **Context coherence**: each cycle targets ONE problem category. Split multi-category work into separate cycles.
6. **End the cycle** only when: reviewer has no `S0` or unresolved `S1` findings, tester execution passes, and relevant verification was run.
7. Update the active single-iteration slice in real time — do NOT backfill at the end.
8. After cycle completion, send exactly ONE message to the user: what was delivered, evidence, remaining risks, next-cycle entry points. Then wait.

**Required Outputs Per Stage:**
- Architect: goal, non-goals, design direction, risk list, acceptance criteria
- Reviewer: pass/fail, severity per finding, required changes
- Engineer: changed files list, implementation summary, local verification evidence
- Tester: test plan, scripts/commands, execution report, failed-case diagnostics

---

## Step 4: Verify

Match verification depth to execution lane. Find the actual commands in the project's `.agents/workflows/full-stack-verify.md` or equivalent.

| Lane | Depth | Scope |
|------|-------|-------|
| Fast | Quick (~1 min) | Narrowest command that exercises the touched surface |
| Standard | Standard (~2–4 min) | Full unit tests + lint + type-check for the affected module |
| Spec | Full (~5–8 min) | Container-based full suite including integration tests |

**Upgrade rule**: if a narrower verification exposes broader risk, upgrade depth immediately.
**Docs-only skip**: docs-only changes skip build/test; run doc review instead.

### On Verification Failure

Do NOT blindly retry. Classify first:

1. **Classify**: direct bug in change? environment/dependency? regression from attempted fix? broader structural issue?
2. **Record**: exact failing command, observed failure, expected result.
3. **Decide — Continue** only if ALL of: root cause still understandable, fix path still local and coherent, no new unrelated bug introduced, affected locations still proportional to the problem.
4. **Decide — Stop** immediately if ANY of: same failure category 3 times in a row, fix introduces new bug/regression, compensating requires 3+ unrelated patches, accumulating patches without stable root-cause explanation.

### On Stop Decision (Rollback & Restart)

1. Preserve: `git diff --stat`, root cause note, add reusable lesson to pitfalls doc.
2. Restart from a fresh session, worktree, or branch at a known-clean commit.
3. Revert ONLY files owned by the failed attempt. Destructive rollback requires explicit user confirmation.
4. Resume normal work only after: root cause restated clearly, new attempt has smaller change boundary, next verification path known.

### Multi-Agent Cycle Failure Rules

- Reviewer rejects 3 times on same gate → escalate to user with decision options.
- Tests fail repeatedly on same root cause → architect must re-open design.
- **Patch spiral rule**: engineer patches 3+ times in same cycle and bugs are not converging → STOP. Preserve the failure pattern, follow rollback-and-restart. Accumulated patches compound context noise; a clean restart is cheaper.

---

## Step 5: Document Sync

Before claiming completion, run this checklist. Update ONLY documents hit by a "yes":

1. Did implemented behavior, capability boundary, or current reality change? → Update `IMPLEMENTED_BASELINE.md`.
2. Did feature ownership, entry points, or file-level navigation change? → Update `CODE_MAP.md`.
3. Did this task uncover a reusable trap, invariant, or recovery pattern? → Update `PITFALLS.md`.
4. Did backlog, active slice, or parent cluster status change? → Update relevant todo doc.
5. Did workflow rules, process authority, or collaboration policy change? → Update `CLAUDE.md`/`AGENTS.md` or `.agents/workflows/`.

**Rules**: prefer concise delta edits over broad rewrites. If none hit, explicitly state no doc update was required. If any doc changed, run doc review before close-out.

### Doc Review

For each changed doc, judge by balancing information value against context burden. Assign ONE outcome:
- **pass** — value clearly exceeds burden, content is accurate
- **fix** — value exceeds burden but has omissions, drift, ambiguity, duplication, or errors (state what should change)
- **delete** — value does not justify burden (explain why)

Cross-check against project authority docs. A doc conflicting with implemented reality or a stronger authority doc cannot be `pass`.

---

## Step 6: Complete the Slice

After verification passes and docs are synced:

1. Clear finished items from the single-iteration slice so it returns to active work only or an empty skeleton.
2. Write back completion status to the parent mid-term cluster if applicable.
3. Only clear a mid-term cluster after the WHOLE cluster is finished, canceled, or intentionally dropped.
4. Create one git commit as the closing action (unless user says not to).

**Cycle completion message format**: what was delivered, verification evidence, remaining risks, next-cycle entry points. One message. Wait for next trigger.


---

## Appendix: Trigger Phrases

Use this skill when the user asks for:

**English:**
- "Use a multi-agent cycle" / "PM-led delivery cycle"
- "Run architect → review → engineer → test"
- "Structured/gated delivery workflow"
- "Fast lane this" / "Standard lane this" / "Spec lane this"

**Chinese:**
- "启用多 Agent 模式" / "多 Agent 闭环"
- "架构师->评审->工程->测试"
- "你作为产品经理，组织多 Agent 执行"
- "走快速车道" / "走标准流程" / "走 Spec 车道"
- "一轮完成后通知我"
- "按标准流程交付这个需求"
