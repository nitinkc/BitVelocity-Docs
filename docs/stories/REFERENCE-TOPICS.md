# Reference Topics — Protocols & Concurrency

This document consolidates key learning topics that span multiple phases. Use this as a reference for protocol-specific labs and concurrency patterns.

## 1. API Protocols (Condensed from API Styles Track)

### 1.1 REST (HTTP/JSON)
- **Use Case**: External/public APIs, CRUD on resources.
- **Lab**: `POST /orders` & `GET /orders/{id}` with OpenAPI docs.
- **Key Patterns**: Idempotency, pagination, HATEOAS, 2xx/4xx status codes.

### 1.2 gRPC (Unary + Streaming)
- **Use Case**: Low-latency internal service-to-service RPC.
- **Lab**: `OrderService` with `GetOrder` (unary) & `ListOrderEvents` (server-streaming).
- **Key Patterns**: Protobuf contracts, codegen, deadlines, metadata.

### 1.3 GraphQL
- **Use Case**: Flexible client queries, aggregating multiple backends.
- **Lab**: `Query { orderById }` & `ordersByStatus`.
- **Key Patterns**: Resolvers, data loaders (N+1), mutations, subscriptions.

### 1.4 Webhooks (Callback APIs)
- **Use Case**: Notifying external systems of events (e.g., payments).
- **Lab**: `POST /webhooks/orders` with HMAC signature verification.
- **Key Patterns**: Idempotency keys, retries/backoff, signature validation.

### 1.5 WebSockets
- **Use Case**: Real-time, bi-directional communication (chat, live dashboards).
- **Lab**: Broadcast "order status changed" to connected clients.
- **Key Patterns**: Sub-protocols (STOMP), fan-out, session management.

### 1.6 Server-Sent Events (SSE)
- **Use Case**: Simpler real-time, server-to-client only (live feeds).
- **Lab**: `/orders/stream` endpoint streaming status updates.
- **Key Patterns**: `text/event-stream`, `Last-Event-ID`, heartbeats.

### 1.7 JSON-RPC
- **Use Case**: Simple RPC over HTTP when gRPC is overkill.
- **Lab**: `order.getById` method endpoint per JSON-RPC 2.0 spec.
- **Key Patterns**: Batch requests, error object structure.

### 1.8 Async Messaging (Kafka/AMQP)
- **Use Case**: Decoupling services, event-driven workflows, buffering.
- **Lab**: Publish `order.created` event; consume and log outcome.
- **Key Patterns**: Event envelope, schema registry, DLQ, idempotency.

### 1.9 Batch/File-based APIs
- **Use Case**: Large dataset exchange, legacy integrations, data loads.
- **Lab**: Parse a CSV upload, validate rows, quarantine failures.
- **Key Patterns**: Checksums, partial failure handling, idempotency.

---

## 2. Concurrency Patterns (Condensed from Java Concurrency Track)

### 2.1 Reactive Programming (Project Reactor)
- **When to Use**: High I/O concurrency, streaming data, backpressure is critical.
- **Scenario A: Reactive Checkout Pipeline**: End-to-end non-blocking flow using WebFlux, R2DBC, and `WebClient`.
- **Scenario B: Reactive Streaming Updates**: Fan-out real-time updates from a Kafka topic to many clients via SSE/WebSockets.
- **Key Operators**: `flatMap`, `zip`, `timeout`, `retryBackoff`, `onErrorResume`.
- **Testing**: Use `StepVerifier`.

### 2.2 Virtual Threads (Java 21 Loom)
- **When to Use**: I/O-bound tasks where thread-per-request simplicity is desired with high scalability.
- **Scenario C: Partner Webhook Dispatcher**: Fan-out blocking HTTP calls in virtual threads with rate limits.
- **Scenario D: Replay Service**: High-throughput batch processing from a file to Kafka.
- **Key Patterns**: `Executors.newVirtualThreadPerTaskExecutor()`, Structured Concurrency.
- **Gotchas**: Avoid pinning (e.g., `synchronized` blocks).

### 2.3 Comparison: Reactive vs. Virtual Threads
- **Goal**: Benchmark a simple I/O-heavy endpoint to inform architectural choices.
- **Recommendation**: Reactive for complex streaming and highest I/O scalability; Virtual Threads for simpler, scalable blocking I/O code.