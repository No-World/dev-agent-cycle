# Dev Agent Cycle

> 通用 PM 驱动的多 Agent 开发闭环 — 架构师、工程师、评审、测试。

[English](README.md) | 简体中文

一个 Claude Code 技能，将 Claude 转变为产品经理（PM），编排结构化、有门控的多 Agent 开发闭环。支持三条执行车道（Fast / Standard / Spec）、Todo 驱动规划、多层评审门控、验证与失败恢复、以及文档同步。

---

## 安装

克隆到 skills 目录即可。

---

## 快速开始

安装后，用以下触发词启动：

- `启用多 Agent 模式` / `多 Agent 闭环`
- `架构师->评审->工程->测试`
- `你作为产品经理，组织多 Agent 执行`
- `走快速车道` / `走标准流程` / `走 Spec 车道`
- `一轮完成后通知我`
- `按标准流程交付这个需求`

---

## 工作流程

```
任务分类 → 检查 Harness 健康 → Todo 对齐 → 执行 → 验证 → 文档同步
```

### 三条执行车道

| 车道 | 适用场景 | 仪式感 |
|------|---------|--------|
| **Fast** 🚀 | 纯文档、措辞、元数据、1–2 文件机械变更 | 极简 — 单 Agent，无产物 |
| **Standard** 📋 | 单模块代码，不涉及契约变更 | Todo 驱动，轻度评审 |
| **Spec** 🏗️ | 跨模块、API/Schema/Auth 契约变更 | 完整多 Agent 4 角色门控闭环 |

### Spec 车道多 Agent 闭环

核心特性 — 4 角色门控闭环：

```
架构师设计方案
    ↓
评审 Gate A（可行性、范围）
    ↓ 通过
工程师实现
    ↓
评审 Gate B（正确性、回归）
    ↓ 通过
测试编写计划 + 脚本
    ↓
评审 Gate C（覆盖率、可执行性）
    ↓ 通过
测试执行
    ↓ 通过
闭环完成 → 通知用户
```

**角色：** 高级架构师 · 高级工程师 · 高级评审 · 高级测试

**任何门控都不能跳过。** 每次驳回必须包含严重级别（S0–S3）和修正要求。

---

## 特性

- **三车道路由** — 为每个任务匹配恰当的流程仪式感
- **Todo 驱动规划** — 三层 backlog（单轮 → 中期 → 项目级）
- **门控评审交接** — 4 角色闭环，评审门不可跳过
- **验证失败恢复** — 分类、继续/停止决策、回滚协议、补丁螺旋检测
- **文档同步** — 每次变更后检查清单驱动的文档更新
- **双语支持** — 中英文触发词和模板
- **项目引导** — 自动生成项目 harness 文件（CLAUDE.md、docs、todos、workflows）

---

## 项目结构

```
dev-agent-cycle/
├── SKILL.md                 # 技能定义（Agent 的指令）
├── README.md                # 英文说明文档
├── README.zh.md             # 中文说明文档
└── templates/               # 项目引导模板
    ├── CLAUDE.md            # 项目指令模板
    ├── docs/                # 文档模板
    │   ├── IMPLEMENTED_BASELINE.md
    │   ├── CODE_MAP.md
    │   └── PITFALLS.md
    ├── todos/               # 三层 Todo 模板
    │   ├── single-iteration-todolist.md
    │   ├── mid-term-feature-todolist.md
    │   └── project-todolist.md
    └── workflows/           # 流程工作流模板
        ├── fast-lane.md
        ├── spec-driven-delivery.md
        ├── todo-driven-planning.md
        ├── full-stack-verify.md
        ├── verification-failure-recovery.md
        ├── fix-bug-workflow.md
        ├── doc-sync-closeout.md
        └── review-doc-changes.md
```

