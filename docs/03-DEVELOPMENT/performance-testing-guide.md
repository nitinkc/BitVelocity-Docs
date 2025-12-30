# Performance Testing Guide

## Overview

This guide provides comprehensive information on performance testing practices for BitVelocity services.

## Performance Testing Pyramid

```
         /\
        /E2E\         Endurance Tests (monthly)
       /------\
      / Stress \      Stress & Spike Tests (weekly)
     /----------\
    /   Load     \    Load Tests (nightly)
   /--------------\
  /     Smoke      \  Smoke Tests (every PR)
 /------------------\
```

## Test Types

### 1. Smoke Tests (CI Pipeline)
**Purpose**: Quick validation, regression detection  
**Tool**: k6  
**Duration**: < 1 minute  
**Load**: 10 VUs  
**Frequency**: Every PR  

**Example**:
```bash
cd bv-performance-testing/k6-scripts
k6 run --vus 10 --duration 30s api-smoke-test.js
```

**Success Criteria**:
- p95 latency < 200ms
- Error rate < 1%
- No degradation > 10% vs baseline

### 2. Load Tests
**Purpose**: Validate performance under expected load  
**Tool**: Gatling (Java)  
**Duration**: 5-15 minutes  
**Load**: Ramp to 100-500 users  
**Frequency**: Nightly  

**Example**:
```bash
cd bv-performance-testing/gatling-tests
./gradlew gatlingRun -Psimulation=OrderFlowSimulation
```

**Success Criteria**:
- Meet SLI targets (see `performance-baselines/sli-targets.yaml`)
- No errors > 1%
- Resource utilization < 80%

### 3. Stress Tests
**Purpose**: Find system breaking points  
**Tool**: Gatling (Java)  
**Duration**: 15-30 minutes  
**Load**: Incrementally increase until failure  
**Frequency**: Weekly  

**Key Metrics**:
- Maximum sustainable load
- Point of degradation
- Recovery behavior
- Error patterns

### 4. Spike Tests
**Purpose**: Test sudden load increases (flash sales, viral content)  
**Tool**: Gatling (Java)  
**Duration**: 5-10 minutes  
**Load**: Immediate spike to 200+ users  
**Frequency**: Weekly  

**Validation**:
- Circuit breakers activate appropriately
- No cascading failures
- System recovers gracefully

### 5. Endurance Tests
**Purpose**: Detect memory leaks, resource exhaustion  
**Tool**: Gatling (Java)  
**Duration**: 2-4 hours  
**Load**: Sustained moderate load  
**Frequency**: Monthly or before major releases  

**Monitor**:
- Memory usage trends
- Connection pool leaks
- Database query performance over time
- Cache behavior

## Writing Performance Tests

### Gatling (Java) Test Structure

```java
public class MySimulation extends Simulation {
    
    // HTTP Protocol Configuration
    HttpProtocolBuilder httpProtocol = http
        .baseUrl("http://localhost:8080")
        .acceptHeader("application/json");
    
    // Test Data Feeder
    Iterator<Map<String, Object>> feeder = 
        Stream.continually(() -> Map.of(
            "userId", "USER-" + random.nextInt(1000)
        )).iterator();
    
    // Scenario Definition
    ScenarioBuilder scenario = scenario("My Scenario")
        .feed(feeder)
        .exec(
            http("Request Name")
                .get("/api/v1/resource")
                .check(status().is(200))
        )
        .pause(Duration.ofSeconds(1));
    
    // Load Profile
    {
        setUp(
            scenario.injectOpen(
                rampUsers(100).during(Duration.ofMinutes(5))
            ).protocols(httpProtocol)
        ).assertions(
            global().responseTime().percentile(95.0).lt(200),
            global().successfulRequests().percent().gt(95.0)
        );
    }
}
```

### k6 Test Structure

```javascript
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
  vus: 10,
  duration: '30s',
  thresholds: {
    http_req_duration: ['p(95)<200'],
    errors: ['rate<0.01'],
  },
};

export default function () {
  const response = http.get('http://localhost:8080/api/v1/resource');
  
  check(response, {
    'status is 200': (r) => r.status === 200,
    'response time < 200ms': (r) => r.timings.duration < 200,
  });
  
  sleep(1);
}
```

