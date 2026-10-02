# How agents consume schemas

Agents should **not** load a full schema into context for every task.

Instead:

1. Resolve the user’s target **surface** (where the output will appear).
2. Optionally resolve the user’s **intent** (buying, researching, service, etc.).
3. Load only the schema sections needed for that surface+intent.
4. Check the generated output against the schema's hard rules before using it.

## Surface + intent

The valid values are defined in the spec ([brand-schema-spec](https://github.com/ramoira/brand-schema-spec), `SPEC.md`, "Shared primitive types"):

- Surface: `OutputSurface` (17 values)
- Intent: `UserIntent` (8 values)

## Checking your own output

A fast check catches the highest-signal failures: zero-tolerance terms (`governance.compliance.zeroToleranceTerms`), forbidden words, and forbidden commercial language.

A check you run on your own output is a self-check. It is useful tooling, but it is not an independent check and not certification. A schema the brand has not ratified is a candidate, and output is not "certified" by having been checked against it.
