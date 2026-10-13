# Brand-aware copy

A workflow for an agent or a team writing for a brand:

1. Decide the **surface** (and, if useful, the user intent).
2. Load what that surface needs from the schema: its rules, the examples the brand judged, approved tones, the surface's context variant and rails, and the approved claims. See [how agents consume schemas](../concepts/how-agents-consume-schemas.md).
3. Write.
4. Check the draft: `ramoira check --surface <surface> draft.txt`.
5. Revise what the check flags, and check again.

Step 4 is a self-check: useful tooling, not an independent check and not certification. The checker flags and cites; it never suggests the revision. A schema the brand has not ratified is a candidate.

See:

- [How agents consume schemas](../concepts/how-agents-consume-schemas.md)
- [Schema fields](../reference/schema-fields.md)
- [CLI reference](../reference/cli.md)
