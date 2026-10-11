# Learning the user's techniques

The profile is how this skill gets better over time: every preference captured is a question that never needs asking again and a default that's right the first time. But a profile full of noise or one-off facts is worse than an empty one — it confidently applies the wrong defaults. These rules keep it useful.

## What to capture

Durable, reusable preferences — things likely to hold for the user's *next* task of this flavor:

- Answers to your open questions that reflect a general stance ("always Postgres unless told otherwise", "I care about p99, not mean").
- Edits they made to your assumptions ("no, use uv not pip", "cap memory at 512 MB").
- Corrections after implementation ("too much abstraction", "always add a benchmark script", "I want notebooks, not scripts for EDA").
- Techniques they use or request repeatedly (evaluation methods, libraries, testing style, report formats, research sources).
- Things they reject — anti-preferences are as valuable as preferences.

## What not to capture

- Facts about this one task (the endpoint names, this dataset's columns) — they belong in the spec, not the profile.
- Facts about one project (its stack, its SLOs) — put those in `<project>/.claude/spec-profile.md`, creating it if needed.
- Your own guesses the user merely didn't object to. Silence on a low-stakes assumption isn't a preference. Only save assumptions the user explicitly confirmed when it was a real choice, or ones confirmed in two or more separate tasks.
- Anything sensitive: credentials, tokens, personal data about other people.

## How to write it

In `profile/profile.md`, under the right flavor heading (or `## Global` if it spans flavors), add or update a single bullet:

```
- Latency target for user-facing APIs: p99 < 50 ms. (confidence: confirmed · 2026-10-12)
```

- Confidence is `confirmed` (user stated or chose it) or `observed` (inferred from repeated behavior, seen 2+ times).
- One fact per bullet; keep each bullet short and actionable as a default.
- If a new preference contradicts an existing bullet, replace the old one (newest wins) and record the change in the log.
- Keep the profile lean — when a section passes ~25 bullets, merge near-duplicates and drop stale items.

Then append to `profile/learnings.md`:

```
## 2026-10-12 — software-dev
- Saved: "p99 < 50 ms for user-facing APIs" (confirmed)
- Evidence: user answered the latency question with "50ms p99, mean doesn't matter"
- Replaced: "p99 < 100 ms" (from 2026-09-30)
```

## Tell the user

Auto-save is transparent, not silent. Finish with one line listing what you saved and where, e.g.:

> Learned: uses uv for Python envs; EDA in notebooks (profile → global, data-science). Say "forget that" to undo.

If they say to undo, remove the bullet and log the removal.

## Version control

The skill directory is a git repository the user keeps on GitHub. After updating the profile, commit locally with a short message (e.g. `learn: data-science — prefers LightGBM baseline`) if the directory is a git repo. Don't push — the user pushes when they choose.
