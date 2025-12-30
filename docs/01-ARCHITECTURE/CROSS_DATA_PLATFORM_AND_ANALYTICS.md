# Cross-Cutting – Data Platform & Analytics

**Last Updated**: December 29, 2025

**Purpose**: Defines data platform architecture, analytics pipelines, data governance, and cross-domain data projections.

**Related Documentation**:

- Data Architecture: `data-architecture.md`
- System Overview: `system-overview.md`
- ML/AI Domain: `domains/ml-ai/DOMAIN_ML_AI_ARCHITECTURE.md`
- ADR-002: Event vs CDC Strategy

**Module References**:

- Analytics Streaming: `bv-eCommerce-core/analytics-streaming-service/`
- Replay Service: `bv-eCommerce-core/replay-service/`

## 1. Layer Overview
OLTP (Postgres) → Domain Events + CDC (Debezium) → Stream Processing (Kafka Streams/Flink) → Serving (Redis/Cassandra/OpenSearch) → OLAP (Parquet + ClickHouse/BigQuery) → Feature Store (Redis) → ML Inference.

## 2. Cross-Domain Projections

| Projection | Source Domains | Store | Consumer Domains |
|:---|:---|:---|:---|
| orders_by_customer | E-Commerce | Cassandra | Analytics, ML |
| inventory_snapshot | E-Commerce + IoT | Redis | GraphQL, Recommendations |
| global_feed | Social | Redis | Clients |
| chat_room_activity | Chat | Cassandra / Redis | Analytics |
| telemetry_anomalies | IoT | Kafka → ClickHouse | E-Commerce (inventory adjust) |
| recommendation_feature_vectors | All (events) | Redis | ML inference service |

## 3. Governance
- Schema registry enforces backward compatibility.
- Data contracts (YAML) per topic: includes retention, PII classification, owners.
- Great Expectations nightly run on curated warehouse dataset.

## 4. Replay Framework
Steps:
1. Select projection & date range
2. Fetch event + CDC parquet sets
3. Apply in chronological sequence
4. Validate row counts & checksums
5. Emit replay completion event

## 5. Retention Policy (Baseline)

| Layer | Hot | Archive |
|:---|:---|:---|
| Kafka domain events | 7–14d | Parquet |
| CDC topics | 3–7d | Parquet |
| Redis | TTL | Rebuild |
| Cassandra | 6–12 mo | Parquet |
| Warehouse | 2–3 yrs | Cloud cold storage |

## 6. Data Quality Checks

| Check | Rule |
|:---|:---|
| Order total | SUM(line_items) = total_amount |
| Non-negative inventory | available >= 0 |
| Telemetry freshness | ingest_ts - device_ts < 5m |
| Null ratio constraints | < threshold for key fields |
| Feature presence | Feature vector completeness >= 95% |

## 7. Latency Targets
| Flow | Target |
|------|--------|
| Event → Projection update | < 5s |
| Order created → Warehouse row | < 2m |
| Telemetry anomaly detection | < 10s |
| Hot feature update propagation | < 1s |

## 8. Cost Optimization
- Postpone ClickHouse/BigQuery until Phase where streaming stable.
- Use DuckDB locally for early analytical queries.
- Downsample telemetry after initial ingestion for cold storage.

## 9. Cross-Domain Responsibilities
| Role | Responsibility |
|------|---------------|
| Data Steward (you) | Approve schema changes |
| Domain Owner | Provide data contract PR |
| ML Owner | Define feature survivability rules |
| Infra Owner | Maintain connectors & storage pipelines |

## 10. Exit Criteria
- At least 3 projections built via streams (not batch).
- Replay successful on sample dataset.
- Data quality run gating merges for schema changes.

---

## Related Documentation

### Architecture References
- [Data Architecture](data-architecture.md) - OLTP→OLAP flows
- [System Overview](system-overview.md)
- [ML/AI Domain](domains/ml-ai/DOMAIN_ML_AI_ARCHITECTURE.md) - Feature store
- [E-Commerce Domain](domains/ecommerce/DOMAIN_ECOMMERCE_ARCHITECTURE.md) - Analytics streaming

### Cross-Cutting
- [Event Contracts](CROSS_EVENT_CONTRACTS_AND_VERSIONING.md)
- [Replay & DR](CROSS_REPLAY_DR_DRILLS.md)

### Module READMEs
- `bv-eCommerce-core/analytics-streaming-service/README.md`
- `bv-eCommerce-core/replay-service/README.md`

### ADRs
- [ADR-002: Event vs CDC Strategy](../adr/ADR-002-event-vs-cdc-strategy.md)

**Document Status**: Active Reference ✅  
**Last Review**: December 29, 2025
