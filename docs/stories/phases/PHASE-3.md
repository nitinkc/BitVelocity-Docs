# Phase 3 — Authentication Service

## 🏷️ Domain Focus
**Primary**: 🔒 **Security** - Authentication and authorization  
**Supporting**: 🏪 **eCommerce** - Product access control  
**Protocol Focus**: 🌐 **REST API** patterns and security

## Epic Integration: Authentication Service (21 Story Points)
**Epic Objective**: Implement secure authentication service with JWT tokens, user management, and role-based access control  
**Success Criteria**: JWT token generation/validation, secure user registration/login, BCrypt password hashing, RBAC implementation, 100 requests/second performance, security audit compliance

## Objectives
- Implement a secure auth boundary with JWT, RBAC, and basic defenses.
- **Learning Focus**: Master REST API design with OpenAPI, authentication patterns.

## Deliverables
- `auth-service/openapi/auth.yaml`
- `bv-auth-service` with endpoints and tests
- Observability wired (traces/metrics/logs)

Tasks (acceptance)
1) Contract-first API (REST Protocol Learning)
   - [ ] OpenAPI covers register/login/refresh/me; error model aligns with `bv-common-exceptions`
   - [ ] Practice proper HTTP status codes (201, 200, 401, 422) and error responses
   - [ ] Document with OpenAPI; test with curl/Postman
2) Implement service
   - [ ] Register/login, JWT issuance/validation
   - [ ] BCrypt, simple RBAC, login rate limit
   - [ ] Unit + integration tests (containerized Postgres)
   - [ ] Observability basics
3) Protocol Lab (REST)
   - [ ] Implement idempotency for registration endpoint
   - [ ] Add rate limiting demonstration
   - [ ] Create API collection for testing
4) Docs
   - [ ] Update auth section in `03-DEVELOPMENT/microservices-patterns.md` with examples


Learning & References
 - Reference Topics (Protocols & Concurrency): `../REFERENCE-TOPICS.md`

Next Phase: [Phase 4](./PHASE-4.md)