## Performance Baselines

All services must define SLI targets in `performance-baselines/sli-targets.yaml`:

```yaml
services:
  order-service:
    availability_target: 99.0
    endpoints:
      - path: "POST /api/v1/orders"
        latency:
          p50: 50ms
          p95: 200ms
          p99: 500ms
        throughput: 100
        error_rate_max: 1.0
```

## Analyzing Results

### Key Metrics

1. **Latency Percentiles**
   - p50 (median): Typical user experience
   - p95: Most users' experience
   - p99: Worst-case scenarios
   - p99.9: Outliers

2. **Throughput**
   - Requests per second
   - Transactions per second
   - Messages per second

3. **Error Rate**
   - HTTP 5xx errors
   - Timeouts
   - Connection failures

4. **Resource Utilization**
   - CPU usage
   - Memory usage
   - Network I/O
   - Disk I/O

### Gatling Reports

Gatling generates HTML reports at `build/reports/gatling/`:

- Request statistics
- Response time distribution
- Response time percentiles over time
- Requests per second
- Active users over time

### k6 Output

```json
{
  "metrics": {
    "http_req_duration": {
      "avg": 45.2,
      "min": 12.3,
      "max": 156.7,
      "p(90)": 89.4,
      "p(95)": 112.8
    },
    "http_reqs": 12543,
    "http_req_failed": 0.002
  }
}
```

## Performance Optimization Strategies

### 1. Database
- Add appropriate indexes
- Optimize N+1 queries
- Use connection pooling
- Consider read replicas
- Implement caching

### 2. Caching
- Cache hot data (80/20 rule)
- Set appropriate TTLs
- Monitor cache hit ratio (target: > 80%)
- Implement cache warming

### 3. HTTP/API
- Use HTTP/2 for multiplexing
- Implement pagination
- Compress responses (gzip)
- Use ETags for conditional requests
- Implement rate limiting

### 4. Messaging
- Batch message processing
- Optimize consumer configuration
- Monitor consumer lag
- Use appropriate partitioning

### 5. JVM
- Tune heap size (-Xms, -Xmx)
- Monitor GC pauses
- Use G1GC or ZGC for low latency
- Enable JVM metrics

## Troubleshooting

### High Latency
1. Check database query performance
2. Look for N+1 query problems
3. Check cache hit ratio
4. Monitor GC pauses
5. Check for network issues
6. Look at distributed traces

### High Error Rate
1. Check logs for error patterns
2. Monitor circuit breaker states
3. Check resource exhaustion
4. Verify connection pool settings
5. Look for cascading failures

### Resource Exhaustion
1. Monitor heap usage
2. Check for connection leaks
3. Monitor thread pool utilization
4. Check disk I/O
5. Monitor network saturation

## CI/CD Integration

Performance tests run in CI pipeline:

```yaml
# .github/workflows/performance-smoke.yml
- name: Run k6 Smoke Test
  run: |
    cd bv-performance-testing/k6-scripts
    k6 run --out json=results.json api-smoke-test.js

- name: Check Performance Regression
  run: |
    # Compare with baseline
    # Fail if degradation > 10%
```

## Best Practices

1. **Always define baselines** before optimization
2. **Test one change at a time** to isolate impact
3. **Run tests multiple times** to account for variance
4. **Monitor system metrics** during tests
5. **Document findings** for future reference
6. **Update baselines** after verified improvements
7. **Test realistic scenarios** not just synthetic loads
8. **Include think time** to simulate real users
9. **Use production-like data** volumes
10. **Test failure scenarios** not just happy paths

## Learning Resources

- [Gatling Documentation](https://gatling.io/docs/)
- [k6 Documentation](https://k6.io/docs/)
- [Performance Testing Guidance](https://martinfowler.com/articles/performance-testing.html)
- ADR-015: Load Testing Strategy

## Getting Help

- Check `bv-performance-testing/README.md`
- Review example tests in `gatling-tests/src/gatling/java/`
- Look at SLI targets in `performance-baselines/sli-targets.yaml`
- Ask in team channel with test results attached
