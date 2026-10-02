# Publishing

Publishing makes your brand schema publicly accessible at a stable URL that remote agents, LLM crawlers, and collaborators can fetch from anywhere.

---

## Quick start

```sh
# Option 1 — environment variable
export RAMOIRA_TOKEN=your_token
ramoira publish

# Option 2 — saved token
ramoira login
ramoira publish
```

Get a token at [ramoira.com/tokens](https://ramoira.com/tokens).

---

## What publish does

1. Validates `brand.schema.json` locally against the Ramoira spec — same as `ramoira validate`. Fails fast if the schema is invalid; no network call is made.
2. POSTs the full schema to `ramoira.com/api/brands/[slug]/publish` with your Bearer token.
3. The platform extracts the summary schema from the full schema and serves it at the canonical URL.
4. Returns the canonical URL.

```
✓ Published to https://ramoira.com/brands/your-brand/schema.summary.json
```

---

## What gets published (the summary)

Only the summary schema is published publicly. The full schema — including commercial rules, governance constraints, and all operational detail — is stored privately by Ramoira and never served publicly.

The summary is defined precisely by `brand-schema-spec/SPEC.summary.schema.json`. Exactly these fields are included:

**meta:** `brandId`, `brandName`, `schemaVersion`, `schemaType: "summary"`, `canonicalURL` (older summaries may also carry the deprecated `certified` and `confidence`; do not rely on them)

**identity:** `summary` (oneLineBrief, threeAdjectives, neverDo) + `prism.relationship` (mode, formality, warmth)

**narrative:** `semiotic.denotative.categoryDescriptor` + `semiotic.connotative` (meaningClusters, emotionalRegister) + `myth` (mythStatement, mythTest) + `contentTest` (mythTest, connotativeTest, toneTest)

**voice:** `base` (sentenceLength, vocabularyLevel, humourPermitted, humourStyle) + `approvedTones` + `forbiddenTones` + `examples` (min 4: 2 approved + 2 rejected)

---

## What stays private

These fields are not in the public summary. `ramoira publish` sends the full schema, so Ramoira stores them privately, but it never serves them publicly:

- `identity.distinctiveAssets` — colors, sonic assets, full typographic voice
- `narrative.semiotic.layerHierarchy`, `forbiddenMeanings`, `minimumConnotativeTest`
- `narrative.mythEvolution`, `narrative.pillars`, `narrative.editorial`
- `voice.base.structuralRules`, `voice.contextVariants`, `voice.rails`
- `commercial` — entire component (pricing rules, claims, offers, social proof)
- `governance` — entire component (severity registry, preflight, compliance routing)

---

## After publishing

Your summary is publicly accessible at:

```
https://ramoira.com/brands/[slug]/schema.summary.json
https://ramoira.com/brands/[slug]/status
```

Check the current state at any time:

```sh
ramoira status
```

---

## Re-publishing

Run `ramoira publish` again after editing `brand.schema.json`. Each publish creates a new version. Previous versions are retained in Ramoira's version history but the canonical URL always serves the latest published version.

---

## Publishing is not ratification

A published schema is still a **candidate**. Publishing makes the summary public; it does not mean the brand has ratified the schema, and it does not certify anything. Ramoira does not certify schemas or content.
