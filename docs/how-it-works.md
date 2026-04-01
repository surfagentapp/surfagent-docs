# How SurfAgent Works

## Architecture

```
┌─────────────────────────────────────────────────┐
│                  Your Machine                    │
│                                                  │
│  ┌──────────────┐    ┌──────────────────────┐   │
│  │  AI Agent    │    │    SurfAgent App      │   │
│  │ (Hermes /   │◄──►│  (Tauri + Vite UI)   │   │
│  │  Claude Code │    └──────────┬───────────┘   │
│  │  / Cursor)   │               │               │
│  └──────┬───────┘               │               │
│         │ MCP / HTTP            │ manages        │
│         ▼                       ▼               │
│  ┌──────────────┐    ┌──────────────────────┐   │
│  │surfagent-mcp │    │   SurfAgent Daemon   │   │
│  │  (npx/local) │◄──►│  (Node.js, port 7201)│   │
│  └──────────────┘    └──────────┬───────────┘   │
│                                 │  CDP           │
│                                 ▼               │
│                      ┌──────────────────────┐   │
│                      │   Chrome (port 9222) │   │
│                      │  Persistent Profile  │   │
│                      └──────────────────────┘   │
└─────────────────────────────────────────────────┘
```

## Components

### SurfAgent App (Tauri Desktop)
The desktop application manages everything. It:
- Launches Chrome with `--remote-debugging-port=9222`
- Starts the daemon process (standalone Node.js binary)
- Provides a settings/status UI
- Handles license activation and trial management

### SurfAgent Daemon (Node.js)
A lightweight HTTP server running on `http://localhost:7201`, compiled to a standalone executable. It:
- Accepts REST API calls from MCP server or direct HTTP clients
- Translates requests into Chrome DevTools Protocol (CDP) commands
- Manages tab lifecycle, navigation, screenshots, DOM access
- Maintains a persistent Chrome session

### surfagent-mcp
A Model Context Protocol server that wraps the daemon API into 24 named tools that AI agents understand. It runs via `npx -y surfagent-mcp` and connects to `localhost:7201`. Compatible with Hermes Agent, Claude Code, Codex, Cursor, Windsurf, and any MCP client.

### Chrome (Managed Instance)
SurfAgent launches Chrome with:
- `--remote-debugging-port=9222` for CDP access
- A dedicated user profile (separate from your personal Chrome)
- Persistent cookies, localStorage, and session state

## Daemon API Reference

Base URL: `http://localhost:7201`

### Navigation
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/browser/navigate` | Navigate to URL |
| POST | `/browser/reload` | Reload current page |
| POST | `/browser/back` | Go back |
| POST | `/browser/forward` | Go forward |

### Tabs
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/browser/tabs` | List all open tabs |
| POST | `/browser/open` | Open new tab |
| POST | `/browser/focus` | Focus a tab |
| POST | `/browser/close` | Close a tab |

### Interaction
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/browser/click` | Click element by selector |
| POST | `/browser/type` | Type text into element |
| POST | `/browser/fill` | Fill form field |
| POST | `/browser/select` | Select dropdown option |
| POST | `/browser/scroll` | Scroll page |
| POST | `/browser/evaluate` | Execute JavaScript |

### Observation
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/browser/screenshot` | Capture screenshot (PNG) |
| POST | `/browser/content` | Get page text/HTML |
| POST | `/browser/inspect` | Get DOM element info |

### State
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/browser/cookies` | Get all cookies |
| POST | `/browser/cookies` | Set a cookie |
| DELETE | `/browser/cookies` | Clear cookies |
| GET | `/browser/status` | Daemon health status |

## Data Flow Example

When your AI agent says "open Twitter and take a screenshot":

1. Claude Code calls `navigate({url: "https://twitter.com"})` via MCP
2. `surfagent-mcp` sends `POST /browser/navigate` to daemon
3. Daemon sends `Page.navigate` via CDP to Chrome
4. Chrome navigates, DOM loads
5. Agent calls `screenshot()` via MCP
6. Daemon sends `Page.captureScreenshot` via CDP
7. Base64 PNG returned to agent

All local. No cloud. Chrome uses your real persistent profile so it's already logged in.
