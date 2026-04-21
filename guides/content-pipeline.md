# Content pipeline integration

In a pipeline, treat the schema as a versioned dependency:

- Load schema by version/alias (e.g. `current`)
- Generate content for a specific surface
- Preflight check
- Store the schema version alongside output for traceability

In the platform implementation, publication and lifecycle are documented in:

- `platform/docs/brand-schema-generation.md`
