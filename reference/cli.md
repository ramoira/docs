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

Publishes the summary schema (may require an account, depending on deployment).

### `ramoira status`

Shows current schema state (local version, last publish, etc.).
