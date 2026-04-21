# How agents consume schemas

Agents should **not** load a full schema into context for every task.

Instead:

1. Resolve the user’s target **surface** (where the output will appear).
2. Optionally resolve the user’s **intent** (buying, researching, service, etc.).
3. Load only the schema sections needed for that surface+intent.
4. Run a **preflight** check on generated output.

## Surface + intent

In the platform canon:

- Surface enum: `platform/lib/brand-schema/types.ts` (`OutputSurface`)
- Intent enum: `platform/lib/brand-schema/types.ts` (`UserIntent`)

## Surface manifest

The surface manifest maps a surface to exactly which schema sections should be loaded:

- `platform/lib/brand-schema/surfaces/manifest.ts`

## Platform helper

- `platform/lib/brand-schema/index.ts` exports `getSchemaForSurface(brandId, surface, intent?)`.

This returns:

- a trimmed schema object (only required/optional sections)
- preflight questions
- fallback behavior

## Preflight

After generating content, run a preflight policy check:

- `platform/lib/brand-schema/index.ts` exports `preflight(schema, content)`.

Preflight is a fast, high-signal check (e.g. zero-tolerance terms, forbidden commercial language).
