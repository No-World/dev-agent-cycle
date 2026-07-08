# CODE_MAP

Feature-to-file navigation map. Use this to find the right file before opening many files at random.

> **Status**: Skeleton — populate with your project's actual structure as it grows.

---

## Fast Entry Points

| What you want to do | Where to look |
|---------------------|---------------|
| (action) | (file path) |
| (action) | (file path) |

---

## Change Impact Matrix

When changing a file in the left column, check the right column for potential impact.

| If you change... | Also check... |
|------------------|---------------|
| (file/module) | (affected modules) |

---

## Directory Layout

```
project-root/
├── backend/
│   ├── main.py (or equivalent entry point)
│   ├── api/          # Route handlers
│   ├── models/       # Data models / ORM
│   ├── services/     # Business logic
│   └── tests/
├── frontend/
│   ├── src/
│   │   ├── api/      # API client
│   │   ├── components/
│   │   └── stores/
│   └── tests/
├── docs/
│   ├── todos/
│   └── specs/
├── deploy/
│   └── docker-compose.yml
└── .agents/
    └── workflows/
```

(Update this tree to match your actual layout.)

---

## Update Triggers

Update this file when:
- A new file is created that serves as a primary entry point for a feature
- File ownership changes (a feature moves to a different file)
- A new cross-module dependency is introduced (update Change Impact Matrix)
- The directory layout changes
