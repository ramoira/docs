# Project conventions

Tools that load brand context automatically can rely on these paths. `ramoira init` writes the protocol and the pages; your agent writes the schema; `push` and `pull` write the sync files.

| Path | What | Commit it |
|:---|:---|:---:|
| `ramoira/brand.schema.json` | The full 3.x schema | ✓ |
| `ramoira/DRAFTING.md` | The drafting protocol a coding agent follows | ✓ |
| `ramoira/spec/` | A local copy of the spec: `SPEC.md`, `SPEC.schema.json`, a minimal example, and a README naming the versions | ✓ |
| `ramoira/README.md` | A page for the person: draft, review, make public, the next version | ✓ |
| `ramoira/sync.json` | Where the file stands on ramoira.com: `brand` (the slug), `base` (the `content_hash` of the version last reviewed, as pulled), `pushed` (the last one pushed). Like a branch's upstream | ✓ |
| `ramoira/REVIEW.md` | The owner's requests, when they sent a version back. Written by `pull`, removed when nothing is waiting | ✓ |
| `ramoira/agents.md` | Only after `init --anchored`: a plain-language brief built from the public summary. Private rules stay out | ✓ |
| `AGENTS.md`, `CLAUDE.md`, `.github/copilot-instructions.md` | A brand schema section between `<!-- ramoira:start -->` and `<!-- ramoira:end -->`; `init` replaces only that section | ✓ |

A public summary lives at `https://ramoira.com/brands/<slug>/schema.summary.json`, where `<slug>` is `ramoira.brand_id` in the schema.

## Environment

| Variable | What |
|:---|:---|
| `RAMOIRA_TOKEN` | An API token (CI). Takes precedence over the token `ramoira login` saved |
| `RAMOIRA_API_URL` | The server to talk to; default `https://ramoira.com` |
| `RAMOIRA_CONFIG_DIR` | Where the CLI keeps its own settings; default `~/.ramoira` |
| `ANTHROPIC_API_KEY` | Your own key, for judged rules in `check` and for `book --probe` |

The CLI's own settings (your API token, and a model key if you entered one) live in `~/.ramoira/config.json`, never in the project.

## `ramoira.config.json` (optional)

A tool that wants an explicit marker that a repository is brand-aware can look for a `ramoira.config.json` at the project root:

```json
{
  "brand_id": "your-brand-slug"
}
```

The Ramoira CLI does not read this file; it is a convention for integrations.
