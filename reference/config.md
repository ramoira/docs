# `ramoira.config.json`

`ramoira.config.json` is a small project-root config file used by tools that automatically load brand context.

## Recommended convention

If you are building an agent/tool integration and want a default:

- presence of `ramoira.config.json` indicates “this repo is brand-aware”
- schema files live at:
  - `./ramoira/brand.schema.json`
  - `./ramoira/brand.schema.summary.json`

A minimal config can include:

```json
{
  "brandId": "your-brand-id"
}
```
