# ADR Format

ADRs go in `docs/adr/` under the project root, sequentially numbered: `0001-slug.md`, `0002-slug.md`, etc.

Only create `docs/adr/` when needed.

## Template

```md
# {Short title for the decision}

{1-3 sentences: what's the context, what was decided, and why.}
```

That's it. An ADR can be a single paragraph. The value is in recording "what was decided" and "why" — not filling out a form.

## Optional sections

Only add when genuinely valuable. Most ADRs don't need these.

- **Status** (`proposed | accepted | deprecated | superseded by ADR-NNNN`) — useful when a decision might be revisited
- **Considered options** — when rejected alternatives are worth remembering
- **Consequences** — when non-obvious downstream effects need noting

## Numbering

Scan `docs/adr/` for the highest number, +1.

## When to write an ADR

All three conditions must be true simultaneously:

1. **Hard to reverse** — changing your mind is costly
2. **Surprising without context** — future code readers will wonder "why was it done this way?"
3. **Real trade-off existed** — there were actual options, you chose one with justification

If easily reversible, skip — just change it back later. If not confusing, nobody will ask why. If no alternatives existed, you "did the obvious thing" — no record needed.

### What kind of decisions are worth recording

- **Architecture shape.** "Use monorepo." "Write model is event-sourced, read model projected to Postgres."
- **Cross-context integration patterns.** "Ordering and Billing communicate via domain events, not sync HTTP."
- **Lock-in tech choices.** Database, message bus, auth scheme, deployment target. Not every library — only ones that take a quarter to swap.
- **Boundary and scope decisions.** "Customer data is owned by Customer context, others reference by ID only." Explicit "not doing" is as valuable as "doing."
- **Deliberate deviation from the obvious path.** "We use hand-written SQL instead of ORM because X." Anywhere a reasonable reader would assume the opposite. Prevents the next engineer from "fixing" an intentional decision.
- **Constraints invisible in code.** "Can't use AWS due to compliance." "Response time must be under 200ms due to partner API contract."
- **Rejected alternatives with non-obvious reasons.** If you considered GraphQL but chose REST, and the reason is subtle — write it down, or someone will propose GraphQL again in six months.
