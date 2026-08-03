# Skill Studio — Releases

Distribution artifacts for the Skill Studio desktop app (internal agency
tool). The source lives in a separate private repo; this repo only hosts
built artifacts and the installer.

## Install on a new Mac (one line)

Run this in **Terminal** (⌘ Space → type "Terminal" → Enter):

```sh
curl -fsSL https://github.com/ctkubik/skill-studio-releases/releases/latest/download/install.sh | bash
```

It downloads the latest `.dmg`, copies **Skill Studio** into `/Applications`,
clears the macOS quarantine flag (the false "is damaged" Gatekeeper block on
un-notarized apps), and installs the Claude CLI if it's missing. Idempotent —
safe to re-run.

**Prefer clicking?** Download `Skill-Studio-<v>.dmg` from the
[latest release](https://github.com/ctkubik/skill-studio-releases/releases/latest),
open it, drag **Skill Studio** into **Applications**, then right-click → Open
the first time (the one-time Gatekeeper bypass).

Full setup + troubleshooting: **[teammate-install.md](teammate-install.md)**.

## Updating

After the first install the app keeps itself current — the in-app auto-updater
fetches each new release on launch. No need to re-run the installer.

## What each release contains

- `install.sh` — the one-line installer above (plus `teammate-install.md`, the guide).
- `Skill-Studio-<v>.dmg` — drag-to-Applications installer for new machines.
- `Skill-Studio-<v>.app.tar.gz` + `.sig` + `latest.json` — the in-app
  auto-updater payload (existing installs update on launch).
