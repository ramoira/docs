# Project conventions

Tools that load brand context automatically can rely on these paths, which `ramoira init` writes:

| Path | What |
|:---|:---|
| `ramoira/brand.schema.json` | The full 3.0.0 schema |
| `ramoira/agents.md` | A plain-language brief for AI tools, built from the public summary: public rules, claims, voice, the examples the brand judged, per-surface notes. Private rules stay out. |

A published summary lives at `https://ramoira.com/brands/<slug>/schema.summary.json`, where `<slug>` is `ramoira.brand_id` in the schema.

## `ramoira.config.json` (optional)

A tool that wants an explicit marker that a repository is brand-aware can look for a `ramoira.config.json` at the project root:

```json
{
  "brand_id": "your-brand-slug"
}
```

The Ramoira CLI does not read this file; it is a convention for integrations. The CLI's own settings (your API token and model key) live in `~/.ramoira/config.json`.
