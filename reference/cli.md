# CLI reference

```bash
npx ramoira init      # no install needed; or: npm install -g ramoira
```

Node.js 20.10 or later. Version 0.4.0 and later use brand schema spec 3.x.

Everything is free. Drafting, `hash`, `validate`, `check` and `book` run locally with no account. `push`, `pull`, `status` and the sign-in commands talk to ramoira.com. Nothing calls a Ramoira model; the two features that use a model (`check` on judged rules, `book --probe`) use your own Anthropic key (`ANTHROPIC_API_KEY`). Full pages for each command: [ramoira/cli docs](https://github.com/ramoira/cli/tree/main/docs).

| Command | What it does | Account | Model key |
|:---|:---|:---:|:---:|
| `ramoira init` | Sets the project up for your coding agent to draft the schema | — | — |
| `ramoira hash [file]` | Prints the schema's `content_hash`; `--write` puts it into the file | — | — |
| `ramoira validate [file]` | Checks a file is a well-formed schema (schema validity, not conformance) | — | — |
| `ramoira check [items…] --surface <s>` | Checks content against your schema's rules | — | for judged rules |
| `ramoira push [file]` | Uploads a private version for your review on ramoira.com | ✓ | — |
| `ramoira pull [file]` | Brings down the version you reviewed, and your requests if you sent it back | ✓ | — |
| `ramoira status [slug]` | Shows what is true for a brand, and where your file stands | — | — |
| `ramoira book [file]` | Renders a brand book (HTML) | — | only with `--probe` |
| `ramoira login` / `logout` / `whoami` | Signs the CLI in through your browser, and out | ✓ | — |
| `ramoira create-token [label]` | Creates an API token for CI | ✓ | — |

---

### `ramoira init`

Installs, in the current project:

- `ramoira/DRAFTING.md`, the drafting protocol your coding agent follows;
- `ramoira/spec/`, a local copy of the spec (`SPEC.md`, `SPEC.schema.json`, a minimal example);
- `ramoira/README.md`, a page for you: what the folder holds, and the flow from draft to public;
- a brand schema section in `AGENTS.md`, and in `CLAUDE.md` and `.github/copilot-instructions.md` where they exist, between `<!-- ramoira:start -->` and `<!-- ramoira:end -->`.

Then ask your agent: *"Read ramoira/DRAFTING.md and draft our brand schema."* It writes `ramoira/brand.schema.json`. Rules it proposes are `inherited` and not affirmed; fields it proposed are marked `inherited`; facts nobody knew are left `unfilled`, never invented. The result reads "Candidate — not ratified".

Run `init` again after updating the CLI: it refreshes these files and leaves your schema alone. A `README.md` you wrote yourself in `ramoira/` is left alone too. `validate`, `hash` and `push` say when the project's `DRAFTING.md` is not the one this CLI ships.

`--anchored` drafts in your browser instead, from reference brands: free, no account, no model writes anything. The CLI saves the candidate when you finish.

### `ramoira hash [file]`

Prints the schema's `content_hash`, the fingerprint of its meaning (rules and the five layers; the `ramoira` block and `draft_provenance` sit outside it). `--write` puts it into `ramoira.content_hash`. Run it after every edit.

### `ramoira validate [file]`

Checks a full schema, summary, archetype template, verdict record or adoption record against the spec's JSON Schemas and invariants. Errors name the invariant behind them; a stale `content_hash` comes with the command that fixes it. No network call; exits 1 on failure. A 2.0.0 file is checked against the old spec, with a pointer to the [migration guide](https://github.com/ramoira/brand-schema-spec/blob/main/migrations/2.0.0-to-3.0.0.md).

### `ramoira check [items…] --surface <surface>`

Runs the spec's open checker over each item (a file, or stdin) and reports what your schema's rules find. Each finding names the rule and quotes the span. Nothing suggests a rewrite.

- Exact and structural rules run without a model. Judged rules use your key and must quote the item and cite the rule's own examples, or they are void and the item needs review.
- Rules it cannot decide here are listed as not checked, with the reason.
- You declare facts (`--surface`, `--market`, `--producer`, `--producer-class`, `--brand`). No option picks or skips rules.
- Results are tooling only: a self-check, not an independent check. Against a candidate schema the verdict reads "Not certifiable", with what the findings give alongside.
- `--json` prints verdict events in the open record format. Exit codes: `0` pass, `1` fail, `2` needs review, `3` could not check.

### `ramoira push [file]`

Uploads the schema as a private version for review; nothing becomes public. `--slug <slug>` names the brand the first time (it sets `ramoira.brand_id`, which sits outside `content_hash`); pushing to a slug no one holds claims it for your account. The push records the reviewed version it is based on, and is refused when that base is out of date (run `pull`) or when the file claims an affirmation you never made in a review. After it, `ramoira/sync.json` records the push and the file's `workflow_state` reads `in_review`. See [review and publishing](../guides/publishing.md).

### `ramoira pull [file]`

Brings down the last version you reviewed, with your decisions, and records it as the base. If you sent it back, your requests are written to `ramoira/REVIEW.md` for your agent. Refuses to overwrite changes that were never pushed; `--force` replaces them. `--slug <slug>` names the brand when the project does not yet.

### `ramoira status [slug]`

Shows the facts for a brand: published, ratified, conformance checking, faithfulness, density. Fields that do not exist yet say so. Nothing here is a score. In a project, it also says where your file stands: the public version, pushed and waiting for review, changed since the last push, or behind a newer version you reviewed or made public. Signed in, it compares with your last reviewed version too.

### `ramoira book [file]`

Renders an HTML brand book from your schema, with no model and no account. Sample copy comes only from examples your brand judged. `--probe` first has your model draft sample lines for you to judge; the ones you mark become examples in your schema.

### `ramoira login`, `logout`, `whoami`

`login` shows a code and opens ramoira.com, where you sign in (GitHub, or a link sent by email) and approve. The CLI saves an API token to `~/.ramoira/config.json` and retires the one it made before on this computer. `logout` revokes the token and removes it; `whoami` shows the account. `login --manual` pastes a token from your dashboard instead.

### `ramoira create-token [label]`

Creates a labelled API token for CI. It is shown once. Set it as `RAMOIRA_TOKEN`; it takes precedence over the token `login` saved. A token can push and pull; only you, signed in, make a version public.

### `ramoira publish`

Retired in 0.4.0: an agent holds every credential the CLI does, so making a version public is the owner's act in the review. The command points to `push`.
