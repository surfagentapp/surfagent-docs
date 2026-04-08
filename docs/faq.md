# FAQ & Troubleshooting

If you are still setting SurfAgent up, the main flow is:
- [Start Here](./start-here.md)
- [Getting Started](./getting-started.md)
- [Connect Your AI Agent (MCP Server)](./mcp-server.md)

## Troubleshooting

## I can't connect to the SurfAgent daemon

1. Make sure SurfAgent is running
2. Check the app shows the browser and daemon are up
3. Verify `http://localhost:7201` responds locally
4. Restart SurfAgent if needed
5. Re-check your MCP client config

## Chrome did not launch properly

1. Make sure Google Chrome is installed
2. Restart SurfAgent
3. Check whether another tool is already occupying the debugging port
4. Re-open the app and confirm the managed browser starts

## Does SurfAgent work on Mac or Linux?

Not yet. SurfAgent is currently Windows-only (Windows 10/11 64-bit). Mac support is on the roadmap.

## Does it use my existing Chrome or a separate one?

SurfAgent launches Chrome with a separate, dedicated user profile — so it doesn't interfere with your personal Chrome tabs, bookmarks, or extensions. However, the same Chrome installation is used.

## Is my data private?

Yes. SurfAgent is 100% local. The daemon runs on `localhost:7201`, Chrome runs on `localhost:9222`. No browser traffic routes through any external server. License validation requires an internet connection, but that's the only external call the app makes.

## Can my AI agent access sites I'm already logged into?

Yes — that's one of SurfAgent's core advantages. The managed Chrome profile persists across sessions. Log in once, and your AI agent can use that session from then on.

## Does SurfAgent interfere with my normal Chrome usage?

No. The managed Chrome instance is isolated to a separate profile directory. Your personal Chrome (with your bookmarks, passwords, etc.) is completely separate and unaffected.

## What if Chrome is not installed?

SurfAgent requires Google Chrome. If it's not installed, download it from [google.com/chrome](https://google.com/chrome).

## Can I use it without the MCP server?

Yes. The daemon at `http://localhost:7201` is a plain HTTP API. You can call it directly from any script, tool, or application. See [How It Works](./how-it-works.md) for the full API reference.

## How does the 48-hour trial work?

The trial starts on first launch, keyed to your machine ID. No account or credit card required. After 48 hours, the app prompts you to purchase. The countdown is shown in the app sidebar.

## Can I transfer my license to a new machine?

Yes. Deactivate on the old machine via Settings → License → Deactivate, then activate on the new machine. If you can't deactivate (machine is gone), email [support@surfagent.app](mailto:support@surfagent.app).

## How do I connect Hermes Agent?

One command:
```bash
hermes mcp add surfagent --command npx --args -y surfagent-mcp
```
Hermes discovers all 24 browser tools automatically. The canonical skill catalog now lives in [`surfagent-skills`](https://github.com/surfagentapp/surfagent-skills). If you need the older single-repo install during migration, this legacy compatibility path still works: `hermes skills install github:surfagentapp/surfagent-skill`

## Is the MCP server free?

Yes. `surfagent-mcp` is open source and free forever: [github.com/surfagentapp/surfagent-mcp](https://github.com/surfagentapp/surfagent-mcp). The license covers the app and daemon — the MCP protocol layer has no cost.

## How do I report a bug or request a feature?

Open an issue on [GitHub](https://github.com/surfagentapp/surfagent-docs/issues) or email [support@surfagent.app](mailto:support@surfagent.app).
