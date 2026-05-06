# Common Patterns

## Scale reads

- **Caching** — serve hot data from memory, skip the DB. Fast; data can be stale.
- **Read replicas** — DB copies that absorb read traffic; primary focuses on writes.
- **Indexing** — pre-built lookup structures so queries find rows without a full scan (like a book index).

## Scale writes

- **Sharding** — split data across DB nodes by a shard key (e.g. `user_id`); each node owns its slice.
- **Batching** — group multiple writes into one operation to reduce per-write overhead.
- **Async writes** — ack the client immediately; persist to DB in the background. Lower latency, risk of data loss on crash.

## Real-time data

- **WebSockets** — persistent, full-duplex connection; both sides can send at any time. Best for chat, live collaboration.
- **SSE (Server-Sent Events)** — server pushes updates over a single long-lived HTTP connection (one direction: server → client). Best for live feeds, streaming responses.
- **Long polling** — client requests, server holds it open until data is ready, then responds. Client immediately re-requests. Simulates push over plain HTTP.

## Long-running processes

- **Message queues** (Kafka, SQS) — offload work so the client gets an immediate ack; task runs async.
- **Worker pools** — fixed set of workers drain the queue concurrently, preventing resource exhaustion.
- **Workflow engines** (Temporal, AWS Step Functions) — orchestrate multi-step jobs with retries, dependencies, and state tracking.

## Failures & reliability

- **Retries** — retry on failure; cap at 2–3 attempts. Use **exponential backoff** (1s → 2s → 4s) to avoid hammering a struggling service.
- **Idempotency** — retrying an operation N times produces the same result as doing it once. Prevents duplicate charges, duplicate records.
- **Self-healing** — system auto-recovers: restarts crashed containers, reroutes traffic away from unhealthy nodes.
- **Circuit breaker** — if a downstream service keeps failing, stop sending requests to it for a cooling-off period. Prevents cascading failures.

## Separate reads / writes

- **CQRS (Command Query Responsibility Segregation)** — writes go to the write model (DB), reads go to a separate optimized read model (cache, replica). Prevents both workloads from fighting over the same resources.
