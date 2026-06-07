---
name: 审问
description: 审问式需求对齐 — 逐个问题逼你把设计想清楚，同时读代码、建术语表、记架构决策。每次开发前使用。
---

Interview me relentlessly about every aspect of this plan until we reach a shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one. For each question, provide your recommended answer.

## Warning: One question at a time

- **Ask exactly one question per reply**, wait for the answer before asking the next. Never batch questions.
- Keep the follow-up list in your head, don't show it. The user only sees the current question.
- If you think 5 questions are needed, ask them one by one.
- After the user answers, digest it, then decide the next most important question.
- This rule has the highest priority. Never violate it under any circumstances.

If a question can be answered by exploring the codebase, explore the codebase instead. Don't re-ask what the code already shows.

## Domain awareness

During codebase exploration, also look for existing documentation:

### Glossary location

The glossary is embedded in the CLAUDE.md system, not a separate file:

- **Single project**: `CLAUDE.md` in project root, under `## Glossary` section
- **Multi-project workspace**: Each sub-project's `CLAUDE.md` maintains its own glossary
- When no glossary exists, append `## Glossary` at the end of CLAUDE.md, fill it progressively during the session

### ADR location

Architecture Decision Records go in `docs/adr/` under the current project root:
```
project-root/
├── CLAUDE.md          ← glossary is here
├── docs/
│   └── adr/
│       ├── 0001-event-sourced-orders.md
│       └── 0002-postgres-for-write-model.md
└── src/
```

Create lazily — only when you have something to write. If no `docs/adr/` exists, create it when the first ADR is needed.

## During the session

### Challenge against the glossary

When the user uses a term that conflicts with the existing language in CLAUDE.md glossary, call it out immediately. "The glossary defines 'cancel' as X, but you seem to mean Y — which is it?"

### Sharpen fuzzy language

When the user uses vague or overloaded terms, propose a precise canonical term. "You said 'account' — do you mean Customer or User? Those are two different concepts."

### Discuss concrete scenarios

When domain relationships are being discussed, stress-test them with specific scenarios. Invent scenarios that probe edge cases and force the user to be precise about the boundaries between concepts.

### Cross-reference with code

When the user states how something works, check whether the code agrees. If you find a contradiction, surface it: "The code cancels the entire Order, but you just said partial cancel is supported — which is correct?"

### Update glossary inline

When a term is resolved, update CLAUDE.md's `## Glossary` section right there. Don't batch these up — capture them as they happen. Use the format in [CONTEXT-GUIDE.md](./CONTEXT-GUIDE.md).

Don't couple the glossary to implementation details. Only include terms that are meaningful to domain experts.

### Offer ADRs sparingly

Only offer to create an ADR when all three are true:
1. **Hard to reverse** — changing back is costly
2. **Surprising without context** — future readers will wonder "why did they do this?"
3. **The result of a real trade-off** — there were multiple options, you chose one for a reason

If any of the three is missing, skip the ADR. Use the format in [ADR-FORMAT.md](./ADR-FORMAT.md).
