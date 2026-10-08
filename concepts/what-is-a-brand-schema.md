# What is a brand schema?

A **brand schema** is a structured, machine-readable statement of what a brand means: its rules, and the facts and voice behind them. Producers (agencies, freelancers, in-house teams, AI tools) read it to make content that fits the brand. Checkers read its rules to test content against it.

The format is open: [brand-schema-spec](https://github.com/ramoira/brand-schema-spec), version 3.0.0. That spec is normative; this page explains it.

---

## Why schemas exist

Given only a brand name and a task, a model writes the voice of the category, not the brand. Even with careful prompting, it tends to:

- swap specific voice for generic labels ("warm", "premium");
- reach for conversion tactics the brand never uses (urgency, discounting, fear);
- make claims the brand cannot support, or name competitors it never names.

A schema makes the brand's rules explicit, once, so every producer and every tool works from the same measure, and so content can be checked against it.

---

## What is in a schema

```json
{
  "ramoira":    { },
  "rules":      [ ],
  "identity":   { },
  "narrative":  { },
  "voice":      { },
  "commercial": { },
  "governance": { }
}
```

### `rules`: everything that can be checked

Every prohibition and requirement lives once, in one list. Each rule has an id, a plain statement, how it is checked, how serious a breach is, and where it applies:

| Field | What it says |
|:---|:---|
| `rule_id`, `statement` | The rule, citable by id |
| `check_class` | `deterministic_exact` (words and phrases), `deterministic_structural` (numbers, required phrases, approved claims) or `judged_bounded` (judgment, grounded in examples the brand judged) |
| `severity` | `absolute` (the item fails), `strong` (the item needs review) or `contextual` (logged) |
| `surfaces`, `markets`, `situations`, `modality` | Where, for whom and when it applies |
| `visibility` | Whether it appears in the public summary |
| `provenance`, `affirmed` | Whether the brand wrote it or affirmed it |

A judged rule is only allowed when the brand has judged examples on both sides of the line ("this is us", "this is not us, because…"). Something the brand cannot yet illustrate is not a rule: it is a guidance question in `narrative.guidance`.

### The five layers: facts and voice

| Layer | What it holds |
|:---|:---|
| `identity` | Who the brand is: the brand identity prism (physique, personality, culture, relationship, reflection, self-image) and distinctive assets (colours, sound, owned phrases) |
| `narrative` | What it stands for: what it makes, the **approved claims**, the myth it tells, its pillars and editorial approach |
| `voice` | How it speaks: vocabulary and humour, approved tones, **examples the brand judged** (approved and rejected, with reasons), per-surface variants and rails |
| `commercial` | Pricing style, approved superlatives, offers, social proof |
| `governance` | How rules interact, per-surface behaviour, situations (a recall, an accusation), who may override a `strong` finding |

Layers hold facts and density. They hold no prohibitions; those are rules.

### `ramoira`: metadata

`spec_version`, `brand_id` (the brand's slug), `schema_version` (a human label), `content_hash`, `workflow_state` (`draft`, `in_review`, `published`, `archived`), `ratification` and a few publishing fields.

**`content_hash` is the version.** It is a SHA-256 over the rules and the five layers, so any change to meaning changes it, and nothing else does. Every check binds to a hash: when the schema changes, earlier results stay attached to the old one.

---

## Candidate or ratified

Anyone can draft a schema: `ramoira init`, an agency, a team. A draft is a **candidate**. It becomes the brand's measure only when the brand **[ratifies](ratification.md)** it. Until then `ramoira.ratification` is `null`, and nothing checked against it can stand as more than a self-check.

## Meaning, never results

A schema says what the brand means. It never carries results about itself: no certification, no confidence, no score. Results of checks live in a separate record. See [conformance and faithfulness](conformance-vs-faithfulness.md).

---

## Full schema and public summary

| | Full (`ramoira/brand.schema.json`) | Summary (`schema.summary.json`) |
|:---|:---|:---|
| **Where** | Your project; sent privately to Ramoira when you publish | Public at `ramoira.com/brands/<slug>/schema.summary.json` |
| **Rules** | All | Public rules only |
| **Layers** | All five | `identity`, `narrative` and `voice` in full; `commercial` and `governance` only if you opt them in |
| **Who reads it** | Your own tools and producers you share it with | Anyone: remote agents, crawlers, collaborators |

What the summary holds is identical for every brand at every tier. See [publishing](../guides/publishing.md).
