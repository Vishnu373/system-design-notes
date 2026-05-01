# Basics

## Reliability

Continuing to work correctly, even when things go wrong.

- A **fault** is one component of the system deviating from its spec.
- A **failure** is when the system as a whole stops providing the required service to the user.
- Systems that anticipate faults and can cope with them are called **fault-tolerant** or **resilient**.

---

## Scalability

A system's ability to cope with increased load.

### 1. Describing load

"How busy is your system, and in what way?" — measured in numbers.

**Examples:**
- Netflix: How many people are streaming simultaneously right now?
- WhatsApp: How many messages are being sent per second?
- Amazon: How many product page views per second during Black Friday?

Load parameters vary by system — pick the ones that matter most for yours:

| System     | Relevant load parameter |
| ---------- | ----------------------- |
| Web server | Requests per second     |
| Database   | Read/write ratio        |
| Cache      | Hit rate                |
| Chat app   | Concurrent active users |

### 2. Describing performance

When you increase the load:
- With current system resources (CPU, memory, network bandwidth etc.), is performance stable?
- How much do you need to increase resources to keep performance stable?

**Throughput** — how much work a system can do in a given time period.
> A database processing 10,000 queries/sec has higher throughput than one processing 1,000/sec.

**Latency** — how long it takes for a single request to be processed (delay between sending a request and getting a response).
> You click a button → it takes 200ms to load the result. That 200ms is the latency.

**Response time** — the total time the client experiences waiting for a response. Includes latency plus network delays, queuing time, and processing time.

> Latency is a component of response time, but response time is what the user actually feels.

### Measuring response time

Percentiles are a better measure than averages.

Sort response times fastest to slowest:

| Percentile | What it means                              |
| ---------- | ------------------------------------------ |
| p50        | Half of requests are faster than this      |
| p95        | 95% of requests are faster than this       |
| p99        | 99% of requests are faster than this       |
| p999       | 99.9% of requests are faster than this     |

The higher the percentile, the more you're measuring extreme slow requests (outliers).

**Why outliers matter — Amazon's example:**
- Amazon measures internal services at **p999** (1 in 1,000 requests).
- Their slowest users are often their most valuable customers (frequent buyers).
- A 100ms slowdown = 1% drop in sales.
- A 1 second slowdown = 16% drop in customer satisfaction.

> A small number of slow requests can directly hurt your most valuable users and your revenue.

**Why not optimize p9999?**
- Those extreme slow requests are often caused by random, uncontrollable events (network blips, GC pauses).
- The engineering effort to fix them outweighs the benefit.

Amazon chose **p999 as the sweet spot**.
