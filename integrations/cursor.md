# Using Ramoira with Cursor

## Setup

1. Install Ramoira CLI
   npm install -g ramoira

2. Generate a schema in your project
   ramoira init

3. Add Ramoira MCP server to Cursor settings
   Settings → MCP → Add server
   URL: ramoira.com/mcp/public

4. Cursor now reads ramoira.config.json automatically
   Brand context available in every session

## How it works

Cursor reads ramoira.config.json in your project root.
When you ask Cursor to write copy or content, it fetches
your brand schema and uses it as context automatically.

No system prompt needed.
No re-prompting between sessions.
Brand context persists across the project.

## Example session

You: Write three subject lines for our next email
Cursor: [fetches brand schema automatically]
Output is brand-consistent without any manual brief.

## Upgrading to check_consistency()

Available on Studio tier.
Cursor can call check_consistency() on generated copy
before returning it to you.
