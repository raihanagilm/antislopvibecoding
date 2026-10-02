---
name: antislop-vibecoding-frontend
description: "Anti-slop rules for frontend code: render order, OOP/reuse, and centralized design tokens. English mirror of docs/frontend.md."
allowed-tools: Read Write Edit Glob Grep Bash
---

# antislop-vibecoding-frontend

> Part of the antislop-vibecoding system. Load together with the core skill `antislop-vibecoding`.
> English mirror of the Indonesian source of truth `docs/frontend.md` (IDs match).

## Scope

Frontend code structure & flow: render/execution order, OOP/component approach, code reuse. For element naming and visual quality, see `antislop-vibecoding-uiux`.

## Rules

### FE-01 — Render order: main content first, secondary after

- **Tell:** when switching pages/menus, secondary elements (e.g. notifications) render before the main content.
- **Why:** main content is the user's goal; if secondary elements appear first, it feels wrong/janky.
- **Fix:** order render/execution by priority: **main page content first**, secondary elements (notifications, toasts, sidebar, etc.) after. When fixing or updating code, check the **order**, not just the visuals.

### FE-02 — OOP & reuse: don't rewrite the same thing

- **Tell:** the same component (input, notification, page fragment) is rewritten on several pages — and inconsistently.
- **Why:** duplication makes code long, hard to maintain, and creates cross-page inconsistency.
- **Fix:** apply OOP/component practice: build the component once (e.g. `InputField`, `Notification`, `CardProduk`) and **call** it wherever needed — never rewrite. One source of truth per component (DRY; see UX-05).

### FE-03 — Separate design tokens (color, size, format)

- **Tell:** color/size/format are written inline in each page/component (hardcoded), so values differ across pages.
- **Why:** hardcoding causes inconsistency and is hard to change; each page "invents" its own values.
- **Fix:** move **design tokens** to one place (e.g. a theme file, or CSS variables: color, size, spacing, radius, ratio). Pages and components **use** the tokens instead of raw values. Goal: consistent color, size, and format across pages (see UX-05, UX-08, FE-02).

## Checklist

All answers must be **yes**:

- [ ] Render order is correct: main content first, secondary elements (notifications, etc.) after. (FE-01)
- [ ] No component/code is rewritten; it's called from one source (OOP/reuse). (FE-02)
- [ ] Color/size/format come from centralized design tokens, not hardcoded per page. (FE-03)
