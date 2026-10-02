---
name: antislop-vibecoding-uiux
description: "Anti-slop rules for UI/UX: element naming, premium (non-AI) visuals, color & notifications, icons, cross-page consistency, ratio, sizing scale, and responsiveness. English mirror of docs/ui-ux.md."
allowed-tools: Read Write Edit Glob Grep Bash
---

# antislop-vibecoding-uiux

> Part of the antislop-vibecoding system. Load together with the core skill `antislop-vibecoding`.
> English mirror of the Indonesian source of truth `docs/ui-ux.md` (IDs match).

## Scope

Element/component naming, visual quality (premium vs "looks AI"), color & notifications, icons, cross-page consistency, ratio, size/multiple system, responsiveness (mobile-first, device range, orientation). For render order and component code structure, see `antislop-vibecoding-frontend`.

## Rules

### UX-01 — Name the element/component, don't just "fixed it"

- **Tell:** the user asks for a fix or asks with a photo / vague description ("this border", "this box"), and the agent fixes it without naming the component.
- **Why:** the user doesn't know the element's name, so they don't grow — they can't ask for or learn the same thing next time.
- **Fix:** whenever fixing or referring to an element, state the component's **technical name** (e.g. "this is a *pagination*", "this is a *card*", "this is a *breadcrumb*", "this is a *toast*"). If the user sends a photo, analyze it and name the elements. The goal is the user learning, not just receiving a result.

### UX-02 — Design must feel premium, not "AI/over the top"

- **Tell:** the UI uses loud colors, many unrelated colors, or AI-style effects like heavy glassmorphism, garish gradients, glow.
- **Why:** these are AI signatures — cheap and generic, not precisely designed.
- **Fix:** use verified premium references (via MCP/web) that are calm, not excessive, and don't "look AI". Concrete limits:
  - At most **1 dominant color + 1 accent** (neutrals as the base); total hues ≤ **3**.
  - Forbidden: glassmorphism, glow, gradients with > 2 colors, colored drop-shadows.
  - "Loud" = more than 1 accent **or** > 3 hues.
  - Take 1–2 premium references (via MCP/web) and state your reasons.

### UX-03 — Notification/status colors must be consistent

- **Tell:** status colors (success/error/warning/info) vary across pages, or use unusual colors for status.
- **Why:** status color meaning must be learned once and used consistently; variation confuses users.
- **Fix:** use consistent standard colors: **red** (error), **yellow** (warning), **green** (success), **blue** (info). One meaning = one color, everywhere. Company icons/logos may use their own brand colors.

### UX-04 — Icons: no keyboard emoji, no overused sets

- **Tell:** using keyboard emoji (✅ 🔥 🚀) as UI icons, or the same overused icon sets.
- **Why:** keyboard emoji and overused sets are the clearest AI signature — instantly recognizable and look unconsidered.
- **Fix:** don't use keyboard emoji as icons, and don't use the icon sets most common in AI output (sign: they appear in nearly every generated result). Source icons from verified providers/references (via MCP or GitHub repos with **> 1000 stars**, see WF-04). Icon style must be consistent within one family. Note: the owner may add more forbidden sets anytime.

### UX-05 — Cross-page consistency (benchmark: existing pages)

