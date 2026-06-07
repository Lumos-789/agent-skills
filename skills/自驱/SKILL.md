---
name: 自驱
description: 自驱主循环 — 完整 OODA 闭环，驱动自我改进。快速扫描、执行改进、或周回顾。
argument-hint: /自驱 [scan|execute|weekly-review]（default: scan）
---

Self-driving engine based on the OODA loop (Observe-Orient-Decide-Act) for continuous improvement.

## Modes

| Mode | Trigger | Behavior |
|------|---------|----------|
| Quick scan | `/自驱` or `/自驱 扫描` | Read → Assess → Output suggestions, no execution |
| Execute | `/自驱 执行` | Read → Assess → Decide → Execute autonomous items → List items needing confirmation |
| Weekly review | `/自驱 周回顾` | Full PDCA: weekly summary → dimension scores → next week goals → insight cleanup |

---

## Execution steps

### Phase 1: Observe (shared across all modes)

Read the following in parallel:

1. **Dashboard** — `self-drive/dashboard.md` <!-- adapt: your dashboard location -->
   - Current dimension scores and trends
   - Items to improve and knowledge gaps

2. **Unexecuted insights** — files with status "pending" in `self-drive/insights/`
   - Sorted by priority

3. **Project cards** — `projects/*.md` <!-- adapt: your project card location -->
   - Blockers and stall days
   - Latest todos

4. **Recent reflections** — last 3 from `self-drive/reflections/`
   - Extracted insights and suggestions

5. **Handoff queue** — `handoff/*.md` <!-- adapt: your handoff queue location -->
   - Any pending tasks to pick up

6. **External services** — provision tracker (if applicable)
   - High-priority pending items

7. **Experience pool recent activity** — last 7 days from `self-drive/experiences/`
   - High-salience experiences
   - Recurring patterns

### Phase 2: Orient

Analyze based on observations:

1. **Dimension analysis**: Which dashboard dimension is weakest? Persistent decline or one-time drop?
2. **Insight priority**: Among unexecuted insights, which has the biggest impact and is easiest to execute?
3. **Project urgency**: Which project needs attention most (longest blocker, longest stall)?
4. **Cross-project patterns**: Are there common issues across projects?
5. **Knowledge gaps**: Are there areas that come up repeatedly but aren't well understood?
6. **Skill health**: Have any skills been executing poorly or inefficiently?

Output a priority-sorted list of action candidates.

### Phase 3: Decide

Varies by mode:

#### Quick scan mode

Output assessment results and suggestions without executing. Format:

```
# Self-drive Scan — YYYY-MM-DD

## Dashboard Status
Overall score X/10, weakest dimension: {dimension} ({score}, trend {arrow})

## Pending Insights (N items)
1. [{priority}] {insight title} — {one-line description}
2. ...

## Project Urgency
1. {project name} — {reason}
2. ...

## Suggested Actions (not executed this run)
1. {action} — expected effect: {effect}
2. ...

## Knowledge Gaps
- {gap description}
```

#### Execute mode

Select 1-3 executable actions from the candidate list. Categorize:

**Execute directly (don't ask user)**:
- Update dashboard.md scores and data
- Update existing knowledge file content
- Write new experiences/insights
- Optimize existing skill instruction wording

**Add to todo, wait for user confirmation**:
- Add new skills or workflow steps
- Add new knowledge topic files
- Modify existing skill flow structure

**List for one-click confirmation**:
- Execute in batch after confirmation

After execution, update the dashboard with this run's actions and results.

#### Weekly review mode

Full PDCA cycle:

1. **Plan (review this week's goals)**:
   - Read last week's weekly review (if any)
   - How much of last week's goals were completed?

2. **Do (summarize this week's execution)**:
   - Count this week's experience files
   - Count this week's reflection files
   - Count insight status changes
   - Measure each project's git activity

3. **Check (assess dimension changes)**:
   - Update all dimension scores in dashboard.md
   - Calculate trends (this week vs last week vs week before)
   - Roll trend data table (if a new week has started)

4. **Act (set next week's goals)**:
   - Based on weakest dimensions, set 1-3 self-drive goals for next week
   - Clean up completed insights (status → "completed" or "abandoned")
   - Check if 30+ day old experiences need archiving

5. **Output weekly review report**:

```
# Self-drive Weekly Review — Week N (YYYY-MM-DD)

## This Week Overview
- New experiences: N
- New reflections: N
- Insight execution: N completed / N abandoned / N pending
- Project activity: {project} X commits, {project} Y commits...

## Dimension Changes
| Dimension | Last Week | This Week | Change | Reason |
|-----------|-----------|-----------|--------|--------|
| ...       | ...       | ...       | ↑↓→    | ...    |

## Highlights
- ...

## Issues
- ...

## Next Week Goals
1. {goal} — acceptance criteria: {criteria}
2. ...

## Insight Cleanup
- Completed: {list}
- Archived: {list}
- New: {list}
```

---

## Core principles

- **OODA speed over perfection**: Loop speed matters more than getting each decision perfect. Better to quickly execute an 80% solution than agonize over a 100% one
- **RPE-driven**: Focus on expected vs actual deviations; deviation = learning opportunity
- **Compound interest**: Daily small improvements beat occasional large ones
- **Curiosity-driven**: Identify knowledge gaps and explore proactively, don't wait for problems
- **Evidence first**: All assessments and decisions based on data (git log, file counts, experience records), not intuition
- **Do > Talk**: In execute mode, prioritize what you can do autonomously; fewer lists, more action
