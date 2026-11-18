# KK Agent — release artifacts

Installers and portable binaries for **KK Agent** (KKAgent4), the Windows desktop app.
This repo holds release assets only — no source code lives here.

Update feed: <https://updates.kkzen.com/kkagent/updates.json>

## Releases

Tag scheme: `kkagent-v<version>` (e.g. `kkagent-v0.1.0`). Each release carries:

| Asset | Meaning |
| --- | --- |
| `KKAgent-windows-amd64.exe` | Portable binary, Windows x64 |
| `KKAgent-windows-amd64.exe.sha256` | SHA-256 of the portable binary |
| `KKAgent-windows-amd64-installer.exe` | NSIS installer (Windows x64) — only when packaging was available |
| `KKAgent-windows-amd64-installer.exe.sha256` | SHA-256 of the installer |

The installer asset is optional: releases built without NSIS ship the portable binary only,
and the feed omits the `installer` object for them.

## Feed

`updates.kkzen.com/kkagent/updates.json` is served from the R2 bucket `kkzen-updates`
(object key `kkagent/updates.json`) and follows the same schema as KKVideo:

```json
{
  "schemaVersion": 1,
  "platform": "win-amd64",
  "version": "0.1.0",
  "tag": "kkagent-v0.1.0",
  "notes": "…",
  "publishedAt": "2026-11-18T00:00:00Z",
  "portable": {
    "url": "https://github.com/kkzen-com/updates-kkagent/releases/download/kkagent-v0.1.0/KKAgent-windows-amd64.exe",
    "sha256": "…",
    "sha256Url": "https://github.com/kkzen-com/updates-kkagent/releases/download/kkagent-v0.1.0/KKAgent-windows-amd64.exe.sha256"
  },
  "installer": {
    "url": "https://github.com/kkzen-com/updates-kkagent/releases/download/kkagent-v0.1.0/KKAgent-windows-amd64-installer.exe",
    "sha256": "…"
  }
}
```

`sha256` is a lowercase hex digest; the `.sha256` asset holds the same digest on one line.

**KK Agent has no in-app updater yet** (roadmap Q2). The feed exists so the website can
show the current version and hand out download links, and so a future updater has a stable
contract to read. Nothing in the app polls it today.

## Publishing

Run from the KK Agent source repo:

```powershell
pwsh scripts/release.ps1 -Version 0.1.0        # build, package, release, feed
pwsh scripts/release.ps1 -Version 0.1.0 -DryRun  # print the plan only
```

See `docs/publish/app-release.md` there for flags, prerequisites and the version rule.
