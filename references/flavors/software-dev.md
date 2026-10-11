# Flavor: Software development

## Dimensions to cover
- **Interface:** API shape (REST/gRPC/CLI/library), inputs/outputs, error format, versioning.
- **Performance:** latency (state p50/p99, not "fast"), throughput, memory/RSS, startup time, binary/bundle size.
- **Data:** storage engine, schema, migrations, consistency needs, retention.
- **Correctness:** edge cases, validation, idempotency, concurrency/race conditions.
- **Reliability:** timeouts, retries, graceful shutdown, behavior when dependencies fail.
- **Security:** authn/authz, input sanitization, secrets handling, dependency hygiene.
- **Operability:** logging, metrics, health checks, config via env, deployment target (container, serverless, bare binary).
- **Maintainability:** language/framework, project layout, test style, lint/format, docs.

## Common defaults ([pattern], override from profile/project)
- User-facing API: p99 < 100 ms at expected load; internal service: p99 < 250 ms.
- Structured JSON errors with stable error codes; input validated at the boundary.
- Config via environment variables; no secrets in code.
- Unit tests for logic, an integration test per endpoint, and a load-test script when performance is a stated requirement.
- Prefer the project's existing stack over introducing new languages or frameworks.

## Typical checks
| Requirement | Check |
|---|---|
| Latency / throughput | load test (k6, wrk, locust) with explicit thresholds |
| Memory | measure peak RSS under the load test |
| Correctness | unit + integration tests, property tests for parsers/serializers |
| Robustness | fuzz or malformed-input tests |
| Security | dependency audit (`npm audit`, `pip-audit`, `cargo audit`), auth tests |
| Style | the project's lint/format/type-check commands |

## Questions that usually matter
- Expected load and read/write mix (drives caching, DB, and language choices).
- Where it runs (local, container, serverless, edge): changes startup time, memory, and state choices.
- Does it need auth, and of what kind?
- Is there an existing codebase or convention to match?
