# Your first schema

From nothing to a published candidate. Everything here is free; only the last step needs an account.

## 1. Draft it

```bash
export ANTHROPIC_API_KEY=sk-ant-…     # your own key: init drafts with your model
npx ramoira init
```

`init` asks who is answering and about the brand: what it makes, what it believes, its tones, the words it never uses, its claims, competitors and the surfaces it writes for. Your model drafts the rest into a fixed shape, and the CLI builds a complete five-layer 3.0.0 schema:

- `ramoira/brand.schema.json`: the schema;
- `ramoira/agents.md`: a plain-language brief for your AI tools.

Along the way it shows you a few sample lines for each rule that needs judgment, and you mark each one "that's us" or "not us". Only lines you judged become examples in your schema. Rules the model proposed are marked as not yet affirmed by you.

The result is a **candidate**: your draft of what the brand means, until the brand [ratifies](../concepts/ratification.md) it.

## 2. Review and edit

Open `ramoira/brand.schema.json`. Affirm, edit or delete each proposed rule (`"affirmed": false`), and fill anything left empty. Then:

```bash
npx ramoira validate
```

## 3. See it

```bash
npx ramoira book            # an HTML brand book, no model needed
npx ramoira book --probe    # judge more sample lines first; the ones you mark become examples
```

## 4. Use it

Point your AI tools at `ramoira/agents.md` or the schema: see [Claude Code](../integrations/claude-code.md), [Cursor](../integrations/cursor.md), [Windsurf](../integrations/windsurf.md). Check drafts against your rules:

```bash
npx ramoira check --surface social_organic post.txt
```

## 5. Publish

```bash
npx ramoira login
npx ramoira publish
```

See [publishing](publishing.md).
