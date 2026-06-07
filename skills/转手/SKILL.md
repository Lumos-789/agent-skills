---
name: 转手
description: 转手 — 把当前任务写成交接文档，存入中心队列。用「接手」拾取。
argument-hint: [optional] file name prefix (e.g. 20260607-131329). Auto-generates timestamp if omitted.
---

Write a handoff document summarising the current conversation so a fresh agent can continue the work. Save to the handoff queue directory — not a temporary directory.

## Write rules

1. Save files to the `handoff/` directory <!-- adapt: point to your handoff queue location -->
2. File name: if the user gave a prefix argument (e.g. `20260607-131329`), use `prefix-short-summary.md`; otherwise auto-generate `YYYYMMDD-HHMMSS-short-summary.md`
3. Include the following sections (pick and choose, don't force all):

```markdown
# Handoff: {one-line description}

## Goal
What the next session should do.

## Working directory
Which project directory to work in (e.g. `your-project/`). Leave blank if staying in the management hub.

## Context
Key information from the current session relevant to this task. Concise, don't pile up.

## Suggested skills
Skills the next session should invoke (e.g. /审问, etc.).

## References
Related file paths (PRDs, plans, ADRs, diffs, etc.) — reference, don't duplicate content.
```

## Core principles

- **Don't duplicate** content already in files (PRDs, plans, ADRs, issues, commits, diffs) — use paths or URLs
- **Sanitize**: remove API keys, passwords, PII
- **If the user provided a subject** (e.g. `/转手 refactor the recommendation engine`), organize the document around it
- **Working directory must be accurate**: reference paths from your project registry

## Attribution

Handoff concept from [Matt Pocock's `handoff` skill](https://github.com/mattpocock/claude-code-skills). Our innovation: FILO queue (timestamped filenames, newest-first pickup) instead of Matt's single-file approach.
