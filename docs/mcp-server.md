# MCP Server Setup

The `surfagent-mcp` package gives any MCP-compatible AI agent 21 browser-control tools powered by your local SurfAgent instance.

**Requires:** SurfAgent running with daemon on port 7201.

## Installation

No install needed — run via npx:

```bash
npx -y surfagent-mcp
```

Or install globally:
```bash
npm install -g surfagent-mcp
```

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

## Codex CLI

Add to `~/.codex/config.yaml`:
```yaml
mcp_servers:
  - name: surfagent
    command: npx
    args: ["-y", "surfagent-mcp"]
```

## Available Tools (21)

### Navigation
- `navigate` — Open a URL in the current tab
- `reload` — Reload the current page
- `go_back` / `go_forward` — Browser history navigation

### Tabs
- `open_tab` — Open a URL in a new tab
- `list_tabs` — List all open tabs with IDs and titles
- `focus_tab` — Switch to a tab by ID
- `close_tab` — Close a tab by ID

### Interaction
- `click` — Click an element by CSS selector
- `type_text` — Type text into a focused element
- `fill_field` — Fill a form field by selector + value
- `select_option` — Select a `<select>` dropdown value
- `press_key` — Send a keyboard key press
- `scroll` — Scroll the page up/down/to element

### Observation
- `screenshot` — Capture full-page or viewport screenshot
- `get_content` — Get visible page text
- `get_html` — Get page HTML source
- `inspect_element` — Get attributes/text of a DOM element

### JavaScript
- `evaluate` — Execute arbitrary JavaScript and return result

### Cookies
- `get_cookies` — Get all cookies for the current domain
- `set_cookie` — Set a cookie
- `clear_cookies` — Clear cookies for domain

## Custom Daemon URL

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
