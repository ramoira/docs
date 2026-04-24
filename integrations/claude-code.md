# Using Ramoira with Claude Code

Claude Code reads `CLAUDE.md` at the start of every session. Add a schema loading instruction there and Claude Code will apply your brand schema automatically — no re-prompting between sessions.

---

## Setup

**1. Generate a schema**

```sh
npx ramoira init
```

This creates `brand.schema.json` in your current directory.

**2. Add a loading instruction to CLAUDE.md**

Create or edit `CLAUDE.md` in your project root:

```markdown
## Brand context

This project uses a Ramoira brand schema. Before generating any copy, marketing content,
or user-facing text, read `brand.schema.json` and apply it.

Priority fields:
- `identity.summary.neverDo` — absolute constraints, always apply
- `voice.approvedTones` and `voice.forbiddenTones`
- `voice.examples` — study the rejected examples; they define what this brand never sounds like
- `voice.base.structuralRules` — sentence-level constraints
- `governance.preflight` — run these three questions on any output before returning it
```

**3. (Optional) Publish for remote access**

If you work across machines or want remote agents to access your schema:

```sh
export RAMOIRA_TOKEN=your_token
npx ramoira publish
```

Published summary URL: `https://ramoira.com/brands/[slug]/schema.summary.json`

---

## How agents should load the schema

For most tasks, load `brand.schema.json` directly from the project directory.

For surface-specific generation, focus on the fields relevant to that surface:

| Surface | Key fields to load |
|:---|:---|
| `social_organic` | `identity.summary`, `voice.approvedTones`, `voice.forbiddenTones`, `voice.examples` |
| `email_acquisition` | above + `commercial.pricing` (check discount/urgency permissions) |
| `product_detail_page` | above + `voice.base.structuralRules`, `commercial.claims` |
| `paid_landing_page` | above + `commercial.claims.forbidden`, `governance.severity.absolute` |

Always run `governance.preflight` questions on output before returning it.

---

## Validating a schema

```sh
npx ramoira validate
```

Exits 0 on success, 1 on failure with error list. CI-friendly — no network call required.
