# Projects and Modules Overview

This page lists the main projects and modules in the BitVelocity workspace and maps them to domains and learning scenarios.

**Last Updated**: December 29, 2025

---

## Core Shared Libraries

### bv-core-common
Shared libraries used across all domains and services.

**Modules**:

- `bv-common-auth` — Authentication helpers, JWT utilities, security models
- `bv-common-entities` — Shared domain entities, DTOs, value objects
- `bv-common-events` — Event types, envelopes, event utilities, serialization
- `bv-common-exceptions` — Standard exception hierarchy, error contracts, error codes
- `bv-common-logging` — Structured logging utilities, JSON logger configuration
- `bv-common-security` — Security helpers (filters, encryption, policy enforcement)

**Usage**: Import specific modules into services to avoid code duplication. All services should depend on relevant common modules.

**Documentation**: See [Event Contracts](../01-ARCHITECTURE/CROSS_EVENT_CONTRACTS_AND_VERSIONING.md) for event patterns.

---

## Build & Dependency Management

### bv-core-platform-bom
Bill of Materials (BOM) - centralized version management for all dependencies.

**Purpose**: Controls versions of Spring Boot, Kafka, database drivers, testing libraries, etc.

**Usage**: Imported by `bv-core-parent` and referenced by all service modules.

**Location**: Root of dependency hierarchy.

### bv-core-parent
Maven parent POM with shared build configuration.

**Purpose**: Defines plugins, compiler settings, build profiles.

**Usage**: Set as `<parent>` in all service POMs to inherit configuration.

**Dependency**: Imports `bv-core-platform-bom` for version management.

**Hierarchy**: `bv-core-platform-bom` → `bv-core-parent` → `<domain-services>`

---

## eCommerce Domain Services

**Location**: `bv-eCommerce-core/`

### Core Transactional Services
- **product-service** — Product catalog, SKU management, product metadata
- **cart-service** — Shopping cart lifecycle, cart item management
- **order-service** — Order creation, state management, order lifecycle (create/approve/fulfill/cancel/refund)
- **inventory-service** — Stock levels, inventory reservations, stock adjustments
- **pricing-service** — Dynamic pricing, fare calculation, discount rules

### Payment & Financial
- **payment-adapter-service** — Payment gateway integration, payment orchestration

### Customer Engagement
- **notification-service** — Multi-channel notifications (email, SMS, push, in-app)
- **partner-webhook-dispatcher** — Webhook fan-out to external partners, retry logic

### Analytics & Data
- **analytics-streaming-service** — Real-time analytics ingestion, stream processing

### Operational Services
- **replay-service** — Event replay for backfills, testing, and recovery

**Architecture Reference**: [eCommerce Domain Architecture](../01-ARCHITECTURE/domains/ecommerce/DOMAIN_ECOMMERCE_ARCHITECTURE.md)

**Learning Tracks**:

- **REST APIs**: product-service, cart-service, order-service
- **Event-Driven**: order-service → inventory-service → notification-service
- **Webhooks**: partner-webhook-dispatcher
- **Streaming**: analytics-streaming-service
- **Saga Pattern**: order-service (orchestrates payment, inventory, notifications)

---

## Authentication & Security

### bv-auth-service
Central authentication and authorization service.

**Responsibilities**: JWT token generation, user authentication, RBAC, OAuth2 integration

**Technology**: Spring Security, JWT, Redis (token cache)

**Documentation**: [Security Architecture](../01-ARCHITECTURE/security-architecture.md), [ADR-005: Security Layering](../adr/ADR-005-security-layering.md)

### bv-security-core
Security library for cross-cutting security concerns.

**Responsibilities**: Encryption utilities, security filters, policy evaluation

**Usage**: Shared by all services requiring security capabilities

### bv-security-testing
Security testing tools and configurations.

**Contents**: OWASP ZAP configurations, penetration test scenarios, security scan scripts

**Documentation**: See `bv-security-testing/README.md` (external module)

---

## Other Domain Services

### bv-chat-stream
Real-time chat and messaging services.

**Capabilities**: WebSocket connections, message streaming, chat room management

**Architecture Reference**: [Chat Domain Architecture](../01-ARCHITECTURE/domains/chat/DOMAIN_CHAT_ARCHITECTURE.md)

**Learning Focus**: WebSocket, Server-Sent Events (SSE), real-time data streaming

### bv-iot-control-hub
IoT device management and control center.

**Capabilities**: Device registration, telemetry ingestion, command dispatch, MQTT

**Architecture Reference**: [IoT Domain Architecture](../01-ARCHITECTURE/domains/iot/DOMAIN_IOT_ARCHITECTURE.md)

**Learning Focus**: MQTT protocol, high-volume data ingestion, device management

### bv-social-pulse
Social media-like features and activity feeds.

**Capabilities**: User feeds, posts, likes, follows, activity streams

**Architecture Reference**: [Social Domain Architecture](../01-ARCHITECTURE/domains/social/DOMAIN_SOCIAL_ARCHITECTURE.md)

**Learning Focus**: Activity streams, fan-out patterns, social graphs

---

## Infrastructure & DevOps

### bv-infra-service
Infrastructure-as-code and cloud resource management.

**Technology**: Pulumi (Java), Gradle

