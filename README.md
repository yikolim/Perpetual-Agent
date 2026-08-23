# Agent Mode

> Close your Mac. Your agents keep working.

A native macOS menu-bar app that turns a Mac into an always-on machine for AI
agents and long-running development tasks. It detects agent processes (Claude
Code, Codex, OpenClaw, Cursor Agent, Aider, or anything you add), keeps the
Mac awake **only while they're working**, protects the battery, notifies you
when work finishes, and automatically restores normal sleep behavior.

---

## Requirements

- macOS 14 (Sonoma) or newer — Apple Silicon or Intel
- Xcode **or** just the Command Line Tools (`xcode-select --install`)
- No other dependencies (no SPM packages, no CocoaPods, no Homebrew formulas required)

## Step 1 — Get the code

```bash
git clone https://github.com/yikolim/Perpetual-Agent.git
cd Perpetual-Agent
```

## Step 2 — Build and run

**Option A — no Xcode needed (fastest):**

```bash
./build.sh --run
```

This compiles everything with `swiftc` into `build/AgentMode.app` and launches
it. Look for a small **moon icon** (or a bolt, if an agent is already running)
in your menu bar — the app has no Dock icon by design.

**Option B — with Xcode:**

```bash
brew install xcodegen     # once
xcodegen generate
open AgentMode.xcodeproj  # then press ⌘R
```

**First launch notes**

- macOS will ask permission to send **notifications** — allow it, that's how
  you learn your overnight task finished.
- If Gatekeeper complains about the ad-hoc-signed app, right-click
  `AgentMode.app` → **Open** → **Open**.

## Step 3 — Use it

1. Click the menu-bar icon to open the panel. You'll see
   `Agent Mode: ON`, detected agents with runtime / CPU / memory, and the
   current power, battery, and sleep state.
2. Start an agent — e.g. run `claude` in a terminal with a long task. Within
   ~3 seconds the icon switches to a **filled bolt** and the panel shows
   `Claude Code — Running — Nm`. Your Mac now won't idle-sleep.
3. Walk away. When the agent exits you get a notification, a **grace period**
   counts down (default 5 minutes, clock-badge icon), and then normal sleep
   is restored — with a second notification confirming it.

### Running with the lid closed (optional)

Out of the box, keep-awake works with the lid **open**, or closed in clamshell
mode (external display + power). To keep running with the lid closed on
battery, enable **Lid-closed mode (admin)** in the menu:

- macOS shows its own administrator prompt (the app never sees or stores your
  password) and runs `pmset -a disablesleep 1`.
- Your previous setting is saved to disk **before** the change and restored
  when you toggle it off, quit the app, or — if the app crashed — at the next
  launch, automatically.

### Settings (menu → Settings…)

| Setting | Default | What it does |
|---|---|---|
| Restore sleep after agents finish | 5 min | Grace period: immediately / 5 / 15 / 30 min / never |
| Failsafe timeout | 12 h | Hard cap — never stay awake longer than this |
| Battery cutoff | 20 % | On battery, release keep-awake below this level |
| Require charger | off | Only keep awake while plugged in |
| Monitored processes | built-ins | Add your own name substrings (e.g. `my-agent`) |
| Launch at login | off | Registers via `SMAppService` |

Agent Mode never engages below 10 % battery and never overrides macOS thermal
or emergency-shutdown protections.

## Verify it's working

```bash
pmset -g assertions | grep -A2 "Agent Mode"
```

While an agent is running you should see a `PreventUserIdleSystemSleep`
assertion named "Agent Mode: AI agents are working".

Quick test without a real agent: toggle **Keep awake even with no agents** in
the menu, or add a custom process name in Settings (e.g. `yes`) and run
`yes > /dev/null` in a terminal — the app should light up within ~3 s.

## Troubleshooting

- **Icon never lights up** — check the agent actually shows in `ps`; built-in
  detection matches `claude`, `codex`, `openclaw`, `cursor-agent`, `aider`
  (including when they run under node/bun/python). Anything else: add a custom
  name in Settings.
- **Mac still sleeps with the lid closed on battery** — that requires
  **Lid-closed mode** (see above); a plain assertion can't do it.
- **`build.sh` fails with `redefinition of module 'SwiftBridging'`** — stale
  Command Line Tools modulemap; remove and reinstall the CLT, or build with
  full Xcode (Option B).
- **Sleep seems stuck disabled** — quit the app (menu → Quit) or run
  `sudo pmset -a disablesleep 0`. The app also self-heals: on every launch it
  checks for an unrestored state and puts your setting back.

## How it works

| Layer | Mechanism |
|---|---|
| Sleep control | `IOPMAssertionCreateWithName(kIOPMAssertionTypePreventUserIdleSystemSleep)` — supported, unprivileged, auto-released by the kernel if the app dies |
| Lid-closed mode (opt-in) | `pmset -a disablesleep 1` via `do shell script … with administrator privileges` — macOS handles credentials |
| State safety | Prior `SleepDisabled` value written to `~/Library/Application Support/AgentMode/power-restore.json` **before** any change; restored on stop, quit, or next launch after a crash |
| Agent detection | `proc_listallpids` / `proc_pidpath` / `proc_name` + `sysctl(KERN_PROCARGS2)` argv inspection for interpreters, polled every 3 s |
| Resource stats | `proc_pid_rusage` — CPU % from mach-time deltas, memory from `ri_phys_footprint` |
| Notifications | `UserNotifications`: finished, disappeared, battery threshold, stopped, restored, crash recovery |
| Launch at login | `SMAppService.mainApp` |

### State flow

```
agent starts → Agent Mode activates → user leaves / closes lid
     → process continues → agent exits → grace period
     → previous sleep state restored → notification
```

## Source layout

```
AgentMode/
├── AgentModeApp.swift         ← @main, MenuBarExtra, app delegate, login item
├── Models/
│   ├── AgentModeEngine.swift  ← supervisor state machine (poll → policy → assert)
│   ├── AgentProcess.swift
│   └── AppSettings.swift
├── Power/
│   ├── SleepAssertion.swift   ← IOPMAssertion wrapper
│   ├── LidCloseController.swift ← privileged pmset path + crash recovery
│   ├── PowerStateStore.swift  ← restore-record persistence
│   └── BatteryMonitor.swift   ← IOKit power snapshot
├── Processes/
│   └── ProcessMonitor.swift   ← libproc/sysctl agent detection
├── Notifications/
│   └── Notifier.swift
└── Views/
    ├── MenuView.swift         ← menu-bar dropdown (status, agents, controls)
    └── SettingsView.swift
project.yml                    ← xcodegen config
build.sh                       ← swiftc-only build
```

## Roadmap

- **Phase 2 — Agent Supervisor**: richer multi-agent dashboard, working/idle/
  waiting states, aggregate resource usage, auto-restart after failure.
- **Phase 3 — Remote monitoring**: iPhone/web status via an encrypted relay,
  remote stop/approve with strong authentication.
