# Using Ramoira with Claude Code

## Setup

1. Install the CLI
   ```bash
   npm install -g ramoira
   ```

2. Generate a schema
   ```bash
   ramoira init
   ```

3. Ensure Claude Code can access your project files

If your integration supports auto-loading schema context, point it at:

- `./ramoira/brand.schema.json` (full)
- `./ramoira/brand.schema.summary.json` (summary)

## Recommended runtime pattern

- Resolve surface + intent
- Load only relevant schema sections via the surface manifest (platform canon)
- Generate content
- Run preflight checks
