# Installing Skill Studio (teammate guide)

Read this if you are setting up Skill Studio for the **first time on
a new machine**. It covers the easy path, the manual fallback, and
the few common gotchas on a fresh Mac.

> **macOS only.** Skill Studio is a desktop app for macOS (Apple
> Silicon and Intel). Windows / Linux are not supported.

The big picture — Skill Studio needs three things, and **the app
sets up the last two for you**:

1. **Skill Studio itself** — the app (you install this).
2. **The Claude Code CLI** — the engine that runs skills. The app
   installs it for you with a button.
3. **A Claude sign-in** — your Claude Pro / Max / Team account. The
   app signs you in with a button. No API key, no separate billing.

---

## The fast path

### 1. Get the app

Easiest — run this one line in **Terminal** (⌘-Space → type
"terminal" → Enter):

```bash
curl -fsSL https://github.com/ctkubik/skill-studio-releases/releases/latest/download/install.sh | bash
```

It downloads the latest Skill Studio, copies it to
`/Applications`, and clears the macOS "is damaged" flag. (It also
installs the Claude CLI if it can — but the app does that step too,
so don't worry if that part is skipped.)

**Prefer not to use Terminal?** Download the `.dmg` from the
**[latest release page](https://github.com/ctkubik/skill-studio-releases/releases/latest)**,
double-click it, drag **Skill Studio** into **Applications** — then
do the one-time quarantine clear in
[step 2 below](#2-open-skill-studio).

### 2. Open Skill Studio

Press ⌘-Space, type **Skill Studio**, hit Enter.

If macOS says **"Skill Studio is damaged and can't be opened"** —
that's a false positive on apps not yet notarized by Apple. Fix it
once: in Terminal run

```bash
xattr -dr com.apple.quarantine /Applications/Skill\ Studio.app
```

…then open the app normally. (The Terminal one-liner above does
this for you; you only need it if you installed by `.dmg`.)

### 3. Let the app finish setup — Install + Sign in

On first launch Skill Studio shows a **System Health** panel (also
always available later at **Settings → System Health**). It checks
everything and gives you a button for whatever's missing:

- **Claude Code CLI** → click **Install**. The app runs the
  installer and shows live progress. ~30 seconds.
- **Signed in to Claude** → click **Sign in**. A browser opens —
  log in with your **Claude Pro / Max / Team** account. The panel
  detects when you're done.
- **Installed skills** → click **Install skills** to pull the team
  skill catalog (~190 skills). Your sidebar fills in.

When every row is green, you're ready. **No Terminal needed for
any of this** — the app does it all.

> **Why a subscription, not an API key?** A Claude Pro / Max
> subscription is a flat monthly fee that covers every skill run.
> API-key billing is metered per-token and gets expensive fast for
> audit-class skills. Subscription sign-in is what we recommend for
> every teammate.

---

## Verify it works

1. Open Skill Studio. **Settings → System Health** — every row
   should be green ("All set").
2. Click any skill, pick a client (or type a prompt), hit **Run**.
   The first run lands in a few seconds with output.

---

## Team features (optional)

Report sharing and the teammate roster work once **team sync** is
enabled (**Settings → Team Sync**). Set your **display name** there
so teammates recognize you — an email is *not* required for in-app
notifications. If team sync is off, the app still works fully for
your own runs; you just won't see teammates' work or share reports.

macOS will ask once to allow notifications from Skill Studio —
allow it so "shared a report" alerts can reach you.

---

## Manual fallback

If the app's System Health buttons can't reach the network (a
corporate proxy, etc.), do the two pieces by hand. Both are safe to
re-run.

### Install the Claude Code CLI

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

When it finishes, **open a new Terminal window** (the installer
edits `~/.zshrc`, which only affects new shells) and verify:

```bash
claude --version
```

If you see **"command not found: claude"**, the binary landed in
`~/.local/bin/` but isn't on your `PATH`:

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
```

…open a new Terminal, try `claude --version` again.

### Sign in to Claude

```bash
claude auth login
```

A browser opens — sign in with your Claude Pro / Max / Team
account. To check status at any time: `claude auth status`.

Then quit and re-open Skill Studio — it picks up the CLI and the
sign-in automatically.

---

## Troubleshooting

**First stop for anything below: Settings → System Health.** It
tells you exactly what's missing and gives you a fix button. The
red **"Claude Code CLI not found"** banner also has an **Open
System Health** button.

### "Claude CLI not found" inside Skill Studio

Open **Settings → System Health** and click **Install** on the
Claude Code CLI row. If it's already installed but not detected,
quit (⌘-Q) and re-open Skill Studio — it re-scans on launch.

### System Health shows "Sign in" even though I signed in

Make sure you signed in on the same Mac. Click **Sign in** in the
panel, or run `claude auth status` in Terminal — if it shows
`"loggedIn": true`, quit and re-open Skill Studio.

### Skill sidebar is empty

Skills don't sync between machines — each machine installs them
from the catalog. **Settings → System Health → Install skills**, or
the onboarding wizard's "Install the team skill catalog" step.

### "command not found: claude" in Terminal after install

The installer edited `~/.zshrc` but your current shell predates the
edit. Open a new Terminal window (or run `source ~/.zshrc`).

### "Couldn't mount the downloaded image"

The `.dmg` download was incomplete or a stale mount is lingering.
Eject any old "Skill Studio" volume from Finder, then re-run the
one-liner. Still stuck → download the `.dmg` from the
[release page](https://github.com/ctkubik/skill-studio-releases/releases/latest)
and open it by hand.

### "Skill Studio is damaged and can't be opened"

The macOS Gatekeeper false positive for un-notarized apps:

```bash
xattr -dr com.apple.quarantine /Applications/Skill\ Studio.app
```

The Terminal one-liner does this automatically; only `.dmg`
installs hit it.

### A teammate isn't in my "Notify" list

They appear in the roster once they've opened Skill Studio with
team sync enabled and synced once — no email needed. They can
confirm their own setup at **Settings → System Health → Team sync**.

### Runs cost more than expected

Check the Claude model:

```bash
claude config get defaultModel
```

If it returns Opus, switch to Sonnet — it handles every catalog
skill and costs far less for crawl/report work:

```bash
claude config set defaultModel claude-sonnet-4-5
```

Restart Skill Studio after changing.

---

## Updating later

Skill Studio **auto-updates on launch** — when a new release ships,
the app fetches it on next open and prompts you to restart. You
never need to re-run the installer. (Re-running the one-liner is
still safe if you ever want to force a clean re-install.)

---

## Uninstalling

```bash
# 1. The app
rm -rf /Applications/Skill\ Studio.app

# 2. Local data (clients DB, run history, settings, branding)
rm -rf ~/Library/Application\ Support/com.skillstudio.app

# 3. The Claude CLI (optional — other Anthropic tools use it too)
rm -f ~/.local/bin/claude
```

`~/.claude/` is the Claude CLI's own home (installed skills +
sign-in credential). Leave it unless you're deliberately resetting
everything.

---

## What gets installed where

| Item | Path |
|---|---|
| Skill Studio app | `/Applications/Skill Studio.app` |
| Skill Studio data (DB, manifests, settings) | `~/Library/Application Support/com.skillstudio.app/` |
| Claude Code CLI binary | `~/.local/bin/claude` (default) |
| Installed skills | `~/.claude/skills/` |
| Claude CLI sign-in credential | macOS Keychain + `~/.claude/` |

No system services, no kernel extensions, no admin privileges
required.
