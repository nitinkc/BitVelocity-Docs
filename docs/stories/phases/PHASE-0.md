# Phase 0 — Repo & Docs Groundwork

## 🏷️ Domain Focus
**Primary**: 🔧 **Infrastructure** - Platform foundation and documentation  
**Secondary**: 📚 **Learning Framework** - Protocol learning structure setup

## Objectives
- Create a single navigation path and baseline docs so contributors can orient in minutes
- Establish foundation for protocol-aware architecture documentation
- Set up learning-integrated development workflow

## Deliverables
- Quick Start page linked from home and nav
- Docs site serving and deploying cleanly
- Protocol learning roadmap integrated into documentation structure

## Tasks (acceptance)
1) **Quick Start entrypoint with Protocol Learning Path**
   - Create/verify `docs/stories/QUICK-START.md`
   - Link from `docs/index.md` and `mkdocs.yml`
   - [ ] Quick Start links to Actionable Build Plan and Reference Topics (Protocols & Concurrency)
   - [ ] **Protocol Learning Integration**: Add section explaining the 9-protocol learning journey
     - Map each phase to specific protocol learning outcomes
     - Link protocol labs to practical implementation tasks
     - Create progression from simple (REST) to advanced (GraphQL federation)
   - [ ] **Files**: `docs/stories/QUICK-START.md`, update `docs/index.md`
   - [ ] **Acceptance**: mkdocs nav includes Quick Start

2) **Verify docs serve & deploy with Learning Structure**
   - Run `mkdocs serve` locally and address any issues
   - Deploy with `mkdocs gh-deploy`
   - [ ] Both steps succeed without errors
   - [ ] **Documentation Architecture**: Ensure REFERENCE-TOPICS.md is properly integrated in navigation
   - [ ] **Learning Validation**: Verify all phase cross-references work correctly
   - [ ] **Files**: `mkdocs.yml`, `docs/index.md`

Dependencies
- None

Learning & References
- Reference Topics (Protocols & Concurrency): `../REFERENCE-TOPICS.md`

Next Phase: [Phase 1](./PHASE-1.md)