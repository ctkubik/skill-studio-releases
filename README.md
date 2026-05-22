# Skill Studio — Releases

Distribution artifacts for the Skill Studio desktop app (internal
agency tool). Each release contains:

- `Skill-Studio-<v>.dmg` — drag-to-Applications installer for new
  machines (right-click → Open the first time for Gatekeeper).
- `Skill-Studio-<v>.app.tar.gz` + `.sig` + `latest.json` — the
  in-app auto-updater payload (existing installs update on launch).

Source lives in a separate private repo; this repo only hosts
built artifacts.

## Installing on a new machine

See **[teammate-install.md](teammate-install.md)** — the full
setup + troubleshooting guide for getting Skill Studio running on
a fresh Mac.
