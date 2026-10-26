# Your first schema

From nothing to a public candidate, with your own coding agent. Drafting needs no account and no model key of yours; only uploading for review needs a free account.

## 1. Set up the project

In your project (any folder your coding agent works in):

```bash
npx ramoira init
```

`init` installs:

- `ramoira/DRAFTING.md`: the drafting protocol your agent follows;
- `ramoira/spec/`: a local copy of the spec, so an agent without web access has the format;
- `ramoira/README.md`: a page for you, from draft to public and on to the next version;
- a short brand schema section in `AGENTS.md` (and in `CLAUDE.md` or `.github/copilot-instructions.md` where you have them), pointing your agents at the schema.

It works with Claude Code, Codex, GitHub Copilot in VS Code, Cursor, Windsurf and JetBrains agents: anything that reads project instructions. No coding agent? Draft in your browser instead: `npx ramoira init --anchored`, or [ramoira.com/draft](https://ramoira.com/draft).

## 2. Draft it with your agent

Ask your agent:

> Read ramoira/DRAFTING.md and draft our brand schema.

It reads your project and anything you give it (your site, a pitch deck, notes), asks only what it can't find, and writes `ramoira/brand.schema.json` with all five layers. It never invents facts and never writes marketing copy into the schema. Every rule it proposes is marked `"affirmed": false`, and every field it proposed is marked as proposed: only you, in the review, can affirm them.

The agent finishes by fingerprinting and validating the file:

```bash
npx ramoira hash --write
npx ramoira validate
```

Run `hash --write` after every edit: `validate` refuses a file whose `content_hash` no longer matches it, and says so.

The result is a **candidate**: your draft of what the brand means, until the brand [ratifies](../concepts/ratification.md) it.

## 3. Upload it for your review

```bash
npx ramoira login                     # free; your browser opens to sign in with GitHub or an emailed link
npx ramoira push --slug your-brand    # the slug only the first time
```

Nothing becomes public. The push is a private version waiting for you on your dashboard. See [review and publishing](publishing.md).

## 4. Review it and make it public

On [ramoira.com/dashboard](https://ramoira.com/dashboard), signed in: affirm, edit or delete each rule; keep or rewrite each proposed field; then **Make public**, or **Send back to your agent** with what to change. Then, in your project:

```bash
npx ramoira pull
```

## 5. Use it

Your agents find the schema through the section `init` added to `AGENTS.md`. For other tools, see [Claude Code](../integrations/claude-code.md), [Cursor](../integrations/cursor.md), [Windsurf](../integrations/windsurf.md), [Lovable](../integrations/lovable.md), [v0](../integrations/v0.md) and [Replit](../integrations/replit.md). Check drafts against your rules:

```bash
npx ramoira check --surface social_organic post.txt
```

And see it as a brand book:

```bash
npx ramoira book            # an HTML brand book, no model needed
npx ramoira book --probe    # judge sample lines first (your own Anthropic key); the ones you mark become examples
```
