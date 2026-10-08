# Publishing

Publishing makes your brand's public summary available at a stable URL that remote agents, crawlers and collaborators can fetch from anywhere. It is free; it needs a free account so that your slug belongs to you.

---

## Quick start

```sh
ramoira login        # opens your browser: sign in with GitHub or an emailed link, then approve
ramoira publish
```

For CI, create a token once (`ramoira create-token ci`) and set it as `RAMOIRA_TOKEN`.

---

## What publish does

1. Checks `ramoira/brand.schema.json` locally: a valid 3.0.0 full schema, with `ramoira.brand_id` set to your slug and `ramoira.ratification` null. If not, it says why and stops, with no network call.
2. Sends the full schema to `ramoira.com/api/brands/<slug>/publish`.
3. Ramoira keeps the full schema privately, bound to its `content_hash`, and serves the public summary.

```
✓ Published.
  The slug "your-brand" is now yours. It is never given to anyone else.

  Public summary: https://ramoira.com/brands/your-brand/schema.summary.json
  Version: 1.0.0 · sha256:eaed480d372b…

  Candidate — not ratified. Publishing does not ratify the schema; your full schema stays private.
```

The first publish to a slug no one holds claims it for your account. A 2.0.0 schema is refused, with a pointer to the [migration guide](https://github.com/ramoira/brand-schema-spec/blob/main/migrations/2.0.0-to-3.0.0.md).

---

## What becomes public

| | Public summary |
|:---|:---|
| Rules with `visibility: public` | ✓ |
| `identity`, `narrative`, `voice` | ✓ in full: approved claims, examples (rejected ones too), context variants, rails, myth evolution, pillars |
| `identity.prism.culture.sacredBoundary` | Only if you opt in |
| `commercial`, `governance` | Only if you opt in (`ramoira.summary_opt_in`) |
| Rules with `visibility: private` | Never |
| `draft_provenance` | Never |

Choose what stays private per rule, with `visibility`. The defaults are the same for every brand.

---

## After publishing

```
https://ramoira.com/brands/<slug>/schema.summary.json    the public summary
https://ramoira.com/brands/<slug>/status                 facts: published, ratified, checked
https://ramoira.com/brands/<slug>                        the brand's page
```

`ramoira status` shows the same facts in your terminal.

## Publishing again

Edit the schema and run `ramoira publish` again. A changed schema has a new `content_hash` and becomes the current version; earlier versions are kept, each bound to its own hash. Publishing an unchanged schema changes nothing.

## Renaming

Rename your slug on your dashboard. The old slug redirects to the new one and is never given to another brand. Update `ramoira.brand_id` in your schema before you next publish.

---

## Publishing is not ratification

A published schema is still a **candidate**. Publishing makes the summary public; it does not mean the brand has [ratified](../concepts/ratification.md) the schema, and it certifies nothing.
