# Architecture Decision Records

Every decision this project has taken, newest first. The numbered files in
this directory are the source of truth; this index is generated from them by
`pix_tool_process_adr`, so editing it by hand is work the next call throws away.

## Index

| #                                                                                              | Title                                                                                | Date       | Status   | Tags                                          |
| ---------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ | ---------- | -------- | --------------------------------------------- |
| [002](002-rebuild-the-reactive-todo-demo-with-custom-elements-and-the-vendored-template-en.md) | Rebuild the reactive Todo demo with custom elements and the vendored template engine | 2026-10-07 | accepted | demo, custom-elements, template-engine, store |
| [001](001-condense-fundamentals-slides-and-move-demo-styling-to-external-css.md)               | Condense Fundamentals slides and move demo styling to external CSS                   | 2026-10-06 | accepted | slides, custom-elements, css, scope           |

## What each decision says

The opening sentence of each decision and of what it cost, lifted from the
file. Where a line stops short the rest is in the ADR.

| ADR                                                                                            | Decision                                                                                                                                                                                                                | Consequence                                                                                                                         |
| ---------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| [002](002-rebuild-the-reactive-todo-demo-with-custom-elements-and-the-vendored-template-en.md) | Create a standalone repo ~/Projects/talks/custom-elements-and-beyond-demo.                                                                                                                                              | Positive: one shared runtime (Store + template engine), a clean store:change render loop, and a demo that matches the talk exactly. |
| [001](001-condense-fundamentals-slides-and-move-demo-styling-to-external-css.md)               | Rewrite \_01 to 3 slides, \_02 to 2, \_03/\_04/\_05 to 3 each; delete \_06-styling-elements.html and its include; move demo styling out of per-slide inline <style> blocks into src/styles/demos.css using element +... | Positive: deck is ~40% shorter, styling centralized and pix-compliant (demos.css passes all 61 guardrails).                         |

## By theme

Tags carried by 3 or more decisions. A decision appears under every
theme it carries, and the early ADRs that predate the tag field are listed last.

## Operating rules

- Record a structural decision with `pix_tool_process_adr`, when it is taken.
- Never rewrite an accepted decision. Supersede it with a new one that says why.
- Never edit this file. Edit the ADR and let the next call regenerate it.
