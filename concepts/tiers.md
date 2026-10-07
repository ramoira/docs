# Tiers

Drafting, validating, publishing and sharing a brand schema are free. No tier makes a schema more trustworthy, and Ramoira does not certify schemas or content.

What will distinguish one schema from another is whether the brand has **ratified** it and whether content is **checked** against it. Neither is available yet. Until a brand ratifies a schema, it is a **candidate**, whether it is local or published.

---

## Local and published

| | Local | Published |
|:---|:---|:---|
| **Account required** | No | Yes (free) |
| **Public URL** | — | ✓ candidate (unratified) |
| **`workflowState`** | `draft` | `published` |
| **`ramoira init`** | ✓ | ✓ |
| **`ramoira validate`** | ✓ | ✓ |
| **`ramoira publish`** | — | ✓ |

The account exists so that a brand's slug belongs to whoever owns it. It is not a pricing tier.

---

## What each option provides to agents

### Local

The full `brand.schema.json` lives in your project directory. Agents with access to the project (Cursor, Claude Code, Windsurf) read it directly from disk. Nothing is accessible remotely.

### Published

Running `ramoira publish` extracts a summary schema and serves it at a stable public URL:

```
https://ramoira.com/brands/[slug]/schema.summary.json
https://ramoira.com/brands/[slug]/status
```

Remote agents, LLM crawlers, and collaborators can fetch this from anywhere. The full schema is never served publicly — only the summary.

Publishing does not ratify the schema. A published summary is still a candidate.

### `certified` and `confidence` (deprecated)

Older summaries may carry `meta.certified` and `meta.confidence`. Both are deprecated and will be removed in spec 3.0.0. Neither says anything about the schema's quality, whether the brand ratified it, or whether content conforms to it. Agent pipelines should not read either field.

---

## What the summary includes

The public summary is defined precisely in `brand-schema-spec/SPEC.summary.schema.json`. It uses `additionalProperties: false` throughout — a document containing fields outside this list fails validation.

**Included:**

| Field | Notes |
|:---|:---|
| `meta.brandId`, `meta.brandName`, `meta.schemaVersion` | Always present |
| `meta.schemaType: "summary"` | Constant |
| `meta.canonicalURL` | Set by Ramoira on publish |
| `meta.certified`, `meta.confidence` | Deprecated; removed in 3.0.0. Do not rely on them. |
| `identity.summary` | `oneLineBrief`, `threeAdjectives` (exactly 3), `neverDo` (min 1) |
| `identity.prism.relationship` | `mode`, `formality`, `warmth` |
| `narrative.semiotic.denotative.categoryDescriptor` | Only this field from denotative |
| `narrative.semiotic.connotative.meaningClusters` | Min 1 item |
| `narrative.semiotic.connotative.emotionalRegister` | |
| `narrative.myth.mythStatement` | |
| `narrative.myth.mythTest` | |
| `narrative.contentTest` | `mythTest`, `connotativeTest`, `toneTest` |
| `voice.base` | `sentenceLength`, `vocabularyLevel`, `humourPermitted`, `humourStyle` only |
| `voice.approvedTones` | Min 1 |
| `voice.forbiddenTones` | Min 1 |
| `voice.examples` | Min 4 total, min 2 approved + min 2 rejected |

**Excluded:**

- `identity.distinctiveAssets` (colors, sonic, full linguistic assets)
- `narrative.semiotic.layerHierarchy`, `forbiddenMeanings`, `minimumConnotativeTest`
- `narrative.mythEvolution`, `narrative.pillars`, `narrative.editorial`
- `voice.base.structuralRules`, `voice.contextVariants`, `voice.rails`
- `commercial` (entire component)
- `governance` (entire component)

These exclusions apply only to the public summary, identically for every brand. They are not reserved for a paid tier: every field stays in your full schema, and no paid tier supplies any of them. Myth evolution, pillars, context variants and rails become includable in the summary from spec 3.0.0.

---

## Market tier encoding

Ramoira does not use a separate market tier field. Commercial positioning (luxury / premium / mid / mass) is encoded into `commercial.pricing.style` and the associated pricing flags:

| Market tier | `pricing.style` | `priceDisplayPermitted` | `discountPermitted` |
|:---|:---|:---:|:---:|
| Luxury | `opaque` | false | false |
| Premium | `transparent` | true | false |
| Mid-market | `anchored` | true | true |
| Mass-market | `value_led` | true | true |

---

## Schema lifecycle

```
draft → published
```

- `draft` — created locally by `ramoira init`. Not on ramoira.com until `ramoira publish` is called.
- `published` — summary is public at the canonical URL. `workflowState: "published"`.

There is no `certified` state. Ratification is not a lifecycle state either: it is a separate act by the brand, and spec 3.0.0 records it separately.

Previous versions are not deleted. `ramoira status` shows the current published version and its state.
