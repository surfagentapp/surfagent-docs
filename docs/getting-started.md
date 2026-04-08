# Getting Started with SurfAgent

This page is the practical install flow.

If you want the higher-level map first, read [Start Here](./start-here.md).

## What you need

- Windows 10 or 11 (64-bit)
- Google Chrome installed
- Node.js 20+ if you want MCP integration
- An AI client such as Claude Code, Codex, Cursor, Windsurf, Hermes, or OpenClaw

## Goal

By the end of this page, you should have:
- SurfAgent installed
- the local daemon running on port `7201`
- managed Chrome running
- your AI ready to connect through MCP

## Step 1: Install SurfAgent

1. Download the installer from [surfagent.app](https://surfagent.app)
2. Run `SurfAgent_Setup.exe`
3. Launch **SurfAgent** from Start Menu or Desktop

## Step 2: Confirm first launch worked

On first launch, SurfAgent should:
1. detect your Chrome installation
2. start a managed Chrome instance
3. start the local daemon on port `7201`
4. show the local dashboard

Quick checks:
- the app is open
- Chrome launched through SurfAgent
- `http://localhost:7201` responds locally

If this fails, jump to [FAQ & Troubleshooting](./faq.md).

## Step 3: Connect your AI agent

Next, wire your agent to `surfagent-mcp`.

Use the full setup page here:
- [Connect Your AI Agent (MCP Server)](./mcp-server.md)

Quick examples:

### Claude Code

```bash
claude mcp add surfagent -- npx -y surfagent-mcp
```

### Hermes Agent

```bash
hermes mcp add surfagent --command npx --args -y surfagent-mcp
```

### Codex CLI

Add to `~/.codex/config.yaml`:
```yaml
mcp_servers:
  - name: surfagent
    command: npx
    args: ["-y", "surfagent-mcp"]
```

### Cursor / Windsurf / Other MCP clients

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

## Step 4: Decide whether you also need skills or adapters

Most users only need:
- SurfAgent
- `surfagent-mcp`

You may also want:
- **skills** for better workflows and execution rules
- **adapters** for site-specific verbs like Gmail or Telegram Web

Read this next:
- [Skills, Adapters, and MCP: What to Use When](./skills-and-adapters.md)

## Step 5: Use it

Once connected, your agent should be able to do things like:
- open a page
- search the web
- click through a workflow
- fill forms
- take screenshots
- extract text or structured content

## Direct HTTP API

If you do not want MCP, you can call the daemon directly at `http://localhost:7201`.

```bash
curl -X POST http://localhost:7201/browser/navigate \
  -H 'Content-Type: application/json' \
  -d '{"url": "https://example.com"}'
```

Architecture and API reference:
- [How It Works](./how-it-works.md)

## Trial and activation

SurfAgent includes a **48-hour free trial**.

When you're ready to buy:
1. click **Activate** in the app
2. enter your key from [app.surfagent.app/portal](https://app.surfagent.app/portal)
3. or purchase directly from the app

Full details:
- [License & Activation](./license.md)

## What to read next

- [Connect Your AI Agent (MCP Server)](./mcp-server.md)
- [Skills, Adapters, and MCP](./skills-and-adapters.md)
- [FAQ & Troubleshooting](./faq.md)
