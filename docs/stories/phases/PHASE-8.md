# Phase 8 — CI/CD & K8s

## 🏷️ Domain Focus
**Primary**: 🔧 **Infrastructure** - CI/CD and Kubernetes deployment  
**Multi-Domain Deployment**: 🏪 **eCommerce**, 💬 **Chat**, 🏭 **IoT**, 📱 **Social**, 🧠 **ML/AI**  
**Protocol Focus**: 🌐 **GraphQL Federation**, 🔄 **JSON-RPC**, 🚀 **Production protocols**

## Epic Integration: CI/CD Pipeline (15 Story Points)
**Epic Objective**: Implement continuous integration and deployment pipeline for automated testing and deployment  
**Success Criteria**: GitHub Actions workflow for automated builds, automated testing (unit, integration, security), container image building and publishing, deployment automation to Kubernetes, environment promotion pipeline

## Objectives
- Build/test pipelines; images; deploy to dev namespace; smoke tests
- Implement advanced API patterns (GraphQL, JSON-RPC) in production deployment
- Establish cloud-native deployment patterns with protocol diversity

## Deliverables
- CI workflows; Dockerfiles; k8s manifests
- GraphQL federation gateway and JSON-RPC service implementations
- Multi-protocol service mesh configuration

## Tasks (acceptance)
1) **Build & test pipelines with Protocol Testing (Story Points: 8)**
   - [ ] Build changed modules; run tests on PR; publish docs artifact
   - [ ] **GraphQL Protocol Lab**: Implement federated schema and gateway
     - Design GraphQL schemas with federation directives
     - Build GraphQL gateway aggregating multiple microservices
     - Add subscription support for real-time updates
     - Performance test vs REST equivalents
   - [ ] **Files**: `.github/workflows/build.yml`, `.github/workflows/test.yml`
   - [ ] **Acceptance**: Automated build and test pipeline

2) **Images & SBOM with Advanced Protocols (Story Points: 7)**
   - [ ] Dockerfiles per service; generate SBOM in CI
   - [ ] **JSON-RPC Protocol Lab**: Implement batch operations and notifications
     - Create JSON-RPC 2.0 compliant service endpoints
     - Implement batch request processing
     - Add WebSocket transport for JSON-RPC over persistent connections
     - Compare with GraphQL for bulk operations
   - [ ] **Files**: `Dockerfile` per service, SBOM generation step
   - [ ] **Acceptance**: Images build; SBOM attached/artifacts uploaded

3) **K8s deploy with Multi-Protocol Support**
   - [ ] Dev namespace deploy; smoke tests
   - [ ] **Protocol Gateway Configuration**: Route traffic based on protocol type
   - [ ] **Service Mesh Integration**: Configure Istio/Envoy for multi-protocol support
   - [ ] **End-to-end Protocol Testing**: Validate all 9 protocols work in K8s environment
   - [ ] **Path**: `k8s/`
   - [ ] **Acceptance**: Dev namespace deploy job; smoke tests pass

Dependencies
- Phase 7

Learning Links
 - [Reference Topics (Protocols & Concurrency)](../REFERENCE-TOPICS.md)
 - Phases Overview: `./README.md` (if exists)

Next Phase: [Phase 9](./PHASE-9.md)