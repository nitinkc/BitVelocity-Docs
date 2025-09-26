# Projects and Modules Overview

This page lists the main projects and modules in the BitVelocity workspace and maps them to domains and learning scenarios.

Updated: 2025-09-22

---

## Platform Foundations

- bv-core-parent
  - Type: Maven parent (manages shared build/config)
  - Use: Set as parent for service modules to inherit dependency management and plugin config.

- bv-core-platform-bom
  - Type: Bill of Materials (BOM)
  - Use: Centralize dependency versions across services.

- bv-core-common
  - Modules:
    - bv-common-auth — Shared auth helpers/models
    - bv-common-entities — Shared domain entities/DTOs
    - bv-common-events — Shared event types/envelopes/utilities
    - bv-common-exceptions — Standard exception hierarchy and error contracts
    - bv-common-logging — Logging utilities and json logger setup
    - bv-common-security — Security helpers (e.g., filters/keys/policies)
  - Use: Import as needed into services to avoid duplication.

---

## Core Services (eCommerce Domain)

Path: `bv-eCommerce-core/`

- analytics-streaming-service — Real-time analytics ingest/transform
- cart-service — Cart lifecycle and item management
- inventory-service — Stock levels and adjustments
- notification-service — Customer notifications and partner messages
- order-service — Order lifecycle (create/approve/cancel/refund)
- partner-webhook-dispatcher — Fan-out callbacks to partners (webhooks)
- payment-adapter-service — Outbound payment integration(s)
- pricing-service — Price/fare calculation
- product-service — Catalog/product data
- replay-service — Replaying historical events (backfills/testing)

Suggested mappings to learning tracks:
- API Styles Track
  - REST: product-service, cart-service, order-service
  - Webhooks: partner-webhook-dispatcher
  - Messaging: order-service, inventory-service, analytics-streaming-service
  - SSE/WebSockets: notification-service, inventory-service
- Java Concurrency Track
  - Reactive A (Checkout): order-service + payment-adapter-service
  - Reactive B (Streaming): inventory-service + notification-service
  - Virtual Threads C (Webhooks): partner-webhook-dispatcher
  - Virtual Threads D (Replay): replay-service

---

## Auth and Security

- bv-auth-service — Authentication/authorization boundary (maps to cross-cutting security ADRs)
- bv-security-core — Security library/components used by services

---

## Observability/Infra/Control

- bv-infra-service — Infra-facing utilities or orchestration entry points (align with ADR-008)
- bv-iot-control-hub — IoT domain control center (align with `docs/01-ARCHITECTURE/domains/iot`)

---

## Social/Chat

- bv-social-pulse — Social domain service(s) (align with `docs/01-ARCHITECTURE/domains/social`)
- bv-chat-stream — Chat/stream processing (align with `docs/01-ARCHITECTURE/domains/chat`)

---

## Environment and Config

- config/ — Shared configuration artifacts
- k8s/ — Kubernetes manifests
- scripts/ — Dev/db/cost scripts (see subfolders)

---

## Domain Docs Cross-Links

- eCommerce: `../01-ARCHITECTURE/domains/ecommerce/DOMAIN_ECOMMERCE_ARCHITECTURE.md`
- Chat: `../01-ARCHITECTURE/domains/chat/DOMAIN_CHAT_ARCHITECTURE.md`
- IoT: `../01-ARCHITECTURE/domains/iot/DOMAIN_IOT_ARCHITECTURE.md`
- ML/AI: `../01-ARCHITECTURE/domains/ml-ai/DOMAIN_ML_AI_ARCHITECTURE.md`
- Social: `../01-ARCHITECTURE/domains/social/DOMAIN_SOCIAL_ARCHITECTURE.md`

---

## Learn-by-Doing Entry Points

- Execution Phases: `../stories/phases/` (Phase 0-9 with integrated learning)
- Protocol & Concurrency Reference: `../stories/REFERENCE-TOPICS.md` (9 API protocols + concurrency patterns)
- Quick Start Guide: `../stories/QUICK-START.md`

Tip: For each phase, follow the protocol labs embedded within execution tasks and document your progress with PRs linked to phase checklists.
