# Updates

SurfAgent supports automatic in-app updates. When a new version is available, the app will notify you and let you install with one click — no re-downloading the installer manually.

## How Updates Work

SurfAgent uses Tauri's built-in updater (`tauri-plugin-updater`). On startup and periodically while running, the app checks for a new release on GitHub. If one is available:

1. A notification appears in the app: **"Update available — v1.x.x"**
2. Click **Install Update**
3. The new version downloads in the background
4. On restart, the update is applied automatically

No admin rights required. No data loss. Your settings, license key, and browser profile are preserved.

## Update Channel

Updates are distributed via [GitHub Releases](https://github.com/surfagentapp/surfagent/releases).

Each release includes:
- `SurfAgent_Setup.exe` — full installer for new installs
- `SurfAgent_x.x.x_x64-setup.nsis.zip` + `update.json` — delta update for existing installs (used by the in-app updater)

## Manual Update

If auto-update doesn't trigger, you can always manually download the latest installer from [surfagent.app](https://surfagent.app). It will upgrade your existing installation in-place.

## Release Notes

Every release is documented in the [Changelog](../CHANGELOG.md) and on the [GitHub Releases](https://github.com/surfagentapp/surfagent/releases) page.

## Upcoming: Update Protocol

After every code change, the development protocol requires:
1. Update `CHANGELOG.md` in `surfagent-docs` with what changed and why
2. Bump the version in `src-tauri/tauri.conf.json` and `package.json`
3. Tag the release: `git tag v1.x.x && git push --tags`
4. GitHub Actions (or manual CI) builds the NSIS installer and publishes the GitHub Release with the update manifest
5. App users get prompted to update on next launch

This ensures every user always has access to the latest fixes and features.
