# agent-skills

Reusable Claude Code skills for personal agent workflows.

**Design principle: Markdown is the skill.** Claude reads it and executes. No code, no runtime, no build step. Just structured instructions that an LLM agent understands and follows.

## Skills Overview

| Skill | Trigger | Description | Standalone |
|-------|---------|-------------|------------|
| **审问** (Grill) | `/审问` | One-question-at-a-time design interrogation. Builds glossary + ADRs. | Yes |
| **转手** (Handoff) | `/转手` | Write a handoff document to a FIFO queue for the next session. | Yes |
| **接手** (Take) | `/接手` | Pick up the oldest task from the handoff queue. | Yes |
| **记忆盘点** (Memory Audit) | `/记忆盘点` | Scan session logs, suggest memory/knowledge promotion/demotion. | Yes |
| **完事** (Done) | `/完事` | Append one line to today's scratch file. | Needs `reports/` |
| **收工** (Wrap-up) | `/收工 {project}` | Scan git + update project card + append to scratch. | Needs project structure |
| **下班** (EOD) | `/下班` | Full EOD: all cards, report, commit. | Needs full environment |
| **looklook** (Look Look) | `/lklk` | Check todos → parked ideas → wander the web. Never "nothing to do". | Yes |
| **整理现状** (Status) | `/整理现状` | Read-only snapshot of all project cards + git activity. | Needs project structure |

### Standalone vs Environment-dependent

**Standalone** — work with just a project directory and `CLAUDE.md`:
审问, 转手, 接手, 记忆盘点, looklook

**Environment-dependent** — expect a management hub with project cards, reports, scratch files:
完事, 收工, 下班, 整理现状

Look for `<!-- adapt -->` comments in each skill file to adapt to your own structure.

## Skill Design Patterns

- **Markdown-as-skill** — Each skill is a `SKILL.md` with YAML frontmatter + instructions. Claude reads and follows.
- **FIFO handoff queue** — Timestamped markdown files in a directory. Oldest = next task. No database, no server.
- **One-question-at-a-time** — The 审问 skill enforces strict one-question-per-reply discipline.
- **Layered wrap-up** — Three levels: `/完事` (one line) → `/收工 {project}` (git + card) → `/下班` (full audit).
- **OODA self-improvement** — Observe-Orient-Decide-Act as recurring agent behavior.

## Attribution

Some skills are adapted from [Matt Pocock's claude-code-skills](https://github.com/mattpocock/claude-code-skills):

| Skill | Source | Our changes |
|-------|--------|-------------|
| **审问** | Matt's `grill` | Added glossary building, ADR archiving, cross-referencing with code |
| **转手** | Matt's `handoff` | FILO queue (timestamped files, newest-first) instead of single-file |
| **looklook** | Matt's `look-look` | Adapted file paths and tone |

**接手** complements 转手 as the pickup side of the handoff queue.

## Origin

Extracted from [Guster](https://github.com/Lumos-789/Guster), a project management hub using Claude Code as CEO/project-manager agent.

## License

MIT
