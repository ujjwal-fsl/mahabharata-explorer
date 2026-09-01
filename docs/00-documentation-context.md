# Mahābhārata Explorer — Documentation Context

## 1. Purpose

The Mahābhārata Explorer project is built upon a rigorous architectural foundation and extensive domain research. This documentation hierarchy establishes clear boundaries of authority, preserves architectural reasoning, ensures absolute information integrity, and guides both human contributors and AI agents. By strictly separating product requirements, architecture, detailed design reference, and historical audits, the project prevents speculative drift, feature creep, and unauthorized architectural changes.

---

## 2. Document Inventory

### 1. Master Consolidated Project Blueprint
- **Filename**: `Mahabharata_Explorer_Master_Consolidated_Blueprint.md`
- **Location**: `docs/01-project-blueprint/`
- **Role**: Executive architectural blueprint defining the product vision, core five-layer architecture, consolidated knowledge graph, UX loops, and visual philosophy.
- **Authority Level**: **Authoritative for overall product architecture, system consolidation, and project direction.**
- **Agent Usage**: Primary architectural reference for understanding how systems fit together, core entity definitions, data integrity rules, and scope boundaries.

### 2. 133-Point Consolidation & Architecture Audit
- **Filename**: `Mahabharata_Explorer_Phase_2_Consolidation_Audit_133_Points.md`
- **Location**: `docs/01-project-blueprint/`
- **Role**: Comprehensive architectural audit capturing the 133 analytical points, consolidation discoveries, and anti-duplication reasoning that formed the master blueprint.
- **Authority Level**: **Reference & Audit Document.** (Preserves historical audit reasoning; does not override the Master Blueprint or PRD).
- **Agent Usage**: Deep reference for understanding *why* specific architectural decisions were made, why systems were consolidated, and what traps were identified.

### 3. Detailed Master Reference
- **Filename**: `Mahabharata_Explorer_Detailed_Master_Reference.md`
- **Location**: `docs/02-detailed-reference/`
- **Role**: Exhaustive reference preserving detailed inventories, component breakdowns, state hierarchies, deep linking specifications, edge-case behaviors, visual-language methodology, and complete UI inventories.
- **Authority Level**: **Authoritative for established detailed decisions, UI/visual component inventories, and deep architectural reasoning.**
- **Agent Usage**: Detailed guide for component design, interaction states, visual asset abstraction guidelines, and exact edge-case handling rules.

### 4. Product Requirements Document (PRD)
- **Filename**: `Mahabharata_Explorer_PRD.md`
- **Location**: `docs/03-prd/`
- **Role**: Formal product requirements converting architectural decisions into concrete functional requirements (APP-xxx, TIM-xxx, CHR-xxx, etc.), scope boundaries, and acceptance criteria.
- **Authority Level**: **Authoritative for explicit product requirements, functional specifications, feature definitions, and V1/V2/V3 release scope.**
- **Agent Usage**: Definitive checklist for verifying what the product must do, acceptance criteria, definition of done, and explicit release boundaries.

---

## 3. Domain-Governed Authority Model

The project operates on a **domain-governed authority model** rather than a simple linear hierarchy where a higher document automatically overrides a lower one. Each document governs its respective domain:

| Domain / Area | Authoritative Document | Role & Authority Scope |
| :--- | :--- | :--- |
| **Overall Consolidated Architecture & System Design** | `Master Consolidated Project Blueprint`<br>`(docs/01-project-blueprint/)` | Authoritative for overall product architecture, 5-layer system design, knowledge graph model, and strategic project direction. |
| **Product Requirements & Scope Boundaries** | `Product Requirements Document (PRD)`<br>`(docs/03-prd/)` | Authoritative for explicit functional requirements (APP, TIM, CHR, etc.), acceptance criteria, feature definitions, and V1/V2/V3 release boundaries. |
| **Detailed UX, Visual Language & Component Inventories** | `Detailed Master Reference`<br>`(docs/02-detailed-reference/)` | Authoritative reference for established UX patterns, state hierarchy, visual-language philosophy, cultural abstraction rules, and complete UI component inventories. |
| **Historical & Consolidation Rationale** | `133-Point Consolidation Audit`<br>`(docs/01-project-blueprint/)` | Reference and audit document explaining the 133 analytical discoveries and anti-duplication reasoning. (Does not override the Blueprint or PRD). |
| **Technical Implementation** *(Future)* | `Technical Specifications`<br>`(docs/04-backend/, docs/05-frontend/, etc.)` | Will become authoritative for code architecture, API contracts, schemas, and build configurations once created and approved in subsequent stages. |

> [!IMPORTANT]
> Where documents address different domains, each governs its own domain. If an agent discovers an apparent contradiction across domains, it must not unilaterally override one document with another; it must record the contradiction and request human clarification.

---

## 4. How Agents Should Read the Documentation

When tasked with any design, evaluation, or implementation work, agents must consult the appropriate authoritative document:

- **For Product Requirements & Acceptance Criteria**: Consult `docs/03-prd/Mahabharata_Explorer_PRD.md`.
- **For Overall Architecture & System Boundaries**: Consult `docs/01-project-blueprint/Mahabharata_Explorer_Master_Consolidated_Blueprint.md`.
- **For Detailed Established Decisions & Component Inventories**: Consult `docs/02-detailed-reference/Mahabharata_Explorer_Detailed_Master_Reference.md`.
- **For Historical Context & Deduplication Reasoning**: Consult `docs/01-project-blueprint/Mahabharata_Explorer_Phase_2_Consolidation_Audit_133_Points.md`.
- **For Technical Implementation**: Consult the future technical specifications in `docs/04-backend/`, `docs/05-frontend/`, `docs/06-integration/`, and `docs/07-build/` once they are formally created and approved in subsequent stages.

---

## 5. Conflict Handling

Agents must **never silently reconcile conflicting requirements, architectural decisions, or factual data**. If an ambiguity, discrepancy, or contradiction between documents is discovered:

1. **Do not modify** the source documents.
2. **Do not choose** one interpretation unilaterally.
3. **Record and document** the contradiction with exact file locations and text citations.
4. **Request explicit human clarification** before proceeding.

---

## 6. Future Documentation

The following technical specification directories are reserved for upcoming project stages:

- `docs/04-backend/` — Backend architecture, data access layer, API contracts, and schema specifications.
- `docs/05-frontend/` — Frontend architecture, component implementations, state management, and design system integration.
- `docs/06-integration/` — Integration protocols, data ingestion pipelines, and cross-system communication models.
- `docs/07-build/` — Build system specifications, deployment configurations, and verification pipelines.

These directories are currently structural placeholders and must not be populated with speculative specifications until formally directed.

---

## 7. Current Project Stage

- **Current Stage**: `STAGE 0.2 — Documentation & Knowledge Context`
- **Status**: Project foundation and documentation hierarchy mapping. Implementation has **NOT** begun. No application code, schemas, frameworks, or dependencies are to be created in Stage 0.
