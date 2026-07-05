---
name: ship
description: Commit, push, and open a PR/MR in one shot — the full git lifecycle via the GitHub (gh) or GitLab (glab) CLI, auto-detected from the remote. INVOKE when the user says "ship", "ship it", "commit and PR", "open a pull request", or finishes a feature and wants it up for review.
disable-model-invocation: true
user-invocable: true
---

# Ship

One command from working tree to a PR/MR URL. Uses `gh` for GitHub remotes and
`glab` for GitLab remotes — detected from `git remote get-url origin`.

## Preflight

1. `git status` + `git diff` to see what's actually changing.
2. **Secret scan** the diff before anything leaves the machine: look for API
   keys, tokens, `.env` contents, private keys, passwords. If found, STOP and
   report — do not commit.
3. Confirm the CLI exists (`gh --version` / `glab --version`) and is authed
   (`gh auth status` / `glab auth status`). If not, tell the user to run the
   login themselves via `! gh auth login` and stop.

## Commit

4. If on the default branch (`main`/`master`), create a feature branch first —
   never commit straight to it. Name it from the change (`feat/…`, `fix/…`).
5. Stage intentionally. Group the work into one or more **conventional commits**
   (`feat(scope): …`, `fix(scope): …`, `chore(scope): …`). Split by type/scope
   when the change is genuinely mixed; otherwise one clean commit.

## Push + open

6. Push with upstream: `git push -u origin <branch>`.
7. Open the PR/MR with a value-first description — what changed and why, not a
   file listing. Scale depth to the change; tiny fix gets a tiny body.
   - GitHub: `gh pr create --fill` then refine, or pass `--title`/`--body`.
   - GitLab: `glab mr create --fill` (or `--title`/`--description`), source =
     current branch, target = default branch.
8. Print the resulting PR/MR URL.

## Rules

- Never force-push a shared branch. Never commit secrets. Confirm before any
  destructive or hard-to-reverse git op (reset --hard, history rewrite).
- End commit messages / PR bodies per the repo's convention if one exists.
