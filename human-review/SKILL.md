---
name: human-review
description: "Explain a code change to a human reviewer."
disable-model-invocation: true
---

# Human Review

Write comments in **zh-tw**.

## Tool
`{baseDir}/scripts/tuicr-review.sh`:
- `start [--repo PATH] [SCOPE]` — open a review and print its slug.
- `comments [--repo PATH]` — print new comments as JSON.
- `add [--repo PATH] <flags> "text"` — post a comment. Flags: `--target-file`, `--line`, `--end-line`, `--side old|new`, `--type issue|suggestion|note|praise`.

Ask once if SCOPE is missing. Use `working`, a revision/range, or `pr <n>`.

## Workflow
1. Read the scoped `git show/diff` and surrounding code.
   Done when you can name the *delta* of each touched function against the old behavior.

2. Group hunks by concern and draft all comments before `start`.
   Every non-trivial function or decision gets its own comment, anchored to that function or the few lines that carry the decision.
   For each, cover the *delta*, why, and the likely objection.
   A transform or protocol change carries one toy before/after.
   Annotate repetitions once, summarize them review-wide, and redact secrets.

3. Run `start`, then `add`:
   - File-level: one-sentence *delta*.
   - Function or decision: *delta*, why, and objection, separated by blank lines.
     Omit the objection for rote changes.
   - Review-level: the *intuition* the later comments hang on,
     which group to read first and why,
     then deliberate scope cuts with reasons.
   - Use `issue` only for problems. Anchor every group to code.

4. Follow-up:
   - `approved` → stop.
   - New comments → address each, fix as needed, then `start` the same scope. `add` only the reply and any new *delta*.
   - Fetch failure → run `comments` manually.

## Gotchas
- Existing open review → ask the human to press `q`, then retry.
- Abnormal exit sends no trigger.
- Amend/rebase creates a new scope identity; start a new session.
