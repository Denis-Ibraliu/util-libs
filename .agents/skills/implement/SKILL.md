---
name: implement
description: Implement a spec, PRD, issue, or clear feature request end-to-end with TDD, then run a single code-review pass at the END across the whole change (not after every phase). INVOKE when scope is clear and it's time to build. Pairs with /commonkit:brainstorm (before) and /commonkit:ship (after).
disable-model-invocation: true
user-invocable: true
---

# Implement

Build the thing, verify continuously, review once at the end, commit. The
defining rule: **code review runs a single time over the complete change, not
after each phase.** Per-phase reviews interrupt flow and re-flag churn that later
phases delete; one review at the end sees the real diff.

## Before writing code

- Read the spec/issue. If scope is fuzzy, stop and suggest `/commonkit:brainstorm`.
- Skim the surrounding code and match its patterns, naming, and comment density.
- Detect the task runner (this repo uses Nx: `nx run <project>:<target>`).
  Prefer it over calling tooling directly.

## Build loop (per phase)

Work in small phases. Each phase:

1. Write the test first at a real integration point (TDD). Not every trivial
   line needs a test — a branch, loop, parser, or money/security path does.
2. Make it pass with the minimum code that works. No speculative abstractions.
3. **Typecheck after each change** and run the touched file's tests. Keep the
   tree green as you go — this is your continuous safety net, and it is separate
   from the final review.
4. Move to the next phase. Do **not** spawn review agents here.

## Finish (once, at the end)

1. Run the **full** test suite + typecheck + lint. Fix failures.
2. **Now** run the review — a single pass over the entire diff via
   `/commonkit:review` (parallel lenses, confidence-gated). It reports; you act.
3. Triage findings: fix real issues, note deliberate simplifications, discard
   noise. Re-run verification after fixes.
4. Commit to the current branch with a conventional-commit message
   (`feat(scope): …`, `fix(scope): …`). To push and open a PR, hand off to
   `/commonkit:ship`.

## Guardrails

- Shortest working diff wins. Delete over add. Don't gold-plate.
- If the spec turns out wrong mid-build, stop and flag it — don't build around it.
