# Changelog

All notable changes to SurfAgent are documented here.

Format: `[version] — YYYY-MM-DD`  
Types: `Added`, `Fixed`, `Changed`, `Removed`, `Security`

---

## [0.1.0] — 2026-03-25

### Added
- **SurfAgent desktop app** (Tauri + Vite + React) — Windows 10/11 installer
- **Local daemon** (Rust) running on `http://localhost:7201` — manages Chrome via CDP
- **Chrome management** — launches Chrome with dedicated profile and `--remote-debugging-port=9222`
- **21-tool MCP server** (`surfagent-mcp`) — works with Claude Code, Cursor, Windsurf, Codex
  - Navigation: navigate, reload, back, forward
  - Tabs: open, list, focus, close
  - Interaction: click, type, fill, select, scroll, key press
  - Observation: screenshot, get content, get HTML, inspect element
  - JavaScript: evaluate
  - Cookies: get, set, clear
- **License system** — one-time $49 purchase, machine-bound activation, 1-device per key
- **7-day free trial** — machine-ID based, no account required, countdown badge in app sidebar
- **Trial expiry modal** — shows when trial ends with direct purchase CTA
- **NowPayments checkout** — crypto payments (BTC, ETH, USDC, SOL and more)
- **Promo/referral codes** — discount codes at checkout
- **Customer portal** — `app.surfagent.app/portal` (Privy auth) — view/download license keys, trial status
- **License activation flow** — enter key in app Settings, validates against license server, binds to machine

### Infrastructure
- License API: `api.surfagent.app` (Node + Express + PostgreSQL)
- Landing site: `surfagent.app` (Vite + React)
- Customer portal: `app.surfagent.app` (Vite + React + Privy)

---

## Changelog Protocol

**Every time a change is shipped, update this file before tagging the release.**

### What to log
- New features (user-visible)
- Bug fixes (what broke, what the fix was)
- Breaking changes (anything that requires user action)
- Security fixes (without full exploit details)

### What NOT to log
- Internal refactors with no user impact
- Dev tooling changes (CI, linting, etc.)
- Dependency bumps (unless they fix a real issue)

### Version bumping
- **Patch** (0.1.x) — bug fixes, minor UI tweaks
- **Minor** (0.x.0) — new features, non-breaking changes  
- **Major** (x.0.0) — breaking changes, major new capabilities

### Release checklist
1. Update this `CHANGELOG.md`
2. Bump version in `src-tauri/tauri.conf.json` and root `package.json`
3. Commit: `git commit -m "chore: release vX.X.X"`
4. Tag: `git tag vX.X.X && git push --tags`
5. Build: `npm run tauri build` — produces NSIS installer
6. Publish GitHub Release with installer + `latest.json` update manifest
7. In-app updater will notify users on next launch
