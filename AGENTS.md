# Mahābhārata Explorer — Project Constitution & Agent Principles

This document establishes the high-level project constitution and core principles for all agents and contributors working on the Mahābhārata Explorer codebase.

## Core Principles

1. **Project Identification**: This is the Mahābhārata Explorer project.
2. **Unified Knowledge Graph**: The product is based on **ONE interconnected knowledge graph** with **MANY exploration lenses**.
3. **Information Integrity**: Information integrity is critical.
4. **Zero Fabrication**: Never fabricate:
   - facts
   - dates
   - coordinates
   - relationships
   - source claims
   - certainty
5. **System Reuse**: Reuse shared systems rather than creating duplicate systems.
6. **Architectural Discipline**: Do not introduce new architecture or major systems without explicit approval.
7. **Document Guidance**: Follow the project's approved documentation and PRD.
8. **Scope Boundaries**: Respect V1/V2/V3 scope boundaries.
9. **Responsiveness**: Responsive behavior is a first-class requirement.
10. **Accessibility**: Accessibility is a first-class requirement.
11. **Visual Philosophy**: **MODERN FIRST. TRADITIONAL SECOND.**
12. **Meaningful Culture**: Cultural elements must have meaningful/semantic purpose rather than being decorative for decoration's sake.
13. **Strict Truthfulness**: Do not treat missing information as permission to invent information.
14. **Verification Requirement**: Do not declare a feature complete merely because it compiles; implementation must eventually be tested against its requirements.
15. **Setup Scope**: During this setup task, do not implement any application functionality.

## Persistent Agent Rules

Detailed persistent rules governing development behavior are located in `.agents/rules/`:
- `01-project-principles.md` — Core identity, exploration loop, and operating principles
- `02-architecture.md` — 5-layer architecture, unified knowledge graph, and system reuse
- `03-data-integrity.md` — Zero fabrication, epistemic data states, and truth standards
- `04-scope-discipline.md` — Phased roadmap stages and V1/V2/V3 scope boundaries
- `05-frontend.md` — Component reuse, state hierarchy, and data-driven UI
- `06-responsive.md` — Multi-device support and interaction adaptation
- `07-visual-language.md` — Modern First / Traditional Second and cultural guidelines
- `08-testing.md` — Quality gates, verification checklist, and definition of done
