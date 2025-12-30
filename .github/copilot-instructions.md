# BitVelocity Documentation - AI Agent Instructions

## Documentation Purpose
This repository contains the **complete technical documentation** for the BitVelocity learning platform. It serves as the single source of truth for architecture, ADRs, implementation guides, and project management.

## Core Principles

### 1. Documentation Synchronization
**CRITICAL**: Whenever new modules, services, or components are added to the BitVelocity platform:
1. **Scan** all existing documentation files for references to module lists
2. **Update** the following key files:
   - `docs/00-OVERVIEW/projects-and-modules.md` - Module catalog
   - `docs/00-OVERVIEW/README.md` - Overview references
   - `mkdocs.yml` - Navigation structure (if new sections needed)
   - Relevant domain architecture files in `docs/01-ARCHITECTURE/domains/`
3. **Verify** that ADRs reference the correct modules
4. **Check** that implementation roadmaps include new components

### 2. Module Organization Structure

#### Current Active Modules (as of December 2025):
- **Core Libraries**: `bv-core-common` (auth, entities, events, logging, security)
- **Build Management**: `bv-core-parent`, `bv-core-platform-bom`
- **eCommerce Domain**: `bv-eCommerce-core` (10+ microservices)
- **Auth & Security**: `bv-auth-service`, `bv-security-core`, `bv-security-testing`
- **Other Domains**: `bv-chat-stream`, `bv-iot-control-hub`, `bv-social-pulse`
- **Infrastructure**: `bv-infra-service`
- **Quality Assurance**: `bv-performance-testing`, `bv-chaos-experiments`, `bv-observability`

### 3. Documentation Standards

#### File Naming Conventions:
- Architecture files: `DOMAIN_[NAME]_ARCHITECTURE.md` (e.g., `DOMAIN_ECOMMERCE_ARCHITECTURE.md`)
- Cross-cutting concerns: `CROSS_[TOPIC].md` (e.g., `CROSS_OBSERVABILITY_AND_TESTING.md`)
- ADRs: `ADR-[NUM]-[kebab-case-title].md` (e.g., `ADR-015-load-testing-strategy.md`)
- Phases: `PHASE-[N].md` (e.g., `PHASE-1.md`)

