# ADR-012: Social Media Domain Architecture

## Status
Proposed

## Context
The Social Media domain is responsible for posts, feeds, social graph, and content moderation. It must support event-driven architecture, GraphQL federation, and scalable feed generation.

## Decision
- Use event-driven architecture for all feed and engagement events.
- Implement GraphQL federation for flexible, aggregated queries.
- Use pub/sub for real-time updates and fan-out.
- Content moderation will be handled via event contracts and ML/AI integration.
- Feed materialization will use hybrid push/pull and stateful Kafka Streams.
- All social events must be versioned and schema-validated.

## Consequences
- Enables scalable, real-time social features.
- Supports future extensibility for moderation and analytics.
- Ensures loose coupling and protocol-agnostic integration.
