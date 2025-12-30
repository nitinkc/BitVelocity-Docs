# Domain Architecture – Social / Feed

**Last Updated**: December 29, 2025

## 1. Purpose
Model post creation, comments, engagement, and feed dissemination with SSE for near real-time updates.

**Module Location**: `bv-social-pulse/`

**Related Documentation**:

- [System Overview](../../system-overview.md)
- [Event-Driven Patterns](../../../03-DEVELOPMENT/microservices-patterns.md)
- [SSE Protocol](../../../03-DEVELOPMENT/api-protocols.md)
- ADR-002: Event vs CDC Strategy

## 2. Services
| Service | Responsibility | Store |
|---------|----------------|-------|
| post-service | CRUD posts | Postgres (early) → Mongo (optional) |
| comment-service | CRUD comments | Postgres/Mongo |
| feed-service | Materialize feed & SSE push | Redis + Kafka |
| integration-service | Outbound webhooks for syndicated content | Postgres |

## 3. Protocol Mapping
| Use Case | Protocol |
|----------|----------|
| Post CRUD | REST |
| Feeds aggregated query | GraphQL extension |
| Feed streaming | SSE |
| Post events | Kafka |
| Outbound syndication | Webhook |
| Search (later) | OpenSearch |

## 4. Events
| Event | Purpose |
|-------|---------|
| social.post.created.v1 | Add to feed pipeline |
| social.comment.created.v1 | Update engagement metrics |
| social.feed.activity.v1 | Aggregated engagement fan-out |
| social.post.promoted.v1 | Boost visibility (optional) |

Payload example:
```
{
  "eventType":"social.post.created.v1",
  "payload":{
     "postId":"P9",
     "authorId":"U2",
     "tags":["sale","electronics"],
     "createdAt":"..."
  }
}
```

## 5. Feed Materialization Options
Strategy: Hybrid push/pull
- SSE stream for “latest posts” channel
- User-personalized feed (future) built via Kafka Streams state store keyed by userId
- Redis lists: feed:global (rolling N), feed:user:{id}

## 6. Caching
- Post read: post:{postId}
- Global feed: list operations
- Engagement counters: hash feed:engagement:{postId} (increment on comment/like)

## 7. Security
- Auth required for posting/commenting.
- Content moderation pipeline optional; events flagged for ML/AI ingestion.
- Rate limiting (gateway): posts/min per user.

## 8. Observability

Metrics:

- posts_created_total
- feed_sse_active_connections
- feed_push_latency_ms
- engagement_counter_update_failures

Tracing:

- REST create post -> Kafka publish -> SSE push chain

## 9. Testing
| Layer | Focus |
|-------|-------|
| Unit | Post validation (length, tags) |
| Integration | Event -> feed list update |
| SSE functional | Client receives new post within latency SLO |
| Performance | SSE connection scaling test |
| Contract | GraphQL schema diff gating |

## 10. Implementation Sequence
1. Post CRUD (REST + Postgres)
2. Event emission (post.created)
3. Feed global list consumer + SSE endpoint
4. Comment service + engagement counters
5. GraphQL aggregator fields
6. Webhook integration (syndicated posts)
7. Personalized feed prototype (user-specific)
8. OpenSearch indexing (optional later)

## 11. Interoperability Checklist
- [ ] Post events schematized & validated
- [ ] SSE channels documented (naming & reconnect strategy)
- [ ] GraphQL additions namespaced (no collisions)
- [ ] No direct dependency on e-commerce code
- [ ] Webhook format documented for consumers

## 12. Cost Controls
- SSE only (skip WebSocket clustering overhead for this domain).
- Delay search infra until event pipeline stable.

## 13. Exit Criteria
- Post creation → SSE push latency < 500ms
- Feed endpoint returns aggregated data from Redis
- Comment events trigger engagement counter updates
- GraphQL federated queries work across Post + User contexts

---

## Related Documentation

### Architecture References
- [System Overview](../../system-overview.md) - Platform architecture
- [Data Architecture](../../data-architecture.md) - Feed materialization patterns
- [Observability](../../CROSS_OBSERVABILITY_AND_TESTING.md)

### Implementation Guides
- [API Protocols Guide](../../../03-DEVELOPMENT/api-protocols.md) - SSE & GraphQL
- [Microservices Patterns](../../../03-DEVELOPMENT/microservices-patterns.md) - Event-driven patterns
- [Testing Strategy](../../../03-DEVELOPMENT/testing-strategy.md)

### ADRs
- [ADR-002: Event vs CDC Strategy](../../../adr/ADR-002-event-vs-cdc-strategy.md)
- [ADR-007: Observability Baseline](../../../adr/ADR-007-observability-baseline.md)

**Current Status**: Active Development ✅  
**Last Review**: December 29, 2025
- Post creation visible via SSE < 500ms median
- Engagement counters accurate under concurrent updates
- GraphQL consolidation query returning post + engagement fields
