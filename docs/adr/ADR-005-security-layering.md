# ADR-005: Security Layering with UserContext

This document describes the security layering approach for BitVelocity, focusing on the use of UserContext and best practices for authentication and authorization.

- UserContext propagation
- Role-based access control
- JWT integration (planned)

# BitVelocity Architecture Overview

This document provides an overview of the BitVelocity architecture, including module structure, service boundaries, and technology choices.

- Core modules: entities, events, security, exceptions
- Authentication service: user management, JWT, roles
- Infrastructure: PostgreSQL, Redis, Redpanda
- Security patterns: see ADR-005

