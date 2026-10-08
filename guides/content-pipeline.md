# Content pipeline integration

In a pipeline, treat the schema as a versioned dependency:

- Pin the schema by its `content_hash`, the true version of its meaning (`schema_version` is only a label).
- Write for a declared surface.
- Run `ramoira check --surface <surface> --json` on each item: one verdict event per item, in the open record format, with exit codes for CI (`0` pass, `1` fail, `2` needs review, `3` could not check). This is a self-check, not an independent check.
- Store the `content_hash` alongside each output, so you can tell which version of the brand's meaning it was written and checked against.

A schema the brand has not ratified is a **candidate**. Content produced with it is not "certified" or "approved" by having used it.
