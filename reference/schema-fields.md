# Schema fields — 3.0.0

A field-by-field guide to the brand schema. The normative definitions are the spec's [`SPEC.md`](https://github.com/ramoira/brand-schema-spec/blob/main/SPEC.md) and its JSON Schemas; where this page and the spec differ, the spec wins. Every object is closed: a field not listed in the spec fails validation.

**Summary** column: ✓ always in the public summary · *opt-in* only if `ramoira.summary_opt_in` includes it · — never.

---

## Top level

| Key | Required | Summary | What |
|:---|:---:|:---:|:---|
| `ramoira` | ✓ | ✓ | Metadata (below) |
| `rules` | ✓ | public rules | Everything that can be checked |
| `identity` | ✓ | ✓ | Who the brand is |
| `narrative` | ✓ | ✓ | What it stands for, and its claims |
| `voice` | ✓ | ✓ | How it speaks |
| `commercial` | ✓ | *opt-in* | Pricing, offers, social proof |
| `governance` | ✓ | *opt-in* | How rules interact, surfaces, situations, overrides |
| `draft_provenance` | | — | How the candidate was drafted (method, participants, reactions, which fields are unfilled) |

---

## `ramoira`

| Field | Values |
|:---|:---|
| `spec_version` | `"3.0.0"` |
| `schema_type` | `full`, `summary` or `archetype` |
| `brand_id` | The brand's slug: lowercase letters, digits and hyphens |
| `schema_version` | A human label. The real version is `content_hash`. |
| `content_hash` | `sha256:` + the hash of the rules and five layers (RFC 8785 canonical JSON). Any change to meaning changes it. |
| `workflow_state` | `draft`, `in_review`, `published`, `archived` |
| `ratification` | `null` (a candidate), or a pointer `{ratification_id, ratified_hash, ratified_at, ratifier_role}` set by Ramoira's record |
| `account_owner_verified` | The account has proven control of the brand's domain. Not ratification. |
| `canonical_url` | Where the published summary lives |
| `summary_opt_in` | Any of `commercial`, `governance`, `sacred_boundary` |

---

## `rules[]`

