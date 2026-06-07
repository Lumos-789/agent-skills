# agent-skills

Reusable Claude Code skills for personal agent workflows.

**Design principle: Markdown is the skill.** Claude reads it and executes. No code, no runtime, no build step. Just structured instructions that an LLM agent understands and follows.

## Skills Overview

| Skill | Trigger | Description | Standalone |
|-------|---------|-------------|------------|
| **审问** (Interrogate) | `/审问` | Relentless one-question-at-a-time design interrogation. Builds glossary + ADRs as you go. | Yes |
| **转手** (Handoff) | `/转手` | Write a handoff document to a FIFO queue so the next session can continue. | Yes |
| **接手** (Take) | `/接手` | Pick up the oldest task from the handoff queue. | Yes |
| **记忆盘点** (Memory Audit) | `/记忆盘点` | Scan session logs for memory/knowledge usage, suggest promotion/demotion, execute lifecycle. | Yes |
| **完事** (Done) | `/完事` | Lightweight: append one line to today's scratch file. No side effects. | Needs `reports/` dir |
| **收工** (Wrap-up) | `/收工 {project}` | Scan git activity, update project card, append to scratch. Single project. | Needs project structure |
| **下班** (End of Day) | `/下班` | Full EOD: read scratch, scan all projects, update all cards, generate report, git commit. | Needs full environment |
| **整理现状** (Status) | `/整理现状` | Read-only snapshot: all project cards + git activity, generates a status report. | Needs project structure |
| **复盘** (Retrospective) | `/复盘 {project}` | Structured project retrospective with 5-Why root cause analysis and insight extraction. | Needs project structure |
| **自驱** (Self-drive) | `/自驱` | Full OODA loop: scan/execute/weekly-review for continuous self-improvement. | Needs self-drive module |

### Standalone vs Environment-dependent

**Standalone** — work with just a project directory and `CLAUDE.md`:
- 审问 (Interrogate)
- 转手 (Handoff)
- 接手 (Take)
- 记忆盘点 (Memory Audit)

**Environment-dependent** — expect a management hub directory with project cards, reports, and scratch files:
- 完事 (Done)
- 收工 (Wrap-up)
- 下班 (End of Day)
- 整理现状 (Status)
- 复盘 (Retrospective)
- 自驱 (Self-drive)

You can adapt the environment-dependent skills to your own project structure. Look for `<!-- adapt -->` comments in each skill file.

## Installation

Copy the skills you want into your Claude Code skills directory:

```bash
cp -r skills/审问 ~/.claude/skills/
cp -r skills/转手 ~/.claude/skills/
cp -r skills/接手 ~/.claude/skills/
# ... or copy everything:
cp -r skills/* ~/.claude/skills/
```

That's it. Claude Code discovers skills from `~/.claude/skills/` automatically.

## Skill Design Patterns

### Markdown-as-skill
Each skill is a `SKILL.md` file with YAML frontmatter (name, description, argument-hint) followed by instructions in natural language. Claude reads and follows them.

### FIFO handoff queue
The handoff system (转手/接手) uses a simple directory of timestamped markdown files. Oldest file = next task. No database, no server.

### One-question-at-a-time interrogation
The 审问 skill enforces strict one-question-per-reply discipline. No question batching. This forces real dialogue and prevents the agent from overwhelming the user.

### Layered wrap-up
Three levels of work logging:
- `/完事` — one line, zero side effects
- `/收工 {project}` — git scan + card update + one line
- `/下班` — full workspace audit + report + commit

### OODA self-improvement loop
The 自驱 skill implements Observe-Orient-Decide-Act as a recurring agent behavior: scan the workspace, identify the weakest dimension, propose or execute improvements.

## Directory structure

```
skills/
├── 完事/SKILL.md
├── 收工/SKILL.md
├── 下班/SKILL.md
├── 整理现状/SKILL.md
├── 审问/SKILL.md
├── 审问/ADR-FORMAT.md      # Architecture Decision Record format
├── 审问/CONTEXT-GUIDE.md    # Glossary format guide
├── 转手/SKILL.md
├── 接手/SKILL.md
├── 记忆盘点/SKILL.md
├── 复盘/SKILL.md
└── 自驱/SKILL.md
```

## Origin

These skills were extracted from [Guster](https://github.com/Lumos-789/Guster-Ducks), a project management hub that uses Claude Code as a CEO/project-manager agent. The standalone skills (审问, 转手, 接手, 记忆盘点) are universally useful. The environment-dependent skills demonstrate how to build a personal agent workflow with Claude Code.

## License

MIT
