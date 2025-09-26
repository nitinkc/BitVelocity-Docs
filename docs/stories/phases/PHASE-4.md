# Phase 4 — Product Service (REST CRUD)

## 🏷️ Domain Focus
**Primary**: 🏪 **eCommerce** - Product catalog management  
**Architecture Reference**: `01-ARCHITECTURE/domains/ecommerce/DOMAIN_ECOMMERCE_ARCHITECTURE.md`  
**Protocol Focus**: 🌐 **Advanced REST** patterns (HATEOAS, pagination, filtering)

## Epic Integration: Product Service (18 Story Points)
**Epic Objective**: Build comprehensive product catalog management service with CRUD operations and data validation  
**Success Criteria**: Product entity with audit fields, REST API with full CRUD, data validation and error handling, database integration, 1000 products searchable in <500ms, complete API documentation

## Objectives
- Build product catalog service with REST CRUD and solid validation.
- **Learning Focus**: Advanced REST patterns - pagination, filtering, validation, HATEOAS.

## Deliverables
- `product-service/openapi/product.yaml`
- Product service code + tests
- Product catalog management with performance requirements

## Tasks (acceptance)
1) **Contract-first API (Advanced REST Learning)**
   - [ ] CRUD paths defined; validation and error model
   - [ ] Implement pagination for GET /products (page, size, sort)
   - [ ] Add filtering capabilities (category, price range)
   - [ ] Practice HATEOAS principles in responses
   - [ ] **Files**: `product-service/openapi/product.yaml`
   - [ ] **Story Points**: 4 pts (Design Product entity with audit fields)

2) **Implement service with Full CRUD**
   - [ ] Entities, migrations, repository, service, controller
   - [ ] Validation and consistent errors
   - [ ] Unit + integration tests
   - [ ] **Module**: `bv-eCommerce-core/product-service`
   - [ ] **Story Points Breakdown**:
     - Create REST API controllers with CRUD (4 pts)
     - Implement data validation and error handling (3 pts)  
     - Setup database integration and migrations (4 pts)
     - Build repository and service layers (3 pts)
   - [ ] **Acceptance**: Request validation, consistent error responses, containerized Postgres tests

3) **Protocol Lab Continuation**
   - [ ] Test idempotency for PUT operations
   - [ ] Implement proper ETags for optimistic locking
   - [ ] Add comprehensive error handling examples
   - [ ] **Performance Requirement**: 1000 products searchable in <500ms

4) **Docs & Collection**
   - [ ] REST examples documented; Postman/Insomnia collection committed
   - [ ] **Files**: `docs/03-DEVELOPMENT/microservices-patterns.md` (REST section)

Dependencies
- Phase 3

Learning & References
- Reference Topics (Protocols & Concurrency): `../REFERENCE-TOPICS.md`

Next Phase: [Phase 5](./PHASE-5.md)