- **Tell:** a new page/menu is built with sizes, colors, or style different from existing pages; the agent "invents" from scratch.
- **Why:** inconsistency damages the tidy/premium feel and forces repeated corrections.
- **Fix:** before building a new page/menu, **look at existing pages/menus** as the example, then match height/width, color, spacing, and typography. These values are stored as **design tokens** — see FE-03 (don't record them in two places).

### UX-06 — Ratio required (cards, images, borders)

- **Tell:** cards, images, or borders are "just sized" with no fixed ratio; each component has a different ratio.
- **Why:** consistent ratios make layout feel planned; arbitrary ratios look messy.
- **Fix:** use fixed ratios for cards/images/borders. Standard set: **`1:1`, `4:3`, `3:4`, `16:9`, `9:16`**. "One step up" = enlarge the longer side (e.g. `3:4` → `9:16` for portrait; `4:3` → `16:9` for landscape). Never change ratios arbitrarily.

### UX-07 — Sizes must follow a scale/multiple

- **Tell:** height/width use random numbers (e.g. 37px, 123px) that don't follow a multiple of existing sizes.
- **Why:** a multiple system keeps consistency and makes resizing safe.
- **Fix:** all sizes follow a fixed **base scale** (default: **multiples of 4** — 4, 8, 12, 16, 32, 64) with consistent units (px for size, rem for type). If the current value is `4x4`: "bigger" = ×2 → `8x8`; "smaller" = ÷2 → `2x2`. If the user still says **"too big"** though you followed the rule, **don't guess** — explain you already followed the rule, then offer a new multiple (e.g. `1.5x`); if approved, make it the new guideline.

### UX-08 — One color = one palette (shade ramp)

- **Tell:** the user asks for "blue", and the agent uses a single blue value — or different blues everywhere — with no shades.
- **Why:** a single value makes the UI flat/stiff; inconsistent colors look messy and "AI".
- **Fix:** for one color, build a **shade ramp** from dark → light toward white, at least **5 steps** with consistent naming (e.g. `blue-900` dark → `blue-700` → `blue-500` base → `blue-300` → `blue-100` light). All components use values from that palette, not random blues. (see FE-03)

### UX-09 — Contrast required (text vs background)

- **Tell:** text and background use related colors without checking contrast — e.g. dark blue background with dark blue text.
- **Why:** the text becomes unreadable. This is the most common mistake and directly ruins quality.
- **Fix:** ensure enough contrast between text and its background: don't use text and background from the same/nearby shade. On dark backgrounds use light text from the same palette, and vice versa. Minimums:
  - Normal text: **≥ 4.5:1**
  - Large text (≥ 24px, or ≥ 18.66px **bold**): **≥ 3:1**
  - Icons/components/borders: **≥ 3:1**
  Check the contrast; don't eyeball it.

### UX-10 — Mobile first

- **Tell:** the page is designed desktop-first, then mobile is "forced to fit" (shrunk, oversized, or horizontal scroll appears).
- **Why:** most traffic comes from small screens. Desktop-first breaks mobile: clipped, cramped, overlapping.
- **Fix:** design and build from the **smallest screen first** (mobile), then scale up. Ensure no horizontal scroll on mobile. (Size coverage: UX-11; orientation: UX-12.)

### UX-11 — All device sizes (smallest mobile → largest laptop/desktop)

- **Tell:** checked at one size only (usually desktop), then considered done.
- **Why:** layout must hold across the whole device range; bugs often appear at the extremes.
- **Fix:** ensure layout is correct from the **smallest mobile** to the **largest laptop/desktop**. Standard breakpoints (px): **320 · 480 · 768 · 1024 · 1280 · 1440 · 1920**, stored as tokens (FE-03). Test the extremes, not just one point.

### UX-12 — Orientation (rotation)

- **Tell:** designed for one orientation only; when the device is rotated (portrait ↔ landscape) the layout breaks or clips.
- **Why:** users rotate screens (video, games, tablets). Ignoring it breaks the UI.
- **Fix:** handle **portrait and landscape**. On rotation the layout must reflow correctly (viewport height changes) without clipping or horizontal scroll. Test with real rotation, at least on mobile and tablet.

## Checklist

All answers must be **yes**:

- [ ] The technical name of every fixed/referenced component is stated. (UX-01)
- [ ] Design feels premium, not "AI/over the top": no glassmorphism/glow, max 1 dominant color + 1 accent (hue ≤ 3). (UX-02)
- [ ] Notification/status colors are consistent (red/yellow/green/blue; one meaning one color). (UX-03)
- [ ] No keyboard emoji or overused icon sets; icons from a verified source (MCP/GitHub stars > 1000). (UX-04)
- [ ] New pages/menus follow existing pages (size, color, spacing, typography). (UX-05)
- [ ] Cards/images/borders use fixed, proportional ratios. (UX-06)
- [ ] All sizes follow a fixed scale/multiple. (UX-07)
- [ ] One color is a shade ramp, not random values. (UX-08)
- [ ] Contrast safe: normal text ≥ 4.5:1, large text & icons/components ≥ 3:1. (UX-09)
- [ ] Built mobile-first, no horizontal scroll on mobile. (UX-10)
- [ ] Layout correct across the standard 320–1920px breakpoints. (UX-11)
- [ ] Portrait & landscape (rotation) tested; layout reflows without clipping. (UX-12)
