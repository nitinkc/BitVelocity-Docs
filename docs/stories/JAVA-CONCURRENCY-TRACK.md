# Java Concurrency Track — Reactive and Virtual Threads

Goal: Use modern Java concurrency models to solve real problems in this project. Two complementary approaches:
- Reactive (Project Reactor + Spring WebFlux + R2DBC + non-blocking clients)
- Virtual Threads (Java 21 Loom) for thread-per-request simplicity with high scalability

Pick scenarios that align with your services. Each scenario includes: when to use, implementation sketch, lab steps, and acceptance criteria.

Prereqs
- JDK 21 installed (for virtual threads)
- Spring Boot familiarity; prefer separate small lab repos per scenario
- For messaging: a local Kafka/Redpanda (or testcontainers)

---

## Scenario A — Reactive Checkout Pipeline (WebFlux + Reactor + Kafka + R2DBC)
Service fit: `bv-eCommerce-core/order-service` and `payment-adapter-service`

Problem
- Implement an end-to-end reactive flow: POST /checkout → validate cart → price calculation → payment authorization (HTTP) → emit `order.created` event → persist order non-blockingly.

When Reactive shines
- High I/O concurrency: external calls (pricing, payment), DB I/O, event publishing.
- Backpressure and timeouts across multiple dependent calls.

Implementation sketch
- Spring WebFlux controller returns `Mono<OrderResponse>`.
- Use reactive clients: `WebClient` for payment; `R2dbcEntityTemplate` (or Spring Data R2DBC) for persistence.
- Use Reactor operators: `flatMap`, `zip`, `timeout`, `retryBackoff`, `onErrorResume`.
- Publish event via reactive Kafka client (reactor-kafka) or use `Mono.fromRunnable` wrapping producer send (ensure you don’t block).
- Correlate tracing context with `Spring Cloud Sleuth OTel` or Micrometer tracing.

Lab steps
1) Define an event contract `order.created.v1.json` (already present — reuse) and a `payment.authorized.v1` if needed for the outbound flow.
2) Implement `POST /checkout` in a WebFlux app using non-blocking adapters throughout.
3) Add R2DBC for order persistence; write a repository returning `Mono<Order>`.
4) Call payment adapter via `WebClient` with `timeout(Duration.ofSeconds(2))` and circuit breaker (Resilience4j Reactor operators).
5) On success, persist and publish `order.created` to Kafka using a reactive publisher.
6) Add tests with `StepVerifier` and mocked WebClient; use Testcontainers for Kafka and Postgres (R2DBC) if possible.

Acceptance criteria
- Under load (e.g., 500 concurrent checks), latency stays stable; no thread starvation.
- Thread dumps show few platform threads; Reactor schedulers active; no blocking calls on event loop.
- StepVerifier tests assert timeouts and retries; event published once (idempotency token).

Gotchas
- Avoid blocking (JDBC) in reactive chain; use R2DBC.
- Bridge to blocking libraries via `Schedulers.boundedElastic()` only when unavoidable, with metrics.

---

## Scenario B — Reactive Streaming Inventory Updates (SSE/WebSockets + Kafka)
Service fit: `inventory-service` + `notification-service`

Problem
- Real-time updates to clients when stock changes, driven by inventory events.

When Reactive shines
- Fan-out streams; efficient handling of many concurrent clients; backpressure.

Implementation sketch
- Consume Kafka topic `stock.adjusted` via reactor-kafka → `Flux<InventoryEvent>`.
- Transform to UI events and expose as SSE `/inventory/stream` or WebSocket endpoint.
- Use `Flux.share()` or `Sinks.many().multicast()` for multi-subscriber fan-out.

Lab steps
1) Build a reactive Kafka consumer producing a `Flux`.
2) Expose SSE endpoint that maps events into text/event-stream with heartbeats.
3) Browser client renders updates; verify reconnection behavior.

Acceptance criteria
- Multiple clients receive updates with minimal latency; no blocking threads per client.
- Backpressure respected (buffer bounds, drop/latest strategy documented).

---