**Capabilities**: Multi-cloud provisioning (AWS, GCP, Azure), policy-as-code, secret management

**Documentation**: See `bv-infra-service/README.md` (external module), [ADR-008: Pulumi Cloud Provider Abstraction](../adr/ADR-008-pulumi-cloud-provider-abstraction.md)

---

## Quality Assurance & Observability

### bv-performance-testing
Load testing and performance benchmarking.

**Tools**: Gatling (Java), k6

**Contents**: Load test scenarios, performance baselines (SLI targets), stress tests

**Documentation**: See `bv-performance-testing/README.md` (external module), [ADR-015: Load Testing Strategy](../adr/ADR-015-load-testing-strategy.md), [Performance Testing Guide](../03-DEVELOPMENT/performance-testing-guide.md)

### bv-chaos-experiments
Chaos engineering experiments and game day runbooks.

**Tool**: Chaos Mesh

**Contents**: Pod failure experiments, network chaos, resource stress tests, game day procedures

**Documentation**: See `bv-chaos-experiments/README.md` (external module), [ADR-016: Chaos Engineering Framework](../adr/ADR-016-chaos-engineering-framework.md)

### bv-observability
Centralized observability stack configuration.

**Components**: OpenTelemetry Collector, Prometheus, Grafana, Jaeger

**Contents**: Collector configs, alert rules, dashboards, instrumentation examples

**Documentation**: See `bv-observability/README.md` (external module), [Observability & Testing](../01-ARCHITECTURE/CROSS_OBSERVABILITY_AND_TESTING.md), [ADR-007: Observability Baseline](../adr/ADR-007-observability-baseline.md)

---

## Configuration & Scripts

### config/
Shared configuration artifacts (e.g., resilience4j configs, feature flags).

### k8s/
Kubernetes manifests for local development (Postgres, Redis, Kafka/Redpanda, services).

### scripts/
Development and operational scripts.

**Structure**:

- `scripts/dev/` — Local development helpers
- `scripts/db/` — Database migration and seed scripts
- `scripts/cost/` — Cloud cost analysis and optimization tools

**Conventions**: See `scripts/README.md` (external module) for scripting standards.

---

## Documentation Cross-References

### Domain Architectures
- **eCommerce**: [eCommerce Domain Architecture](../01-ARCHITECTURE/domains/ecommerce/DOMAIN_ECOMMERCE_ARCHITECTURE.md)
- **Chat**: [Chat Domain Architecture](../01-ARCHITECTURE/domains/chat/DOMAIN_CHAT_ARCHITECTURE.md)
- **IoT**: [IoT Domain Architecture](../01-ARCHITECTURE/domains/iot/DOMAIN_IOT_ARCHITECTURE.md)
- **Social**: [Social Domain Architecture](../01-ARCHITECTURE/domains/social/DOMAIN_SOCIAL_ARCHITECTURE.md)
- **ML/AI**: [ML/AI Domain Architecture](../01-ARCHITECTURE/domains/ml-ai/DOMAIN_ML_AI_ARCHITECTURE.md)

### Key ADRs
- **ADR-001**: Multi-repo vs Monorepo
- **ADR-002**: Event vs CDC Strategy
- **ADR-003**: Protocol Introduction Order
- **ADR-005**: Security Layering
- **ADR-007**: Observability Baseline
- **ADR-015**: Load Testing Strategy
- **ADR-016**: Chaos Engineering Framework
- **ADR-017**: CI/CD Pipeline Architecture

---

## Learning Paths

### REST API Development

**Modules**: product-service, cart-service, order-service

**Skills**: Spring Boot REST, OpenAPI/Swagger, validation, pagination

### Event-Driven Architecture

**Modules**: order-service, inventory-service, notification-service, analytics-streaming-service

**Skills**: Kafka, event sourcing, CQRS, saga pattern, event schemas

### Real-Time Communication

**Modules**: notification-service, chat-stream, iot-control-hub

**Skills**: WebSocket, SSE, MQTT, pub/sub patterns

### Resilience & Chaos Engineering

**Modules**: All services (resilience), bv-chaos-experiments

**Skills**: Circuit breakers, retries, bulkheads, chaos testing, game days

### Observability

**Modules**: bv-observability + all services

**Skills**: OpenTelemetry, distributed tracing, metrics, logging, dashboards, SLOs

### Performance Engineering

**Modules**: bv-performance-testing + target services

**Skills**: Load testing, performance tuning, profiling, capacity planning

### Security

**Modules**: bv-auth-service, bv-security-core, bv-security-testing

**Skills**: JWT, OAuth2, RBAC, encryption, security scanning, OWASP Top 10

---

## Module Status Legend

✅ **Active** — Currently in development/use
🚧 **In Progress** — Under active development
📋 **Planned** — Design complete, implementation pending
💡 **Concept** — Exploratory phase

Current status as of December 29, 2025: All listed modules are **Active** ✅

- Execution Phases: [Phase Guides](../stories/phases/) (Phase 0-9 with integrated learning)
- Protocol & Concurrency Reference: [Reference Topics](../stories/REFERENCE-TOPICS.md) (9 API protocols + concurrency patterns)
- Quick Start Guide: [Quick Start](../stories/QUICK-START.md)

Tip: For each phase, follow the protocol labs embedded within execution tasks and document your progress with PRs linked to phase checklists.
