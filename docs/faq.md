# FAQ

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

## How does the 7-day trial work?

The trial starts on first launch, keyed to your machine ID. No account or credit card required. After 7 days, the app prompts you to purchase. The countdown is shown in the app sidebar.

## Can I transfer my license to a new machine?

Yes. Deactivate on the old machine via Settings → License → Deactivate, then activate on the new machine. If you can't deactivate (machine is gone), email [support@surfagent.app](mailto:support@surfagent.app).

## Is the MCP server free?

Yes. `surfagent-mcp` is open source and free forever: [github.com/surfagentapp/surfagent-mcp](https://github.com/surfagentapp/surfagent-mcp). The license covers the app and daemon — the MCP protocol layer has no cost.

## I'm getting "Cannot connect to SurfAgent daemon" errors

1. Make sure SurfAgent is running (check system tray)
2. Check the app shows "Browser: Running" in the dashboard
3. Verify port 7201 isn't blocked by a firewall
4. Try restarting SurfAgent from the app

## How do I report a bug or request a feature?

Open an issue on [GitHub](https://github.com/surfagentapp/surfagent-docs/issues) or email [support@surfagent.app](mailto:support@surfagent.app).
