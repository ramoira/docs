# First schema guide

This guide describes the expected workflow with the Ramoira CLI.

## 1) Install

```bash
npm install -g ramoira
```

## 2) Generate a schema

From your project root:

```bash
ramoira init
```

Expected output (by convention):

- `./ramoira/brand.schema.json` (full schema)
- `./ramoira/brand.schema.summary.json` (summary schema)

## 3) Use it in an agent

Most MCP-aware tools can be configured to load schema context automatically.

See integrations:

- `integrations/cursor.md`
- `integrations/claude-code.md`
- `integrations/windsurf.md`
