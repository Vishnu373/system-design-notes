# Common Patterns

## Case 1: Scale reads

- **Caching** — store frequently read data in memory so you skip the DB entirely. Fast, but data can be stale.
- **Read replicas** — copies of the DB that handle read traffic, offloading the primary DB which focuses on writes.
- **Indexing** — pre-computed lookup structures on the DB so queries find data faster without scanning every row (just like the index of a book).

## Case 2: Scale writes

- **Sharding** — split data across multiple DB nodes by a shard key (e.g., `user_id`). Each node handles writes for its own slice of data, distributing the load.
- **Batching** — group multiple writes together and send them in one operation instead of one by one. Reduces overhead (like the memtable in LSM trees).
- **Asynchronous writes** — return success to the client immediately and persist to the DB in the background. Lower latency, but with a risk of data loss on crash.

## Case 3: Real-time data

- **WebSockets** — full-duplex, persistent connection between client and server. Both can send messages at any time. Best for chat and live collaboration.
- **Server-Sent Events (SSE)** — server pushes updates to the client over a single long-lived HTTP connection. One direction only (server → client). Best for live feeds and streaming responses.
- **Long polling** — client sends a request, server holds it open until data is ready, then responds. Client immediately opens the next request. Simulates push over regular HTTP.

## Case 4: Long-running processes

- **Message queues** — offload work to a queue (e.g., Kafka, SQS) so the client gets an immediate response while the task is processed asynchronously in the background.
- **Worker pools** — a fixed set of workers pick up tasks from the queue and process them concurrently, preventing resource exhaustion from unlimited parallel execution.
- **Workflow engines** — orchestrate multi-step processes with dependencies, retries, and state tracking (e.g., Temporal, AWS Step Functions). Best when steps depend on each other's output.

## Case 5: Failures and reliability

- **Retries** — automatically retry a failed request after a delay. Limit to 2–3 retries max; beyond that it's likely a deeper issue. Use **exponential backoff** (wait longer after each failure: 1s → 2s → 4s → 8s) to avoid hammering a struggling service.
- **Idempotency** — designing operations so that retrying them multiple times produces the same result as doing it once. Prevents duplicate charges, duplicate records, etc.
- **Self-healing** — system automatically detects and recovers from failures (e.g., restarting crashed containers, rerouting traffic away from unhealthy nodes).
- **Circuit breakers** — if a downstream service keeps failing, stop sending requests to it for a period instead of endlessly retrying. Prevents cascading failures.

## Case 6: Separate reads/writes

- **CQRS (Command Query Responsibility Segregation)** — split read and write operations into separate models. Writes (commands) update the DB; reads (queries) use a separate optimized read model (e.g., a cache or read replica). Prevents read and write workloads from competing for the same resources.
