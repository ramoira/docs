# Brand-aware copy

A brand-aware copy workflow is:

1. Resolve surface + intent.
2. Load trimmed schema via surface manifest.
3. Generate copy with positive rails.
4. Preflight the result.
5. If preflight fails, regenerate using the violations as constraints.

See:

- `docs/concepts/how-agents-consume-schemas.md`
- `docs/reference/schema-fields.md`
