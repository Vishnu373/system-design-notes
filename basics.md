# Basics

## Reliability

Continuing to work correctly, even when things go wrong.

- **Fault** — one component deviates from its spec.
- **Failure** — the whole system stops serving the user.
- **Fault-tolerant / resilient** — system anticipates faults and copes with them.

---

## Scalability

A system's ability to cope with increased load.

### Describing load

Pick the number that captures how "busy" your system is:

| System     | Load parameter          |
| ---------- | ----------------------- |
| Web server | Requests per second     |
| Database   | Read/write ratio        |
| Cache      | Hit rate                |
| Chat app   | Concurrent active users |

### Describing performance

- **Throughput** — how much work done per unit time (e.g. 10k queries/sec).
- **Latency** — delay between sending a request and getting the first byte back.
- **Response time** — total wait the client feels: latency + network + queue + processing.

### Measuring response time — use percentiles, not averages

Sort all response times fastest → slowest:

| Percentile | Meaning                            |
| ---------- | ---------------------------------- |
| p50        | Median — half of requests faster   |
| p95        | 95% of requests faster than this   |
| p99        | 99% of requests faster than this   |
| p999       | 99.9% of requests faster than this |

Higher percentile = measuring your slowest (worst) users.

**Amazon uses p999** — their slowest users tend to be their most valuable (frequent buyers). A 100ms slowdown = 1% sales drop. Don't optimize p9999 — those are random noise (GC pauses, network blips) and cost too much to fix.

**SLOs / SLAs** — percentiles appear in contracts. SLO = internal target, SLA = external commitment (breach = refund).

### Coping with load

**Vertical scaling (scale up)** — move to a more powerful machine.
- e.g. upgrading from an Apple M1 to an M4 chip.

**Horizontal scaling (scale out)** — distribute load across multiple smaller machines. Also called **shared-nothing architecture**.
- e.g. running your service across 3 M1 machines instead of one M4.

**Elastic systems (auto-scaling)** — automatically add resources when load increases. Best for unpredictable or spiky traffic.

**Manual scaling** — a human analyzes the load and provisions resources. Simpler but slower to react.

> In practice, most systems combine both: scale up to a reasonable single-machine limit, then scale out beyond that.

---

## Maintainability

Making life easier for the people who operate and extend the system over time.

### Operability

Making routine operations easy so the team can focus on high-value work instead of firefighting.

- **Health monitoring** — dashboards and alerts that show whether the system is healthy (e.g. Grafana, Datadog).
- **Quick recovery** — runbooks and automated restarts so a crashed service is back up in seconds, not hours (e.g. Kubernetes auto-restarts failed pods).
- **Root cause tracking** — structured logs and distributed tracing so you can pinpoint *why* something failed (e.g. Datadog APM, OpenTelemetry).
- **Automation & standard tooling** — CI/CD pipelines, infrastructure-as-code (Terraform, Ansible) so changes are repeatable and auditable, not manual and fragile.
- **Good documentation** — runbooks, architecture diagrams, and on-call guides so anyone can operate the system, not just its original author.

### Simplicity

Managing complexity — not reducing functionality.

**Abstraction** — implement the complexity once and hide it behind a clean interface.
- e.g. programming languages abstract away binary machine code; you write `x + y`, not CPU instructions.
- e.g. an SDK abstracts away raw HTTP calls to an API.
