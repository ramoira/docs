# Tiers

Ramoira has three tiers. They differ in what is stored, what is public, and what agents can access.

---

## Three-tier model

| | Local | Published | Studio |
|:---|:---|:---|:---|
| **Account required** | No | Yes (free) | Yes (paid) |
| **Schema stored by Ramoira** | Nothing | Summary only | Full (private) |
| **Public URL** | — | ✓ draft | ✓ certified |
| **`workflowState`** | `draft` | `published` | `published` + `certified: true` |
| **`ramoira init`** | ✓ | ✓ | ✓ |
| **`ramoira validate`** | ✓ | ✓ | ✓ |
| **`ramoira publish`** | — | ✓ | ✓ |
| **Confidence score** | — | — | ✓ |

---

## What each tier provides to agents

### Local

The full `brand.schema.json` lives in your project directory. Agents with access to the project (Cursor, Claude Code, Windsurf) read it directly from disk. Nothing is accessible remotely.

### Published

Running `ramoira publish` extracts a summary schema and serves it at a stable public URL:

```
https://ramoira.com/brands/[slug]/schema.summary.json
https://ramoira.com/brands/[slug]/status
```

Remote agents, LLM crawlers, and collaborators can fetch this from anywhere. The full schema is never stored publicly — only the summary.

### Studio

Studio submits a published schema for certification. The platform runs archetype alignment analysis and assigns a `confidence` score (0–1). Above the threshold, `certified: true` is set in the summary meta.

Certified schemas carry higher weight in agent pipelines that check the `certified` flag before using the schema as context.

---

## What the summary includes

The public summary is defined precisely in `brand-schema-spec/SPEC.summary.schema.json`. It uses `additionalProperties: false` throughout — a document containing fields outside this list fails validation.

**Included:**

| Field | Notes |
|:---|:---|
| `meta.brandId`, `meta.brandName`, `meta.schemaVersion` | Always present |
| `meta.schemaType: "summary"` | Constant |
| `meta.canonicalURL`, `meta.certified`, `meta.confidence` | Set by the platform on publish |
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

- `draft` — created locally by `ramoira init`. Not on the platform until `ramoira publish` is called.
- `published` — summary is public at the canonical URL. `workflowState: "published"`.
- `certified` — Studio only. `workflowState: "published"` + `certified: true` + `confidence > threshold`.

Previous versions are not deleted. `ramoira status` shows the current published version and its state.