#### Markdown Style:
- Use consistent heading levels (## for main sections, ### for subsections)
- Include mermaid diagrams for architecture visualizations
- Add cross-references using relative paths: `../01-ARCHITECTURE/...`
- Include code blocks with appropriate language tags

### 4. Navigation Management (mkdocs.yml)

When adding new documentation:
1. Determine correct section: Overview, Architecture, Infrastructure, Development, Stories, Project Management, ADRs, Event Contracts
2. Add to navigation in alphabetical or logical order
3. Use meaningful display names (not filenames)
4. Maintain hierarchical structure with proper indentation

### 5. Content Review Triggers

**Scan and update documentation when:**
- New microservice added to any domain → Update domain architecture + projects-and-modules.md
- New submodule added to repo → Update projects-and-modules.md, copilot-instructions.md
- New ADR created → Update mkdocs.yml navigation, check for related doc updates
- New testing/observability module → Update relevant cross-cutting docs
- Phase implementation completed → Update phase status, roadmap progress

### 6. Cross-References Integrity

**Maintain link integrity:**
- When renaming files, search for all references across docs
- When removing modules, remove from all documentation files
- When restructuring, update relative paths
- Test links by building MkDocs site: `~/mkdocs-venv/bin/mkdocs build`

### 7. Documentation Sections Explained

#### 00-OVERVIEW
- **Purpose**: Entry point for new users, high-level project information
- **Key Files**: `README.md`, `project-charter.md`, `stakeholder-guide.md`, `projects-and-modules.md`
- **Update When**: New modules, major project changes, stakeholder updates

#### 01-ARCHITECTURE
- **Purpose**: Technical architecture, design decisions, domain models
- **Subsections**: 
  - Root: System-wide architecture (system-overview, data-architecture, security-architecture)
  - `domains/`: Domain-specific architectures (ecommerce, chat, iot, social, ml-ai)
  - `CROSS_*`: Cross-cutting concerns (observability, events, cost, data platform, replay/DR)
- **Update When**: Architecture changes, new domains, pattern changes

#### 02-INFRASTRUCTURE
- **Purpose**: Cloud strategy, deployment, infrastructure patterns
- **Update When**: Cloud provider changes, deployment updates, infra tooling changes

#### 03-DEVELOPMENT
- **Purpose**: Development guides, patterns, testing strategies
- **Key Files**: `microservices-patterns.md`, `api-protocols.md`, `testing-strategy.md`, `performance-testing-guide.md`
- **Update When**: New patterns adopted, testing strategies change

#### 04-STORIES (if exists)
- **Purpose**: User stories, quick start guides, phase tracking
- **Update When**: Phase progress, new learning tracks

#### 05-PROJECT-MANAGEMENT
- **Purpose**: Roadmaps, sprint planning, execution tracking
- **Key Files**: `IMPLEMENTATION-ROADMAP.md`, `WEEKLY-CHECKLIST.md`, `execution-roadmap.md`
- **Update When**: Roadmap changes, sprint updates, milestone completion

#### adr/
- **Purpose**: Architectural Decision Records (why decisions were made)
- **Naming**: ADR-[001-999]-[title].md
- **Update When**: Major technical decisions, pattern adoptions, tool selections
- **Always**: Add to mkdocs.yml navigation when creating new ADR

#### event-contracts/
- **Purpose**: Event schema definitions, versioning, usage guides
- **Update When**: New event types, schema changes, contract updates

## Common Documentation Tasks

### Task 1: Adding a New Module
```markdown
1. Update `docs/00-OVERVIEW/projects-and-modules.md`:
   - Add module under appropriate category
   - Include: name, type, purpose, usage
   - Add cross-links to relevant architecture docs

2. Update relevant domain architecture file:
   - Add service to architecture diagram
   - Document integration points
   - Update component list

3. Update `mkdocs.yml` if new navigation needed

4. Update parent `.github/copilot-instructions.md` reference
```

### Task 2: Creating New ADR
```markdown
1. Create `docs/adr/ADR-[NUM]-[title].md` using ADR-TEMPLATE.md
2. Add to `mkdocs.yml` navigation under ADRs section
3. Update relevant architecture docs to reference the ADR
4. Add to `docs/adr/GUIDE_ADR_STARTER_PACK.md` if pattern-establishing
```

### Task 3: Updating Phase Documentation
```markdown
1. Update `docs/stories/phases/PHASE-[N].md` with progress
2. Update `docs/05-PROJECT-MANAGEMENT/WEEKLY-CHECKLIST.md` checkboxes
3. Update `docs/05-PROJECT-MANAGEMENT/IMPLEMENTATION-ROADMAP.md` status
4. Reflect changes in `docs/00-OVERVIEW/README.md` if major milestone
```

### Task 4: Removing Obsolete Content
```markdown
1. Search for all references: grep -r "module-name" docs/
2. Update/remove references from:
   - projects-and-modules.md
   - Relevant architecture files
   - Phase documentation
   - Copilot instructions (both docs and parent)
3. Remove from mkdocs.yml navigation if applicable
4. Archive or delete the markdown file
```

## MkDocs Build & Serve

### Build documentation:
```bash
~/mkdocs-venv/bin/mkdocs build
```

### Preview locally:
```bash
~/mkdocs-venv/bin/mkdocs serve
# Open: http://127.0.0.1:8000/BitVelocity-Docs/
```

### Deploy to GitHub Pages:
```bash
~/mkdocs-venv/bin/mkdocs gh-deploy
```

## Integration with Parent Repository

This documentation repository is a **submodule** of the main BitVelocity repository. 

### When Parent Project Changes:
1. Parent project's `.github/copilot-instructions.md` should reference this docs repo
2. This repo's copilot instructions can be consulted independently
3. Changes to parent modules should trigger documentation updates here

### Referencing Pattern:
```markdown
From parent copilot-instructions.md:
"For documentation standards and module catalog, see BitVelocity-Docs/.github/copilot-instructions.md"

From this copilot-instructions.md:
"For actual module implementation details, see parent project's .github/copilot-instructions.md"
```

## Quality Checks Before Committing

- [ ] All module references are current and accurate
- [ ] No broken internal links (test with `mkdocs build`)
- [ ] New ADRs are in navigation
- [ ] Cross-references use correct relative paths
- [ ] Mermaid diagrams render correctly
- [ ] File naming follows conventions
- [ ] TOC/navigation is logical and complete

## Special Notes

- **Never hardcode absolute paths** - use relative paths for internal links
- **Keep projects-and-modules.md in sync** with actual repository structure
- **Update dates** when making significant content changes
- **Version ADRs properly** - they are immutable records of decisions
- **Test the build** before pushing to ensure no broken links

## Contact & Maintenance

- Documentation owner: BitVelocity maintainers
- Last major update: December 2025
- Review cycle: Quarterly or when major platform changes occur
