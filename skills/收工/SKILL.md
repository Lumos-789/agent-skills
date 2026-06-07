---
name: 收工
description: 项目级收工 — 完成一个项目工作时，扫描 git 活动，更新项目卡片，记一笔流水。
argument-hint: 项目名（可选，默认当前项目）
---

对一个项目做轻量收工：扫描 git 活动 → 更新项目卡片 → 追加一条流水到 scratch。

## 项目名映射

<!-- adapt: point this to your own project registry or config -->
读取项目注册表（如 `projects/_registry.md`）获取项目列表（slug、显示名、路径、别名）。用户输入的参数按别名和显示名模糊匹配。

## 步骤

### 1. 确定目标项目

- 如果用户指定了项目名，按注册表匹配
- 如果没指定，从当前工作目录（`pwd`）提取项目名，再按注册表映射
- 如果都推断不出来，问用户

### 2. 扫描 git 活动

在项目路径下运行：
```bash
git log --since="midnight" --oneline --format="%h %s"
git status --porcelain | head -20
```

总结今天做了什么（一句话）。

### 3. 更新项目卡片

<!-- adapt: point this to your own project card location -->
读取项目卡片（如 `projects/{slug}.md`），根据 git 活动更新：
- **当前进度**：补充今天完成的事项
- **最新待办**：移除已完成项，补充新发现的需要做的事
- **阻塞项**：更新已解决或新出现的阻塞

保持原有格式和风格不变，只更新事实性内容。写回文件。

### 4. 追加流水到 scratch

目标文件：`reports/daily-YYYY-MM-DD.md`（当天日期）<!-- adapt to your project structure -->

如果文件不存在，先创建，顶部加 `# 流水账 — YYYY-MM-DD`

追加一条记录，格式：
```markdown
- HH:MM **{项目}** — 收工：{git 活动一句话总结}
```

### 5. 经验提取（可选，配合自驱模块）

<!-- adapt: if using self-drive module -->
扫描本次收工涉及的 git diff 和流水内容，识别以下模式并自动写入经验目录：
- **意外发现**：没预料到的行为或结果
- **失败经历**：尝试了但没成功的事
- **有效做法**：特别有效、值得复用的做法
- **跨项目模式**：与其他项目类似的模式

如果本项目的 git diff 中没有值得记录的经验 → 跳过此步，不强制生成空经验。

### 约束

- **不**生成独立的收工报告
- **不** git commit
- **不**扫描其他项目
- **不**清空 scratch 文件
- 幂等：重复执行不会破坏已有内容
