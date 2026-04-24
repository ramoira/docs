# Using Ramoira with Windsurf

Windsurf reads project files and uses them as context. Add `brand.schema.json` to your project and reference it in your Windsurf rules — the schema will be applied automatically.

---

## Setup

**1. Generate a schema**

```sh
npx ramoira init
```

This creates `brand.schema.json` in your current directory.

**2. Add a brand rule**

Add to your Windsurf rules file or system prompt:

```
This project has a Ramoira brand schema at brand.schema.json.

Before generating any copy or user-facing content, read brand.schema.json and apply:
- identity.summary.neverDo as absolute constraints
- voice.approvedTones / voice.forbiddenTones
- voice.examples (pay attention to rejected examples)
- voice.base.structuralRules
- governance.preflight — verify output against all three questions
```

**3. (Optional) Publish for remote access**

```sh
export RAMOIRA_TOKEN=your_token
npx ramoira publish
```

Published summary URL: `https://ramoira.com/brands/[slug]/schema.summary.json`

---

## Priority fields

| Field | Why it matters |
|:---|:---|
| `identity.summary.neverDo` | Absolute constraints — apply to every surface |
| `voice.forbiddenTones` | Tonal hard stops |
| `voice.examples` (rejected) | The contrast signal — what this brand never sounds like |
| `voice.base.structuralRules` | Sentence-level shaping |
| `governance.severity.absolute.constraints` | Non-negotiable constraints |
