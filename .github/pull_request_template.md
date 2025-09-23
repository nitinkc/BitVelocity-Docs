# Pull Request: Protocol-First Lab / Change

## Summary
- What protocol(s) and services are impacted?
- Link to the lab issue: <!-- #123 -->

## Protocol-First Checklist
- [ ] Contract added/updated
  - [ ] OpenAPI / gRPC proto / GraphQL SDL / Event schema committed
  - [ ] Versioning and change log updated
- [ ] Resilience
  - [ ] Error model documented (ADR-006 alignment)
  - [ ] Retries/backoff/circuit breaker configured where relevant
- [ ] Observability
  - [ ] Traces, metrics, logs per ADR-007
  - [ ] OTel config updated if needed
- [ ] Documentation
  - [ ] Protocol Curriculum updated (sprint section)
  - [ ] API Styles Track updated (subsection)
  - [ ] Projects & Modules updated if module boundaries changed
  - [ ] Event Contracts updated (if applicable)

## Links
- Protocol Curriculum: docs/stories/PROTOCOL-CURRICULUM.md
- API Styles Track: docs/stories/API-STYLES-TRACK.md
- Developer Learning Plan: docs/stories/DEVELOPER-LEARNING-PLAN.md
- Projects & Modules: docs/00-OVERVIEW/projects-and-modules.md
- Relevant ADRs: docs/adr/

## Testing
- Steps to validate behavior and acceptance criteria

## Notes
- Risks, trade-offs, and follow-ups
