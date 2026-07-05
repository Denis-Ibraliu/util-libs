---
name: review
description: Code review over a diff, PR, commit, or branch — one pass with parallel review lenses, confidence-gated findings, and optional adversarial verification. INVOKE when the user says "review", "code review", "review this PR/diff/branch", or when /commonkit:implement finishes a build. Reports findings ranked by severity; does not commit or edit.
disable-model-invocation: true
user-invocable: true
---

# Review

One review pass, real findings only. This is the reviewer `/commonkit:implement`
hands off to at the end, and it works standalone on any change. It reports — it
does not fix, commit, or push.

## Pick the scope

Detect what to review, in this order:

1. Explicit target the user named (a PR number, commit hash, path).
2. Uncommitted work — `git diff HEAD` (and `git status` for new files).
3. Nothing pending → the branch vs its base (`git diff <base>...HEAD`).

For a PR, pull the diff with `gh pr diff <n>` / `glab mr diff <n>`. State the
scope you settled on before reviewing.

## Review (parallel lenses, one pass)

Read the diff, then apply these lenses — run them in parallel as sub-agents when
the diff is large (>~50 lines or many files), inline otherwise:

- **Correctness** — logic errors, edge cases, off-by-one, null/empty, error
  paths, intent vs implementation.
- **Simplicity / YAGNI** — reinvented stdlib, speculative abstraction, dead
  flexibility, duplication. What can be deleted.
- **Tests** — missing coverage on branches/loops/parsers/money-security paths;
  weak or implementation-coupled assertions.
- **Context lens (conditional)** — add security when the diff touches auth,
  input handling, or secrets; performance when it touches hot loops, queries, or
  I/O. Skip if nothing warrants it.

## Gate, dedup, report

1. **Confidence gate** — drop anything you can't tie to a concrete failure. Every
   finding names the input/state and the wrong result it produces. No "consider"
   or style nits unless asked.
2. **Adversarial verify (for High/Critical)** — before reporting a severe
   finding, try to refute it. If you can't reproduce the failure, downgrade or
   drop it.
3. **Dedup** across lenses; one finding per real issue.
4. **Report** ranked most-severe first. Per finding: `file:line`, one-sentence
   defect, the failure scenario, and a suggested fix. End with the scope
   reviewed and a one-line verdict (ship / fix-first). Empty list is a valid,
   good result — say so plainly.

## Bias

High signal over coverage. A short list of real bugs beats a long list of
maybes. When unsure a finding is real, refute it first; report only what survives.