## Scenario C — Virtual Threads: Partner Webhook Dispatcher (Java 21)
Service fit: `partner-webhook-dispatcher` and `notification-service`

Problem
- Fan-out HTTP callbacks to many partner endpoints on specific events, with retries and per-partner rate limits.

When Virtual Threads shine
- Thread-per-request model that’s easy to reason about, but scalable; many I/O-bound operations using simple blocking code.

Implementation sketch
- Spring MVC (Servlet stack) or plain Java HTTP client with virtual-thread executor.
- Create an `ExecutorService` with `Executors.newVirtualThreadPerTaskExecutor()`.
- For each event, submit one task per partner callback; block on HTTP send (simple code) but uses virtual threads underneath.
- Use structured concurrency for groups of callbacks; timeouts and cancellation.

Lab steps
1) Enable virtual threads in the app: configure Tomcat/Undertow and task execution to use virtual threads (Spring Boot 3.2+ supports opt-in).
2) Implement a dispatcher that loads partner endpoints and posts JSON payloads with retries/backoff (Resilience4j or manual).
3) Add per-partner rate limits and a bulkhead using semaphores.
4) Instrument latency and concurrency metrics; capture thread dump showing many virtual threads.

Acceptance criteria
- Dispatch to 1k partners completes within an acceptable time on a dev machine with modest CPU/mem.
- Code remains idiomatic blocking style; error handling straightforward; metrics show high concurrency.

Gotchas
- Use HTTP clients that play well with virtual threads (JDK `HttpClient` is good).
- Avoid pinning: don’t use synchronized blocks or long-running platform-thread operations in virtual threads.

---

## Scenario D — Virtual Threads: Replay Service (Batch/File + Messaging)
Service fit: `replay-service`

Problem
- Replaying historical events from a CSV or object storage into Kafka with dedupe and pacing.

When Virtual Threads shine
- Simple batch pipelines that are I/O-bound (read file, publish event), easy to write/read, but need high concurrency.

Implementation sketch
- Read file line-by-line; create a task per valid event; publish to Kafka using a blocking client in each virtual thread.
- Use structured concurrency to bound in-flight tasks, with cancellation on fatal errors.

Lab steps
1) Implement CSV parser producing event envelopes from `docs/event-contracts/schema/envelope.schema.json`.
2) Submit tasks to a virtual-thread executor; throttle (tokens/second) to respect downstream capacity.
3) Idempotency: compute hash keys to avoid duplicate publish; write a small dedupe cache.

Acceptance criteria
- Can process 100k records locally with good throughput; CPU remains modest.
- Clean shutdown and resumability after failure (offset/line tracking).

Gotchas
- Backpressure must be explicit (semaphores/queues) since virtual threads make it easy to oversubscribe downstream systems.

---

## Scenario E — Compare and Choose (Benchmark & ADR)
Goal
- Run a simple benchmark comparing Reactive vs Virtual Threads on a small I/O-heavy workflow to inform future choices.

Steps
1) Implement the same endpoint twice: WebFlux (Reactive) vs MVC + Virtual Threads.
2) Load-test with 500–2000 concurrent requests on a laptop; record latency, CPU, thread counts.
3) Document results and write a short ADR: "Choosing concurrency model for <service/use case>" referencing ADR-006 and ADR-007.

Acceptance criteria
- Clear recommendation grounded in metrics and operational considerations.

---

## Cross-References
- Projects & Modules overview: `docs/00-OVERVIEW/projects-and-modules.md`
- Event contracts: `docs/event-contracts/`
- Microservice patterns: `docs/03-DEVELOPMENT/microservices-patterns.md`
- Retry/backoff policies: `docs/adr/ADR-006-retry-backoff-policies.md`
- Observability baseline: `docs/adr/ADR-007-observability-baseline.md`

---

## Mentor Guidance
- Start with one Reactive scenario (A or B) and one Virtual Threads scenario (C or D).
- Ensure tests: StepVerifier for Reactive; integration tests for Virtual Threads flow.
- Encourage a small ADR after the comparison.
