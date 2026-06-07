# Glossary Format Guide

The glossary is written as a `## Glossary` section in the project's `CLAUDE.md`.

## Format

```md
## Glossary

**Order**:
A purchase request initiated by a customer, containing one or more items.
_Avoid_: Purchase, transaction

**Invoice**:
A payment request sent to the customer after shipment.
_Avoid_: Bill, payment request

**Customer**:
The individual or organization placing orders.
_Avoid_: Client, buyer, account

### Relationships
- One **Order** produces one or more **Invoice**
- One **Invoice** belongs to exactly one **Customer**

### Resolved ambiguities
- "account" was used for both **Customer** and **User** — clarified: these are two distinct concepts
```

## Rules

- **Be decisive.** When one concept has multiple words, pick the best one, list the rest as "avoid."
- **Explicitly note conflicts.** If a word is used ambiguously, record it under "Resolved ambiguities."
- **Keep definitions short.** One sentence. Define "what it is," not "what it does."
- **Annotate relationships.** Use bold term names, note obvious cardinality.
- **Only include project-specific terms.** General programming concepts (timeout, error type, utility pattern) don't belong. Before adding a word, ask: is this a concept unique to this project, or a general programming concept? Only the former.
- **Natural grouping.** Use sub-headers when terms naturally cluster; flat layout when all in the same domain.
