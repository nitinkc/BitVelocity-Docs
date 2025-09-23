# Developer Learning Plan (6–8 weeks)

Audience: Any backend developer (junior to mid) comfortable with Spring Boot who wants to learn modern event-driven architecture, API protocols, and observability using this repo as the curriculum.

Mentorship mode: Weekly 45–60 min 1:1 (or self-guided checkpoints), async code/doc reviews, and a short pair session per week on the current milestone.

Success criteria: Design, implement, observe, and document a small event-driven workflow end-to-end; contribute at least one ADR and one contract update to this repo.

Protocol-first note: Each phase highlights the protocol(s) in focus (REST, gRPC, GraphQL, Webhooks, WebSockets, SSE, JSON-RPC, Messaging, Batch). Start by learning the protocol, then implement it in one of the mapped services.

---

## How to use this plan
- Follow phases in order; use optional tracks to go deeper without overload.
- Each phase provides readings, hands-on tasks, deliverables with acceptance criteria, and review points.
- Keep deliverables small (1–3 files/PR, <300 lines) to reduce review load.
- Track work as issues and link PRs. Prefer conventional commits.

---

## Phase 0 — Orientation (0.5 week)
Same as the original plan (repo overview, quick fix PR, conventions).

---

## Phase 1 — Architecture Big Picture (0.5–1 week)
Same as the original plan (system/data views, pick a domain and key events).

---

## Phase 1.5 — API Styles Overview (optional, 1–2 weeks)
- Read: [API Styles Track](./API-STYLES-TRACK.md)
- Protocol-first: choose 3–5 protocols most relevant to your domain now; finish the rest later.
- Deliverable: tiny branch/PR + README + acceptance checks per style.

---

## Phase 1.6 — Java Concurrency Track (optional, 1–2 weeks)
- Read: [Java Concurrency Track](./JAVA-CONCURRENCY-TRACK.md)
- Choose one Reactive scenario and one Virtual Threads scenario.
- Deliverable: small lab repo/branch with tests and a short doc.

---

## Phase 2 — Decision Records (ADRs) (0.5 week)
- Read: `adr/GUIDE_ADR_STARTER_PACK.md`, `adr/ADR-TEMPLATE.md`
- Hands-on: Write a focused ADR (e.g., choosing local event broker).
- Deliverable: New ADR PR with Consequences and Alternatives.

---

## Phase 3 — Event Contracts (1 week)
- Read: `event-contracts/GUIDE_EVENT_CONTRACTS_USAGE.md`, `schema/envelope.schema.json`
- Hands-on: Propose a new event contract and example.
- Deliverable: Contract PR follows envelope and versioning guidance.

---

## Phase 4 — Minimal Service + Patterns (1–1.5 weeks)
- Read: `03-DEVELOPMENT/microservices-patterns.md`
- Hands-on: Implement either a producer (REST→event) or consumer (event→handler) with tests.
- Deliverable: Repo link + README + mapping tests.

---

## Phase 5 — Observability (0.5–1 week)
- Read: `adr/ADR-007-observability-baseline.md`, `opentelemetry.properties`
- Hands-on: Add OTel; show traces across HTTP↔event path.
- Deliverable: Screenshots/logs + short story note.

---

## Phase 6 — Cross Topics Deep Dive (0.5–1 week)
Pick one: versioning, replay, observability testing, cost, analytics. Produce a short before/after note.

---

## Phase 7 — Infrastructure Awareness (0.5 week)
Summarize required prod resources for your service (topics, queues, secrets, dashboards).

---

## Phase 8 — Capstone E2E Demo (1 week)
Produce an end-to-end demo, one ADR reflection (e.g., retry/backoff), and link to your contract + service repo.

---

## Weekly Sprint Cadence (learn + build)
- Capacity planning per sprint (example):
  - 70% build (features/tests/docs)
  - 20% protocol learning (pick 1 protocol, 2 hr focused study + mini-lab)
  - 10% reflection/documentation (notes, ADRs, diagrams)
- Template per sprint:
  - Week 1: learn 1 protocol (from API Styles track), apply in a tiny lab; build small feature.
  - Week 2: integrate the protocol into a service scenario; measure and document.
- Definition of Done extended: includes “protocol note added to `docs/stories/` with links to PRs.”

---

## Protocol-first quick router
- REST → product-service, cart-service, order-service
- gRPC → internal service boundaries (order/inventory)
- GraphQL → aggregating reads across services (BFF)
- Webhooks → partner-webhook-dispatcher
- WebSockets/SSE → notification-service, inventory-service
- Messaging (Kafka) → order-service, inventory-service, analytics-streaming-service
- Batch → replay-service

---

## Link map (quick access)
- Projects & Modules: `../00-OVERVIEW/projects-and-modules.md`
- API Styles Track: `./API-STYLES-TRACK.md`
- Java Concurrency Track: `./JAVA-CONCURRENCY-TRACK.md`
- Event Contracts: `../event-contracts/`
- ADRs: `../adr/`
- Patterns: `../03-DEVELOPMENT/microservices-patterns.md`

---

## Notes
- The plan scales for junior to mid-level; depth comes from how many optional tracks you tackle.
- Prefer shipping small, reviewable artifacts every few days.
