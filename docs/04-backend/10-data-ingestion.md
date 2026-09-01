# Data Ingestion & Curation Architecture (Block B10)

## 1. Purpose

This document establishes the **Data Ingestion & Curation Architecture** for the **Mahābhārata Explorer** backend. It specifies the multi-stage ingestion lifecycle, staging and validation pipelines, data quality gates, referential integrity verification, provenance validation, and publication safeguards governing how scholarly data enters the canonical PostgreSQL knowledge graph.

In accordance with project principles:
- **Zero Fabrication (Rule 03, PRD §11, REQ-ING-02)**: The ingestion pipeline is the primary structural safeguard against data fabrication. Unverified facts, speculative dates, invented coordinates, uncorroborated relationships, and synthetic citations must be intercepted and rejected before entering the published dataset.
- **One Knowledge Graph, Offline Curation Boundary (Rule 01, REQ-ING-03, B7 §4.1)**: Data curation operates strictly as an offline/administrative batch process outside public runtime query paths.
- **System Reuse & Provenance Fidelity (Rule 02, REQ-ING-04, B4 §2)**: Incremental additions of new scholarly editions, variant traditions, and archaeological research must integrate seamlessly into the existing Entity $\rightarrow$ Claim $\rightarrow$ Evidence $\rightarrow$ Source provenance architecture without requiring schema rewrites or visualization redesigns.

---

## 2. Ingestion Pipeline Architecture & 5-Stage Lifecycle

The ingestion process transitions source research into published canonical data through five deterministic, sequential stages:

