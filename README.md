# Dev Agent Cycle

> Generic PM-led multi-agent development cycle — architect, engineer, reviewer, tester.

English | [简体中文](README.zh.md)

A Claude Code skill that transforms Claude into a project manager (PM) who orchestrates a structured, gated multi-agent development cycle. Supports three execution lanes (Fast / Standard / Spec), todo-driven planning, multi-layer review gates, verification with failure recovery, and documentation sync.

---

## Installation

Clone into your skills directory.

---

## Quick Start

Once installed, trigger the skill with any of these phrases:

- `Use a multi-agent cycle` / `PM-led delivery cycle`
- `Run architect → review → engineer → test`
- `Fast lane this` / `Standard lane this` / `Spec lane this`

---

## How It Works

```
Task Classification → Harness Health Check → Todo Alignment → Execute → Verify → Doc Sync
```

### Three Execution Lanes

| Lane | For | Ceremony |
|------|-----|----------|
| **Fast** 🚀 | Docs-only, wording, 1–2 file mechanical changes | Minimal — single agent, no artifacts |
| **Standard** 📋 | Single-module code, no contract changes | Todo-driven, light review |
| **Spec** 🏗️ | Cross-module, API/schema/auth changes | Full multi-agent 4-role cycle with gated reviews |

### Spec Lane Multi-Agent Cycle

The flagship feature — a 4-role gated cycle:

```
Architect design
    ↓
Reviewer Gate A (feasibility, scope)
    ↓ approve
Engineer implementation
    ↓
Reviewer Gate B (correctness, regression)
    ↓ approve
Tester plan + scripts
    ↓
Reviewer Gate C (coverage, executability)
    ↓ approve
Tester execution
    ↓ pass
Cycle Complete → notify user
```

**Roles:** Senior Architect · Senior Engineer · Senior Reviewer · Senior Tester

**No gate can be skipped.** Every rejection includes severity (S0–S3) and required corrections.

---

## Features

- **Three-lane routing** — right-size ceremony for every task
- **Todo-driven planning** — three-layer backlog (single-iteration → mid-term → project)
- **Gated review handoffs** — 4-role cycle with non-skippable reviewer gates
- **Verification failure recovery** — classify, decide continue/stop, rollback protocol, patch-spiral detection
- **Documentation sync** — checklist-driven doc updates after every change
- **Bilingual** — English + Chinese trigger phrases and templates
- **Bootstrap** — auto-generates project harness files (CLAUDE.md, docs, todos, workflows)

---

## Project Structure

```
dev-agent-cycle/
├── SKILL.md                 # Skill definition (the agent's instructions)
├── README.md                # English readme
├── README.zh.md             # Chinese readme
└── templates/               # Skeleton files for project bootstrapping
    ├── CLAUDE.md            # Project instruction template
    ├── docs/                # Documentation templates
    │   ├── IMPLEMENTED_BASELINE.md
    │   ├── CODE_MAP.md
    │   └── PITFALLS.md
    ├── todos/               # Three-layer todo templates
    │   ├── single-iteration-todolist.md
    │   ├── mid-term-feature-todolist.md
    │   └── project-todolist.md
    └── workflows/           # Process workflow templates
        ├── fast-lane.md
        ├── spec-driven-delivery.md
        ├── todo-driven-planning.md
        ├── full-stack-verify.md
        ├── verification-failure-recovery.md
        ├── fix-bug-workflow.md
        ├── doc-sync-closeout.md
        └── review-doc-changes.md
```

