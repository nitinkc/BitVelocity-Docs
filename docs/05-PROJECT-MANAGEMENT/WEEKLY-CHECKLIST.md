# Weekly Progress Checklist

Use this checklist to track your weekly progress through the implementation roadmap.

---

## Week 1: Foundation Setup ✅

### Infrastructure
- [ ] Docker Desktop installed and running
- [ ] Java 17 (Temurin) installed
- [ ] Maven installed and configured
- [ ] Local infra running (Postgres, Redis, Kafka)

### Product Service
- [ ] Product entity created
- [ ] ProductRepository implemented
- [ ] REST API endpoints working (CRUD)
- [ ] Validation added
- [ ] Unit tests passing
- [ ] Integration tests with Testcontainers
- [ ] OpenAPI/Swagger documentation accessible

### Validation Checks
- [ ] `docker ps` shows 3 containers
- [ ] `mvn test` passes
- [ ] Can create/read/update/delete products via Postman
- [ ] Swagger UI accessible at localhost:8080/swagger-ui.html
- [ ] Test coverage > 80%

### Learning Verification
- [ ] Understand Spring Boot basics
- [ ] Can explain JPA entities and repositories
- [ ] Know how to write REST controllers
- [ ] Comfortable with Docker Compose
- [ ] Can write unit and integration tests

---

## Week 2: Cart Service + Basic Observability ✅

### Cart Service
- [ ] Cart and CartItem entities created
- [ ] Cart CRUD operations working
- [ ] Redis caching implemented
- [ ] Cart-to-Product integration working

### Observability
- [ ] Actuator endpoints enabled
- [ ] Health checks working
- [ ] Structured JSON logging configured
- [ ] Prometheus metrics exposed
- [ ] Custom business metrics added

### Integration
- [ ] Cart service calls Product service
- [ ] Retry logic implemented (Spring Retry)
- [ ] Error handling graceful

### Validation Checks
- [ ] Can add product to cart
- [ ] Cache hit/miss logged correctly
- [ ] `/actuator/health` returns 200
- [ ] `/actuator/prometheus` shows metrics
- [ ] Retry happens on failure

### Learning Verification
- [ ] Understand service-to-service communication
- [ ] Know Redis caching strategies
- [ ] Can configure Actuator
- [ ] Understand structured logging
- [ ] Can create custom metrics

---

## Week 3: Kafka Integration ✅

### Kafka Setup
- [ ] Kafka (Redpanda) verified running
- [ ] Topics created
- [ ] Can produce/consume manually

### Event Publishing
- [ ] ProductCreatedEvent defined
- [ ] Event published on product creation
- [ ] Event visible in Kafka topic

### Event Consumption
- [ ] Inventory service created
- [ ] Consumes ProductCreatedEvent
- [ ] Creates inventory record

### Event Patterns
- [ ] Event schemas defined
- [ ] Schema validation implemented
- [ ] Idempotency keys added
- [ ] Duplicate detection working

### Validation Checks
- [ ] Event appears in Kafka after product creation
- [ ] Inventory record created on event
- [ ] Duplicate events don't create duplicates
- [ ] Schema violations fail

### Learning Verification
- [ ] Understand Kafka concepts (topics, partitions, offsets)
- [ ] Know event-driven architecture
- [ ] Can implement idempotency
- [ ] Understand schema evolution

---

## Week 4: Order Service + Saga ✅

### Order Service
- [ ] Order and OrderItem entities
- [ ] State machine implemented (PENDING → CONFIRMED → PAID → FULFILLED)
- [ ] State transitions validated

### Saga Pattern
- [ ] Inventory reservation working
- [ ] Order creation saga functional
- [ ] OrderCreatedEvent published

### Compensation
- [ ] Rollback on inventory failure
- [ ] Compensation events published
- [ ] Saga timeout configured
- [ ] Resource cleanup working