```
┌────────────────────────────────────────────────────────────────────────┐
│                      5-STAGE INGESTION LIFECYCLE                       │
├────────────────────────────────────────────────────────────────────────┤
│ STAGE 1: STRUCTURED AUTHORING & STAGING                                │
│ - Scholarly source extraction into structured staging files (JSON/YAML)│
│ - Strict separation of canonical domain data from authoring notes      │
├──────────────────────────────────┬─────────────────────────────────────┤
│ STAGE 2: SYNTACTIC & TYPE GATES  │ STAGE 3: GRAPH & REFERENTIAL GATES  │
├──────────────────────────────────┼─────────────────────────────────────┤
│ - Strict schema & type parsing   │ - Slug uniqueness verification      │
│ - Enum & controlled vocabulary   │ - Foreign key reference resolution  │
│ - Mandatory field validation     │ - Polymorphic endpoint compatibility│
│ - Slug format & naming syntax    │ - Lineage DAG acyclicity check      │
├──────────────────────────────────┴─────────────────────────────────────┤
│ STAGE 4: PROVENANCE & EPISTEMIC FIDELITY GATES                         │
│ - Entity/Edge → Claim → Evidence → Source link verification            │
│ - Native citation locator preservation check (B4 §5)                   │
│ - Epistemic status consistency (explicit unknown/conflicting states)   │
├────────────────────────────────────────────────────────────────────────┤
│ STAGE 5: ATOMIC PUBLICATION & VERSIONING                               │
│ - Required database-level integrity checks and publication operations  │
│ - Generation of dataset release version & cache epoch trigger (B9 §7)  │
│ - Structured administrative audit logging (B7 §12)                     │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Canonical Staging Data Formats

We evaluate structured data formats for authoring and staging curated data:

### 3.1. Evaluation of Staging Formats

| Format | Evaluation & Characteristics | V1 Ingestion Status |
| :--- | :--- | :--- |
| **JSON / JSONC** | **Pros**: Universal native parser in Node.js/TypeScript; strict type mappings; direct mapping to PostgreSQL JSONB/relational rows.<br>**Cons**: Verbose syntax for manual editing. | **SELECTED CANONICAL FORMAT** |
| **YAML** | **Pros**: Human-readable, excellent for manual scholarly curation.<br>**Cons**: Parsing ambiguities (type coercion of dates/strings). | **SUPPORTED AUTHORING FORMAT** (compiled to JSON) |
| **CSV / TSV** | **Pros**: Familiar tabular representation.<br>**Cons**: Unsuitable for nested arrays (`alternate_names`), polymorphic links, and multi-tier provenance. | **REJECTED FOR CORE GRAPH DATA** |

### 3.2. Staging File Organization
Curated source datasets are organized in structured staging directories:
- `data/seeds/characters.json`
- `data/seeds/events.json`
- `data/seeds/locations.json`
- `data/seeds/groups.json`
- `data/seeds/wars.json`
- `data/seeds/war_days.json`
- `data/seeds/formations.json`
- `data/seeds/sources.json`
- `data/seeds/claims.json`
- `data/seeds/evidence.json`
- `data/seeds/relationships.json`
- `data/seeds/family_relationships.json`
- `data/seeds/event_participants.json` *(Staging representation for relational participant associations linking `Event` and `Character`; publication maps this into the canonical data model established by B2/B3 rather than introducing a separate top-level entity)*.

---

## 4. Schema & Data Quality Validation Gates (REQ-ING-02)

To satisfy **REQ-ING-02**, data must pass through two distinct classes of quality gates: **Hard Blocking Gates** and **Soft Review Warnings**.

```
┌────────────────────────────────────────────────────────────────────────┐
│                       DATA QUALITY GATE TAXONOMY                       │
├───────────────────────────────────┬────────────────────────────────────┤
│ HARD GATES (PUBLICATION BLOCKING) │ SOFT WARNINGS (REVIEW REQUIRED)    │
├───────────────────────────────────┼────────────────────────────────────┤
│ 1. Schema / Type Mismatch         │ 1. Unmapped Location Coordinates   │
│ 2. Broken Foreign Key References  │    (Allowed: coordinate_status set)│
│ 3. Duplicate Entity Slugs         │ 2. Disputed Relative Chronology    │
│ 4. Invalid Polymorphic Pair       │    (Allowed: chronology_status set)│
│ 5. Cyclic Parent-Child Lineage    │ 3. Conflicting Scholarly Claims    │
│ 6. Unlinked Claim or Evidence     │    (Allowed: epistemic_status set) │
│ 7. Invalid Directionality for     │ 4. Missing Optional Description    │
│    Symmetric Types (B3 Spec)      │ 5. Character without Portrait      │
│ 8. Missing Native Locator Text    │                                    │
└───────────────────────────────────┴────────────────────────────────────┘
```

### 4.1. Hard Quality Gates (Publication Blockers)
Any violation of a Hard Gate immediately halts the ingestion pipeline with a non-zero exit code and halts publication:

1. **Syntactic & Type Integrity**: All records must strictly conform to the TypeScript/Zod schema definitions established in [02-data-architecture.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/02-data-architecture.md).
2. **Slug Uniqueness**: Slugs must be globally unique within their entity domain, kebab-case, and URL-safe (`^[a-z0-9-]+$`).
3. **Polymorphic Referential Integrity**: Every `(source_entity_type, source_entity_id)` and `(target_entity_type, target_entity_id)` in `relationships` and `claims` must resolve to an existing canonical record and satisfy the compatibility matrix in [03-knowledge-graph.md §5.2](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/03-knowledge-graph.md#L125).
4. **Generational Acyclicity**: Invalid cycles within relationship types that are required to represent acyclic generational/ancestry structures (e.g., parent-child lineage in `family_relationships`) are strictly prohibited.
5. **Relationship Directionality Integrity**: Invalid directionality for relationship types declared symmetric in the B3 relationship-type specification (e.g., `allied_with` in [03-knowledge-graph.md §4.1](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/03-knowledge-graph.md#L88)) is strictly prohibited.
6. **Provenance Completeness (B4 §2)**: Every `Claim` must have a valid `source_id`; every `Evidence` must link to a valid `claim_id` and `source_id`; every disputed `Relationship` must link to a valid `claim_id`.
7. **Native Citation Preservation (B4 §5)**: `Evidence.locator` must not be empty or null and must preserve the source edition's native citation string verbatim.
8. **Prohibition of Narrative Co-occurrence Inference (Rule 03, B3 §1)**: Graph relationships must be backed by explicit textual evidence claims, never auto-inferred from co-appearance in an event summary.

---

## 5. Zero-Fabrication & Epistemic State Enforcement (Rule 03)

The ingestion pipeline enforces strict truthfulness standards at the boundary of data entry:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   ZERO-FABRICATION INGESTION RULES                     │
├────────────────────────────────────────────────────────────────────────┤
│ 1. NO SILENT GUESSING: Missing values must be stored as NULL with an   │
│    explicit epistemic status ('unknown', 'not_researched', etc.).      │
│ 2. NO SPECULATIVE COORDINATES: If ancient location is disputed,        │
│    coordinates must be NULL with coordinate_status = 'unmapped' or     │
│    'approximate'.                                                      │
│ 3. NO GENERATIVE SYNTHESIS: Descriptions must be human-curated and     │
│    academically referenced; zero AI hallucinations.                    │
│ 4. PRESERVATION OF CONFLICTING TRADITIONS: Variant accounts (e.g.,     │
│    Critical Edition vs Vulgate) must be ingested as separate Claim     │
│    records with epistemic_status = 'conflicting', never averaged.      │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 6. Incremental Ingestion & Deterministic Idempotency (REQ-ING-04)

In accordance with **REQ-ING-04**, the ingestion pipeline requires deterministic, idempotent processing of unchanged canonical input:

### 6.1. Deterministic Identity & Duplicate Prevention
- Stable identity keys must prevent duplicate canonical entities, relationships, claims, and evidence across repeated ingestion runs.
- Entities are matched deterministically by canonical `slug`.
- Canonical relationships are matched deterministically by composite endpoint identifiers.
- Claim and Evidence records must maintain stable identity resolution to prevent duplicate citation assertions.
- Concrete identity mapping and upsert mechanics are deferred to implementation.

### 6.2. Non-Disruptive Incremental Scholarly Ingestion
- When adding a newly researched Sanskrit commentary or critical edition:
  1. Add new record to `sources.json`.
  2. Add new claims and verse excerpts to `claims.json` and `evidence.json`.
  3. Execute validation pipeline: newly linked evidence seamlessly enhances entity provenance cards without modifying frontend components or database schemas.

---

## 7. Media Asset Ingestion Coordination (B8 Alignment)

In alignment with the environment-configurable asset hosting architecture in [08-storage-and-media.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/08-storage-and-media.md):
1. **Asset Reference Resolution**:
   - Ingestion scripts verify that any media asset referenced by `portrait_url`, `metadata.diagram_url`, or spatial overlays resolves successfully against the configured asset source appropriate to the environment (such as `public/assets/...` in local development).
2. **SVG Sanitization Gate**:
   - All SVG files referenced by formation or icon records must pass automated XML sanitization (stripping `<script>`, event handlers, external entity definitions) before publication.
3. **Zero Binary Storage**:
   - Ingestion scripts store string URIs only in PostgreSQL columns; binary files are never inserted into database rows.

---

## 8. Atomic Publication, Release Versioning & Rollback

### 8.1. Atomic Publication Principle
Publication must be atomic: a dataset release either becomes fully published after all required quality and referential gates pass, or remains unpublished. The concrete transactional batch mechanism is deferred to implementation.

### 8.2. Release Lifecycle & Cache Invalidation Flow (B9 Alignment)
The publication lifecycle integrates directly with the performance and caching model established in [09-performance-and-caching.md §7](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/09-performance-and-caching.md#L164):

$$\text{Staged Dataset} \longrightarrow \text{Validation Gates} \longrightarrow \text{Approved Release} \longrightarrow \text{Atomic Publication} \longrightarrow \text{Release Version Tag} \longrightarrow \text{Cache Epoch / ETag Invalidation}$$

1. **Release Version Identifier**: Successful publication tags the database state with a release identifier (e.g., semantic version `release-v1.0.0` or git commit hash).
2. **Cache Epoch & ETag Invalidation**: Updates to canonical facts, relationships, claims, or epistemic statuses increment the cache release version, ensuring modified `ETags` trigger appropriate HTTP cache revalidation.
3. **Administrative Audit Logging**: All publication events produce structured audit logs containing timestamp, operator ID, record counts, and validation metrics (B7 §12).
4. **Rollback Principle**: If an issue is discovered post-publication, rollback is executed by re-publishing a known valid prior release version.

---

## 9. Seed Dataset Handoff Boundary (Block B10 vs. Block B11)

The backend roadmap establishes a clean separation of concerns between ingestion architecture and seed data population:

| Architectural Responsibility | Block B10 (This Document) | Block B11 (Subsequent Block) |
| :--- | :--- | :--- |
| **Pipeline Governance & Lifecycle** | **ESTABLISHED (B10)** | Follows B10 rules |
| **Data Quality & Validation Gates** | **ESTABLISHED (B10)** | Enforced during seed load |
| **Provenance & Citation Rules** | **ESTABLISHED (B10)** | Populated with verified citations |
| **Actual Seed Dataset Population** | *Deferred to B11* | **CREATED & POPULATED (B11)** |
| **Representative Test Corpus Content** | *Deferred to B11* | **AUTHORED (B11)** |

---

## 10. Requirement Traceability Matrix

| Ingestion Requirement | Source Document | Implemented Architectural Mechanism |
| :--- | :--- | :--- |
| **Structured Dataset Ingestion** | Context §4.N (REQ-ING-01); Blueprint §14 | Section 2, Section 3: 5-stage lifecycle and JSON/YAML staging format. |
| **Pre-Publication Quality Gates** | Context §4.N (REQ-ING-02); PRD §10 | Section 4: Hard blocking gates and soft review warnings. |
| **Offline Ingestion Boundary** | Context §4.N (REQ-ING-03); B7 §4.1 | Section 1, Section 8: CLI execution boundary outside public runtime paths. |
| **Incremental Scholarly Addition** | Context §4.N (REQ-ING-04); Detailed Ref §14 | Section 6: Idempotent matching and non-disruptive evidence linkage. |
| **Provenance Integrity** | B4 §2; PRD §9.10 | Section 4.1 (Gate 6): Mandatory Entity $\rightarrow$ Claim $\rightarrow$ Evidence $\rightarrow$ Source link validation. |
| **Zero Fabrication Safeguards** | Rule 03; PRD §11 | Section 5: Explicit epistemic states, unmapped coordinate handling, zero AI synthesis. |
| **Release Cache Invalidation** | B9 §7 | Section 8: Dataset release tagging triggering ETag cache revalidation. |

---

## 11. Decisions Resolved & Deferred

### Decisions Resolved in Block B10:
1. **RESOLVED B10-01**: Established the **5-Stage Ingestion Lifecycle** (Authoring $\rightarrow$ Syntactic Validation $\rightarrow$ Graph Integrity $\rightarrow$ Provenance Verification $\rightarrow$ Atomic Publication).
2. **RESOLVED B10-02**: Selected **Strongly Typed JSON / JSONC** as the canonical staging format, with YAML authoring support.
3. **RESOLVED B10-03**: Established the **Two-Tier Quality Gate Taxonomy** (Hard Blockers vs. Soft Review Warnings).
4. **RESOLVED B10-04**: Formulated **Zero-Fabrication Ingestion Invariants** (explicit null/epistemic states, multi-claim conflict preservation).
5. **RESOLVED B10-05**: Defined **Deterministic Idempotency & Stable Identity Principles**.
6. **RESOLVED B10-06**: Established the **Seed Dataset Handoff Boundary** between B10 (governance/pipeline) and B11 (seed population).

### Decisions Deferred to Subsequent Blocks:
1. **Authoring of the Representative V1 Seed Dataset** → *Deferred to Block B11 (Seed Dataset)*.
2. **Concrete Validation CLI Tool Implementation (TypeScript / Zod scripts)** → *Deferred to Stage 4 (Backend Implementation)*.
3. **Synthetic Validation Error Test Suite** → *Deferred to Block B12 (Backend Testing)*.
