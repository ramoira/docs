# What is a brand schema?

A **brand schema** is a structured, versioned definition of a brand that both humans and AI systems can read.

Instead of re-briefing an LLM every session, the schema becomes the canonical context layer for:

- brand voice (what to do and what to avoid)
- narrative meaning (myth, semiotics, pillars)
- commercial guardrails (pricing/claims/offers/social proof)
- governance (severity, conflict resolution, compliance routing)

## Why schemas exist

LLMs drift toward **category norms**.

Even with good prompts, models tend to:

- substitute generic tone (“warm”, “friendly”, “premium”)
- introduce conversion tactics that break premium brands (urgency, discounting)
- violate taboo territory (comparatives, claims, trend language)

A schema makes those constraints explicit and machine-checkable.

## The canonical model

Ramoira’s platform schema is composed of five components:

- `identity`
- `narrative`
- `voice`
- `commercial`
- `governance`

See `docs/reference/schema-fields.md` for the canonical shape.
