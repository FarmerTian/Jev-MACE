# Jev-MACE 产品需求文档

## 项目目标

待确认：仓库当前没有 README、业务代码或既有设计文档，无法从现有事实确定产品目标。

已确认的工程目标：

- 使用 PRD、Tasks 和 Evidence 管理需求、进度与完成状态。
- 支持 Codex、Claude、Gemini、Hermes 等 Agent 围绕同一套项目事实协作。
- 第一阶段以 Shadow Mode 引入 Jev Decision Layer，只评估建议价值，不控制现有工作流。

## 用户角色

- 项目负责人：确认需求、处理阻塞问题，并对高风险或生产操作作最终决策。
- 开发与审核 Agent：按 PRD 和 Tasks 执行开发、测试与 Review。
- 待确认：最终产品用户及其业务角色。

## 功能模块

### 项目事实管理

- PRD 是需求唯一事实源。
- Tasks 是统一任务池和唯一进度来源。
- 测试、CI 与 Review 结果构成完成证据。

### Jev Decision Layer（第一阶段）

- Task Router：只在 `PROCEED`、`SPLIT`、`BLOCK`、`REVIEW` 中给出建议。
- Review Router：只在 `NO_REVIEW`、`NORMAL_REVIEW`、`DEEP_REVIEW`、`MULTI_REVIEW` 中给出建议。
- 两个 Router 均运行于 Shadow Mode，不改变真实 Task 状态或执行流程。
- Hard Policy 始终优先于 Jev 建议。
- 每次真实决策应记录 Task、决策点、建议、置信度、实际动作、最终结果和时间戳，以便后续评估。

## 业务规则

- 编译失败或单元测试失败时，Task 不得进入 `DONE`。
- CI 失败时不得合并。
- 数据库结构修改必须 Review。
- 安全相关修改必须 Deep Review。
- 生产环境操作必须人工确认。
- 第一阶段不实现 Model Router、Skill Router、Completion Gate 或完整自治循环。

## 核心流程

`PRD -> Tasks -> State -> Policy（Hard Rules 优先，Jev 仅建议）-> Action -> Evidence -> DONE`

需求变化时：`更新 PRD -> 分析影响 -> 更新 Tasks -> 开发与验证`。

## 验收标准

- 项目目标、目标用户和首期业务范围经项目负责人确认并写入本文件。
- 每项开发工作都能追溯到 `docs/TASKS.md` 中的 Task 和本 PRD 的对应条目。
- Jev 只运行在 Shadow Mode，Router 输出受固定候选约束，且不能覆盖 Hard Policy。
- State 只保存决策所需的结构化事实，Decision Log 可用于评估准确率、节省量、延迟与漏判风险。
- Task 只有在必要测试和 Review 提供足够 Evidence 后才能标记为 `DONE`。

## 待确认

- 产品要解决的具体问题、目标用户和核心使用场景。
- 首期业务功能范围与明确的非目标。
- 技术栈、运行环境、数据模型和外部接口。
- Jev 的具体 SDK/API 接入方式、输入契约和部署方式。
- 项目统一的构建、测试、静态检查和 CI 命令。
