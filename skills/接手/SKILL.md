---
name: 接手
description: 接手 — 从交接队列中拾取一个任务，展示内容后确认删除并继续执行。
argument-hint: [optional] file name prefix (e.g. 20260607-131329). Picks the oldest if omitted.
---

Pick up a task from the handoff queue.

## Read rules

1. Check the `handoff/` directory <!-- adapt: point to your handoff queue location -->
2. If the user gave a prefix argument (e.g. `20260607-131329`), find the file starting with that prefix (`startswith` match); if not found, error out and list all available files
3. If no prefix argument, sort by filename and pick the newest one (FILO)
4. If the directory is empty, tell the user "no tasks to pick up" and end
5. If a file exists:
   a. Read the file content, display it fully to the user
   b. Wait for user confirmation (say "ok", "continue", "confirm", etc.)
   c. After confirmation, delete the file
   d. Follow the instructions in the file (switch to the "working directory" specified, execute the "goal" task)

## Core principles

- Pick up one at a time, no batch processing
- Display full content, don't abbreviate
- Only delete the file after user confirmation
- If the user rejects (says "no", "skip", etc.), don't delete, keep in queue
