# Skills, Adapters, and MCP: What to Use When

This is the page that should remove most confusion.

SurfAgent has three layers, and they solve different problems.

## 1. MCP server = browser tool access

Use `surfagent-mcp` when you want your AI agent to control the browser directly.

Examples:
- open a page
- click a button
- fill a form
- take a screenshot
- extract page text

This is the default integration for Claude Code, Codex, Cursor, Windsurf, Hermes, OpenClaw, and other MCP clients.

Start here:
- [MCP Server Setup](./mcp-server.md)
- MCP repo: <https://github.com/surfagentapp/surfagent-mcp>

## 2. Skills = better instructions and workflows

Use skills when you want your AI agent to operate better, not just harder.

Skills teach:
- browser hygiene
- proof rules
- token-efficient execution
- common workflows for specific sites or use cases

Examples:
- browser-operations
- gmail
- x
- tradingview

Canonical skills live here:
- <https://github.com/surfagentapp/surfagent-skills>

Legacy compatibility repo:
- <https://github.com/surfagentapp/surfagent-skill>

If you're starting fresh, prefer `surfagent-skills`.

## 3. Adapters = site-native verbs

Use adapters when raw browser clicks are too generic and you want a cleaner, site-specific interface.

Examples:
- Gmail: open compose, fill draft, send, inspect sent message
- Telegram Web: inspect chats, draft a message, send
- TradingView: site-specific extraction and navigation

Current public adapter repos:
- `surfagent-gmail`
- `surfagent-telegram-web`
- `surfagent-tradingview`
- `surfagent-x`
- `surfagent-discord`

Each adapter repo should own its own adapter-specific README, tools, proof rules, and examples.

## Recommended decision ladder

### Use raw MCP when:
- the task is general browser work
- you do not need site-specific verbs
- you want the lowest setup overhead

### Add skills when:
- the agent is making bad choices
- you want better execution patterns
- you need reusable operating rules

### Add adapters when:
- the site has repeated workflows worth formalizing
- you want cleaner verbs than generic click/type flows
- proof rules matter and should be encoded explicitly

## What most users should do

For most people, the right setup is:
1. Install SurfAgent
2. Connect `surfagent-mcp`
3. Add skills from `surfagent-skills` if needed
4. Use a site adapter only when the workflow benefits from it

## What not to do

Do not treat every repo as required.

You do **not** need all of these to get started:
- the docs repo
- every adapter repo
- the legacy `surfagent-skill` repo
- deep architecture knowledge

You mainly need the app and MCP. Everything else is optional layering.
