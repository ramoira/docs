# CLI reference

Install:

```bash
npm install -g ramoira
```

## Commands

### `ramoira init`

Generates a brand schema locally.

Expected outputs (by convention):

- `./ramoira/brand.schema.json`
- `./ramoira/brand.schema.summary.json`

### `ramoira validate`

Validates schema files against the spec/validators.

### `ramoira publish`

Publishes the summary schema to ramoira.com (free account required). Publishing does not ratify the schema; it stays a candidate.

### `ramoira status`

Shows current schema state (local version, last publish, etc.).
