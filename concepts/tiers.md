# What is free

Everything you do with your own brand schema is free: drafting it, validating it, rendering a brand book, checking content against it yourself, publishing it and sharing it. None of it sits behind a paywall, and none of it, except publishing, needs an account.

No tier makes a schema more trustworthy. Ramoira does not certify schemas or content. What distinguishes one schema from another is whether the brand has **[ratified](ratification.md)** it and whether content is **[checked](conformance-vs-faithfulness.md)** against it, independently. Neither is available yet. Until a brand ratifies a schema, it is a **candidate**, local or published.

---

## Local and published

| | Local | Published |
|:---|:---|:---|
| **Account** | None | Free account (GitHub or an emailed sign-in link) |
| **`ramoira init`, `validate`, `book`, `check`** | ✓ | ✓ |
| **`ramoira publish`** | — | ✓ |
| **Public summary URL** | — | ✓ (a candidate until ratified) |

The account exists so that a brand's slug belongs to whoever owns it. Sign-up is self-serve, with nothing to approve. It is not a pricing tier.

### Local

`ramoira/brand.schema.json` lives in your project. Tools with access to the project read it, or the plain-language brief `ramoira init` writes beside it (`ramoira/agents.md`). Nothing is reachable remotely.

### Published

`ramoira publish` sends the full schema to Ramoira, which keeps it private and serves the public summary:

```
https://ramoira.com/brands/<slug>/schema.summary.json
https://ramoira.com/brands/<slug>/status
```

Publishing does not ratify the schema. A published summary is still a candidate.

---

## What the summary includes

| | In the public summary |
|:---|:---|
| `ramoira` | Always, with the full schema's `content_hash` |
| Rules with `visibility: public` | Always |
| `identity`, `narrative`, `voice` | Always, in full: rejected examples, context variants, rails, myth evolution and pillars included |
| `identity.prism.culture.sacredBoundary` | Only if you opt in |
| `commercial`, `governance` | Only if you opt in (`ramoira.summary_opt_in`) |
| Rules with `visibility: private` | Never |
| `draft_provenance` | Never |

These defaults are the same for every brand. Nothing is withheld from the summary because of pricing.

---

## What is paid

Ramoira's hosted **diagnostics** of a schema (the first is a stress test of how the rules hold up) are a paid service. They describe the schema, never as a quality or certification score, and they never return generated content. Validating, checking and publishing your own schema stay free.

---

## Lifecycle

`ramoira.workflow_state` is the publication lifecycle only: `draft`, `in_review`, `published`, `archived`. There is no `certified` state. Ratification is not a lifecycle state either: it is a separate act by the brand, recorded by Ramoira and pointed to from `ramoira.ratification`.

Each publish keeps the version it published, bound to its `content_hash`. `ramoira status` shows what is true now.

### `certified` and `confidence`

Schemas from spec 2.0.0 could carry `meta.certified` and `meta.confidence`. Both were removed in 3.0.0. Neither said anything about the schema's quality, whether the brand ratified it, or whether content conforms to it.
