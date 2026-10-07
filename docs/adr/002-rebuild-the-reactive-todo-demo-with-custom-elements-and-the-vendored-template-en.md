# 002: Rebuild the reactive Todo demo with custom elements and the vendored template engine

- **Date**: 2026-10-07
- **Status**: accepted
- **Tags**: demo, custom-elements, template-engine, store
- **Author**: Emiliano Pisu <emiliano.pisu@webidoo.com>

## Context

The talk needs a companion demo that replicates ~/Projects/talks/reactive-apps-without-frameworks-demo but proves the talk thesis: custom elements + a small store + a template engine, with no framework and no signals.

## Decision

Create a standalone repo ~/Projects/talks/custom-elements-and-beyond-demo. Vendor the store and template engine from @pix-galaxy/pix-vanilla-reactive into src/scripts/core (signals kept only for the engine's internal isSignalLike). Replace the signal-based computed helpers with pure selectors over store.snapshot(). Bridge the store to the DOM with a global store:change event and connectStore(element, view). Use customized built-ins for controls (button is=ceb-button) and autonomous elements for containers. No innerText/value/innerHTML in components.

## Consequences

Positive: one shared runtime (Store + template engine), a clean store:change render loop, and a demo that matches the talk exactly. Negative: signals still ship inside the vendored engine (internal only); full parity for modals/debug/bulk is pending; the talk deck gains four new sections (Store, DOM Parts, Template Engine, Demo).

## What would end this

The assumption this leans on, and the observation that would make it wrong.

## Alternatives Considered

1. pnpm workspace dependency on the published package (rejected: user chose a vendored copy).
1. Copy the reactive demo and keep signals (rejected: the talk must not teach signals).
1. Customized built-ins everywhere (rejected: Safari needs a polyfill; hybrid is safer).
