# Talk Recap — customElements & beyond

> Source of truth: the actual slide files in `src/`. Nothing here is invented; anything not present in the code is explicitly marked as _planned/disabled_.

---

## 1. Project Overview

- Static **reveal.js** talk, bundled with **Parcel**, package `custom-elements-and-beyond` (v0.3.2).
- Deploy: GitHub Pages (`.github/workflows/static.yml`).
- Dev command: `pnpm dev` → `parcel src/index.html -p 6001`.
- CSS is nearly empty: `src/styles/index.css` only imports `@pixu-talks/fonts` and `@pixu-talks/theme`. The design system lives in the external workspace `../talks-libraries/packages/{core,theme,fonts}` (linked via `pnpm-workspace.yaml`).

Composition chain (PostHTML):

```
src/index.html
└─ src/slides/slides.html
   ├─ intro/intro.html
   ├─ summary/summary.html
   ├─ topics/topics.html
   └─ outro/outro.html
+ src/brand/brand.html   (Dev Dojo logo)
+ src/scripts/index.js   (module entry)
```

---

## 2. ACTIVE Deck (what is actually rendered)

> **Key point:** `topics.html` only includes `_01`–`_07` + `_44` + `_45`. All files `_10`–`_43` exist but are **commented out**. `summary/_topics.html`, `outro/_wrap-up.html` and several internal sections are commented out too. The ROADMAP claiming "45/45 complete" does **not** reflect the served deck.

### Intro (`src/slides/intro/`)

| File               | Content                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `_doodle.html`     | Title slide: "**custom**Elements & **beyond**" (zoom transition) + speaker notes                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `_pixu.html`       | Speaker slide: Emiliano Pisu, "Father of 2 monsters", D&D, Senior Design Engineer / Sensei & Co-Host @ Dev Dojo IT + 7 social links                                                                                                                                                                                                                                                                                                                                                                             |
| `_main-topic.html` | **Hook + scope** (10 sections): "How I hope you feel after this talk" → Die Hard meme; "Bold Opinion Alert" (framework APIs are becoming canonical); "web standards are coming back"; "customElements === Web Components?" → `!true`; Web Components = 3 APIs (`<baseline-status>` for `autonomous-custom-elements`, `shadow-dom`, `template`); focus **only on customElements v1 / customized built-ins**; `<baseline-status featureId="customized-built-in-elements">` + Safari note + WebReflection polyfill |

### Summary (`src/slides/summary/`)

| File            | Status       | Content                                                                                                       |
| --------------- | ------------ | ------------------------------------------------------------------------------------------------------------- |
| `_dive-in.html` | ✅ active    | "Let's dive in" with animated `<dive-in><stack-container>`                                                    |
| `_topics.html`  | ❌ commented | Agenda "What are we going to talk about": Fundamentals / Practical patterns / Advanced / Integration / Future |

### Topics (`src/slides/topics/`) — talk body

**`_01-what-are-custom-elements.html`** (3 slides — condensed)

- Defining a customElement: autonomous vs customized built-in, both in one code block (`<pix-button-std>` vs `<button is="pix-button">`, `{ extends: 'button' }`).
- Closing slide "Can you see the difference?": both use class + `customElements.define`, but the extends pattern **keeps native behavior, a11y and SEO**.

**`_02-first-custom-element.html`** (2 slides — condensed)

- Click counter: **define** + **register** in one code block, then **use** `<button is="counter-button">` with a **real live demo** (`counter-button.js`).

**`_03-lifecycle-callbacks.html`** (3 slides — condensed)

- Lifecycle timeline (`constructor`, `connectedCallback`, `attributeChangedCallback`, `disconnectedCallback`), all four callbacks in one code block, and rules of thumb.

**`_04-attributes-properties.html`** (3 slides — condensed)

- **Attributes (HTML)** vs **Properties (JS)** comparison table; **reflection pattern** + **observed attributes** code; `ValidatedInput` example (uses `aria-invalid`, no CSS class).

**`_05-extending-natives.html`** (3 slides — condensed)

- Extending **buttons** (`IconButton`) and **inputs** (`DateInput`) with live demos.
- Extending **semantic elements** (`CollapsiblePanel` on `<details>`) and **links** (`SafeLink`), with live demos.
- "Choosing the right base" table (`button`, `input`, `div`, `a`, `img`).

