---
name: architect
description: Find the architectural friction in a codebase and propose refactors that make it deeper, more testable, and easier to navigate. INVOKE when the user says "architect", "improve the architecture", "this is hard to test/change", or asks how to restructure a module/subsystem. Produces an RFC doc, not code.
disable-model-invocation: true
user-invocable: true
---

# Architect

Surface where the code fights you, then design the interface that removes the
fight. Framed around a single lens: **shallow vs deep modules** (Ousterhout) —
small interfaces hiding large implementations beat big interfaces exposing small
ones.

This skill analyzes and proposes. It does not refactor. Output is a written RFC
the user (or `/commonkit:implement`) can act on.

## Vocabulary (use consistently)

module · interface · depth · seam (where you'd test/substitute) · adapter ·
leverage (how much one change buys) · locality (what lives together).

## Flow

1. **Read the ground truth first.** Any `CONTEXT.md`/domain glossary, existing
   ADRs, and the README. Don't re-litigate decisions already recorded in an ADR.
2. **Explore for friction, not rules.** Use the `Explore` agent to walk the
   relevant area and surface: shallow modules (thin wrappers, pass-through
   interfaces), tight coupling between things that should be independent, and
   untested seams. Report symptoms you actually saw, with file:line — not a
   generic checklist.
3. **Generate rival designs in parallel.** For the worst 1–2 friction points,
   spawn sub-agents to draft genuinely different interfaces: a minimalist one, a
   flexibility-first one, a caller-optimized one, and a ports-&-adapters one.
   Compare depth, blast radius, and migration cost.
4. **Recommend one.** State the winner and *why*, and name what you'd graft from
   the runners-up.
5. **Write the RFC.** Save to `docs/rfcs/<slug>.md`: problem + evidence, the
   options with tradeoffs, the recommendation, and a phased migration path.
   Offer to open it as a GitHub/GitLab issue only if the user wants it tracked.

## Bias

Deeper module, fewer moving parts, more testable seams — measured against the
actual pain, not architectural fashion. If the friction is small, say so and
propose the smallest change instead of a rewrite.
