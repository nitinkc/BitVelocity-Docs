# Phase 2 — Local Infrastructure

## 🏷️ Domain Focus
**Primary**: 🔧 **Infrastructure** - Local development environment  
**Secondary**: 💬 **Chat** (WebSocket), 📱 **Social** (SSE), 🏭 **IoT** (Batch processing)

## Objectives
- Provide containerized infra for local dev: Postgres, Redis, Kafka (+Schema Registry optional)
- Set up protocol testing infrastructure for WebSocket, SSE, and batch processing
- Establish local environment supporting all 9 communication protocols

## Deliverables
- `docker-compose.yml` with health checks
- Protocol testing infrastructure (WebSocket, SSE endpoints)
- Optional: `k8s/` base manifests for future

## Tasks (acceptance)
1) **Docker Compose stack with Protocol Support**
   - [ ] `docker compose up -d` brings services up
   - [ ] Health checks pass for each service
   - [ ] **WebSocket Protocol Lab**: Add WebSocket test server to docker-compose
     - Set up simple WebSocket echo server for connection testing
     - Include WebSocket client testing utilities
     - Configure nginx proxy for WebSocket upgrade headers
   - [ ] **SSE Protocol Lab**: Configure SSE event stream endpoints
     - Add server-sent events mock service
     - Test persistent connection handling
     - Configure proper CORS headers for browser clients
   - [ ] **Files**: `docker-compose.yml`
   - [ ] **Services**: Postgres, Redis, Kafka+ZK, Schema Registry (optional)
   - [ ] **Acceptance**: Health checks for each service pass

2) **K8s manifests scaffold with Protocol Gateway (optional now)**
   - [ ] Namespace, sample deployment/svc/configmap templates
   - [ ] **Batch Processing Lab**: Set up file processing pipeline mockup
     - Create simple batch job definition
     - Test file upload/download endpoints
     - Configure volume mounts for batch processing
   - [ ] **Path**: `k8s/`
   - [ ] **Acceptance**: Base namespace and shared secrets examples, service template manifests checked in

Dependencies
- Phase 1

Learning & References
- Reference Topics (Protocols & Concurrency): `../REFERENCE-TOPICS.md`

Next Phase: [Phase 3](./PHASE-3.md)