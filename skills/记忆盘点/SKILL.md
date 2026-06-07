---
name: 记忆盘点
description: 记忆盘点 — 扫描会话记录，统计 memory/knowledge 使用频率，提议升降级并执行转化。
argument-hint: （无需参数）
---

Memory/knowledge usage audit and lifecycle management.

## 执行步骤

### 1. 运行扫描脚本

<!-- adapt: point to your audit script, or implement your own scanning logic -->
Run a scan script that analyzes session records for memory/knowledge references, outputting a JSON report.

### 2. 当前 session 补录

Review the current session's conversation history, find memory/knowledge entries you referenced. For each referenced entry:
- Find it in the JSON report
- If its `lastReferenced` is not today, update to today
- `sessionsReferenced` +1

This is non-intrusive — only recorded when the skill runs, not a global behavior.

### 3. 展示报告

Display the report to the user in the following format:

```
# Memory Audit — {scanDate}

## Overview
- Scanned {totalSessionsScanned} session records
- Memory entries: {N}
- Knowledge entries: {M}

## Memory Entries

| Entry | Session Refs | Days Span | Last Ref | Indexed | Suggestion |
|-------|-------------|-----------|----------|---------|------------|
| {name} | {n} | {days} | {lastRef} | Y/N | suggestion or — |

## Knowledge Entries

| Entry | Session Refs | Days Span | Last Ref | Suggestion |
|-------|-------------|-----------|----------|------------|
| {name} | {n} | {days} | {lastRef} | suggestion or — |

## Suggested Actions

### Demotion candidates (Memory → Knowledge, 7+ days unreferenced)
{list or "none"}

### Promotion candidates (Knowledge → Memory, 3+ sessions referenced)
{list or "none"}

### Index issues
{orphan/stale or "none"}
```

### 4. 用户确认

Ask the user: "Which actions to execute?" Wait for user reply:
- "Execute all" → execute all suggestions
- Specify particular actions (e.g. "demote entry-a", "promote entry-b") → only execute specified ones
- "Skip" → end

### 5. 执行转化

#### Demotion (Memory → Knowledge)

1. Read the memory file content
2. Remove memory-specific frontmatter fields, keep name and description
3. Categorize: general knowledge → `knowledge/general/`; project knowledge → `knowledge/projects/`
4. Write to target knowledge file
5. Delete the original memory file
6. Update the memory index: remove that entry
7. Update the knowledge index: add that entry

#### Promotion (Knowledge → Memory)

1. Read the knowledge file content
2. Add memory frontmatter (name, description, metadata with node_type: memory, type: reference)
3. Write to memory directory
4. Update the memory index: add an entry
5. Knowledge file is preserved (knowledge is the source of truth)

#### Fix index

- **Orphan** (file exists but not in index): add entry to index
- **Stale** (in index but file doesn't exist): remove entry from index

### 6. Report results

After execution, list all completed operations:
- "Demoted: xxx → knowledge/general/"
- "Promoted: xxx → memory"
- "Fixed index: xxx"

## Core principles

- Data-driven: all suggestions based on session scan data, not guessing
- User confirmation: must get user confirmation before any file operations
- Idempotent: repeated runs won't cause data loss (knowledge files are never deleted, memory demotion preserves content in knowledge)
- No git commit: only file operations, commit is handled separately
