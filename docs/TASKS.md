# Tasks

本文件是唯一任务池和项目进度来源。优先级顺序为 `P0 -> P1 -> P2 -> P3`。

## 任务列表

### TASK-001 确认产品范围与技术基线

- 对应 PRD：项目目标、用户角色、功能模块、待确认
- 优先级：P0
- 状态：BLOCKED
- 任务说明：由项目负责人确认产品问题、目标用户、首期范围、非目标、技术栈、运行环境及 Jev 接入方式，并据此更新 PRD。
- 验收标准：
  - PRD 不再以“待确认”代替产品目标、角色和首期业务范围。
  - 技术栈、运行环境和 Jev 接入契约已有可执行结论。
  - 如范围变化，已同步新增、修改或废弃受影响 Task。
- 阻塞原因：仓库没有 README、业务代码或既有产品/设计资料，Agent 不得猜测。
- 验证：项目负责人确认 PRD 内容。

### TASK-002 建立最小可运行项目骨架

- 对应 PRD：待确认的首期业务范围、技术栈与运行环境
- 优先级：P1
- 状态：TODO
- 任务说明：在 TASK-001 完成后，按确认的技术方案建立最小可运行骨架及统一验证命令。
- 验收标准：
  - 项目可按文档启动或构建。
  - 至少提供一个统一的编译/测试/静态检查入口。
  - 不包含未经 PRD 确认的业务功能。
- 依赖：TASK-001
- 验证：待技术栈确认后补充精确命令。

### TASK-003 接入 Jev Shadow Router

- 对应 PRD：Jev Decision Layer（第一阶段）
- 优先级：P1
- 状态：TODO
- 任务说明：在 Jev 接入方式确认后，将 Task Router 和 Review Router 接入真实任务流程；现阶段已有状态与策略契约，不执行真实模型调用。
- 验收标准：
  - 两个 Router 仅返回各自固定候选。
  - Hard Policy 可阻止不合规动作，Jev 建议不能覆盖它。
  - Router 只记录建议，不自动修改 Task、执行代码、Git、合并或部署。
  - 决策按 JSON Lines 追加到 `.harness/decision.log`。
- 依赖：TASK-001、TASK-002
- 验证：覆盖固定输出、Hard Policy 优先级、Shadow Mode 无副作用及日志字段的自动测试。

### TASK-004 评估 Jev 第一阶段效果

- 对应 PRD：Jev Decision Layer（第一阶段）
- 优先级：P2
- 状态：TODO
- 任务说明：累计 20～50 个真实 Task 后，评估 Decision Accuracy、Token Saving、Latency Saving 和 Missed Risk。
- 验收标准：
  - 数据来自真实 Decision Log。
  - Missed Risk 被优先分析并记录。
  - 扩大 Jev 权限前形成明确结论；没有证据时继续保持 Shadow Mode。
- 依赖：TASK-003 及足量真实样本
- 验证：评估结果可由 Decision Log 重算。

## 进度汇总

| 状态 | 数量 |
| --- | ---: |
| TODO | 3 |
| IN_PROGRESS | 0 |
| BLOCKED | 1 |
| REVIEW | 0 |
| DONE | 0 |
| **总数** | **4** |
