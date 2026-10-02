# Brand-aware copy

A brand-aware copy workflow is:

1. Resolve surface + intent.
2. Load only the schema sections that surface needs.
3. Generate copy with positive rails.
4. Check the result against the schema's hard rules.
5. If the check fails, regenerate using the violations as constraints.

The check in step 4 is a self-check: useful tooling, not an independent check and not certification. A schema the brand has not ratified is a candidate.

See:

- `docs/concepts/how-agents-consume-schemas.md`
- `docs/reference/schema-fields.md`
