# Content pipeline integration

In a pipeline, treat the schema as a versioned dependency:

- Load schema by version/alias (e.g. `current`)
- Generate content for a specific surface
- Check your own output against the schema (a self-check: useful tooling, not an independent check)
- Store the schema version alongside output for traceability

A schema the brand has not ratified is a **candidate**. Content produced with it is not "certified" or "approved" by having used it.