### Validation Checks
- [ ] Happy path: order created successfully
- [ ] Failure path: inventory released on failure
- [ ] Saga times out after 30 seconds
- [ ] End-to-end order flow works

### Learning Verification
- [ ] Understand saga pattern
- [ ] Know compensation logic
- [ ] Can implement state machines
- [ ] Understand eventual consistency

---

## Week 5: OpenTelemetry & Tracing ✅

### Distributed Tracing
- [ ] OpenTelemetry agent added
- [ ] OTLP exporter configured
- [ ] Jaeger deployed locally
- [ ] Traces visible in Jaeger

### Custom Spans
- [ ] Business operation spans added
- [ ] Span attributes configured
- [ ] Spans linked across services

### Prometheus + Grafana
- [ ] Observability stack deployed
- [ ] Prometheus scraping metrics
- [ ] Grafana accessible
- [ ] First dashboard created

### Alerting
- [ ] Alert rules applied
- [ ] Alertmanager configured
- [ ] Test alert fired

### Validation Checks
- [ ] Traces visible in Jaeger UI
- [ ] Spans have business context
- [ ] Grafana shows live metrics
- [ ] Alert fires on simulated error

### Learning Verification
- [ ] Understand distributed tracing
- [ ] Can create custom spans
- [ ] Know Prometheus/Grafana basics
- [ ] Can configure alerts

---

## Week 6: Advanced Observability ✅

### SLO Monitoring
- [ ] SLOs defined in yaml
- [ ] SLO dashboard created
- [ ] Error budget tracked

### Latency Monitoring
- [ ] Histogram buckets configured
- [ ] Latency distribution graphs
- [ ] p95/p99 alerts set

### Log Aggregation (Optional)
- [ ] Loki installed
- [ ] Logs searchable

### Correlation
- [ ] Trace IDs in logs
- [ ] Logs linked to traces
- [ ] Exemplars in metrics

### Validation Checks
- [ ] SLO compliance visible
- [ ] Latency percentiles charted
- [ ] Can jump from metric → trace → logs

### Learning Verification
- [ ] Understand SLI/SLO/SLA
- [ ] Know percentiles and histograms
- [ ] Can correlate signals

---

## Week 7: Resilience Patterns ✅

### Resilience4j
- [ ] Circuit breaker configured
- [ ] Circuit opens on failures
- [ ] Retry with exponential backoff
- [ ] Jitter added

### Bulkhead & Rate Limiting
- [ ] Thread pool isolation
- [ ] Rate limiter on APIs
- [ ] 429 responses returned

### Fallbacks
- [ ] Fallback to cached data
- [ ] Default responses
- [ ] Graceful degradation

### Validation Checks
- [ ] Circuit breaker opens/closes correctly
- [ ] Retries visible in logs
- [ ] Rate limiting throttles requests
- [ ] Fallbacks work

### Learning Verification
- [ ] Understand circuit breaker pattern
- [ ] Know retry strategies
- [ ] Can implement bulkheads
- [ ] Understand graceful degradation

---

## Week 8: Chaos Engineering ✅

### Chaos Mesh
- [ ] Chaos Mesh installed
- [ ] Dashboard accessible

### Experiments
- [ ] Pod kill experiment run
- [ ] Network latency injected
- [ ] Packet loss tested

### Game Day
- [ ] Runbook completed
- [ ] Roles assigned
- [ ] Game day scheduled

### Validation Checks
- [ ] Service recovers from pod kill
- [ ] Circuit breaker activates on latency
- [ ] No cascading failures
- [ ] Metrics show impact

### Learning Verification
- [ ] Understand chaos engineering principles
- [ ] Can run chaos experiments
- [ ] Know how to document findings
- [ ] Can prepare runbooks

---

## Week 9: Load Testing Setup ✅

### Gatling
- [ ] Gatling project built
- [ ] Order flow simulation created
- [ ] Load profile configured

### Baseline
- [ ] Baseline tests run
- [ ] Latencies recorded
- [ ] Bottlenecks identified

