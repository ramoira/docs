# Schema Fields (Platform Canon)

This page documents the **canonical schema shape** used by `platform/lib/brand-schema`.

If you are working from the open spec repo, see `brand-schema-spec/layers/*` for component-by-component documentation.

## Top-level object

A full schema is a JSON object with these keys:

- `meta`
- `identity`
- `narrative`
- `voice`
- `commercial`
- `governance`

## Shared primitives

Common primitives (and enums) live in:

- `platform/lib/brand-schema/types.ts`

Notable primitives:

- `ConstraintSeverity`: `absolute | strong | contextual`
- `FallbackBehaviour`: `refuse_to_generate | escalate_to_human | use_brand_default | use_minimal_safe`
- `OutputSurface`: fixed enum of supported surfaces (search, PDP, email, social, etc.)
- `UserIntent`: fixed enum of user intents (high-intent buyer, comparison, service enquiry, etc.)
- `Constrained<T>`: `{ value, severity, rationale? }`
- `Rail`: `{ context, instruction, example?, antiExample? }`

## Component field maps

- Identity: `brand-schema-spec/layers/identity.md`
- Narrative: `brand-schema-spec/layers/narrative.md`
- Voice: `brand-schema-spec/layers/voice.md`
- Commercial: `brand-schema-spec/layers/commercial.md`
- Governance: `brand-schema-spec/layers/governance.md`

## Surface manifest (how agents load the schema)

Agents should **not** load the entire schema by default.

Use the surface manifest to load only the relevant sections:

- `platform/lib/brand-schema/surfaces/manifest.ts`

Platform helper:

- `platform/lib/brand-schema/index.ts` exports `getSchemaForSurface(brandId, surface, intent?)`.
