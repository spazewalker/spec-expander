# Flavor: System design

## Dimensions to cover
- **Workload:** users/requests per second (average and peak), read/write ratio, payload sizes, growth over 1–2 years.
- **Latency budget:** end-to-end p99 target, and how it splits across components.
- **Data:** volume, access patterns, consistency model (strong vs eventual), durability, retention, multi-region needs.
- **Availability:** target uptime (99.9% vs 99.99% changes the design a lot), failure domains, RTO/RPO.
- **Scaling:** horizontal vs vertical, partitioning/sharding key, hot-spot risks, backpressure.
- **Cost:** monthly budget or cost per request; managed vs self-hosted trade-off.
- **Security and compliance:** auth model, tenant isolation, encryption, data residency.
- **Operations:** deploys, observability, on-call burden, team size and skills.

## Common defaults ([pattern], override from profile/project)
- Start with the simplest design that meets the stated load with roughly 3–5× headroom; name the scale-out path rather than building it now.
- 99.9% availability unless stated otherwise; single region with backups.
- Managed services (managed DB, queue, object storage) for small teams.
- Back-of-envelope numbers shown explicitly: QPS, storage per day, bandwidth.

## Typical checks
| Requirement | Check |
|---|---|
| Capacity | written back-of-envelope calculation, then a load test of the critical path |
| Latency budget | per-component budget table; trace-based measurement once built |
| Availability | failure-mode table (component down → effect → mitigation) |
| Cost | monthly cost estimate at current and 10× load |
| Design soundness | alternatives considered with reasons for rejection (ADR style) |

## Questions that usually matter
- Real expected load now and in a year? Over-designing for imaginary scale is the most common failure here.
- What consistency is actually needed (e.g. can a feed be a few seconds stale)?
- Budget and team size: who operates this?
- Any hard constraints: cloud provider, region, compliance?
