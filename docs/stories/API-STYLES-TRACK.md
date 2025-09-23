# API Styles Track — 9 Types, Mini-Labs, and Acceptance

Goal: Give a developer practical exposure to nine common API styles without overwhelming them. Each section includes: overview, when to use, a small lab, acceptance criteria, and gotchas.

Time-box: 1.5–2 weeks total, 60–120 minutes per style. Keep changes tiny (1–3 files/PR).

Prereqs: Comfortable with Spring Boot basics and HTTP. For messaging: pair as needed.

---

## 1) REST (HTTP/JSON)
Overview
- Resource-oriented, stateless, cacheable; ubiquitous for external/public APIs.
When to use
- CRUD on resources; broad client compatibility; HTTP caching helps; human-friendly.
Reactive vs Virtual Threads
- Reactive: Spring WebFlux controller + WebClient; non-blocking, great for high concurrency.
- Virtual Threads: Spring MVC + JDK HttpClient; simpler blocking style with Loom scalability.
Mini-lab
- Add a simple `POST /orders` and `GET /orders/{id}`. Produce proper status codes (201, 200, 404, 400). Document with OpenAPI.
Acceptance
- OpenAPI renders locally; curl for POST and GET returns expected 2xx/4xx.
Gotchas
- Over/under-fetching; pagination; idempotency for PUT/DELETE; validation errors format.

---

## 2) gRPC (Unary + Streaming)
Overview
- Contract-first (proto), HTTP/2, binary, fast. Great for inter-service RPC.
When to use
- Low-latency service-to-service calls; typed contracts; streaming.
Reactive vs Virtual Threads
- Reactive: Use non-blocking stubs with Reactor wrappers; backpressure with Flux.
- Virtual Threads: Standard blocking stubs executed in virtual threads for simplicity.
Mini-lab
- Define `OrderService` with `GetOrder` (unary) and `ListOrderEvents` (server-streaming). Implement a simple in-memory server, call from a client.
Acceptance
- Both unary and streaming calls succeed locally; basic error status codes used.
Gotchas
- Browser clients need a gateway; payload inspection harder; versioning protobufs with care.

---

## 3) GraphQL
Overview
- Query language for APIs; clients specify fields; single endpoint for reads/writes.
When to use
- Aggregating multiple backends; flexible clients; mobile/web with tailored queries.
Reactive vs Virtual Threads
- Reactive: DataFetchers returning Mono/Flux; non-blocking data loaders.
- Virtual Threads: Blocking fetchers with data loader batching running on virtual threads.
Mini-lab
- Define schema with `type Query { orderById(id: ID!): Order }` and `ordersByStatus(status: String!): [Order!]!`. Implement resolvers over in-memory data.
Acceptance
- Two queries return typed results; basic error reported; docs visible in GraphiQL.
Gotchas
- N+1 queries; caching complexity; mutation design; authorization at field level.

---

## 4) Webhooks (Callback APIs)
Overview
- Provider calls your endpoint on subscribed events. Push, not pull.
When to use
- Notify external systems on events (payments, shipments). Integrations-friendly.
Reactive vs Virtual Threads
- Reactive: WebFlux handler verifies HMAC and enqueues processing; great for bursty inbound traffic.
- Virtual Threads: MVC controller processes synchronously in a virtual-thread pool; simple retry logic.
Mini-lab
- Expose `POST /webhooks/orders` that verifies HMAC signature (shared secret), logs event.
Acceptance
- Invalid signature returns 401; valid signature returns 200 and is idempotent (retries safe).
Gotchas
- Security (signature, allowlists); retries; ordering; replay strategy.

---

## 5) WebSockets
Overview
- Bi-directional, stateful connection; real-time updates.
When to use
- Live dashboards; chat; collaborative apps.
Reactive vs Virtual Threads
- Reactive: Reactor Netty + WebFlux WebSocket; scalable to many connections.
- Virtual Threads: MVC stack with STOMP can run handlers in virtual threads; simpler logic, watch pinning.
Mini-lab
- Spring WebSocket endpoint that broadcasts "order status changed" to connected clients.
Acceptance
- Simple HTML client receives live message after a simulated update.
Gotchas
- Scale and fanout; auth/session; backpressure; fallbacks.

---

