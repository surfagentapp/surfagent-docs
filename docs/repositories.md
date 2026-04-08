# SurfAgent Repositories & Ownership

This page is the public repo map.

Most users do **not** need this page first.

If you are just trying to get SurfAgent working, start here instead:
- [Start Here](./start-here.md)
- [Getting Started](./getting-started.md)
- [Connect Your AI Agent (MCP Server)](./mcp-server.md)

Use this page when you need to know which public repo owns which part of the product.

## Canonical public repos

### `surfagent-docs`
- Purpose: public product documentation
- Use it for: install docs, setup guides, FAQ, licensing, updates, product explanations
- Repo: <https://github.com/surfagentapp/surfagent-docs>

### `surfagent-skills`
- Purpose: canonical public skills catalog
- Use it for: modular skills like `surfagent-browser`, `surfagent-perception`, `browser-operations`, `gmail`, `x`, `tradingview`
- Repo: <https://github.com/surfagentapp/surfagent-skills>

### `surfagent-mcp`
- Purpose: generic MCP server for SurfAgent browser control
- Use it for: MCP installation in Claude Code, Codex, Cursor, Windsurf, Hermes, and other MCP clients
- Repo: <https://github.com/surfagentapp/surfagent-mcp>

## Public adapter repos

These are platform-specific MCP adapters. Use them when you need site-native verbs instead of raw browser control.

- `surfagent-gmail`
- `surfagent-telegram-web`
- `surfagent-tradingview`
- `surfagent-x`
- `surfagent-discord`

Each adapter repo should stay adapter-specific: concise README, tool surface, proof rules, and usage.

## Legacy compatibility repo

### `surfagent-skill`
- Status: legacy compatibility only
- Why it still exists: older Hermes/OpenClaw install flows still point here
- Do not treat it as the canonical skills catalog for new users
- Repo: <https://github.com/surfagentapp/surfagent-skill>

If you are starting fresh, prefer `surfagent-skills`.

## Ownership rules

- `surfagent-docs` owns public docs
- `surfagent-skills` owns public skill content
- `surfagent-mcp` owns generic MCP installation and server docs
- adapter repos own adapter-specific docs and examples
- `surfagent-skill` exists only to avoid breaking older references during migration

## Anti-duplication rule

Do not publish the same canonical instructions in multiple repos unless one clearly points back to the owner.

Preferred structure:
- broad product docs → `surfagent-docs`
- modular skills → `surfagent-skills`
- adapter-specific docs → adapter repos
- generic MCP install/setup → `surfagent-mcp`

That keeps repo sprawl from turning into contradictory docs.
