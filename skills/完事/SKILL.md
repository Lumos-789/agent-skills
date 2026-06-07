---
name: 完事
description: 轻量事项收工 — 做完一个任务或要切换时，快速记一笔到当天流水账。
argument-hint: 做了什么？（可选：去哪个项目、接下来做什么）
---

Append a one-line entry to today's daily scratch file.

## 规则

1. 目标文件：`reports/daily-YYYY-MM-DD.md`（当天日期）<!-- adapt to your project structure -->
2. 如果文件不存在，先创建，顶部加 `# 流水账 — YYYY-MM-DD`
3. 追加一条记录，格式：

```markdown
- HH:MM **{项目}** — {做了什么} → {下一步}（如果有）
```

4. 项目名用简称（匹配你的项目列表）
5. 如果用户没说项目，标 **General**
6. 如果用户没说下一步，不加 `→` 部分
7. 只追加，不改已有内容
8. 不更新项目卡片、不 git commit、不扫 git log

## 示例输入/输出

用户：「完事 API 写完了，接下来做测试」
→ 追加：`- 14:32 **ProjectA** — 推荐 API 完成 → 写单元测试`

用户：「/完事 切到另一个项目，刚修了个热键 bug」
→ 追加：`- 16:05 **ProjectB** — 修复热键 bug`

用户：「完事 讨论了两级收工的设计」
→ 追加：`- 22:10 **General** — 讨论两级收工体系设计 → 实现 note skill 和改造日终收工`