### k6 CI
- [ ] k6 smoke test created
- [ ] Added to GitHub Actions
- [ ] Performance gates set

### Validation Checks
- [ ] Gatling simulation runs successfully
- [ ] Baseline in sli-targets.yaml
- [ ] k6 runs in CI
- [ ] Bottlenecks documented

### Learning Verification
- [ ] Understand load testing concepts
- [ ] Can model realistic user behavior
- [ ] Know how to analyze results
- [ ] Can identify bottlenecks

---

## Week 10: Performance Optimization ✅

### Database
- [ ] Indexes added
- [ ] Connection pool tuned
- [ ] Query times improved 50%+

### Caching
- [ ] Cache warming implemented
- [ ] Cache hit ratio > 80%
- [ ] HTTP caching (ETags)

### Validation
- [ ] Re-run load tests
- [ ] 30%+ improvement in p95
- [ ] Improvements documented

### Validation Checks
- [ ] No connection timeouts
- [ ] Cache hit ratio metric
- [ ] 304 responses for unchanged resources
- [ ] Performance improved

### Learning Verification
- [ ] Understand database optimization
- [ ] Know caching strategies
- [ ] Can measure performance improvements
- [ ] Understand HTTP caching

---

## Week 11: Security Hardening ✅

### Dependency Scanning
- [ ] OWASP Dependency Check run
- [ ] Vulnerable deps updated
- [ ] No CRITICAL CVEs

### Container Security
- [ ] Trivy scan clean
- [ ] Distroless images used

### Authentication
- [ ] JWT generation working
- [ ] JWT validation filter
- [ ] Endpoints secured

### OWASP ZAP
- [ ] ZAP baseline scan run
- [ ] Issues fixed
- [ ] No HIGH findings

### Secrets
- [ ] Secrets externalized
- [ ] No secrets in code

### Validation Checks
- [ ] Protected endpoints require auth
- [ ] Clean security scans
- [ ] Secrets in env vars

### Learning Verification
- [ ] Understand common vulnerabilities
- [ ] Know JWT authentication
- [ ] Can use security scanning tools
- [ ] Understand secrets management

---

## Week 12: CI/CD Pipeline ✅

### Build Pipeline
- [ ] GitHub Actions configured
- [ ] Build runs on push
- [ ] Dependencies cached

### Quality Gates
- [ ] Test coverage gate (80%)
- [ ] Security scan gate
- [ ] Build fails on issues

### Contract Testing
- [ ] Pact consumer tests
- [ ] Contracts published
- [ ] Contract stage in CI

### Docker
- [ ] Images built in CI
- [ ] Tagged with SHA
- [ ] Pushed to registry

### Validation Checks
- [ ] Build passes on push
- [ ] Poor quality blocked
- [ ] Contract breaking changes detected
- [ ] Images available in registry

### Learning Verification
- [ ] Understand CI/CD concepts
- [ ] Know quality gates
- [ ] Can write contract tests
- [ ] Understand image tagging

---

## Week 13: gRPC Implementation ✅

### gRPC Service
- [ ] Proto file defined
- [ ] Java code generated
- [ ] gRPC service implemented
- [ ] gRPC server running

### gRPC Client
- [ ] Client stub created
- [ ] Order service calls inventory
- [ ] gRPC errors handled

### Observability
- [ ] gRPC interceptors added
- [ ] gRPC metrics exposed
- [ ] Traces in Jaeger

### Resilience
- [ ] Retry policy configured
- [ ] Deadlines set
- [ ] Failure scenarios tested

### Validation Checks
- [ ] gRPC calls work via grpcurl
- [ ] Order → Inventory via gRPC
- [ ] Traces show gRPC spans
- [ ] Retries work

### Learning Verification
- [ ] Understand Protocol Buffers
- [ ] Know gRPC vs REST tradeoffs
- [ ] Can implement gRPC services
- [ ] Understand gRPC error handling

---

## Week 14: WebSocket & SSE ✅

