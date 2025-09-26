# Phase 7 — Concurrency Scenarios (Reactive & Virtual Threads)

## 🏷️ Domain Focus
**Primary**: 🏪 **eCommerce** - Checkout and inventory streaming  
**Secondary**: 💬 **Chat** (SSE/WebSocket), 📱 **Social** (Feed streaming), 🏭 **IoT** (Batch processing)  
**Architecture Reference**: `01-ARCHITECTURE/domains/ecommerce/DOMAIN_ECOMMERCE_ARCHITECTURE.md`  
**Protocol Focus**: 🔄 **SSE/WebSocket** streaming, 🔗 **Webhooks**, ⚡ **Concurrency patterns**

## Objectives
- Implement A/B concurrency scenarios for learning and selection.
- **Learning Focus**: Modern Java concurrency - Reactive Programming vs Virtual Threads, WebSockets/SSE, Webhooks.

Deliverables
- Reactive checkout; streaming inventory; VT webhooks; VT replay
- A/B comparison notes and architectural decision

## Tasks (acceptance)
1) **Reactive Checkout (Scenario A - Reactive Learning)**
   - [ ] Non-blocking REST calls; R2DBC; resilience configured
   - [ ] Practice Reactor operators: flatMap, zip, timeout, retryBackoff
   - [ ] Use StepVerifier for testing reactive flows
   - [ ] Implement backpressure handling
   - [ ] **🏪 eCommerce Focus**: order-service + payment-adapter-service

2) **Streaming Inventory (Protocol Labs - SSE/WebSockets)**
   - [ ] SSE + WebSocket notifications; basic load test
   - [ ] Practice fan-out patterns with Flux.share()
   - [ ] Implement heartbeat and reconnection strategies
   - [ ] Add authentication and authorization for streams
   - [ ] **🏪 eCommerce**: inventory-service stock updates
   - [ ] **💬 Chat**: Real-time message delivery patterns
   - [ ] **📱 Social**: Live feed updates with SSE

3) **VT Webhooks (Scenario C - Virtual Threads Learning)**
   - [ ] VT executor; bounded concurrency; tracing
   - [ ] Practice structured concurrency patterns
   - [ ] Implement HMAC signature verification
   - [ ] Add per-partner rate limiting and bulkheads
   - [ ] **🏪 eCommerce**: partner-webhook-dispatcher service

4) **VT Replay (Scenario D - Batch Processing)**
   - [ ] Batch replay with backoff and checkpoints
   - [ ] Practice file processing with virtual threads
   - [ ] Implement resumable processing with state management
   - [ ] **🏭 IoT**: Batch telemetry processing
   - [ ] **🧠 ML/AI**: Feature batch processing
5) Protocol Integration
   - [ ] Webhook protocol lab: implement retry policies
   - [ ] WebSocket protocol lab: handle connection lifecycle
   - [ ] SSE protocol lab: implement Last-Event-ID support
6) Docs & Comparison
   - [ ] Document A/B comparison with performance metrics
   - [ ] Create ADR documenting concurrency model choice
   - [ ] Update protocol examples with lessons learned

Dependencies
- Phase 6

Learning & References
- Reference Topics (Protocols & Concurrency): `../REFERENCE-TOPICS.md`Next Phase: [Phase 8](./PHASE-8.md)