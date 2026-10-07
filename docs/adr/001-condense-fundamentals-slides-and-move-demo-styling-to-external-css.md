# 001: Condense Fundamentals slides and move demo styling to external CSS

- **Date**: 2026-10-06
- **Status**: accepted
- **Tags**: slides, custom-elements, css, scope
- **Author**: Emiliano Pisu <emiliano.pisu@webidoo.com>

## Context

The active Fundamentals deck (topics \_01-\_05) had 34 slides total, several overlapping, plus a full styling chapter (\_06). The talk owner asked to shrink each topic to 2-3 slides and delete the styling chapter, while keeping the live customized built-in demos working.

## Decision

Rewrite \_01 to 3 slides, \_02 to 2, \_03/\_04/\_05 to 3 each; delete \_06-styling-elements.html and its include; move demo styling out of per-slide inline <style> blocks into src/styles/demos.css using element + attribute selectors only, imported from src/styles/index.css. Live demos keep using the existing globally registered components (icon-button, date-input, collapsible-panel, counter-button, safe-link).

## Consequences

Positive: deck is ~40% shorter, styling centralized and pix-compliant (demos.css passes all 61 guardrails). Negative: slide code samples are still parsed as JS by pix_tool_check, so data-part-naming reports a false positive on reveal.js vertical-stack <section> markup; accepted as a known checker limitation. \_07 remains untouched and is a separate workstream.

## What would end this

The assumption this leans on, and the observation that would make it wrong.

## Alternatives Considered

1. Keep inline <style> blocks in each slide (rejected: violates pix no-inline-styles / no-css-classes).
1. Refactor the shared @pixu-talks/theme package (rejected: out of scope, external workspace).
1. Leave demos unstyled with browser defaults (rejected: degrades the talk visuals).
