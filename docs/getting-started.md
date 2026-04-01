# Getting Started with SurfAgent

SurfAgent is a Windows desktop app that runs a managed Chrome browser on your machine. Your AI agent connects to it and gets full browser control — no cloud, no subscriptions beyond the one-time purchase, no data leaving your machine.

## Requirements

- Windows 10 or 11 (64-bit)
- Google Chrome installed
- Node.js 20+ (for MCP server)
- Hermes Agent, Claude Code, Cursor, Windsurf, Codex, or any MCP-compatible AI agent

## Install

1. Download the installer from [surfagent.app](https://surfagent.app)
2. Run `SurfAgent_Setup.exe` — standard Windows installer, no admin required
3. Launch **SurfAgent** from Start Menu or Desktop

## First Launch

On first launch, SurfAgent will:

1. Detect your Chrome installation
2. Start a managed Chrome instance with remote debugging enabled
3. Start the local daemon on port `7201`
4. Show your browser control dashboard at `http://localhost:7201`

## Connect Your AI Agent

### Claude Code (Recommended)

```bash
claude mcp add surfagent -- npx -y surfagent-mcp
```

Then in any Claude Code session:
```
"Open YouTube and search for Tauri tutorials"
"Take a screenshot of the current tab"
"Fill in this form: name=John, email=john@example.com"
```

### Hermes Agent

```bash
hermes mcp add surfagent --command npx --args -y surfagent-mcp
```

Then ask Hermes to browse, extract, or automate anything. All 24 browser tools are discovered automatically.

Or install the SurfAgent skill for enhanced instructions:
```bash
hermes skills install github:surfagentapp/surfagent-skill
```

### Codex CLI

Add to `~/.codex/config.yaml`:
```yaml
mcp_servers:
  - name: surfagent
    command: npx
    args: ["-y", "surfagent-mcp"]
```

### Cursor / Windsurf / Other MCP Clients

Add to your MCP config:
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

### Direct HTTP API

The daemon exposes a REST API at `http://localhost:7201`:

```bash
# Open a URL
curl -X POST http://localhost:7201/browser/navigate \
  -H 'Content-Type: application/json' \
  -d '{"url": "https://example.com"}'

# Take a screenshot
curl http://localhost:7201/browser/screenshot > screenshot.png

# List open tabs
curl http://localhost:7201/browser/tabs
```

Full API reference: [How It Works](./how-it-works.md)

## Activate Your License

After downloading, you have a **48-hour free trial** — no credit card required. The trial counter starts on first launch.

When you're ready to buy ($49 one-time):
1. Click **Activate** in the app sidebar
2. Enter your license key from [app.surfagent.app/portal](https://app.surfagent.app/portal)
3. Or purchase directly from the app

Full details: [License & Activation](./license.md)
