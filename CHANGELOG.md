# Changelog

All notable changes to SurfAgent are documented here.

Format: `[version] - YYYY-MM-DD`
Types: `Added`, `Fixed`, `Changed`, `Security`

---

## [Unreleased]

### Added
- `docs/start-here.md` as the new public entry point for first-time users
- `docs/skills-and-adapters.md` to explain when to use MCP, skills, and site adapters

### Changed
- Reworked `README.md` around a guided start flow instead of a flat page list
- Rewrote `docs/getting-started.md` as a practical zero-to-working setup path
- Reframed `docs/mcp-server.md` around connecting an AI client, with clearer prerequisites and next steps
- Updated `docs/faq.md` into a combined FAQ and troubleshooting page
- Demoted `docs/repositories.md` from an implied starting point to an advanced repo map

## [0.2.0] - 2026-04-01

### Added
- **Structured Page State** (`/browser/state`) — single-call page classification, auth detection, blocker detection, element ranking, delta snapshots
- **Blocker Detection & Auto-Resolve** — cookie banners, auth walls, CAPTCHAs, paywalls, interstitials. Auto-dismiss via `/browser/resolve-blocker`
- **Site Adapters** — X/Twitter and GitHub for accurate page classification
- **Workflow Skills** — create, store, replay parameterized browser workflows (`/browser/skill/*`)
- **Browser Memory** — per-domain learning with hit/miss tracking (`/browser/memory/*`)
- **Dry-Run Plans** — validate actions before executing (`/browser/plan/*`)
- **CAPTCHA Solver** — detect + checkbox auto-click + challenge screenshot for AI vision (`/browser/captcha/*`)
- **Macro Recording** — record and replay browser action sequences (`/browser/macro/*`)
- **Tab Session Restore** — auto-save/reopen tabs across restarts (`/browser/tab/restore/*`)
- **Screenshot Diff** — pixel-level change detection (`/browser/screenshot/diff`)
- **CDP Noise Filter** — suppress WebSocket flooding from JS-heavy sites
- **Network Blocklist** — block ads/trackers via CDP
- **Health Webhook** — push notification on browser crash
- **Action Receipts** — structured proof of every action with ring buffer
- **Recovery** — 5-step auto-recovery from stuck states
- **91 total API routes**, up from ~50

### Fixed
- Screenshot diff now uses real pixel comparison (was meaningless base64 string compare)
- CORS for Tauri WebView2 (`https://tauri.localhost`)
- Skill list query parameter handling
- Browser memory disk cleanup on eviction

---

## [0.1.0] - 2026-03-25

### Added
- SurfAgent desktop app (Tauri + React) — Windows installer
- Local daemon on port 7201 with Chrome CDP control
- 21-tool MCP server for Claude Code / Cursor / Codex
- License system with 48-hour free trial
- Extract/Crawl/Map endpoints
- Screencast live view
- Agent loop with LLM-driven automation
