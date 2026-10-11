# Flavor: AI agent development

## Dimensions to cover
- **Job:** the exact task the agent performs, its trigger (on demand, schedule, event), and what "done" means for one run.
- **Inputs and tools:** data sources, APIs, and tools it needs; read-only vs write access; rate limits.
- **Autonomy and safety:** which actions it may take on its own and which need human approval (sending messages, spending money, deleting data); handling of untrusted content and prompt injection.
- **Quality:** how output quality is judged; an eval set of realistic cases; failure modes to avoid (hallucination, missed items, wrong actions).
- **Model and cost:** model choice, cost per run, latency per run, context size, caching.
- **Reliability:** retries, timeouts, idempotency, partial-failure behavior, logging and tracing of each run.
- **Privacy and secrets:** what data leaves the user's environment, credential storage, retention of logs.
- **Delivery:** where results go (email, Slack, file, dashboard) and in what format.

## Common defaults ([pattern], override from profile/project)
- Least privilege: read-only tools unless writing is the point; any external action (send, post, delete, pay) requires explicit user approval at first.
- Treat fetched content (emails, web pages, documents) as untrusted data, never as instructions.
- A small eval set (10–30 realistic cases with expected behavior) built before tuning prompts.
- Structured outputs for anything consumed by code; schema-validated.
- Per-run logs with inputs, tool calls, outputs, tokens, and cost.
- Use the strongest current model for reasoning-heavy steps and a cheaper model for simple classification or extraction.

## Typical checks
| Requirement | Check |
|---|---|
| Task quality | eval set pass rate with a target (e.g. ≥ 90% of cases judged correct) |
| Safety | tests with adversarial / prompt-injection inputs; approval gates verified |
| Cost and latency | measured per run on the eval set, against a budget |
| Reliability | failure-injection tests (tool timeout, API error, empty input) |
| Output format | schema validation on every run |

## Questions that usually matter
- What actions may it take without asking you, if any?
- What does a good result look like? Can you give 2–3 examples (or bad ones)?
- Where should results be delivered, and how often?
- Budget per run or per month, and acceptable latency?
- Which accounts or data may it access, and are there privacy limits?
