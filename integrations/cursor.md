# Using Ramoira with Cursor

Cursor reads files in your project directory. Add `brand.schema.json` to your project and reference it in your system prompt or rules file — Cursor will apply your brand schema automatically.

---

## Setup

**1. Generate a schema**

```sh
npx ramoira init
```

This creates `brand.schema.json` in your current directory.

**2. Add a brand rule to `.cursorrules`**

Create or edit `.cursorrules` in your project root:

```
## Brand context

This project uses a Ramoira brand schema at brand.schema.json.

Before generating any copy, marketing content, or user-facing text:
1. Read brand.schema.json
2. Apply identity.summary.neverDo as absolute constraints
3. Match voice.approvedTones; avoid voice.forbiddenTones
4. Study voice.examples — especially the rejected ones
5. Apply voice.base.structuralRules to every sentence
6. Check output against governance.preflight before returning
```

**3. (Optional) Publish for remote access**

```sh
export RAMOIRA_TOKEN=your_token
npx ramoira publish
```

Published summary URL: `https://ramoira.com/brands/[slug]/schema.summary.json`

---

## How agents should load the schema

For most tasks, Cursor reads `brand.schema.json` directly from the project.

Priority fields for generation:

1. `identity.summary` — one-line brief, three adjectives, never-do list
2. `voice.approvedTones` and `voice.forbiddenTones`
3. `voice.examples` — the rejected examples define what this brand will never sound like
4. `voice.base.structuralRules` — prose-level constraints
5. `governance.severity.absolute.constraints` — hard stops

---

## Remote schema access

For agents outside your project directory, the published summary is publicly accessible:

```
https://ramoira.com/brands/[slug]/schema.summary.json
```

This URL can be added to any agent context that accepts external URLs.
