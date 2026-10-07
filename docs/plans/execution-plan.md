# Execution Plan — Slide condensation (Fundamentals chapter)

Goal: shrink the active Fundamentals deck and remove the styling chapter, keeping every live customized built-in demo working.

## Scope

- [x] **1. Delete `_06-styling-elements.html`** and remove its include from `topics.html`.
- [x] **2. `_01-what-are-custom-elements.html`** → 3 slides max (was 4).
- [x] **3. `_02-first-custom-element.html`** → 2 slides max (was 4).
- [x] **4. `_03-lifecycle-callbacks.html`** → 3 slides max (was 6).
- [x] **5. `_04-attributes-properties.html`** → 3 slides max (was 10).
- [x] **6. `_05-extending-natives.html`** → 3 slides max (was 10).

## Out of scope (later workstreams)

- `_07-a-lean-0-deps-way.html` rework.
- Re-enabling the commented-out `_10`–`_43` slides.
- Updating `.github/ROADMAP.md` / `.github/docs/STRUCTURE.md` (already stale).

## Supporting change

- [x] Move demo styling out of the slides into `src/styles/demos.css` (element + attribute selectors, no classes, no inline styles), imported by `src/styles/index.css`.

## Verification

- [x] `pix_tool_check` run on the rewritten slide HTML and on `demos.css`.
- [x] Project build (`parcel build`) succeeds.
- [x] Dev server serves the condensed deck.
- [x] ADR recorded with `pix_tool_process_adr`.

---

# Workstream 2 — Companion demo + Store/DOM Parts/Template Engine slides

Branch: `refactor/fundamentals-and-store`.

## Repository setup

- [x] Branch `refactor/fundamentals-and-store` created in the talk repo.
- [x] `dist/` restored to HEAD; `pnpm-lock.yaml` kept.
- [x] Standalone repo `~/Projects/talks/custom-elements-and-beyond-demo` created (git init, Parcel, ports 6002).
- [x] Engine vendored from `@pix-galaxy/pix-vanilla-reactive` into `src/scripts/core/` (store, template-engine, disposable, signals).
- [x] Public `core/index.js` limited to Store + template engine.
- [x] Pure selectors replace the signal `computed` helpers; `store:change` + `connectStore` bridge added.

## Demo components

- [x] `ceb-button` customized built-in (`data-action` → `ceb:action`).
- [x] `ceb-app`, `ceb-header`, `ceb-stats-row`, `ceb-filters`, `ceb-todo-list`, `ceb-todo-item` autonomous elements.
- [x] Demo builds (`pnpm build`) and dev server serves on `http://localhost:6002`.
- [ ] Full parity: BulkActions, DebugPanel + DebugLogEntry, TodoModal, CategoryModal, QuickAdd.

## Talk slides

- [x] Store section (`store/`).
- [x] DOM Parts section (`dom-parts/`, 2 slides).
- [x] Template Engine section (`template-engine/`, 3 slides).
- [x] Demo section (`demo/`, 2 slides).
- [x] `topics.html` updated: `_01`–`_05`, `_07`, then Store → DOM Parts → Template Engine → Demo.
- [ ] QR codes for demo + demo repo in the links slide.

## Docs

- [x] `CONTEXT.md` in the demo repo.
- [x] `docs/implementation-journey.md` in the demo repo.
- [x] ADR 002 recorded.
- [ ] Update `docs/talk-recap.md` with the new deck structure.

## Verification

- [x] Demo build passes.
- [ ] Playwright + a11y checks on the demo.
- [ ] Manual browser check of the live demo.
- [ ] Single commit per repo.
