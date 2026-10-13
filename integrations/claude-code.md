# Using Ramoira with Claude Code

Claude Code reads `CLAUDE.md` at the start of every session. Point it at your brand brief there, and it works from your schema without re-briefing.

> **A drafted schema is a candidate.** What your agent drafts is a candidate until the brand reviews and [ratifies](../concepts/ratification.md) it. Content your tools write with it is not "certified" or "approved" by having used it.

---

## Setup

**1. Draft a schema**

```sh
npx ramoira init
```

Then ask your coding agent: *"Read ramoira/DRAFTING.md and draft our brand schema."* It writes `ramoira/brand.schema.json`. See [your first schema](../guides/first-schema.md).

**2. Point `CLAUDE.md` at the schema**

`init` already adds a brand schema section to `CLAUDE.md` when the file exists, and always to `AGENTS.md`. If your project has no `CLAUDE.md`, create one with the line `@AGENTS.md`, or add this yourself:

```markdown
## Brand context

Before writing any copy or user-facing text, run `npx ramoira status` (if it says the file is behind,
`npx ramoira pull`), then read `ramoira/brand.schema.json`. Follow its rules exactly: absolute rules are never broken.
Make only the product claims it lists. Match its voice and its judged examples.
After drafting, run `npx ramoira check --surface <surface> <file>` and fix what it flags.
```

---

## What to load, in order

1. The **rules** for the surface (`surfaces` is `"all"` or includes it). `absolute` rules are never broken; `strong` ones need the brand's sign-off.
2. **`voice.examples`**: the brand's approved and rejected lines, with reasons. The rejected ones show what it never sounds like.
3. **`voice.approvedTones`** and `voice.base` (vocabulary, humour).
4. **`voice.contextVariants`** for the surface, and the **rails**.
5. **`narrative.semiotic.denotative.claims`**: the only product claims the brand makes.

The schema holds everything: rules (private ones too), claims, tones, the examples the brand judged, context variants and rails. Tools without project files can use the public summary instead (below), which leaves out private rules and, unless the brand opted in, the commercial and governance layers.

## Checking a draft

```sh
npx ramoira check --surface product_detail_page draft.txt
```

`check` runs the schema's rules over the draft and names each rule it breaks, quoting the span. It does not suggest rewrites; revising is yours. It is a self-check, not an independent check.

---

## Remote access (optional)

```sh
npx ramoira login
npx ramoira push --slug your-brand
```

Review it on your dashboard and press **Make public**. The public summary is then at `https://ramoira.com/brands/<slug>/schema.summary.json`. It holds the public rules and the identity, narrative and voice layers; private rules and the commercial and governance layers stay out unless the brand opted them in.
