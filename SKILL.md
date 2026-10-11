---
name: spec-expander
description: Turns a short or sloppy request into a precise, checkable spec before any work starts — fills in unstated requirements (latency, memory, scale, accuracy, cost, failure handling, evaluation) from the user's learned preferences and domain patterns, asks only the few questions that really matter, converts each requirement into a test/benchmark/metric, and learns the user's techniques over time. Use this whenever the user asks to build, design, train, analyze, research, or automate something non-trivial — a service, API, app, pipeline, model, experiment, agent, system design, or study — especially when the request is brief, vague, or uses words like "efficient", "fast", "good", "scalable", "production-ready", "robust", even if they don't ask for a spec. Skip it for small edits, quick questions, and requests that are already fully specified.
---

# Spec Expander

Most bad outputs aren't caused by weak coding — they come from information the request never contained. "Build an efficient API" leaves hundreds of decisions open, and the model silently fills them with generic defaults that may not match what the user meant. This skill closes that gap *before* implementation: expand the request into an explicit spec, confirm the risky parts with the user, turn every requirement into something measurable, and remember the user's preferences so the next expansion needs fewer questions.

Spending a little extra time up front is the point. The user has said they prefer some added latency over getting the wrong, slow, or buggy thing.

## Files this skill uses

All paths are relative to this skill's directory unless noted.

- `profile/profile.md` — the user's learned global preferences, organized by flavor. Read it at the start of every run. Create it from `assets/profile-template.md` if missing.
- `profile/learnings.md` — append-only log of what was learned and why (the audit trail behind the profile).
- `<project root>/.claude/spec-profile.md` — optional project-specific overrides (stack, SLOs, conventions). Project settings beat global ones.
- `references/flavors/<flavor>.md` — domain checklists. Read only the ones that match the request.
- `references/learning.md` — rules for updating the profile. Read it at the learning step.

## Step 1 — Load what you already know

Read `profile/profile.md`, and the project override if one exists. Glance at the project itself when there is one (README, dependency manifest, existing tests/benchmarks, CI config) — just enough to know the stack and conventions, so defaults fit the real context instead of a generic one.

## Step 2 — Pick the flavor(s) and size the effort

Flavors (read the matching reference file):

| Flavor | File | Typical requests |
|---|---|---|
| Software development | `software-dev.md` | APIs, apps, CLIs, libraries, features, bug-prone refactors |
| System design | `system-design.md` | architectures, scaling, distributed systems, infra choices |
| Data science / ML | `data-science.md` | analyses, models, dashboards, pipelines, experiments on data |
| Research | `research.md` | literature reviews, studies, experiments, papers, investigations |
| AI agent development | `ai-agents.md` | LLM apps, agents, tools, RAG, evals, prompts, automations |

A request can mix flavors (e.g., "agent backend that scales" = ai-agents + system-design). Read each relevant file; don't read the others.

Scale the output to the task. Over-specifying is its own failure: it wastes the user's attention, buries the questions that matter, and can push the implementation toward needless complexity. Rough budgets for what you show in chat:

- **Small** (hours): ~10–20 lines. Goal, a few numbered requirements with checks, 0–2 questions. Skip the tables.
- **Medium** (a day or two): ~40–70 lines. All sections, but one line per item.
- **Large** (multi-day system): full spec. Show a summary in chat, and put the complete version in a file if the user accepts the offer in Step 4.

Write tersely: one line per requirement, assumption, and check. The user should be able to review the whole spec in a minute or two.

## Step 3 — Draft the expanded spec

Use this structure (drop sections that don't apply):

```markdown
# Spec: <short title>
**Request (verbatim):** <user's words>
**Flavor:** <flavor(s)> · **Size:** small | medium | large

## Goal
One or two sentences: what success looks like for the user, not just what gets built.

## Scope
- In: ...
- Out (explicitly not doing): ...

## Requirements
Functional — what it must do.
Quality / non-functional — with numbers: latency, throughput, memory, accuracy, cost, reliability, security, etc.

## Assumptions
Each decision you filled in, tagged with its source:
- [profile] from the user's saved preferences
- [project] from the repo/project override
- [pattern] a common default for this kind of task
- [guess] your judgment, lower confidence
Mark anything high-impact and [guess] as a question instead of an assumption.

## Open questions  (max ~3, highest impact first)
Each with your proposed default, so the user can just say "ok".

## Acceptance checks
Every requirement maps to a check that can actually be run or inspected:
| Requirement | Check | How |
|---|---|---|
| p99 < 50 ms @ 2k rps | load test | `k6 run bench/load.js` threshold |
| handles malformed input | tests | `pytest tests/test_validation.py` |

## Plan
Short ordered steps, with the checks built early (tests/benchmark harness before or alongside the implementation).
```

Why the numbers and checks matter: words like "efficient" or "good" can't be verified, so the implementation drifts toward whatever is typical. A number plus a runnable check gives both you and the user an objective finish line, and lets you iterate until it passes instead of declaring victory.

Choosing defaults: prefer, in order, the project override, the profile, then the flavor file's patterns, then your judgment. Pick numbers that fit the actual context — a hobby script doesn't need p99 targets; an internal batch job doesn't need 10k rps. If a default would add real complexity (caching layers, distributed infra, custom allocators), make it a question instead of assuming it.

Be exact about permissions and access. When the work touches accounts, data, or external systems, list the precise scopes or rights each component needs (e.g. `gmail.readonly` for fetching, `gmail.send` for delivery) and keep every section consistent with that list. Don't call something "read-only" if any part of the default design writes or sends. Users decide whether to trust the result based on this, so an overstatement here is worse than a gap.

Choosing questions: ask only where (a) the answer changes the design substantially and (b) neither the profile, the project, nor a safe default resolves it. Good questions are concrete and come with a proposed answer: "Read-heavy or write-heavy? (I'll assume ~95% reads.)" Avoid questionnaires — if you have more than three, the rest become labeled assumptions.

## Step 4 — Confirm with the user

Show the spec compactly and ask the open questions (use AskUserQuestion when available, otherwise ask in chat). Make it easy to accept: "Reply ok to proceed, or change anything." If the user said to just go ahead, or is unavailable, proceed with your proposed defaults and say so.

For large or multi-session work, offer to save the spec into the project (e.g. `docs/specs/<slug>.md`) so later sessions inherit it. Don't write into the user's repo without that offer being accepted, unless the profile says to.

## Step 5 — Build against the checks

Implement normally, but build the acceptance checks early and run them. When reporting completion, report against the checks with real measured results (e.g. "p99 = 38 ms at 2k rps; 24/24 tests pass; RSS 140 MB"), and say plainly which checks failed or weren't run. This is what makes the spec worth having — otherwise it's just a longer prompt.

## Step 6 — Learn

The user wants this skill to keep learning their techniques, and has chosen **auto-save**: update the profile without asking first, but always tell them in one line what you saved so they can veto it.

Read `references/learning.md` and follow it. In short: capture durable preferences revealed by their answers, edits, and corrections (during the spec or later in the task) into `profile/profile.md`, log each change with evidence in `profile/learnings.md`, and put project-specific facts in the project override instead of the global profile.

Only when you actually saved something, end your response with one line like:
> Learned: prefers Go for latency-critical services; default p99 target 50 ms (saved to profile → software-dev).

If nothing was learned, say nothing about the profile. Explaining why nothing was saved is noise the user didn't ask for.