> `_06-styling-elements.html` was **deleted**. Demo styling moved to `src/styles/demos.css` (element + attribute selectors, imported by `src/styles/index.css`).

**`_07-a-lean-0-deps-way.html`** (23 slides — the largest chapter)

- "Expected Question" flow #1/#2/#3 (boilerplate? mimic Lit/React/Vue? metaprogramming/mixins/decorators/reactivity?) with "Enough jokes" + 5× "Oops" memes.
- "**extend native HTML**" (`PixButton/PixInput/PixDialog/PixDetails`), "no base classes === no centralization".
- **Static Initialization Blocks** (`static { componentDecorator(...) }`).
- **Decorators & Mixins**: `componentDecorator`, `defineCustomElement`, `buildExtendOptions`, `applyMixins`, `buildAttributeHandlers`, `buildEventHandlers`, `buildLifecycleMethods`.
- **Metaprogramming**: `static attributes` / `static events` → `handleXAttributeChanged` / `handleXEvent`.
- **Lifecycle methods**: `handleEvent`, `attributeChangedCallback`, `connectedCallback`, `disconnectedCallback`.
- **Reactivity & two-way data binding**: `createStore` (Proxy + `Map` listeners), `BoundInput extends HTMLInputElement` + `BoundText extends HTMLSpanElement`, HTML usage, and a **live demo** with customized built-ins feature detection + manual fallback. A "reactive counter with effects" section is **commented out**.

**`store/`** (2 slides) → Proxy Store theory + `emitChange()` `CustomEvent` (`store:change`).

**`dom-parts/`** (2 slides) → DOM Part theory + parts overview (`ChildNodePart`, `AttributePart`, `PropertyPart`, `EventPart`, `Range`).

**`template-engine/`** (4 slides) → `html`/`render`, a custom element with the decorator pattern, declarative form bindings (`value=`/`.checked=` + `@input`/`@change`), `<for each>` and `<if condition>` blocks.

**`demo/`** (3 slides) → the custom-elements Todo demo, the click→`store:change`→DOM commit flow, and the metadata-driven decorator pattern (`static {}` + `static attributes`/`static events`).

**`_44-resources.html`** (1 slide) → MDN, WHATWG spec, Web.dev Baseline, Web Components GitHub org, Can I Use. (Libraries/tools section commented out.)

**`_45-conclusions.html`** (1 slide) → **Call to action**: "Build on the platform. Extend, don't replace." + 4 bullets (extends, clear contracts, progressive enhancement/a11y, ship ESM + adapters). ("What to do next" / "Thank you" sections commented out.)

### Outro (`src/slides/outro/`)

| File            | Status       | Content                                                                             |
| --------------- | ------------ | ----------------------------------------------------------------------------------- |
| `_links.html`   | ✅ (partial) | QR codes "Slides" and "Slides Repo" (personal/devdojo sections commented out)       |
| `_taf.html`     | ✅           | "That's All Folks" with autoplay `taf.mp3`, circles animation, Pixu photo, SVG path |
| `_thanks.html`  | ✅           | "Grazie ❤️!" (speaker slide)                                                        |
| `_wrap-up.html` | ❌ commented | Pros/Cons placeholder + Conclusions                                                 |

---

## 3. Live examples and JS components

`src/scripts/index.js` imports **only**:

- `baseline-status/baseline-status.js` (autonomous component, ~330 LOC + utils/constants/templates/icons; powers the `<baseline-status featureId="…">` tags)
- `components/`: **`icon-button`**, **`date-input`**, **`collapsible-panel`**, **`counter-button`**, **`safe-link`**

Present but **not imported** (effectively unused in the active deck): `components/smart-img.js` (`img is="smart-img"`) and `components/sortable-list.js` (`ul is="sortable-list"`).

Active components (all customized built-ins, light DOM):

- `counter-button.js` → `CounterButton extends HTMLButtonElement`, `handleClick` as arrow field, `aria-live="polite"`.
- `icon-button.js` → injects `<span class="icon">` in `connectedCallback`.
- `date-input.js` → `this.type='date'`, `aria-label`, change log.
- `collapsible-panel.js` → `extends HTMLDetailsElement`, `role=region`, `aria-expanded` toggle.
- `safe-link.js` → `extends HTMLAnchorElement`, adds `noopener`/`noreferrer` and `data-confirm`.

