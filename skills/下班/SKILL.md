---
name: 下班
description: 日终下班 — 读 scratch + 全量扫 git → 更新所有项目卡片 → 生成收工报告 → git commit → 清空 scratch。每天结束时使用。
---

执行日终下班工作流。这是一个完整的日终盘点流程。

## 执行步骤

1. 读取项目注册表获取全部项目列表 <!-- adapt to your project structure -->
2. 读取今天的流水账 `reports/daily-YYYY-MM-DD.md`，汇总今天做了什么
3. 全量扫描所有项目的 git 状态（`git log --since="today"` 或类似），确认每个项目的实际变更
4. 更新所有相关项目卡片的状态（当前进度、最新待办、阻塞项）
5. 生成收工报告 `reports/wrap-up-YYYY-MM-DD.md`，包含：
   - 今日完成事项
   - 各项目进度变化
   - 明日待办
   - 阻塞项汇总
6. Git commit：`docs: wrap-up YYYY-MM-DD — update project cards and report`
7. 清空 scratch 文件（当天流水账已归档到收工报告中）

## 规则

- 必须在管理项目目录下执行 <!-- adapt: e.g. Guster root -->
- 如果今天的流水账为空，仍然扫描 git 并更新卡片
- commit 前确认所有变更文件都是文档类（.md），不提交非文档文件
- 如果某个项目无变化，跳过该卡片更新

## 自驱模块扩展步骤（可选）

<!-- adapt: if using self-drive module, include these steps -->

### 步骤 A：日级反思

在生成收工报告之后、git commit 之前执行。

读取以下数据：
- `self-drive/experiences/` 中今天的经验文件
- 今天各项目的收工流水
- 刚生成的收工报告

执行日级反思，写入 `self-drive/reflections/daily/YYYY-MM-DD.md`：
- **今日概览**：做了什么，时间分布
- **工作分布评估**：是否合理
- **跨项目模式**：不同项目间的共性问题
- **经验汇总**：从 experiences/ 提炼最值得记住的 2-3 条教训
- **明日关注**：明天最需要关注的事

### 步骤 B：仪表盘更新

基于今天的执行数据和反思结果，更新 `self-drive/dashboard.md`：
- 更新「最后更新」日期
- 更新本周的维度评分（1-5 分）
- 更新「待改进项」和「知识空白」
