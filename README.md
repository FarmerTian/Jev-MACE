# Jev-MACE

**Multi-Agent Collaboration Environment** — 以结构化文档为事实源、以 Jev Decision Layer 为辅助决策层的多 Agent 协作框架。

---

## 项目概述

Jev-MACE 是一套让 Codex、Claude、Gemini、Hermes 等 AI Agent 围绕**同一套项目事实**协作的工程框架。它通过 PRD、Tasks 和 Evidence 管理需求、进度与完成状态，并在第一阶段以 **Shadow Mode** 引入 Jev Decision Layer，评估建议价值，而不干预现有工作流。

> **当前阶段**：项目处于初始化阶段，核心产品目标与技术方案待项目负责人确认（见 [TASK-001](docs/TASKS.md)）。

---

## 项目结构

```
Jev-MACE/
├── AGENTS.md              # Agent 协作入口与行为规范
├── docs/
│   ├── PRD.md             # 产品需求文档（唯一需求事实源）
│   ├── TASKS.md           # 任务池与进度追踪（唯一进度来源）
│   └── reviews/           # Task Review 文件（按需创建）
└── .harness/
    ├── state.json          # 结构化决策状态
    ├── policy.json         # Hard Policy 与 Router 约束
    └── decision.log        # Jev 决策记录（JSONL 格式）
```

---

## 核心机制

### 开发流程

```
PRD → Tasks → State → 开发 → 测试/Review → Evidence → DONE
```

需求变化时：`更新 PRD → 分析影响 → 更新 Tasks → 开发与验证`

### Task 状态

| 状态 | 含义 |
|------|------|
| `TODO` | 待开始 |
| `IN_PROGRESS` | 进行中 |
| `BLOCKED` | 被阻塞，需要解除阻塞条件 |
| `REVIEW` | 代码完成，等待验证或 Review |
| `DONE` | 需求实现、验证通过、Review 完成 |

### 优先级

`P0`（最高）→ `P1` → `P2` → `P3`（最低）

---

## Jev Decision Layer（Shadow Mode）

Jev 是框架内置的决策辅助层，当前处于 **Shadow Mode**，即只给出建议、不执行任何操作。

| Router | 候选输出 |
|--------|----------|
| Task Router | `PROCEED` / `SPLIT` / `BLOCK` / `REVIEW` |
| Review Router | `NO_REVIEW` / `NORMAL_REVIEW` / `DEEP_REVIEW` / `MULTI_REVIEW` |

**关键约束：**

- Hard Policy 始终优先于 Jev 建议。
- Jev 不执行代码、Shell、Git、合并或部署操作。
- Jev 不自动修改 Task 状态。
- 每次决策以 JSON Lines 格式追加到 `.harness/decision.log`。

**当前禁用：** `MODEL_ROUTER`、`SKILL_ROUTER`、`COMPLETION_GATE`、`AUTONOMOUS_LOOP`

---

## Hard Policy（强制规则）

| 条件 | 强制要求 |
|------|----------|
| 编译失败 | 禁止标记为 `DONE` |
| 单元测试失败 | 禁止标记为 `DONE` |
| CI 失败 | 禁止合并 |
| 数据库结构修改 | 必须 Review |
| 安全相关修改 | 必须 Deep Review |
| 生产环境操作 | 必须人工确认 |

---

## 当前任务状态

| Task | 优先级 | 状态 | 说明 |
|------|--------|------|------|
| [TASK-001](docs/TASKS.md) | P0 | BLOCKED | 确认产品范围与技术基线（需项目负责人介入） |
| [TASK-002](docs/TASKS.md) | P1 | TODO | 建立最小可运行项目骨架（依赖 TASK-001） |
| [TASK-003](docs/TASKS.md) | P1 | TODO | 接入 Jev Shadow Router（依赖 TASK-001、002） |
| [TASK-004](docs/TASKS.md) | P2 | TODO | 评估 Jev 第一阶段效果（依赖 TASK-003） |

---

## Agent 参与指南

1. 开始任务前，按顺序读取 `AGENTS.md` → `docs/PRD.md` → `docs/TASKS.md` → 相关代码。
2. 仅执行一个已确认且可执行的 Task。
3. PRD、Task 与代码冲突时，停止猜测，标记"待确认"并请求项目负责人确认。
4. 不扩大需求，不顺手重构或实现无关功能。

---

## 待确认事项

以下内容尚未由项目负责人确认，Agent 不得自行补全：

- 产品要解决的具体问题、目标用户和核心使用场景
- 首期业务功能范围与明确的非目标
- 技术栈、运行环境、数据模型和外部接口
- Jev 的具体 SDK/API 接入方式、输入契约和部署方式
- 项目统一的构建、测试、静态检查和 CI 命令
