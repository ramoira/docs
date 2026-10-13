# Working with Claude on brand content

Use your brand schema as standing context for Claude (Claude Desktop, claude.ai or Claude Code), so you write against your brand's rules without long "you are a brand copywriter" prompts.

> **A drafted schema is a candidate.** What your agent drafts is a candidate until the brand reviews and [ratifies](../concepts/ratification.md) it. Content written with it is not "certified" or "approved" by having used it, and a check you run on your own drafts is a self-check, not an independent check.

## 1. Capture the brand

```bash
npx ramoira init
```

Then, in Claude Code, ask: *"Read ramoira/DRAFTING.md and draft our brand schema."* Claude reads your project and anything you give it, asks what it can't find, and writes `ramoira/brand.schema.json`. No model key of yours is needed for drafting. Review it on your dashboard and make it public when it says what your brand means: see [review and publishing](publishing.md). Without Claude Code, draft in your browser at [ramoira.com/draft](https://ramoira.com/draft).

Then read it back as a person would:

```bash
npx ramoira book
```

This writes an HTML brand book (open it in a browser). If something reads wrong, change it: ask Claude, or use **Revise** on your dashboard. `npx ramoira book --probe` lets you judge sample lines first (your own Anthropic key).

## 2. Give Claude the context

- **Claude Desktop or claude.ai:** attach `ramoira/brand.schema.json` to a Project or a conversation, or give it your public summary URL.
- **Claude Code:** `init` already added a brand schema section to `AGENTS.md` (and to `CLAUDE.md` if you have one). See [Claude Code](../integrations/claude-code.md).

## 3. Prompts

Because Claude has the schema, the prompt only needs the task.

**Landing page hero**
> Using the attached brand brief, write three options for a hero headline and subheadline for [product, one line]. Follow every rule, make only the product claims the brief lists, and match the examples the brand judged.

**Error and empty states**
> Using the brand brief, write the copy for: a wrong password, a 404 page, and a "form sent" confirmation. Keep to the brand's tones and humour setting, and to the customer service surface notes if there are any.

**Social launch post**
> Using the brand brief, draft a LinkedIn post and a shorter X post announcing [feature]. Ground it in the brand's myth and pillars, and keep to the rules for social surfaces.

## 4. Check your drafts

Run the brand's rules over a draft, yours or Claude's:

```bash
npx ramoira check --surface social_organic post.txt
```

`check` names each rule a draft breaks and quotes the span; judged rules use your own key. It does not rewrite anything: deciding the fix is yours (or ask Claude, with the finding in hand). It is a self-check, useful before review, not a substitute for the brand's own review.
