# Cross-Cutting – Replay & Disaster Recovery Drills

**Last Updated**: December 29, 2025

**Purpose**: Defines replay mechanisms for rebuilding projections and disaster recovery procedures for system resilience.

**Related Documentation**:

- [Data Platform](CROSS_DATA_PLATFORM_AND_ANALYTICS.md)
- [Data Architecture](data-architecture.md)
- [Chaos Engineering](../adr/ADR-016-chaos-engineering-framework.md)

**Module References**:

- Replay Service: `bv-eCommerce-core/replay-service/`
- Chaos Experiments: `bv-chaos-experiments/`

## 1. Replay Goals
Rebuild derived projections and recover from data corruption or missed events.

## 2. Replay Data Sources
- Parquet archived domain events
- CDC parquet (Debezium output)
- Snapshot manifests (optional)

## 3. Replay Workflow
1. Identify target projection & timespan
2. Acquire input set (list parquet segments)
3. Stream sorted by timestamp/LSN
4. Apply transformation (same logic as live stream code)
5. Validation: sample checksums, row counts
6. Emit system.replay.completed.v1 event

## 4. CLI (Conceptual)
```
replay \
  --projection orders_by_customer \
  --from 2025-02-01T00:00Z \
  --to 2025-02-02T00:00Z \
  --events s3://archive/events/ecommerce.order/2025/02/01 \
  --cdc s3://archive/cdc/orders/2025/02/01 \
  --dry-run
```

## 5. DR Drill Types

| Drill | Scenario | Goal |
|:---|:---|:---|
| Failover DB | East Postgres down | Promote replica |
| Kafka Partition Loss | Topic partial outage | Replay missing partition events |
| Cache Flush | Redis cleared | Rehydrate via events |
| Cross-Region Latency Spike | Simulate 500ms RTT | Validate fallbacks |
| Replay Integrity | Random projection deletion | Full rebuild parity |

## 6. Metrics
- replay_duration_seconds
- replay_events_processed_total
- replay_validation_failures_total
- dr_rto_seconds
- dr_rpo_seconds

## 7. DR Runbook (High-Level)
1. Detect incident (alerts)
2. Declare severity
3. Promote replica (script)
4. Update endpoints / DNS
5. Replay gap (if RPO > 0)
6. Verify service health & user flows
7. Post-mortem record & improvements

## 8. Exit Criteria
- At least one successful replay of each major projection (orders, feed)
- DR drill executed with RTO < 10m and RPO < 2m (learning targets)
- Replay job idempotent & documented

---

## Related Documentation

### Architecture References
- [Data Platform & Analytics](CROSS_DATA_PLATFORM_AND_ANALYTICS.md)
- [Data Architecture](data-architecture.md)
- [System Overview](system-overview.md)

### Module READMEs
- `bv-eCommerce-core/replay-service/README.md`
- `bv-chaos-experiments/README.md` - DR drill procedures

### ADRs
- [ADR-016: Chaos Engineering Framework](../adr/ADR-016-chaos-engineering-framework.md)
- [ADR-002: Event vs CDC Strategy](../adr/ADR-002-event-vs-cdc-strategy.md)

**Document Status**: Active Reference ✅  
**Last Review**: December 29, 2025
