# Noesis — Branding Spec

> Design brief for creating the Noesis product branding. Written to hand
> directly to a design agent/tool. Companion doc: `PRODUCT.md` (full copy bank,
> features, links).

---

## 1. The name

**Noesis** (no-EE-sis), from Greek *νόησις* — in Plato, the highest form of
knowing: the mind directly apprehending truth, no reasoning steps needed.

Why it fits: the app *knows* what your AI agents are doing — instantly,
without being told. It watches, understands, and acts (keeps the Mac awake,
restores sleep, alerts you) purely from direct observation.

Pronunciation guide worth putting in small print somewhere: **/noʊˈiːsɪs/**.

## 2. What the product is (one breath)

A native macOS menu-bar app that keeps your Mac awake while AI coding agents
(Claude Code, Codex…) are working, shows what's running vs. stalled, and
restores normal sleep the moment work ends. Tagline: **"Close your Mac. Your
agents keep working."**

## 3. Brand personality

- **Calm intelligence** — it watches quietly; it never nags. The brand should
  feel serene, not busy.
- **Nocturnal** — its natural habitat is an unattended desk at 2 a.m. Night,
  moonlight, a single point of light doing work in the dark.
- **Precise / native** — real Swift, zero dependencies, Apple-quality. Not a
  startup-gradient SaaS look; closer to a beautifully made tool.
- **A touch classical** — the Greek name invites restrained classical cues
  (a serif accent, a geometric form), but keep it subtle — no columns, no togas.

Mood in five words: *nocturnal, lucid, minimal, trustworthy, Greek.*

## 4. Logo directions (pick or blend)

1. **The open eye at night** — a minimal eye whose iris is a crescent moon or
   a spark: awareness that never sleeps. Works as an outline glyph.
2. **Moon + spark** — a crescent moon with a small orbiting bolt/star: the
   app's own state icons (moon = idle, bolt = working) elevated into a mark.
   Most true to the product UI.
3. **The letter Ν (nu)** — a geometric Ν/N whose negative space forms a
   crescent; wordmark-first direction.

The mark must reduce to a **single-color glyph** that stays legible at 16 px
(see constraints) — test every concept at that size first.

## 5. Color

Night-sky base + one "awake" accent. Suggested starting palette (adjust
freely, keep the roles):

| Role | Suggestion | Notes |
|---|---|---|
| Base / background | `#0B0E1A` deep space indigo | landing page + icon field |
| Surface | `#151A2E` | cards, panels |
| Moonlight (text/idle) | `#C7CCDB` silver | primary text on dark |
| Awake accent | `#4ADE80` green **or** `#FACC15` amber | the "agents working" state — pick ONE as hero accent |
| Secondary accent | `#7C6FFF` violet | links, highlights, sparingly |
| Alert | `#F87171` | idle-too-long / warnings only |

Rules: dark-first design with a correct light variant; accent must pass
WCAG AA on the base; never more than one accent per composition.

## 6. Typography

- **Wordmark / headings:** a clean geometric sans (Inter, Söhne-like, or
  SF Pro Display). Optionally a single classical touch — e.g. wordmark set in
  a serif like Freight or GT Sectra — but only if it stays quiet.
- **Body:** system sans (SF Pro Text / Inter).
- **Code/dev accents:** a monospace (SF Mono / JetBrains Mono) for command
  snippets and the "built for terminal people" feel.

## 7. Deliverables

| Asset | Spec |
|---|---|
| Logomark | vector, single-color + full-color; must read at 16 px |
| Wordmark | "Noesis" horizontal lockup, dark + light versions |
| macOS app icon | 1024×1024 in Apple's squircle grid, plus 512/256/128/32/16 renders — night-sky field + mark; follow current macOS icon style (soft depth, no flat web look) |
| Menu-bar icon | 16×16 pt **template image** (pure black + alpha only, macOS tints it) in 3 states: idle moon, active bolt, grace bolt+clock — may restyle the current SF Symbols into custom glyphs |
| GitHub social preview | 1280×640 — mark + tagline on night field |
| OG / landing hero | 1200×630 social card + a hero illustration for the landing page (e.g. a closed MacBook in the dark, one status light glowing; or the menu-bar panel floating in night space) |
| Favicon | 32/16 px from the logomark |
| Badge | small "Runs your agents all night — Noesis" shield for READMEs |

## 8. Applications to mock

1. Landing page hero (headline "Close your Mac. Your agents keep working." +
   download button + hero visual).
2. The menu-bar dropdown restyled with brand colors (structure is fixed — see
   PRODUCT.md §6 for the exact panel contents; don't invent new UI).
3. GitHub repo header (social preview) for github.com/yikolim/Perpetual-Agent.
4. Release page banner for the `Noesis.zip` download.

## 9. Voice

Calm, declarative, second person, slightly nocturnal. Short sentences.
Technical words allowed, hype words banned ("revolutionary", "supercharge",
"10x"). Example register:

> It's 2 a.m. Your agent is still working. So is Noesis.
> When the work ends, your Mac goes back to sleep. You already knew it would.

Full copy bank (taglines, features, FAQ): `PRODUCT.md`.

## 10. Don'ts

- No robot mascots, no sparkly "AI" gradients, no brain imagery.
- No literal Greek-temple clichés.
- Don't redesign the app's actual layout — brand it, don't rebuild it.
- Menu-bar glyphs must stay monochrome template images — no color in the
  menu bar, ever.
- Keep the Apple-native feel: generous spacing, SF-style geometry, restraint.
