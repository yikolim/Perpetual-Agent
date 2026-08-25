# Agent Mode — Product Description Package

> Handoff document for building the product/landing page. Everything the page
> needs — copy, features, links, UI description, FAQ — is in this file.

---

## 1. Identity

- **Product name:** Agent Mode
- **Repository:** https://github.com/yikolim/Perpetual-Agent
- **Download (always latest build):** https://github.com/yikolim/Perpetual-Agent/releases/download/latest/AgentMode.zip
- **All releases:** https://github.com/yikolim/Perpetual-Agent/releases
- **Platform:** macOS 14 (Sonoma) or newer · universal binary (Apple Silicon + Intel)
- **Price:** Free, open source
- **Category:** Developer tool / AI-agent utility / menu-bar app

## 2. Tagline & one-liners

Primary tagline (hero):

> **Close your Mac. Your agents keep working.**

Alternates:

- Your Mac sleeps when your agents finish — not when you close it.
- Turn your Mac into an always-on AI agent machine.
- Amphetamine manages sleep. Agent Mode manages unattended AI work.

One-paragraph description (for meta/OG and app directories):

> Agent Mode is a free macOS menu-bar app that keeps your Mac awake while AI
> coding agents like Claude Code and Codex are working — and automatically
> restores normal sleep the moment they finish. It detects agents on its own,
> shows their live status, protects your battery, and notifies you when your
> overnight task is done. No Terminal, no forgotten sleep settings, no drained
> battery.

## 3. The problem (page section)

AI coding agents now run for tens of minutes or hours. Close the lid or walk
away, and macOS sleeps — killing the task or its network connection mid-run.
The workarounds are all bad:

- `caffeinate` and `pmset` require Terminal knowledge and sudo.
- Generic keep-awake apps hold the Mac awake **forever**, draining the battery
  long after the agent finished at 2 a.m.
- Nothing tells you whether the agent is still working, finished, or crashed.

## 4. The solution (page section)

Agent Mode is an **agent-aware supervisor**, not a keep-awake toggle:

1. **Start your agent.** Run Claude Code, Codex, or any long task. Agent Mode
   detects it within seconds — no configuration.
2. **Walk away.** The Mac stays awake exactly as long as the work exists.
   Optional lid-closed mode keeps it running with the laptop shut.
3. **It finishes — you know.** Notification fires, a short grace period runs,
   your previous sleep settings come back automatically. On battery, it can
   even put the Mac to sleep to save power.

## 5. Feature list (for feature grid)

| Feature | Copy |
|---|---|
| Automatic agent detection | Recognizes Claude Code, Codex, OpenClaw, Cursor Agent, and Aider out of the box — even running under node, bun, or python. Add any process of your own. |
| Live status | Menu-bar panel shows each agent's runtime, CPU %, and memory, plus power, battery, and sleep state at a glance. |
| Smart sleep | Stays awake only while work exists. Configurable grace period after the last agent exits: immediately, 5, 15, 30 minutes, or never. |
| Lid-closed mode | Optional: keep running with the laptop closed, on battery. Uses the macOS admin dialog — the app never sees your password. |
| Battery protection | Configurable cutoff (default 20 %), optional require-charger mode, hard floor at 10 %. Never fights macOS thermal protection. |
| Active sleep | Opt-in: when the last agent finishes on battery, the Mac is actively put to sleep to save power — never while anything is running. |
| Screen keep-awake | Opt-in: keep the display on while agents are detected — watch the run live without touching the mouse; screen sleep returns the moment work ends. |
| Bulletproof restore | Your previous sleep settings are recorded before any change and restored on stop, quit — or automatically at next launch after a crash or force-quit. |
| Failsafe timeout | Never stays awake past a configurable limit (default 12 h), even if a process wedges. |
| Notifications | Agent finished, agent disappeared, battery threshold, mode stopped, sleep restored. |
| Invisible by design | No Dock icon, no window clutter — one small moon/bolt icon in the menu bar. Launch-at-login optional. |
| Zero dependencies | Native Swift/SwiftUI. No Electron, no background services, ~280 KB download. |

## 6. UI description (for illustrations/mockups — no screenshots exist yet)

Menu-bar icon states:
- 🌙 `moon.zzz` (SF Symbol) — idle, watching for agents
- ⚡ `bolt.circle.fill` — actively keeping the Mac awake
- ⏱ `bolt.badge.clock` — grace period counting down

Dropdown panel (≈300 px wide, native macOS styling), top to bottom:
1. Header: green status dot, "**Agent Mode: ON**", on/off switch
2. Status line: "Keeping Mac awake" / "Waiting for agents" / "Grace period"
3. Agent rows: `Claude Code — Running — 37 min` with `pid 4821 · 62% CPU · 412 MB`
4. Power block: `Power: Connected · Battery: 92% · Sleep: Disabled while agents are active`
5. Toggles: "Keep awake even with no agents", "Sleep when agents finish (on battery)", "Lid-closed mode (admin)", grace-period picker
6. Footer: Settings… · Quit

Suggested aesthetic for the page: dark, terminal-adjacent developer tool;
accent colors that read "asleep vs. alert" (deep blue/violet for sleep, green
or amber bolt for active). Keep Apple-native feel — this is a real native app.

## 7. How to get it (CTA section)

1. **[Download AgentMode.zip](https://github.com/yikolim/Perpetual-Agent/releases/download/latest/AgentMode.zip)**
2. Unzip → drag **AgentMode.app** to Applications
3. First launch: right-click → **Open** → Open (build is ad-hoc signed, not notarized)
4. Allow notifications; find the moon icon in your menu bar

Or build from source in ~30 s with just Command Line Tools:
`git clone https://github.com/yikolim/Perpetual-Agent && cd Perpetual-Agent && ./build.sh --run`

## 8. Audience

- Claude Code / Codex users running long autonomous tasks
- Developers with long builds, test suites, training runs, downloads
- Local-LLM users
- Anyone who wants unattended work to survive walking away from the desk

## 9. Competitive positioning

| vs. | Difference |
|---|---|
| Amphetamine / KeepingYouAwake | Those keep the Mac awake until you remember to turn them off. Agent Mode knows what's running and returns to normal sleep by itself. |
| caffeinate / pmset | No Terminal, no sudo memorized, no forgotten `disablesleep 1`. State is always restored — even after a crash. |
| Cloud agent platforms | Your code, your machine, your API keys — nothing leaves the laptop. |

## 10. FAQ (page section)

- **Why does macOS warn me on first open?** The build is signed but not
  notarized (no paid Apple Developer account). Right-click → Open once;
  normal double-click after that.
- **Does it need my admin password?** Only if you enable optional lid-closed
  mode — and macOS itself shows that dialog; the app never sees or stores
  credentials.
- **What if the app crashes while sleep is disabled?** The keep-awake
  assertion is released by the kernel automatically, and any `pmset` change is
  restored at next launch from a saved record. Your Mac can't get stuck awake.
- **Will it cook my battery?** No — configurable battery cutoff (default
  20 %), optional require-charger mode, a 10 % hard floor, and an optional
  "sleep when agents finish on battery" mode.
- **Is it safe?** ~1,500 lines of readable Swift, open source, zero
  dependencies, no network access at all.

## 11. Tech specs (footer)

- macOS 14+, universal (arm64 + x86_64), ~280 KB zip
- Swift 5.9 · SwiftUI MenuBarExtra · IOKit power assertions · libproc process
  detection · UserNotifications · SMAppService login item
- Built and released automatically by GitHub Actions from `main`
