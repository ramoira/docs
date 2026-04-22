# docs — Roadmap

> Goal: every user-facing doc reflects v2.0.0 field names and structure. No v1.0 field names after this phase.
> Cross-repo context: [cli/ROADMAP.md](../cli/ROADMAP.md)

**Last updated:** 2026-04-22
**Blocked on:** brand-schema-spec Phase 1 examples (1.5–1.8)

---

## Progress

| Task | Status |
| :--- | :--- |
| 2.1 Rewrite schema-fields.md | ⬜ |
| 2.2 Rewrite tiers.md | ⬜ |
| 2.3 Rewrite what-is-a-brand-schema.md | ⬜ |
| 2.4 Rewrite api.md | ⬜ |
| 2.5 Expand publishing.md | ⬜ |
| 2.6 Update integration docs | ⬜ |

---

## Ground truth sources

- `platform/lib/brand-schema/types.ts` — all enum values
- `platform/lib/brand-schema/components/` — all 5 component type definitions
- `brand-schema-spec/SPEC.md` — field documentation (read this after Phase 1 is stable)
- `brand-schema-spec/SPEC.summary.schema.json` — exact summary field selection policy

---

## 2.1 — Rewrite schema-fields.md

**File:** `reference/schema-fields.md`

Replace v1.0 field names with v2.0.0 component architecture. Include:

- Field tables for all 5 components using v2.0.0 names
- Enum value lists: OutputSurface (17), UserIntent (8), RelationshipMode (8), PricingStyle (5), ConstraintSeverity (3), FallbackBehaviour (4), SentenceLength (4), HumourStyle (6)
- `Constrained<T>` and `Rail` type documentation with usage rule (use Constrained when severity changes how a generation pipeline responds; plain string otherwise)
- Full/Summary column marking which fields appear in each tier

---

## 2.2 — Rewrite tiers.md

**File:** `concepts/tiers.md`

Expand the current 14-line stub:

- Three-tier model (Local / Published / Studio) with v2.0.0 field-level detail
- How brand market tier encodes into `commercial.pricing.style` and pricing flags — not a separate field
- Summary field selection policy — exactly which v2.0.0 fields are in the free summary (source: `brand-schema-spec/SPEC.summary.schema.json`)
- What Studio adds: full contrast cluster (typographicVoice, minimumConnotativeTest, contextVariants, zeroToleranceTerms)
- Schema lifecycle: workflowState transitions (draft → in_review → published) and certified flag

---

## 2.3 — Rewrite what-is-a-brand-schema.md

**File:** `concepts/what-is-a-brand-schema.md`

Currently describes v1.0. Rewrite to describe v2.0.0:

- Five components and what problem each solves
- Component architecture — what `_component` and `_version` mean
- The difference between full schema and summary

---

## 2.4 — Rewrite api.md

**File:** `reference/api.md`

Replace internal-only endpoint list with Brand API reference:

- Public schema endpoints (GET — no auth): `GET /brands/[slug]/schema.summary.json`, `GET /brands/[slug]/status`
- CLI-facing endpoints (POST — requires token): publish, validate
- Governance webhook endpoints
- Auth model and token scoping
- Schema for request/response bodies (reference platform Zod schemas)

---

## 2.5 — Expand publishing.md

**File:** `guides/publishing.md`

Add v2.0.0-specific detail:

- What gets extracted into the summary (exact field list from `brand-schema-spec/SPEC.summary.schema.json`)
- What stays local (commercial + governance components; full contextVariants; full linguistic assets)
- What Studio unlocks in the public summary

---

## 2.6 — Update integration docs

**Files:** `integrations/claude-code.md`, `cursor.md`, `windsurf.md`, `lovable.md`, `v0.md`

Each integration doc describes schema loading in v1.0 terms. Update to v2.0.0:

- Reference `identity.summary.neverDo` and `voice.base.structuralRules` as highest-priority agent load
- Reference surface-trimmed loading (surfaces manifest from `platform/lib/brand-schema/surfaces/manifest.ts`)
- Update schema URL patterns to `ramoira.com/brands/[slug]/schema.summary.json`
