# Using Ramoira with Windsurf

## Setup

1. Install the CLI
   ```bash
   npm install -g ramoira
   ```

2. Generate a schema in your project
   ```bash
   ramoira init
   ```

3. Configure Windsurf to include schema context

Recommended schema locations:

- `./ramoira/brand.schema.summary.json` for public-safe context
- `./ramoira/brand.schema.json` for internal/high-fidelity context
