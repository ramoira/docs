# Using Ramoira with Lovable

Lovable generates UI and copy from prompts. Attach your brand schema as context so generated copy is on-brand without manual briefing.

---

## Setup

**1. Generate a schema**

```sh
npx ramoira init
```

**2. Attach schema context**

For most tasks, attach `brand.schema.json` directly in your Lovable session.

For tasks that do not require commercial or governance detail (e.g. UI copy, social content), the published summary is sufficient and smaller:

```
https://ramoira.com/brands/[slug]/schema.summary.json
```

Get a published URL by running `ramoira publish` after creating an account at ramoira.com.

---

## What to include in your prompt

When attaching the schema, tell Lovable which fields to prioritise:

```
Apply the attached brand schema. Constraints to apply:
- identity.summary.neverDo — never violate these
- voice.approvedTones — match these tones
- voice.forbiddenTones — avoid these tones
- voice.examples — the rejected examples show what this brand will never sound like
- governance.preflight — verify copy against these three questions before finalising
```

---

## Surface-specific guidance

| Task | Schema to attach | Key fields |
|:---|:---|:---|
| UI copy, labels | Summary | `identity.summary`, `voice` |
| Landing page copy | Full | `voice`, `commercial.claims`, `commercial.pricing` |
| Social content | Summary | `voice.examples`, `narrative.myth` |
| Email | Full | `voice`, `commercial.pricing` (check urgency permissions) |
