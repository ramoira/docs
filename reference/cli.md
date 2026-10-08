# CLI reference

```bash
npm install -g ramoira      # or run any command with npx ramoira …
```

Everything is free. Only `publish`, `status`, `login` and the token commands talk to ramoira.com; the rest run locally. Commands that use a model use your own Anthropic key (`ANTHROPIC_API_KEY`). Full pages for each command: [ramoira/cli docs](https://github.com/ramoira/cli/tree/main/docs).

| Command | What it does | Account | Model key |
|:---|:---|:---:|:---:|
| `ramoira init` | Drafts a candidate 3.0.0 schema from a short questionnaire | — | ✓ |
| `ramoira validate [file]` | Checks a file is a well-formed schema (schema validity, not conformance) | — | — |
| `ramoira check [items…] --surface <s>` | Checks content against your schema's rules | — | for judged rules |
| `ramoira book [file]` | Renders a brand book (HTML) | — | only with `--probe` |
| `ramoira publish [file]` | Publishes the schema; the summary becomes public | ✓ | — |
| `ramoira status [slug]` | Shows what is true for a brand: published, ratified, checked | — | — |
| `ramoira login` / `logout` / `whoami` | Signs the CLI in through your browser, and out | ✓ | — |
| `ramoira create-token [label]` | Creates an API token for CI | ✓ | — |

---

### `ramoira init`

Asks who is answering, what the brand makes and believes, its tones, the words it never uses, its claims, competitors and surfaces. Your model drafts the rest into a fixed shape, and the CLI builds a complete five-layer schema at `ramoira/brand.schema.json`, plus `ramoira/agents.md`, a plain-language brief for your AI tools.

- Rules you typed are `authored`. Rules the model proposes are `inherited` and not affirmed: the schema cannot be ratified until you affirm, edit or delete each one.
- For judged rules, your model drafts a few sample lines and you mark each "that's us" or "not us". Only lines you judged become examples. A judged rule you can't back with both becomes a guidance question instead. `--no-probes` skips this.
- The result reads "Candidate — not ratified".

### `ramoira validate [file]`

Checks a full schema, summary, archetype template, verdict record or adoption record against the 3.0.0 JSON Schemas and the spec's invariants. Errors name the invariant behind them. No network call; exits 1 on failure. A 2.0.0 file is checked against the old spec, with a pointer to the [migration guide](https://github.com/ramoira/brand-schema-spec/blob/main/migrations/2.0.0-to-3.0.0.md).

### `ramoira check [items…] --surface <surface>`

Runs the spec's open checker over each item (a file, or stdin) and reports what your schema's rules find. Each finding names the rule and quotes the span. Nothing suggests a rewrite.

- Exact and structural rules run without a model. Judged rules use your key and must quote the item and cite the rule's own examples, or they are void and the item needs review.
- Rules it cannot decide here are listed as not checked, with the reason.
- You declare facts (`--surface`, `--market`, `--producer`, `--producer-class`, `--brand`). No option picks or skips rules.
- Results are tooling only: a self-check, not an independent check. Against a candidate schema the verdict reads "Not certifiable", with what the findings give alongside.
- `--json` prints verdict events in the open record format. Exit codes: `0` pass, `1` fail, `2` needs review, `3` could not check.

### `ramoira book [file]`

Renders an HTML brand book from your schema, with no model and no account. Sample copy comes only from examples your brand judged. `--probe` first has your model draft sample lines for you to judge; the ones you mark become examples in your schema.

### `ramoira publish [file]`

Sends your full 3.0.0 schema to ramoira.com. The slug is `ramoira.brand_id`; publishing to a slug no one holds claims it. Ramoira keeps the full schema private and serves the public summary. Publishing again with the same content changes nothing. Publishing does not ratify the schema. See [publishing](../guides/publishing.md).

### `ramoira status [slug]`

Shows the facts for a brand: published, ratified, conformance checking, faithfulness, density. Fields that do not exist yet say so. Nothing here is a score.

### `ramoira login`, `logout`, `whoami`

`login` shows a code and opens ramoira.com, where you sign in (GitHub, or a link sent by email) and approve. The CLI then saves an API token to `~/.ramoira/config.json`. `logout` removes it; `whoami` shows the account.

### `ramoira create-token [label]`

Creates a labelled API token for CI. It is shown once. Set it as `RAMOIRA_TOKEN`.
