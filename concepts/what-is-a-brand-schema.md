# What is a brand schema?

A **brand schema** is a structured, versioned, machine-readable definition of a brand. It encodes what an AI system needs to generate on-brand content — not just tone of voice, but narrative meaning, commercial rules, and governance constraints.

Instead of re-briefing an LLM every session, the schema becomes the canonical context layer.

---

## Why schemas exist

LLMs drift toward **category norms**.

Given only a brand name and a task, a model produces output that is technically coherent but tonally generic — the voice of the category, not the brand. Even with careful prompting, models tend to:

- substitute generic tonal labels ("warm", "friendly", "premium") for specific brand voice
- introduce conversion tactics that break brand positioning (urgency, discounting, fear framing)
- violate taboo territory (forbidden comparisons, unsupported claims, wrong cultural register)

A schema makes brand constraints explicit and machine-checkable. The rejected voice examples, the governance constraints, the preflight questions — these prevent drift.

---

## The five components

### identity

Who the brand is.

- **`prism`** — the Kapferer Brand Identity Prism: physique, personality, culture, relationship, reflection, selfImage. Encodes how the brand relates to its customers (`relationship.mode`), its personality dimensions (5 Brand Personality scores), and what it looks like.
- **`distinctiveAssets`** — visual (primary color, photography style), sonic (genre, tempo), linguistic (owned phrases, typographic voice). `typographicVoice.sentenceStructure` is one of the highest-signal fields for preventing voice drift.
- **`summary`** — the three fields agents load first: `oneLineBrief`, `threeAdjectives`, `neverDo`.

### narrative

What the brand stands for.

- **`semiotic`** — two layers: `denotative` (what the brand literally makes or does) and `connotative` (what it means, the emotional register, the semantic territories it owns). `layerHierarchy` determines which leads in generation.
- **`myth`** — the brand's core belief (`mythStatement`) and the test for whether content carries it (`mythTest`).
- **`contentTest`** — three testable questions (myth, connotative, tone) to run on any generated content.
- **`pillars`**, **`mythEvolution`**, **`editorial`** — deeper narrative structure for content strategy and long-form generation.

### voice

How the brand speaks.

- **`base`** — core voice parameters: sentence length, vocabulary level, humour, structural rules. `structuralRules` are the prose-level constraints that shape every sentence.
- **`approvedTones`** and **`forbiddenTones`** — explicit tonal labels. Both are required.
- **`examples`** — approved and rejected writing samples with reasons. Rejected examples (verdict: `"rejected"`) carry the contrast signal — what this brand will never sound like.
- **`contextVariants`** — surface-specific voice adjustments for each `OutputSurface`.
- **`rails`** — global and conditional generation instructions.

### commercial

What the brand can and cannot say about money, claims, and competitors.

- **`pricing`** — encodes the commercial tier (`style`), whether prices can be shown, whether discounting is permitted, and what urgency/scarcity language is allowed.
- **`claims`** — approved claims (with evidence requirements), forbidden claims, comparative rules, superlative rules.
- **`globalForbiddenTerms`** — terms that can never appear in any generated output, with severity and rationale.
- **`offers`**, **`socialProof`**, **`surfaceRules`** — per-surface commercial rules.

### governance

The rules that override everything else.

- **`severity`** — three tiers: `absolute` (never violated, block output), `strong` (override process required, flag for review), `contextual` (judgment permitted within stated bounds, log for audit).
- **`preflight`** — three brand-specific yes/no questions to run on any generated content before it is used.
- **`surfaceRules`** — per-surface and per-intent governance rules, including `intentRules` mapped to `UserIntent`.
- **`conflictResolution`** — how to resolve conflicts between components.
- **`compliance`** — violation routing, geographic overrides, zero-tolerance terms.

---

## Component architecture

Every component carries two identifying fields:

```json
{
  "_component": "voice",
  "_version": "2.0.0"
}
```

`_component` is a constant string matching the component name. `_version` is the semver for that component. This allows tools to validate and migrate components independently.

---

## Full schema vs summary schema

| | Full (`brand.schema.json`) | Summary (`brand.schema.summary.json`) |
|:---|:---|:---|
| **Location** | Local project directory | Public URL at ramoira.com |
| **Includes** | All five components | Identity + narrative + voice (subset) |
| **Excludes** | — | Commercial, governance, full linguistic assets, contextVariants, rails |
| **Who reads it** | Local agents (Cursor, Claude Code, Windsurf) | Remote agents, LLM crawlers, collaborators |
| **Auth required** | No (local file) | No (public URL) |

The summary is designed for accurate representation — enough signal for on-brand generation without exposing commercially sensitive rules. See [tiers.md](tiers.md) for the exact field list.
