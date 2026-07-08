# CLAUDE.md — Project Instruction Template

> Fill in your project's details. Keep sections that apply; remove the rest.
> This file is the primary authority for AI agents working in this repository.

---

## Project Overview

**{{PROJECT_NAME}}** — (one-line description of what this project is and does.)

### Tech Stack

- **Backend**: {{BACKEND_FRAMEWORK}} ({{BACKEND_LANGUAGE}})
- **Frontend**: {{FRONTEND_FRAMEWORK}}
- **Database**: {{DATABASE}}
- **Cache/Queue**: {{CACHE_QUEUE}}
- **Infra**: {{INFRA_TOOLS}}

---

## Documentation Map

### Core Docs

- `CLAUDE.md`: this file — primary authority
- `docs/IMPLEMENTED_BASELINE.md`: what is actually implemented now (treat as more authoritative than design docs)
- `docs/CODE_MAP.md`: feature-to-file navigation
- `docs/PITFALLS.md`: known pitfalls and traps

### Todo Docs (three-layer planning)

- `docs/todos/single-iteration-todolist.md`: active execution slice (only todo that drives implementation)
- `docs/todos/mid-term-feature-todolist.md`: feature-cluster planning
- `docs/todos/project-todolist.md`: long-range roadmap

### Workflow Docs (`.agents/workflows/`)

- `todo-driven-planning.md`: decomposition, approval, maintenance, cleanup
- `fast-lane.md`: lightweight lane for docs-only and low-risk changes
- `spec-driven-delivery.md`: proposal/spec/tasks for cross-boundary work
- `fix-bug-workflow.md`: bug fix with rollback decision points
- `full-stack-verify.md`: layered verification matrix
- `verification-failure-recovery.md`: recovery for repeated failures
- `doc-sync-closeout.md`: doc sync checklist
- `review-doc-changes.md`: doc review by value vs context burden

---

## Instruction Priority

1. User's current explicit instructions
2. This file (`CLAUDE.md`)
3. Repository docs
4. Workflow files in `.agents/workflows/`
5. General default conventions

---

## Task-Size Routing

- **docs-only / metadata / wording / 1–2 file mechanical change**: Fast lane
- **single-module code change**: Standard lane
- **cross-module / contract / risky change**: Spec lane
- **todo-driven**: follow `todo-driven-planning.md`

---

## Architecture Rules

(Describe your project's non-negotiable architecture constraints here.)

### (Constraint 1)

**Rule**: (what must be followed)

**Why**: (why this matters)

---

## Current Phase Scope

**Goal**: (what the current phase delivers)

### Must Deliver

- (item)
- (item)

### Deferred

- (item intentionally not built yet)

---

## Verification Standards

### Endpoints

- `/health` and `/ready` on every service
- Structured errors with appropriate HTTP status codes

### Testing

- Every new endpoint: happy path + validation error test
- Every new service/component: contract or integration test

### Environment

- `.env.example` maintained; never commit real `.env`
- All config from environment variables with sensible defaults

---

## Local Development

### Prerequisites

(List required tools and versions.)

### Setup

```bash
# {{SETUP_COMMANDS}}
```

### Run

```bash
# {{RUN_COMMANDS}}
```

---

## Git Practices

- Daily integration; no long-running personal branches
- Commit messages describe what and why
- Run relevant verification before pushing
- Key interfaces documented — no verbal-only API contracts
