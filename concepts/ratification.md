# Ratification

**Ratification** is the act by which a brand makes a schema its own. Until a brand ratifies a schema, the schema is a **candidate**: a proposal, however good, with no more standing than a well-written brief.

> **Status.** Ratification is not available yet. How a brand records a ratification, and who inside a brand may do it, is still being defined. Until then every schema, local or published, is a candidate (in a 3.0.0 schema, `ramoira.ratification` is `null`). Recording a ratification will be free.

---

## Proposing is not authoring

Anyone can draft a schema: `ramoira init`, an agency, a brand team, a drafting tool working from an archetype. Drafting is useful, and it is free. But whoever drafts a schema is *proposing* what the brand means. The brand decides.

Ratification is where that decision is recorded. It is made by someone with authority over what the brand means, not by the tool or the producer that drafted the schema. A producer that defined the measure its own work is checked against would be marking its own homework.

Two things are **not** ratification:

- **Publishing.** `ramoira publish` puts a summary on a public URL. A published schema is still a candidate until it is ratified.
- **Account ownership.** `account_owner_verified` means the account controls the brand's slug. It says nothing about whether the brand has ratified the schema.

---

## A ratification binds to one version

A ratification is recorded against a schema's `content_hash`: a fingerprint of the rules and the five layers (see the [spec](https://github.com/ramoira/brand-schema-spec/blob/main/SPEC.md), section 4.1).

- **Edit the schema and the hash changes.** The edited file is an unratified edit of a ratified version until the brand ratifies the new hash.
- **Earlier results stay with the earlier version.** A check run against the old hash still describes the old hash. A new ratification applies from then on; it never rewrites past results.
- **Process notes are outside the hash.** Changing the workflow state, a publishing choice or the drafting notes does not create a new version.

In the file, `ramoira.ratification` is a pointer:

```json
"ratification": {
  "ratification_id": "…",
  "ratified_hash": "sha256:…",
  "ratified_at": "2026-…",
  "ratifier_role": "…"
}
```

It is valid only if `ratified_hash` equals the file's `content_hash`. The authoritative record of the ratification is kept by Ramoira, not in the file: a record kept in a file the brand can edit would be self-report.

---

## What must be true before a schema can be ratified

Spec 3.0.0 builds in two checks. The reference validator refuses a ratification pointer when either fails.

1. **No inherited rule is unaffirmed.** A schema drafted from a template starts with that template's rules marked `provenance: inherited, affirmed: false`. A template's prohibitions have no reason to bind a particular brand until the brand says so. Each must be affirmed, edited or deleted.
2. **Every example a judged rule relies on was judged by the brand.** A `judged_bounded` rule is decided by comparing content with approved and rejected examples. In a ratified schema those examples must be `judged_by: brand_owner` or `brand_team`. Examples written by a template, a draft or a producer can guide writers, but cannot decide a check until the brand affirms them.

Fields the drafter left empty (`unfilled` in `draft_provenance`) do not block ratification, but they are shown as gaps: a thin schema checks little.

---

## What ratification does and does not mean

**It means** the brand stands behind this version as its own measure. Content can then be checked against it with standing: a check against a candidate is only ever a self-check.

**It does not mean:**

- that the schema is good, complete or faithful to the brand (see [Conformance and faithfulness](conformance-vs-faithfulness.md));
- that any content is approved or certified;
- anything about price or tier. Ratifying is free, and a ratified schema is the same format at every tier.
