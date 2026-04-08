# Connect Your AI Agent (MCP Server)

This page is for connecting your AI tool to SurfAgent.

If SurfAgent is not installed yet, start here first:
- [Getting Started](./getting-started.md)

The `surfagent-mcp` package gives any MCP-compatible AI agent browser-control tools powered by your local SurfAgent instance.

**Requires:** SurfAgent running locally with the daemon on port `7201`.

## The short version

Most users only need this:

```bash
npx -y surfagent-mcp
```

Then add that MCP server to their AI client.

## Before you begin

Make sure:
- SurfAgent is installed
- the app is running
- the local daemon is reachable

If MCP cannot connect, check [FAQ & Troubleshooting](./faq.md).

## Claude Code

```bash
claude mcp add surfagent -- npx -y surfagent-mcp
```

Or add to `~/.claude/mcp.json`:
```json
{
  "mcpServers": {
    "surfagent": {
      "command": "npx",
      "args": ["-y", "surfagent-mcp"]
    }
  }
}
```

## Hermes Agent

```bash
hermes mcp add surfagent --command npx --args -y surfagent-mcp
```

Or add to `~/.hermes/config.yaml`:
```yaml
mcp:
  servers:
    - name: surfagent
      command: npx
      args: ["-y", "surfagent-mcp"]
```

You can also use the canonical skills catalog in [`surfagent-skills`](https://github.com/surfagentapp/surfagent-skills) for enhanced browser automation instructions. The older single-repo install below remains as a legacy compatibility path during migration:
```bash
hermes skills install github:surfagentapp/surfagent-skill
```

## Codex CLI

Add to `~/.codex/config.yaml`:
```yaml
mcp_servers:
  - name: surfagent
    command: npx
    args: ["-y", "surfagent-mcp"]
```

## Cursor

Add to `.cursor/mcp.json` in your project, or global Cursor MCP settings:
```json
{
  "mcpServers": {
    "surfagent": {
      "command": "npx",
      "args": ["-y", "surfagent-mcp"]
    }
  }
}
```

## Windsurf

Add to `~/.codeium/windsurf/mcp_config.json`:
```json
{
  "mcpServers": {
    "surfagent": {
      "command": "npx",
      "args": ["-y", "surfagent-mcp"]
    }
  }
}
```

> Canonical repo ownership: `surfagent-mcp` = MCP server, `surfagent-skills` = public skills catalog, `surfagent-skill` = legacy compatibility repo.

## Skills and adapters

MCP is the browser tool layer.

If you also want better workflows or site-specific behavior, read:
- [Skills, Adapters, and MCP: What to Use When](./skills-and-adapters.md)
- canonical skills catalog: <https://github.com/surfagentapp/surfagent-skills>
- legacy compatibility repo: <https://github.com/surfagentapp/surfagent-skill>

## Available tools

### Navigation (3)
- `browser_navigate` — Open a URL in the current tab
- `browser_back` — Navigate browser history back
- `browser_forward` — Navigate browser history forward

### Tabs (4)
- `browser_list_tabs` — List open browser tabs with IDs and titles
- `browser_new_tab` — Open a new browser tab
- `browser_switch_tab` — Switch to a tab by id, index, or title
- `browser_close_tab` — Close a browser tab by id (or current active tab)

### Interaction (5)
- `browser_click` — Click an element by CSS selector, text content, or coordinates
- `browser_type` — Type text into a focused element
- `browser_fill_form` — Fill multiple form fields using label/name to value mappings
- `browser_select` — Select an option from a dropdown element
- `browser_scroll` — Scroll the page up/down or to a specific element

### Observation (6)
- `browser_screenshot` — Capture full-page or viewport screenshot (PNG)
- `browser_get_text` — Get visible text content from the page or a matched element
- `browser_get_html` — Get page HTML source
- `browser_get_url` — Get the current browser tab URL
- `browser_get_title` — Get the current browser tab title
- `browser_find_elements` — Find elements matching a CSS selector

### JavaScript (1)
- `browser_evaluate` — Run JavaScript in the page context and return the result

### Waiting (1)
- `browser_wait` — Wait for an element to appear

### Cookies (1)
- `browser_cookies` — Get or set browser cookies

### Extraction (3)
- `browser_extract` — Extract structured data from a page (markdown, JSON, links, screenshot)
- `browser_crawl` — Crawl a website using BFS, extracting content from each page
- `browser_map` — Discover all URLs on a website without extracting full content

## Custom daemon URL

By default, the MCP server connects to `http://localhost:7201`. Override with:

```json
{
  "mcpServers": {
    "surfagent": {
      "command": "npx",
      "args": ["-y", "surfagent-mcp"],
      "env": {
        "SURFAGENT_DAEMON_URL": "http://localhost:7201"
      }
    }
  }
}
```
