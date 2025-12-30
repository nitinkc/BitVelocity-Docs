# Phase 1 — Build System & Shared Foundations

## 🏷️ Domain Focus  
**Primary**: 🔧 **Infrastructure** - Build system and shared libraries  
**Secondary**: 🏪 **eCommerce**, 💬 **Chat**, 🏭 **IoT**, 📱 **Social**, 🧠 **ML/AI** - Multi-domain foundation setup

## Objectives
- Establish parent POM, BOM, and shared libs skeletons to standardize builds
- Create protocol-agnostic shared libraries for multi-protocol support
- Implement foundation for reactive and virtual threads concurrency

## Deliverables
- `bv-core-parent` and `bv-core-platform-bom`
- `bv-core-common/*` modules compiling with minimal tests
- CI build on PRs for changed modules
- Shared protocol abstraction libraries

## Tasks (acceptance)
1) **Parent POM & BOM with Protocol Dependencies**
   - [ ] Parent POM with plugin mgmt and shared config
   - [ ] BOM pins dependencies; imported by at least one service
   - [ ] **Multi-Protocol Foundation**: Include dependencies for all 9 protocols
     - Spring WebFlux (REST, SSE, WebSocket)
     - gRPC starters and protobuf plugins
     - GraphQL Spring Boot starter
     - Kafka and messaging libraries
     - JSON-RPC and webhook handling
   - [ ] **Modules**: `bv-core-parent`, `bv-core-platform-bom`
   - [ ] **Acceptance**: At least one service builds off parent with BOM imported

2) **Shared libraries skeleton with Protocol Abstractions**
   - [ ] Modules compile: `bv-common-entities`, `bv-common-events`, `bv-common-exceptions`, `bv-common-logging`, `bv-common-auth`, `bv-common-security`
   - [ ] Each has a minimal unit test and README
   - [ ] **Protocol Common Module**: Create `bv-common-protocols` with:
     - Abstract protocol handler interfaces
     - Common request/response wrapper classes
     - Protocol-specific error handling
     - Tracing and metrics abstractions
   - [ ] **Modules**: `bv-core-common` + submodules
   - [ ] **Acceptance**: Basic unit test per module (one assertion), README documents purpose and usage

3) **CI sanity pipeline with Multi-Protocol Testing**
   - [ ] `.github/workflows/build.yml` builds changed modules and fails on tests/style
   - [ ] **Protocol Compatibility Matrix**: Test framework setup for cross-protocol scenarios
   - [ ] **Files**: `.github/workflows/build.yml`
   - [ ] **Acceptance**: Build on PR for changed modules, fails on test or style errors

Dependencies
- Phase 0

Learning & References
- [Reference Topics (Protocols & Concurrency)](../REFERENCE-TOPICS.md)
- [Actionable Build Plan](../ACTIONABLE-BUILD-PLAN.md)

Next Phase: [Phase 2](./PHASE-2.md)