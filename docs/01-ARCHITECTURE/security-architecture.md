# Security Architecture

**Last Updated**: December 29, 2025

## Purpose
This document defines the comprehensive security architecture for BitVelocity, including authentication, authorization, secrets management, network security, and compliance patterns.

**Related Documentation**:

- [System Overview](system-overview.md)
- [ADR-005: Security Layering](../adr/ADR-005-security-layering.md)
- [Auth Service](../00-OVERVIEW/projects-and-modules.md#bv-auth-service)
- Security Testing: `bv-security-testing/README.md` (external module)

## Security Principles

### Core Tenets
1. **Security by Design**: Security integrated from the beginning, not retrofitted
2. **Defense in Depth**: Multiple layers of security controls
3. **Least Privilege**: Minimal access rights for users and services
4. **Zero Trust**: Never trust, always verify
5. **Audit Everything**: Comprehensive logging and monitoring of security events

## Authentication & Identity

### User Authentication

**JWT-Based Authentication**:
```java
// JWT token structure
{
  "sub": "user-id-123",
  "email": "user@example.com",
  "roles": ["CUSTOMER", "PREMIUM"],
  "iat": 1703865600,
  "exp": 1703952000,
  "jti": "unique-token-id"
}
```

**Authentication Service** (`bv-auth-service`):
- JWT token generation and validation
- Token refresh mechanism
- Password hashing (bcrypt/Argon2)
- Multi-factor authentication (MFA) support
- Session management with Redis

**Token Lifecycle**:

- Access tokens: Short-lived (15 minutes)
- Refresh tokens: Longer-lived (7 days), stored securely
- Token rotation on refresh
- Revocation support via blacklist in Redis

### Service-to-Service Authentication

**mTLS (Mutual TLS)**:
- All internal service communication authenticated
- Certificate-based authentication
- Automated certificate rotation
- Service mesh integration (Istio)

**API Keys for External Integration**:

- Hashed storage of API keys
- Rate limiting per key
- Key rotation policies
- Audit logging of all API key usage

## Authorization

### Role-Based Access Control (RBAC)

**Roles**:

- `ADMIN`: Full system access
- `CUSTOMER`: Standard user access
- `PARTNER`: External partner integration access
- `SERVICE`: Internal service-to-service access
- `AUDITOR`: Read-only audit access

**Permission Model**:
```java
@PreAuthorize("hasRole('ADMIN') or @orderService.isOwner(#orderId, authentication.principal)")
public OrderDto cancelOrder(String orderId) {
    // Cancel order logic
}
```

### Policy-Based Access Control

**Open Policy Agent (OPA) Integration**:
```rego
# Order cancellation policy
package orders

default allow_cancel = false

allow_cancel {
    input.order.status == "PENDING"
    input.user.id == input.order.customerId
}

allow_cancel {
    input.user.roles[_] == "ADMIN"
}
```

## Secrets Management

### HashiCorp Vault Integration

**Secret Types**:

- Database credentials
- API keys and tokens
- Encryption keys
- TLS certificates
- Cloud provider credentials

**Vault Configuration**:
```yaml
# Vault path structure
secret/
  ├── database/
  │   ├── postgres/credentials
  │   └── redis/credentials
  ├── services/
  │   ├── payment-gateway/api-key
  │   └── notification/smtp-password
  └── encryption/
      └── transit/data-encryption-key
```

**Dynamic Secrets**:

- Database credentials generated on-demand
- Short TTL (24 hours)
- Automatic rotation
- Lease renewal

### Application Secrets

**Spring Boot Integration**:
```java
@Configuration
@EnableVaultRepositories
public class VaultConfig {
    
    @Bean
    public VaultTemplate vaultTemplate() {
        VaultEndpoint endpoint = VaultEndpoint.create("vault.service", 8200);
        ClientAuthentication auth = new TokenAuthentication("s.token");
        return new VaultTemplate(endpoint, auth);
    }
}
```

## Data Security

### Encryption at Rest

**Database Encryption**:

- PostgreSQL: Transparent Data Encryption (TDE)
- Column-level encryption for PII
- Encryption key management via Vault

**Sensitive Data Handling**:
```java
@Entity
@Table(name = "users")
public class User {
    
    @Id
    private UUID id;
    
    private String email;
    
    @Convert(converter = EncryptedStringConverter.class)
    @Column(name = "phone_number")
    private String phoneNumber; // Encrypted
    
    @Convert(converter = EncryptedStringConverter.class)
    @Column(name = "ssn")
    private String ssn; // Encrypted
}
```

### Encryption in Transit

**TLS Everywhere**:

- External APIs: TLS 1.3
- Internal services: mTLS
- Database connections: TLS
- Message queues: TLS

**Certificate Management**:

- Automated certificate issuance (Let's Encrypt)
- Certificate rotation (90-day lifecycle)
- Certificate pinning for critical services

## Network Security

### API Gateway Security

**Kong/Envoy Configuration**:
- Rate limiting per client
- IP whitelisting/blacklisting
- Request size limits
- CORS policies
- DDoS protection

**Rate Limiting Example**:
```yaml
apiVersion: configuration.konghq.com/v1
kind: KongPlugin
metadata:
  name: rate-limiting
config:
  minute: 100
  hour: 10000
  policy: local
```

### Service Mesh Security

**Istio Security Policies**:
```yaml
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: order-service-policy
spec:
  selector:
    matchLabels:
      app: order-service
  rules:
  - from:
    - source:
        principals: ["cluster.local/ns/default/sa/cart-service"]
    to:
    - operation:
        methods: ["POST"]
        paths: ["/api/v1/orders"]
```

## Security Monitoring & Auditing

### Audit Logging

**Audit Events**:

- Authentication attempts (success/failure)
- Authorization decisions
- Data access (read/write/delete)
- Configuration changes
- Secret access
- Administrative actions

**Audit Log Structure**:
```json
{
  "timestamp": "2025-12-29T10:15:30Z",
  "eventType": "ORDER_CANCELLED",
  "userId": "user-123",
  "action": "CANCEL",
  "resource": "orders/ORDER-456",
  "outcome": "SUCCESS",
  "ipAddress": "192.168.1.100",
  "userAgent": "Mozilla/5.0...",
  "correlationId": "trace-abc-123"
}
```

### Security Monitoring

**Metrics**:

- `auth_failed_attempts_total` - Failed authentication attempts
- `authz_denied_total` - Authorization denials
- `secret_access_total` - Secret retrieval operations
- `tls_handshake_failures_total` - TLS handshake failures

**Alerts**:

- Multiple failed login attempts
- Unauthorized access attempts
- Secret access anomalies
- TLS certificate expiration

## Vulnerability Management

### Security Testing

**Automated Security Scans** (`bv-security-testing/`):
- **OWASP ZAP**: Web application security testing
- **Dependency Check**: Vulnerability scanning of dependencies
- **Container Scanning**: Image vulnerability scanning
- **SAST**: Static application security testing
- **DAST**: Dynamic application security testing

**CI/CD Integration**:
```yaml
# Security scan in pipeline
security-scan:
  stage: test
  script:
    - dependency-check --project BitVelocity --scan .
    - trivy image bitvelocity/order-service:latest
    - owasp-zap baseline scan
  allow_failure: false
```

### Penetration Testing

**Regular Testing Schedule**:

- Quarterly automated penetration tests
- Annual third-party penetration tests
- Continuous bug bounty program (future)

## Compliance & Governance

### Data Privacy

**GDPR Compliance**:

- Right to access (data export)
- Right to erasure (data deletion)
- Data minimization
- Consent management
- Data breach notification

**PII Handling**:
```java
@Service
public class GDPRService {
    
    public DataExportDto exportUserData(UUID userId) {
        // Export all user data
    }
    
    public void deleteUserData(UUID userId) {
        // Anonymize or delete user data
        // Maintain audit trail
    }
}
```

### SOC 2 Compliance

**Control Objectives**:

- Security: Access controls, encryption
- Availability: Uptime, disaster recovery
- Processing Integrity: Data validation, error handling
- Confidentiality: Data classification, encryption
- Privacy: Data handling, consent

## Security Incident Response

### Incident Response Plan

**Phases**:
1. **Detection**: Automated monitoring and alerting
2. **Containment**: Isolate affected systems
3. **Eradication**: Remove threat and vulnerabilities
4. **Recovery**: Restore systems and services
5. **Lessons Learned**: Post-incident review

**Security Contacts**:

- Security team email: security@bitvelocity.com
- On-call rotation for critical incidents
- Escalation procedures

## Best Practices

### Secure Coding Guidelines

**Input Validation**:
```java
@RestController
public class OrderController {
    
    @PostMapping("/api/v1/orders")
    public ResponseEntity<OrderDto> createOrder(
        @Valid @RequestBody OrderCreateRequest request) {
        // @Valid triggers validation
        // Additional business rule validation
    }
}
```

**SQL Injection Prevention**:
```java
// Use parameterized queries
@Query("SELECT o FROM Order o WHERE o.customerId = :customerId")
List<Order> findByCustomerId(@Param("customerId") UUID customerId);
```

**XSS Prevention**:

- Content Security Policy headers
- Output encoding
- Input sanitization

### Dependency Management

**Dependency Scanning**:

- Automated dependency updates
- Vulnerability alerting
- Version pinning
- License compliance checking

---

## Related Documentation

### Architecture References
- [System Overview](system-overview.md) - Platform architecture
- [Data Architecture](data-architecture.md) - Data encryption
- [All Domain Architectures](domains/) - Domain-specific security

### Implementation Guides
- [Microservices Patterns](../03-DEVELOPMENT/microservices-patterns.md)
- [Testing Strategy](../03-DEVELOPMENT/testing-strategy.md) - Security testing

### ADRs
- [ADR-005: Security Layering](../adr/ADR-005-security-layering.md)
- [ADR-007: Observability Baseline](../adr/ADR-007-observability-baseline.md)

---

**Document Status**: Active Reference ✅  
**Last Review**: December 29, 2025