Inline examples inside slides (not from `index.js`): the **BoundInput/BoundText** script in `_07` (with `supportsCustomizedBuiltins()` feature detection and fallback). The `date-input` demo in `_05` is now the plain component (the previous inline `<output>` wiring was dropped during condensation).

Demo styling for the active topic slides lives in `src/styles/demos.css`, imported by `src/styles/index.css`.

---

## 4. Planned but DISABLED content

`src/slides/topics/` contains **44** `_*.html` files. Besides the active ones, ~35 slides are ready but commented out, in 4 blocks (per comments in `topics.html`):

- **Patterns & Use Cases**: `_10` composition, `_11` binding, `_12` event-handling, `_15` accessibility, `_16` form-integration, `_17` state-management, `_18` error-handling, `_19` api-design, `_20` real-world-examples
- **Advanced**: `_21` advanced-dom, `_22` content-distribution, `_23` lifecycle-patterns, `_24` advanced-events, `_25` performance, `_26` testing, `_27` typescript, `_28` build-tools, `_29` debugging, `_30` versioning, `_31` advanced-recap
- **Integration**: `_32` vue, `_33` react, `_34` angular, `_35` design-systems, `_36` framework-agnostic, `_37` interoperability, `_38` case-studies, `_39` micro-frontends, `_40` ssr-and-hydration, `_41` wrappers-and-adapters, `_42` integration-recap, `_43` future-features
- **Reactivity (outside main numbering)**: `_18-reactivity-observable`, `_19-reactivity-signals`, `_20-reactivity-binding` (coexist with `_18-error-handling`, `_19-api-design`, `_20-real-world-examples` → **duplicate prefixes**)

---

## 5. Reference docs and detected inconsistencies

- **`.github/copilot-instructions.md`**: talk scope, HTML/CSS/JS standards, "every example extends a native element", always `{extends}` + `is=`.
- **`.github/ROADMAP.md`**: **6 phases / 45–47 slides**, declared "100% Complete". The slide↔file map is **stale** (e.g. ROADMAP "Slide 5 → `_02-custom-elements-api.html`" but the real file is `_02-first-custom-element.html`); it also contains duplicated blocks (Slides 27–37 repeated).
- **`.github/docs/STRUCTURE.md`**: alternative "Pixu-style" 45-min structure with timing (Hook, Lean Web Mindset, Customized Built-ins, Observables/Signals, Constructable Stylesheets/adoptedStyleSheets, Progressive Enhancements, Mini demos). It describes concepts (Observable, Signal, router, popover, ElementInternals) that are **not** in the active deck: the only implemented reactivity is the `createStore` Proxy in `_07`.
- Uncommitted changes present: `.github/copilot-instructions.md`, `.github/docs/STRUCTURE.md`, `package.json`.

**Themes actually covered by the active deck**: CE definition + extends vs standard → first CE (counter) → lifecycle → attributes/properties/observed → extending button/input/details/a → "0-deps" (static blocks, mixin/decorator, metaprogramming, reactive store + two-way binding) → Store (Proxy + `store:change`) → DOM Parts → Template Engine (`html`/`render`/`model`/`repeat`) → Demo → resources → call to action.

**Companion demo**: `~/Projects/talks/custom-elements-and-beyond-demo` — a standalone repo that mirrors `reactive-apps-without-frameworks-demo` with custom elements and the vendored template engine. See its `CONTEXT.md` and `docs/implementation-journey.md`.

**Themes only planned (commented files)**: advanced composition/binding/events, a11y, forms/ElementInternals, state management, observable/signals, error handling, API design, performance, testing, TS, build, debugging, versioning, Vue/React/Angular integrations, design systems, SSR/hydration, micro-frontends, future features.

---

## 6. Tooling notes

- `pix_tool` routing → `pix-frontend-vanilla-reactive` (alternative: `pix-frontend-custom-element`).
- MDN references: `Using custom elements`, `Window.customElements`.
- **tokensave has no graph for this project** (the root is not among the registered projects; `graph_root` is rejected), so all facts above were verified directly against the real files.