### WebSocket
- [ ] Notification service created
- [ ] WebSocket endpoint configured
- [ ] Order status notifications
- [ ] JWT authentication

### SSE
- [ ] SSE endpoint for flash sales
- [ ] Events broadcast
- [ ] Reconnection handled

### Load Testing
- [ ] Gatling WebSocket scenario
- [ ] 1000+ concurrent connections
- [ ] Resource usage monitored

### Validation Checks
- [ ] WebSocket connection works
- [ ] Real-time updates received
- [ ] SSE events delivered
- [ ] Handle 1000+ connections

### Learning Verification
- [ ] Understand WebSocket protocol
- [ ] Know SSE use cases
- [ ] Can secure WebSocket
- [ ] Understand real-time scaling

---

## Week 15: CDC & Projections ✅

### Debezium
- [ ] Debezium connector deployed
- [ ] CDC for orders table
- [ ] CDC events in Kafka

### Projections
- [ ] orders_by_customer projection
- [ ] Built from CDC events
- [ ] Stored in database

### Kafka Streams
- [ ] Streams app created
- [ ] Order totals aggregated
- [ ] Windowing configured

### Replay
- [ ] Replay service skeleton
- [ ] Events replayed
- [ ] Projection validated

### Validation Checks
- [ ] DB changes in Kafka
- [ ] Projection stays in sync
- [ ] Aggregations correct
- [ ] Replay rebuilds projection

### Learning Verification
- [ ] Understand CDC concepts
- [ ] Know projection patterns
- [ ] Can use Kafka Streams
- [ ] Understand event replay

---

## Week 16: Production Readiness ✅

### Documentation
- [ ] READMEs updated
- [ ] ADRs documented
- [ ] Runbooks created

### Kubernetes
- [ ] Helm charts created
- [ ] Resource limits set
- [ ] Health probes added
- [ ] Services deployed to K8s

### Game Day
- [ ] Multi-service chaos
- [ ] Team participated
- [ ] Incident documented

### Retrospective
- [ ] Retro completed
- [ ] Learnings documented
- [ ] Phase 2 planned

### Validation Checks
- [ ] Docs peer-reviewed
- [ ] Services run in K8s
- [ ] Chaos handled gracefully
- [ ] Retro notes captured

### Learning Verification
- [ ] Can document architecture
- [ ] Understand Kubernetes
- [ ] Know incident management
- [ ] Can reflect and improve

---

## Progress Tracking

| Week | Status | Completion % | Notes |
|------|--------|--------------|-------|
| 1 | 🔄 | 0% | |
| 2 | ⏳ | 0% | |
| 3 | ⏳ | 0% | |
| 4 | ⏳ | 0% | |
| 5 | ⏳ | 0% | |
| 6 | ⏳ | 0% | |
| 7 | ⏳ | 0% | |
| 8 | ⏳ | 0% | |
| 9 | ⏳ | 0% | |
| 10 | ⏳ | 0% | |
| 11 | ⏳ | 0% | |
| 12 | ⏳ | 0% | |
| 13 | ⏳ | 0% | |
| 14 | ⏳ | 0% | |
| 15 | ⏳ | 0% | |
| 16 | ⏳ | 0% | |

Legend: ✅ Complete | 🔄 In Progress | ⏳ Not Started

---

## Tips for Success

1. **Check off items as you complete them** - Immediate satisfaction!
2. **Don't skip validation checks** - They ensure quality
3. **Learning verification is crucial** - Understanding > completion
4. **Document blockers in notes** - Help for troubleshooting
5. **Celebrate weekly wins** - Track your progress!

---

## When Blocked

- [ ] Checked existing documentation
- [ ] Searched official docs
- [ ] Reviewed similar code
- [ ] Asked AI assistant with context
- [ ] Took break and came back
- [ ] Documented issue for help

---

**Start Date**: __________  
**Target Completion**: __________  
**Actual Completion**: __________
