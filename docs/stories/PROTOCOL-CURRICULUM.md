# Protocol Curriculum — 12 Sprints (Protocol-first)

Purpose: A practical sequence for learning and applying API protocols in this repo. Each sprint allocates capacity to learn 1 protocol (mini‑lab) and apply it in a mapped service. Mix and match based on goals.

Format per sprint
- Learn: 2h focused study + mini-lab from API Styles / Concurrency Tracks
- Build: 1–3 small feature PRs in a mapped service
- Reflect: short note in `docs/stories/` + ADR if decisions made

Legend: (R) = Reactive option; (VT) = Virtual Threads option

---

## Sprints 1–12 sequence

1) REST (HTTP/JSON)
- Why first: universal baseline for external/public APIs.
- Lab: POST/GET in product-service or order-service; OpenAPI; 2xx/4xx behaviors.
- (R) WebFlux + WebClient; (VT) MVC + JDK HttpClient

2) Messaging (Kafka)
- Why now: backbone for event-driven eCommerce.
- Lab: Publish order.created; consume in inventory-service; idempotency + DLQ note.
- (R) reactor-kafka; (VT) blocking clients in virtual threads

3) Webhooks
- Why now: partner integrations and push notifications.
- Lab: partner-webhook-dispatcher with HMAC verification; retries and rate limits.
- (VT) natural fit; (R) good for burst handling

4) gRPC
- Why now: fast internal service calls.
- Lab: order/inventory unary + streaming methods; basic error mapping.
- (R) Reactor wrappers; (VT) blocking stubs with virtual threads

5) GraphQL
- Why now: flexible read aggregation for BFF.
- Lab: Query orderById and ordersByStatus; handle N+1 with batching.
- (R) Mono/Flux resolvers; (VT) blocking resolvers on virtual threads

6) SSE
- Why now: simpler server→client updates than WebSockets.
- Lab: inventory stream endpoint; reconnect/heartbeats.
- (R) Flux<Event>; (VT) ResponseBodyEmitter

7) WebSockets
- Why now: bi-directional realtime (chat, live dashboards).
- Lab: live order status or chat echo; session/auth basics.
- (R) WebFlux WebSocket; (VT) STOMP handlers in virtual threads

8) JSON-RPC
- Why now: simple RPC when REST semantics don’t fit.
- Lab: order.getById with JSON-RPC 2.0 success/error patterns.
- (R) Mono pipeline; (VT) blocking handler

9) Batch/File APIs
- Why now: partner/legacy backfills and replay.
- Lab: replay-service CSV → Kafka with dedupe and throttling.
- (VT) recommended; (R) only if heavy backpressure is needed

10) Advanced Messaging
- Why now: reinforce event patterns.
- Lab: exactly-once-ish semantics, idempotent consumer, DLQ processing, replay script.
- (R) reactor-kafka; (VT) virtual-thread consumers

11) Protocol Integration + Observability
- Why now: end-to-end tracing/metrics/logs.
- Lab: instrument REST↔Messaging path; add span attributes; capture screenshots.

12) Compare & Decide
- Why now: codify defaults for services.
- Lab: benchmark a small I/O flow using Reactive vs Virtual Threads; write an ADR.

---

## Mappings (services per protocol)
- REST → product-service, cart-service, order-service
- Messaging → order-service, inventory-service, analytics-streaming-service
- Webhooks → partner-webhook-dispatcher
- gRPC → internal order/inventory boundaries
- GraphQL → BFF aggregations
- SSE/WebSockets → notification-service, inventory-service
- Batch → replay-service

---

## Acceptance checklist per sprint
- Mini-lab PR merged (code + README + tests)
- Short story note in `docs/stories/` linking PRs and what was learned
- If a choice impacts architecture, add/extend an ADR

---

## Links
- Developer Learning Plan: `./DEVELOPER-LEARNING-PLAN.md`
- API Styles Track: `./API-STYLES-TRACK.md`
- Java Concurrency Track: `./JAVA-CONCURRENCY-TRACK.md`
- Projects & Modules: `../00-OVERVIEW/projects-and-modules.md`
- Sprint Planning: `../05-PROJECT-MANAGEMENT/sprint-planning.md`
