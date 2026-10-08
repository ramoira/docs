# Using Ramoira with Lovable

Lovable generates UI and copy from prompts. Give it your brand brief as context, so its copy starts from your schema instead of the category default.

> **A drafted schema is a candidate.** What `ramoira init` produces is a draft until the brand reviews and [ratifies](../concepts/ratification.md) it. Content your tools write with it is not "certified" or "approved" by having used it.

---

## Setup

**1. Draft a schema**

```sh
npx ramoira init
```

This writes `ramoira/brand.schema.json` and `ramoira/agents.md`, a plain-language brief for AI tools.

**2. Attach the brief**

Attach `ramoira/agents.md` to your Lovable session, or give it the published summary URL (see below). Then:

```
Apply the attached brand brief. Follow its rules exactly; make only the product claims it lists;
match its approved tones and the examples the brand judged.
```

---

## What to load, in order

1. The **rules** for the surface (`surfaces` is `"all"` or includes it). `absolute` rules are never broken; `strong` ones need the brand's sign-off.
2. **`voice.examples`**: the brand's approved and rejected lines, with reasons. The rejected ones show what it never sounds like.
3. **`voice.approvedTones`** and `voice.base` (vocabulary, humour).
4. **`voice.contextVariants`** for the surface, and the **rails**.
5. **`narrative.semiotic.denotative.claims`**: the only product claims the brand makes.

`ramoira/agents.md` covers the public rules, claims, tones, the examples the brand judged and per-surface notes, taken from the public summary. Read the schema itself for rails and private rules.

## Which context for which task

| Task | Attach | Why |
|:---|:---|:---|
| UI copy, labels, social | `ramoira/agents.md` or the summary | Rules, tones, examples |
| Landing pages, pricing, claims | `ramoira/brand.schema.json` | Adds private rules and the commercial layer, if the brand keeps them private |

## Checking a draft

```sh
npx ramoira check --surface product_detail_page draft.txt
```

`check` runs the schema's rules over the draft and names each rule it breaks, quoting the span. It does not suggest rewrites; revising is yours. It is a self-check, not an independent check.

---

## Remote access (optional)

```sh
npx ramoira login
npx ramoira publish
```

The public summary is then at `https://ramoira.com/brands/<slug>/schema.summary.json`. It holds the public rules and the identity, narrative and voice layers; private rules and the commercial and governance layers stay out unless the brand opted them in.
