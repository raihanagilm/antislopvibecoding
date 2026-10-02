---
name: antislop-vibecoding
description: "Anti-slop guide for vibe coding: process rules and principles that stop generic AI output across backend, frontend, UI/UX, and database. Core skill; load with the area skills."
allowed-tools: Read Write Edit Glob Grep Bash
---

# antislop-vibecoding

> Anti Slop: rules for AI coding agents during vibe coding. **Core skill** (process + principles).
> English mirror of the Indonesian source of truth in `docs/`. Rule IDs (WF-xx, BE-xx, FE-xx, UX-xx, DB-xx, OT-xx) match across both.
> Load this together with the relevant area skills:
> `antislop-vibecoding-backend`, `antislop-vibecoding-frontend`, `antislop-vibecoding-uiux`,
> `antislop-vibecoding-database`, `antislop-vibecoding-others`.

## What "slop" is

**Slop** = output (code, UI, text, schema) that looks machine-made because of **defaults**, not **decisions**.
Signs: generic (fits any project), repeats popular patterns with no reason, and nothing can be explained with a "why".
The opposite is not "tidy" but **specific and justified** — every choice can be explained in one sentence.

## How to use

- Read everything before acting (WF-01). In a docs setup, read all files in `docs/` plus the root `README.md`; in a skills setup, load this skill and every relevant area skill.
- Every rule has the same shape: **Tell** (the pattern), **Why** (why it's a problem), **Fix** (what to do), with the rule ID in the heading.
- Run the relevant area checklists before declaring work done (see Release Gate).

## Rules

### WF-01 — Read the docs first, then act

- **Tell:** the agent writes/runs code without reading the guide files in `docs/` first.
- **Why:** without reading first, the agent falls back to generic defaults — exactly what slop is.
- **Fix:** before doing anything, read **all** files in `docs/` (`README.md`, `principles.md`, `workflow.md`, and every area file) **plus the root `README.md`**. (`_TEMPLATE.md` is just a template; skip it.) Then start.

### WF-02 — Docs/context empty? Ask first, don't guess

- **Tell:** the guide is empty or project context is missing, but the agent starts building by guessing (e.g. inventing a project structure).
- **Why:** guessing invents assumptions that may not fit the need — the source of slop.
- **Fix:** if docs/context are empty or unclear, stop and ask in the user's language what to build (web app, CLI, API, mobile, script, other). Do not act before an answer.

### WF-03 — Tech recommendations need reasons (pros & cons)

- **Tell:** the agent picks a stack (or the popular default) without explaining reasons or trade-offs.
- **Why:** a choice without reasons is a generic default and gives the user nothing to judge risk by.
- **Fix:** ask needs/target first, then give options with a recommendation + *why*, plus pros and cons for each. The user decides.

### WF-04 — Stuck/frustrated? Search GitHub (stars > 1000)

- **Tell:** the agent is stuck (≥ 2 failed attempts in a row) or the user is frustrated by no progress, and the agent invents a solution or gives up.
- **Why:** inventing while stuck adds slop; proven patterns usually exist in open repos. 1000+ stars signals quality and real use.
- **Fix:** search GitHub (repos with **> 1000 stars**) or MCP/web for references, use them, then **verify and adapt** — never blind-copy. If there is **no network access**, say so honestly and ask for direction instead of inventing.

### WF-05 — Build the MVP first (no over-engineering)

- **Tell:** the agent builds a full/overblown feature set (extra abstractions, config, "future-proofing") before a minimal version runs.
- **Why:** building big up front yields unused code — a source of bugs and slop — and is hard to verify.
- **Fix:** build the **smallest working version** that solves the core need (MVP). Resist extra features/abstractions. Add more only after the MVP runs.

### WF-06 — Documentation at the top of every file

- **Tell:** a code file starts with imports/statements and no explanation, or the agent writes no documentation.
- **Why:** without a header, readers (including the next agent) must guess the file's purpose — a trigger for duplication and slop.
- **Fix:** start every file with a short doc header (comment/docstring): purpose + main contents. **Keep it short** (1–3 lines); don't ramble or restate the code.

### WF-07 — Always update README.md

- **Tell:** something changed (feature, structure, usage, tech) but `README.md` is left stale.
- **Why:** a stale README misleads. Documentation that doesn't change with the code is documentation slop.
- **Fix:** whenever structure/usage/tech changes, update `README.md` in the same piece of work; also update the `docs/` index when rules/areas change.

### WF-08 — Skills/MCP are mandatory, and evaluated from user feedback

- **Tell:** the skill/MCP ran, then the user corrects a part (e.g. "this table is off") and the agent only fixes the code without evaluating the skill/MCP.
- **Why:** repeated corrections on the same thing mean the skill/MCP isn't good enough. Fixing the code alone won't prevent a repeat.
- **Fix:** apply the relevant skill/MCP **always** — while this system is still docs, "skill/MCP" = the `docs/` documents (or these skills); once real skills/MCP exist, use them. Then watch user corrections: treat them as a signal the rules are lacking, and **update the rules** (add or tighten). Record changes in the relevant area file / skill.

### WF-09 — Record the folder structure

- **Tell:** the project folder structure isn't recorded; to know the repo, the agent must search or read files one by one.
- **Why:** without a structure map, every fix starts "blind" — slow and likely to touch the wrong place.
- **Fix:** keep a folder-structure map in a file that WF-01 reads (e.g. `docs/README.md` or the root `README.md`): important folders + contents + key files. Update it when structure changes. The agent reads this map first before exploring.

## Rule tiers (source of truth: `principles.md`)

| Tier | Meaning | Consequence |
|---|---|---|
| **Hard** | Violation = cannot be done. | Must be met before release. |
| **Purpose** | Technique allowed, with a written reason. | Can't justify → change it. |
| **Quality** | Lower priority, for cleanliness. | Fix when convenient. |

## Release Gate

Before declaring work done, run **all checklists** from the areas the work touches, plus:

1. All `docs/` files (or all relevant skills) have been read (WF-01).
2. All relevant area checklists are "yes".
3. No **Hard** rule is violated.

If even one is "no", the work is **not** done.