| Field | Values |
|:---|:---|
| `rule_id` | Unique; findings cite it |
| `statement` | The rule in plain language |
| `check_class` | `deterministic_exact`, `deterministic_structural`, `judged_bounded` |
| `severity` | `absolute` (the item fails), `strong` (needs review unless the brand's override clears it), `contextual` (logged) |
| `topic` | A dot path naming what the rule is about, e.g. `commercial.pricing.urgency` |
| `surfaces` | `"all"` or a list of surfaces |
| `markets` | `"all"` or a list of market codes |
| `situations` | `"any"` or a list of `situation_id`s; the rule applies only while one is active |
| `modality` | `text`, `visual`, `audio` |
| `visibility` | `public` or `private`. Private rules never appear in the summary. |
| `match` | Exact rules: `{terms, mode: substring \| word \| phrase, normalization: casefold_nfkc \| exact}` |
| `predicate` | Structural rules: `{type, params}`, e.g. `max_number {field: "discount_percent", max: 15}`, `claim_must_be_approved`, `required_phrase_on_surface`, `max_character_count`, `max_sentence_words` |
| `rubric` | Judged rules: `{question, example_refs, rail_refs}`, citing at least one approved and one rejected example, or a rail with an example and an anti-example |
| `provenance` | `authored`, `adapted`, `inherited` |
| `affirmed` | Whether the brand affirmed it. A schema cannot be ratified while an inherited rule is unaffirmed. |
| `rationale` | Why the rule exists (optional) |

More: [`layers/rules.md`](https://github.com/ramoira/brand-schema-spec/blob/main/layers/rules.md).

---

## `identity`

| Field | Summary |
|:---|:---:|
| `prism.physique.{permitted, posture, referenceURL}` | ✓ |
| `prism.personality.characterBrief` | ✓ |
| `prism.culture.{coreValues, originNarrative}` | ✓ |
| `prism.culture.sacredBoundary` | *opt-in* |
| `prism.relationship.{formality, warmth}` (0–10, shared scales) | ✓ |
| `prism.reflection.{depictedCustomer, ageSignal}` | ✓ |
| `prism.selfImage.{feelingDescriptors, identityStatement}` | ✓ |
| `distinctiveAssets.visual` (colours, logo clear space, iconography, photography) | ✓ |
| `distinctiveAssets.sonic` (sonic logo, genres, mood) | ✓ |
| `distinctiveAssets.linguistic.{ownedPhrases, ownedWords, typographicVoice}` | ✓ |

## `narrative`

| Field | Summary |
|:---|:---:|
| `semiotic.denotative.categoryDescriptor` (required), `specifications` | ✓ |
| `semiotic.denotative.claims[]` `{claim_id, claim, evidenceRequired, evidenceType, markets, surfaces}`: the one list of approved claims | ✓ |
| `semiotic.connotative.{meaningClusters, emotionalRegister}` (required) | ✓ |
| `myth.{mythStatement (required), culturalTension, protagonistRole, antagonist}` | ✓ |
| `mythEvolution.{principle, immutableCore, modernTensions}` | ✓ |
| `pillars[]` `{name, description, coreClaim, approvedArcs, surfaces, rails}` | ✓ |
| `editorial.{openingPrinciple, structuralApproach, referencePool, timeScaleLanguage}` | ✓ |
| `guidance[]` `{question, applies_to}`: questions without brand-judged examples yet; never a verdict | ✓ |

## `voice`

| Field | Summary |
|:---|:---:|
| `base.vocabularyLevel` (0–10, required), `base.humourStyle` `{style, frequency}` (required), `base.permittedDevices` | ✓ |
| `approvedTones` (at least one) | ✓ |
| `examples[]` `{example_id, surface, text, verdict: approved \| rejected, reason, judged_by, source, captured_at}` | ✓ |
| `contextVariants[]` `{surface, formalityDelta, warmthDelta, sentenceLength, openingInstruction, closingInstruction, rails, fallbackInstruction}` | ✓ |
| `rails.{global, alternatives}`: rails `{rail_id, context, instruction, example, antiExample}` | ✓ |

`judged_by` records who judged an example: `brand_owner`, `brand_team`, `agency`, `freelancer`, `ramoira_facilitator`, `archetype_template` or `ramoira_draft`. Only `brand_owner` and `brand_team` examples can ground a judged rule in a ratified schema.

## `commercial` (*opt-in*)

`pricing.{style, displayFormat, surfaceOverrides, permittedLanguage}`, `claims.superlatives.approved`, `offers.{permittedTypes, communicationRules}`, `socialProof.{celebrityEndorsementStyle, permittedAuthoritySignals}`. Discount caps, urgency and forbidden pricing language are rules, not fields.

## `governance` (*opt-in*)

`conflictResolution`, `situations[]` (a crisis, an accusation, a competitor's claim: activated by the brand, never declared by a producer), `surfaces[]` `{surface, objective, primaryRail, rails, fallback, intentRules, suspended_rule_ids}`, `override` (who may clear a `strong` finding, and how), `reviewTopics`.

---

## Shared scales

`formality`, `warmth` and `vocabularyLevel` are integers from 0 to 10 on anchored scales defined once in the spec (§8.6), so a value means the same thing in every schema. Context-variant and situation deltas move along them and must stay within 0–10.

## Enumerations

`OutputSurface` (17 values) and `UserIntent` (8 values): [SPEC.md, Appendix A](https://github.com/ramoira/brand-schema-spec/blob/main/SPEC.md#appendix-a--enumerations).

## From 2.0.0

`meta` became `ramoira`; `_component` and `_version` are gone; every forbidden list (`neverDo`, forbidden tones and words, zero-tolerance terms, severity strings) became rules; `certified` and `confidence` were removed. Field by field: [migration guide](https://github.com/ramoira/brand-schema-spec/blob/main/migrations/2.0.0-to-3.0.0.md).