## 6) Server-Sent Events (SSE)
Overview
- Uni-directional stream over HTTP; simpler than WebSockets for server->client.
When to use
- Live notifications/feeds where client-only listening is enough.
Reactive vs Virtual Threads
- Reactive: Flux<Event> mapped to text/event-stream; natural fit.
- Virtual Threads: MVC + ResponseBodyEmitter; easy but less efficient than reactive for many clients.
Mini-lab
- Implement `/orders/stream` SSE endpoint streaming status updates.
Acceptance
- JS EventSource receives events; reconnect resumes cleanly.
Gotchas
- Proxy buffering; reconnect logic; event naming/conventions.

---

## 7) JSON-RPC (RPC over HTTP/JSON)
Overview
- Method-call semantics over HTTP using JSON-RPC 2.0.
When to use
- Simple RPC when gRPC is overkill and REST semantics don’t fit.
Reactive vs Virtual Threads
- Reactive: Non-blocking handler mapping JSON-RPC requests to Mono results.
- Virtual Threads: Blocking method handlers executed per request in virtual threads.
Mini-lab
- Implement `order.getById` method endpoint per JSON-RPC 2.0. Handle error object properly.
Acceptance
- Success and error payloads conform to JSON-RPC 2.0; id correlates request/response.
Gotchas
- Discoverability; versioning methods; batch requests handling.

---

## 8) Async Messaging APIs (AMQP/Kafka)
Overview
- Publish/subscribe or queues; commands/events via broker.
When to use
- Decoupling producers/consumers; buffering; eventual consistency; event-driven workflows.
Reactive vs Virtual Threads
- Reactive: reactor-kafka for non-blocking backpressure-aware consumption/production.
- Virtual Threads: Use regular blocking clients in virtual threads for simpler code.
Mini-lab
- Publish `order.created` per `docs/event-contracts/...` envelope; consume and log derived outcome.
Acceptance
- Message conforms to envelope schema; consumer is idempotent; retry/backoff strategy noted.
Gotchas
- Exactly-once is hard; schema evolution; dead-letter handling; ordering guarantees.

---

## 9) Batch/File-based APIs (CSV/S3/SFTP)
Overview
- Scheduled data exchanges for large datasets or backfills.
When to use
- Partners and legacy systems; data warehouse loads; nightly syncs.
Reactive vs Virtual Threads
- Reactive: Project Reactor + Async file I/O and non-blocking sinks; complex, only if very high throughput and backpressure are needed.
- Virtual Threads: Natural fit for file processing pipelines with blocking I/O; simple and scalable.
Mini-lab
- Parse a CSV upload; validate rows; emit events for valid rows; quarantine invalid rows.
Acceptance
- Clear summary metrics: rows processed/ok/errors; sample CSV included; error file produced.
Gotchas
- Idempotency across re-runs; PII handling; partial failures; schema drift.

---

## Decision Matrix (When to choose what)
- External public API: REST first; GraphQL if clients need flexible reads; webhooks for push.
- Internal microservice-to-microservice: gRPC for low-latency RPC; messaging for decoupling.
- Real-time UI: WebSockets (bi-directional) or SSE (server->client only).
- Concurrency model: Reactive for highest I/O scalability and streaming; Virtual Threads for simpler blocking code with great scalability on I/O-bound workloads.

---

## Labs Logistics
- For each style, keep a tiny branch/PR with: code, short README, and 3–5 bullets on trade-offs.
- Prefer tests demonstrating success criteria (e.g., JSON schema, HMAC signature, streaming message count).
- Add a short note in `docs/stories/` summarizing what you built and what you learned.

---

## Cross-links into this repo
- Projects & Modules overview: `docs/00-OVERVIEW/projects-and-modules.md`
- Event contracts and envelope: `docs/event-contracts/`
- Microservice patterns: `docs/03-DEVELOPMENT/microservices-patterns.md`
- Java Concurrency Track: `docs/stories/JAVA-CONCURRENCY-TRACK.md`
- Observability baseline: `docs/adr/ADR-007-observability-baseline.md`
- Retry/backoff: `docs/adr/ADR-006-retry-backoff-policies.md`

---

## Mentor Tips
- One style per session; demo in 5 minutes or less.
- Ask: What problem does this style solve? What’s a poor fit?
- Capture a short ADR if a style or concurrency choice impacts architecture.
