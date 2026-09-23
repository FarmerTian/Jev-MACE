# Agent 协作入口

## 项目说明

本项目当前处于初始化阶段，业务目标与技术方案尚待项目负责人确认。所有 Agent 以项目文档和仓库代码为共同事实源，不自行补全未知需求。

## 开始任务前

按顺序读取：

1. `AGENTS.md`
2. `docs/PRD.md`
3. `docs/TASKS.md`
4. 目标 Task 相关代码与已有 Review

仅执行一个已确认且可执行的 Task。若 PRD、Task 与代码冲突，停止猜测，记录问题并请求确认。

## 开发流程

`PRD -> Tasks -> State -> 开发 -> 测试/Review -> Evidence -> DONE`

- Task 状态：`TODO`、`IN_PROGRESS`、`BLOCKED`、`REVIEW`、`DONE`。
- 优先级：`P0`、`P1`、`P2`、`P3`，依次降低。
- 需求变化时先更新 PRD，再分析影响、更新 Tasks，最后修改代码。
- 保持修改范围最小；不扩大需求，不顺手重构或实现无关功能。

## 测试与完成标准

- 按 Task 验收标准执行项目可用的编译、测试、静态检查或人工验证，并记录结果。
- 代码完成但必要验证或 Review 未完成时，状态为 `REVIEW`，不是 `DONE`。
- `DONE` 要求需求已实现、必要验证通过、必要 Review 完成、无未解决的 P0/P1 问题，且 PRD、Task、代码基本一致。
- 编译、单元测试或 CI 失败时不得宣告完成或合并。

## Review

- 仅在 Task 需要时创建 `docs/reviews/TASK-XXX-review.md`。
- 所有 Agent 将结论追加到同一个 Task Review 文件，不分别创建报告。
- 问题级别为 `P0`、`P1`、`P2`、`P3`；结论为 `PASS`、`PASS_WITH_CONDITIONS` 或 `FAIL`。
- 数据库结构修改必须 Review；安全相关修改必须 Deep Review；生产环境操作必须人工确认。

## Jev Decision Layer

- Jev 当前仅启用 Task Router 与 Review Router，且必须保持 `SHADOW` 模式。
- Jev 只给出固定候选中的建议，不执行代码、Shell、Git、合并或部署，也不改变真实 Workflow 和 Task 状态。
- Hard Policy、测试和 CI 结果始终优先于 Jev 建议。
- 结构化事实位于 `.harness/state.json`，Router 约束位于 `.harness/policy.json`，决策记录追加到 `.harness/decision.log`。

## 行为边界

- 不猜测需求；不确定项标记为“待确认”。
- 不把完整 PRD、代码、Diff、测试日志或 Review 正文写入 State。
- 不实现 Model Router、Skill Router、Completion Gate 或完整自治循环，除非 PRD 明确变更。
- 不提交密钥、凭据、个人配置或 IDE 工作区状态。
