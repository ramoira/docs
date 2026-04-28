# Schema Fields — v2.0.0

The Ramoira brand schema is a JSON object with six required top-level keys. This page documents every field, its type, and whether it appears in the full schema, the public summary, or both.

Full spec and JSON validators: [github.com/ramoira/brand-schema-spec](https://github.com/ramoira/brand-schema-spec)

---

## Top-level structure

```json
{
  "meta":       { ... },
  "identity":   { "_component": "identity",   "_version": "2.0.0", ... },
  "narrative":  { "_component": "narrative",  "_version": "2.0.0", ... },
  "voice":      { "_component": "voice",      "_version": "2.0.0", ... },
  "commercial": { "_component": "commercial", "_version": "2.0.0", ... },
  "governance": { "_component": "governance", "_version": "2.0.0", ... }
}
```

Every component carries `_component` (constant string) and `_version` (semver string).

---

## Full vs Summary

| Layer | Full schema | Public summary |
|:---|:---:|:---:|
| `meta` | ✓ | ✓ (subset) |
| `identity.summary` | ✓ | ✓ |
| `identity.prism.relationship` | ✓ | ✓ |
| `identity.prism.*` (other) | ✓ | — |
| `identity.distinctiveAssets` | ✓ | — |
| `narrative.semiotic.denotative.categoryDescriptor` | ✓ | ✓ |
| `narrative.semiotic.connotative` (meaningClusters, emotionalRegister) | ✓ | ✓ |
| `narrative.myth` | ✓ | ✓ |
| `narrative.contentTest` | ✓ | ✓ |
| `narrative.*` (other) | ✓ | — |
| `voice.base` (4 core params) | ✓ | ✓ |
| `voice.approvedTones` | ✓ | ✓ |
| `voice.forbiddenTones` | ✓ | ✓ |
| `voice.examples` (min 2 approved + 2 rejected) | ✓ | ✓ |
| `voice.base.structuralRules` | ✓ | — |
| `voice.contextVariants` | ✓ | — |
| `voice.rails` | ✓ | — |
| `commercial` | ✓ | — |
| `governance` | ✓ | — |

---

## Shared types

### ConstraintSeverity

```
"absolute" | "strong" | "contextual"
```

- `absolute` — never violated; violation response is block or flag-and-block
- `strong` — violated only with explicit override process; violation response is flag for review
- `contextual` — judgment permitted within stated bounds; violation response is log for audit

### FallbackBehaviour

```
"refuse_to_generate" | "escalate_to_human" | "use_brand_default" | "use_minimal_safe"
```

### OutputSurface

```
search_result_page   paid_landing_page    product_detail_page
comparison_page      editorial            brand_narrative
social_organic       social_paid          email_acquisition
email_retention      display_ad           video_script
audio_script         press_release        customer_service
packaging_copy       out_of_home
```

### UserIntent

```
high_intent_buyer    early_research       brand_discovery
competitor_comparison   post_purchase     service_enquiry
press_media          investor
```

### RelationshipMode

How the brand shows up for people — expressed as the brand's own posture statement:

```
"We're like you. We just happen to know a bit more about this one thing."
"Things can be better. Here is a small thing that helps."
"We believe in what you can do before you do."
"We know more. Here is the proof."
"Built to outlast everything. Excellence as philosophy, not strategy."
"Business as a force for change. Profit is the fuel, not the point."
"Limits are the starting point. Mediocrity is the only enemy."
"The category is broken. We are what replaces it."
```

### PricingStyle

Encodes the brand market tier:

| Value | Market tier | Meaning |
|:---|:---|:---|
| `opaque` | Luxury | Price never shown, no discount language |
| `transparent` | Premium | Price shown, no discount |
| `anchored` | Mid-market | Original price shown, discount permitted |
| `value_led` | Mass-market | Price leads the message |
| `simple` | Flat/subscription | Single price, no anchoring |

### SentenceLength

```
"short" | "varied" | "long" | "fragments_permitted"
```

### HumourStyle

```
"dry" | "self_deprecating" | "absurdist" | "warm" | "irreverent" | "none"
```

### Score

A number from 0 to 10. Used for personality dimensions, formality, warmth, vocabulary level.

### Constrained\<T\>

```json
{ "value": T, "severity": ConstraintSeverity, "rationale": "string (optional)" }
```

Use `Constrained<T>` when the severity level changes how a generation pipeline responds to a violation — i.e. when a strong vs. contextual distinction would change what the agent does. Use a plain string when the constraint is always treated the same way.

Appropriate for: `forbiddenWords`, `forbiddenClaims`, `ownedPhrases`, `globalForbiddenTerms`.
Not needed for: `approvedTones`, `meaningClusters`, `structuralRules`.

### Rail

```json
{
  "context": "string",
  "instruction": "string",
  "example": "string (optional)",
  "antiExample": "string (optional)"
}
```

A scoped generation instruction. `context` names the situation; `instruction` is what to do.

---

## meta

| Field | Required | Type | Notes |
|:---|:---:|:---|:---|
| `brandId` | ✓ | string | URL-safe brand identifier |
| `brandName` | ✓ | string | Human-readable brand name |
| `schemaVersion` | ✓ | string | Semver, e.g. `"2.0.0"` |
| `effectiveDate` | — | string | ISO date |
| `previousVersion` | — | string | Previous semver |
| `changelog` | — | string[] | Change notes |

Summary meta additionally includes `schemaType: "summary"`, `canonicalURL`, `certified` (boolean), `confidence` (0–1).

---

## identity

### identity.summary *(required)*

| Field | Required | Type | Notes |
|:---|:---:|:---|:---|
| `oneLineBrief` | ✓ | string | One sentence capturing who this brand is |
| `threeAdjectives` | ✓ | string[3] | Exactly 3 |
| `neverDo` | ✓ | string[] | Min 1 — highest-signal agent constraint |

### identity.prism

**relationship** *(mode required)*

| Field | Required | Type |
|:---|:---:|:---|
| `mode` | ✓ | `RelationshipMode` (archetype posture statement) |
| `formality` | — | Score |
| `warmth` | — | Score |
| `pronoun` | — | `"we" | "I" | "brand_name_only"` |
| `powerDynamic` | — | `"brand_leads" | "equal" | "customer_leads"` |

**personality**

| Field | Type |
|:---|:---|
| `sincerity` | Score |
| `excitement` | Score |
| `competence` | Score |
| `sophistication` | Score |
| `ruggedness` | Score |
| `characterBrief` | string |

`culture`, `physique`, `reflection`, `selfImage` — see `brand-schema-spec/layers/identity.md`.

### identity.distinctiveAssets

| Sub-section | Key fields |
|:---|:---|
| `visual` | `primaryColor` (ConstrainedHexColor), `secondaryColors`, `forbiddenColors`, `photographyStyle` |
| `sonic` | `permittedGenres`, `forbiddenGenres`, `tempoRange`, `instrumentalMood` |
| `linguistic` | `ownedPhrases` (Constrained[]), `ownedWords`, `forbiddenWords` (Constrained[]), `typographicVoice` |

`typographicVoice` includes `sentenceStructure`, `punctuationStyle`, `numeralStyle` — high-signal fields for preventing voice drift.

---

## narrative

### narrative.semiotic *(required)*

**denotative** *(categoryDescriptor required)*

| Field | Required | Type |
|:---|:---:|:---|
| `categoryDescriptor` | ✓ | string — plain statement of what the brand makes or does |
| `functionalClaims` | — | string[] |
| `specifications` | — | string[] |
| `forbiddenClaims` | — | string[] |

**connotative** *(meaningClusters, emotionalRegister required)*

| Field | Required | Type |
|:---|:---:|:---|
| `meaningClusters` | ✓ | string[] (min 1) — semantic territories the brand owns |
| `emotionalRegister` | ✓ | string — how the brand makes people feel |
| `forbiddenMeanings` | — | string[] |
| `minimumConnotativeTest` | — | string — one-sentence test for connotative fit |

**layerHierarchy** *(required)*: `"connotative_first" | "balanced" | "denotative_first"`

### narrative.myth *(mythStatement, mythTest required)*

| Field | Required | Type |
|:---|:---:|:---|
| `mythStatement` | ✓ | string — the belief that defines the brand's worldview |
| `mythTest` | ✓ | string — yes/no test for content |
| `culturalTension` | — | string |
| `protagonistRole` | — | string |
| `antagonist` | — | string |
| `constraints` | — | `{ constraint, severity, rationale?, example? }[]` |

### narrative.contentTest *(all three required)*

| Field | Required | Type |
|:---|:---:|:---|
| `mythTest` | ✓ | string |
| `connotativeTest` | ✓ | string |
| `toneTest` | ✓ | string |

`mythEvolution`, `pillars`, `editorial` — see `brand-schema-spec/layers/narrative.md`.

---

## voice

### voice.base *(all four required)*

| Field | Required | Type |
|:---|:---:|:---|
| `sentenceLength` | ✓ | SentenceLength |
| `vocabularyLevel` | ✓ | Score |
| `humourPermitted` | ✓ | boolean |
| `humourStyle` | ✓ | HumourStyle |
| `permittedDevices` | — | string[] |
| `forbiddenDevices` | — | string[] |
| `structuralRules` | — | string[] — prose-level constraints; prevents voice drift |

### voice.approvedTones *(min 1, required)*

Strings, optionally with em-dash explanation: `"warm and informed — the brilliant friend who researched this"`.

### voice.forbiddenTones *(min 1, required)*

Same format.

### voice.examples *(min 2, required)*

Summary requires min 4 total with min 2 approved + min 2 rejected.

| Field | Required | Type |
|:---|:---:|:---|
| `text` | ✓ | string |
| `verdict` | ✓ | `"approved" | "rejected"` |
| `reason` | ✓ | string |
| `context` | — | string — surface or scenario |

Rejected examples carry the contrast signal. They are the most important examples for preventing LLM voice drift.

### voice.contextVariants

| Field | Required | Type |
|:---|:---:|:---|
| `surface` | ✓ | OutputSurface |
| `formalityDelta` | — | number |
| `warmthDelta` | — | number |
| `sentenceLength` | — | SentenceLength override |
| `openingInstruction` | — | string |
| `closingInstruction` | — | string |
| `rails` | — | Rail[] |
| `additionalForbidden` | — | string[] |

### voice.rails

| Field | Required | Type |
|:---|:---:|:---|
| `global` | ✓ | Rail[] (min 1) |
| `alternatives` | — | per-restriction rail sets |

---

## commercial

### commercial.pricing *(style, priceDisplayPermitted, discountPermitted required)*

| Field | Required | Type |
|:---|:---:|:---|
| `style` | ✓ | PricingStyle |
| `priceDisplayPermitted` | ✓ | boolean |
| `discountPermitted` | ✓ | boolean |
| `urgencyLanguagePermitted` | — | boolean |
| `scarcityLanguagePermitted` | — | boolean |
| `maxDiscountPercent` | — | number 0–100 |
| `forbiddenLanguage` | — | ConstrainedString[] |

### commercial.claims *(required)*

| Field | Type |
|:---|:---|
| `approved` | `{ claim, evidenceRequired?, surfaces? }[]` |
| `forbidden` | ConstrainedString[] |
| `comparative` | `{ competitorMentionPermitted, comparativeClaimsPermitted, ... }` |
| `superlatives` | `{ permitted, approved[], forbidden[] }` |

`offers`, `socialProof`, `surfaceRules`, `globalForbiddenTerms` — see `brand-schema-spec/layers/commercial.md`.

---

## governance

### governance.severity *(required)*

**absolute** *(constraints min 1, violationResponse required)*

| Field | Required | Type |
|:---|:---:|:---|
| `constraints` | ✓ | string[] (min 1) |
| `violationResponse` | ✓ | `"block_output" | "flag_and_block"` |

**strong** *(required)*

| Field | Required | Type |
|:---|:---:|:---|
| `constraints` | ✓ | string[] |
| `violationResponse` | ✓ | `"flag_for_review" | "block_output"` |
| `overrideProcess` | — | string |

**contextual** *(required)*

| Field | Required | Type |
|:---|:---:|:---|
| `constraints` | ✓ | string[] |
| `judgmentBounds` | — | string |
| `violationResponse` | — | `"log_for_audit"` |

### governance.preflight *(all three required)*

Three brand-specific yes/no questions to run on any generated content before publishing.

| Field | Required | Type |
|:---|:---:|:---|
| `question1` | ✓ | string |
| `question2` | ✓ | string |
| `question3` | ✓ | string |

`conflictResolution`, `surfaceRules` (with `intentRules` per UserIntent), `overrideProtocol`, `compliance` (with `geographicOverrides`) — see `brand-schema-spec/layers/governance.md`.
