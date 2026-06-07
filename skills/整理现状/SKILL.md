---
name: 整理现状
description: 整理现状 — 全量只读快照，读取所有项目卡片和 git 活跃度，生成现状报告。
argument-hint: 日期（可选，默认今天）
---

全量只读快照：并行读取所有项目卡片 + 检查 git 活跃度 → 生成 `reports/status-YYYY-MM-DD.md`。

## 步骤

### 1. 读项目注册表

<!-- adapt: point this to your own project registry -->
读取项目注册表（如 `projects/_registry.md`），解析出所有项目（slug、显示名、路径）。

### 2. 并行读取所有项目卡片

对每个项目，读取对应的卡片文件，提取：
- 状态（规划中 / 开发中 / 测试中 / 已部署 / 维护中 / 已归档）
- 当前进度
- 最新待办
- 阻塞项

### 3. 并行检查 git 状态

对每个项目，在项目路径下运行：
```bash
git log --oneline -5 --format="%h %s (%cr)"
git status --porcelain | head -20
```
判断：最近 7 天有无活动、有无未提交变更。

### 4. 生成现状报告

输出到 `reports/status-YYYY-MM-DD.md`，结构：

```markdown
# Status Snapshot — YYYY-MM-DD

## Overview
One-line summary of workspace state.

## Project Status

| Project | Card Status | Git Activity | Needs Attention |
|---|---|---|---|
| ... | ... | ... | ... |

### ProjectA
- **Card**: current progress summary
- **Git**: recent activity
- **Todos**: top 3 items
- **Blockers**: list or "none"

(every project follows the same pattern)

## Cross-project Observations
Notable items at the global level.
```

## 规则

- **只读**：不修改任何项目卡片或文件（除了生成报告本身）
- 在管理项目目录下执行
