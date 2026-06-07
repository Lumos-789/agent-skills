---
name: 复盘
description: 项目复盘 — 结构化反思项目执行过程，提炼洞察，驱动自我改进。
argument-hint: /复盘 {project name} — defaults to the most recently active project
---

Execute a project-level retrospective. Structured reflection based on git activity, experience records, and project cards.

## Execution steps

### 1. Determine the retrospective target

- If the user specified a project name → find the matching project
- If not specified → read the project registry, select the project with git activity in the last 7 days
- If multiple projects are active → list them for the user to choose

### 2. Collect data

Read in parallel:
- Project card: `projects/{slug}.md` <!-- adapt: your project card location -->
- Recent experiences for this project from the experience directory (last 7 days)
- Recent wrap-up reports mentioning this project
- Git log for the last 7 days (in the project directory)
- Project-level reflection history (if any)

### 3. Structured analysis

Analyze across four dimensions:

#### 3.1 Execution assessment
- Goals vs actual completion
- Where did the plan deviate

#### 3.2 Process assessment
- What went well (worth keeping)
- What could improve (specific improvement points)
- What was unexpected

#### 3.3 Root cause analysis (if failures/deviations exist)
Use 5-Why method to drill down to root causes.

#### 3.4 Predictive coding
- What were the expectations before execution?
- Actual results vs expectations (RPE)?
- What does the deviation tell us about model corrections needed?

### 4. Extract insights

Extract 1-3 actionable insights from the analysis:
- If an insight is directly executable (small change) → auto-write to `insights/`
- If an insight involves major changes → write to `insights/` but mark as "pending confirmation"

### 5. Generate experience entries

If this retrospective discovered new experiences worth recording (not captured by earlier wrap-ups),
auto-write to the experience directory.

### 6. Display retrospective results

Use the following format:

```
# Retrospective — {project name}

## Execution Assessment
{analysis results}

## Process Highlights
- Good: {what went well}
- To improve: {what could be better}

## Root Cause Analysis (if applicable)
{5-Why results}

## Prediction Deviations
- Expected: {X}  Actual: {Y}  Reason: {Z}

## Extracted Insights
- {insight 1}
- {insight 2}

## Auto-written
- Experience: {filename}
- Insight: {filename}
```

## Rules

- The reflection process is fully autonomous, no user confirmation needed
- Experience and insight writes are append-only, just do it
- Insights involving modification of existing knowledge or skill files → mark as "pending confirmation", don't modify directly
- Be direct. Say what's good, say what's bad. No empty talk
- 5-Why should not stop at surface causes, drill at least 3 levels deep
