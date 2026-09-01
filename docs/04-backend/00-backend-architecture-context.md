# Backend Architecture Context

## 1. Purpose

This document establishes the comprehensive architectural requirements, constraints, epistemic obligations, and planning matrix for the backend of the **Mahābhārata Explorer**. 

The goal of **Stage 1.0** is to define *what* capabilities, data structures, query patterns, and integrity boundaries the backend must satisfy before any technology stack selections (databases, frameworks, APIs, search engines, hosting) are made in subsequent Stage 1 work blocks (B1–B12).

---

## 2. Backend Role in the Overall Architecture

In the project's established **Five-Layer Architecture**, the backend is the authoritative custodian of the first two foundational layers:

1. **Layer 1: Data Layer (What exists)** — Manages core canonical entities (Characters, Events, Locations, Groups, Relationships, FamilyRelationships, Wars, WarDays, Formations).
2. **Layer 2: Evidence Layer (Why it is represented)** — Governs the epistemic truth model (`Entity → Claim → Evidence → Source`), preserving citations, uncertainty, provenance, and variant traditions.

Additionally, the backend serves the **Exploration Layer (Layer 3)** by exposing high-performance query capabilities for timelines, geographic maps, relationship graphs, full-text search, global focus traversal, and deep-link route resolution, while enforcing data validation rules independently of presentation or user state.

```
┌─────────────────────────────────────────────────────────────┐
│                 Layer 5: User State Layer                   │
├─────────────────────────────────────────────────────────────┤
│                 Layer 4: Presentation Layer                 │
├─────────────────────────────────────────────────────────────┤
│                 Layer 3: Exploration Layer                  │
├─────────────────────────────────────────────────────────────┤
│   BACKEND RESPONSIBILITY:                                   │
│   Layer 2: Evidence Layer (Claims, Evidence, Sources)       │
│   Layer 1: Data Layer (Core Knowledge Graph & Entities)     │
└─────────────────────────────────────────────────────────────┘
```

---

## 3. Backend Requirements by Category

Every requirement is classified by type:
- **MUST**: Mandatory requirement for V1.
- **SHOULD**: Strongly recommended architectural practice for V1.
- **MAY**: Optional capability or future consideration.
- **CONSTRAINT**: Non-negotiable architectural boundary or negative constraint.
- **OPEN DECISION**: Requirement needing technical evaluation in a future block.

---

### A. Core Product & Architectural Requirements

| ID | Requirement | Classification | Source |
| :--- | :--- | :--- | :--- |
| **REQ-CORE-01** | Support **ONE unified Knowledge Graph** queried across multiple exploration lenses (Time, Space, People, Relationships, Events, Evidence). | **MUST** | Blueprint §1, §5; PRD §2 |
| **REQ-CORE-02** | Enforce **System Reuse**: A single shared data model and API layer for Events (including War Events), Maps, Timelines, Search, and Focus. | **MUST** | Blueprint §9.3; PRD §8; Rule 02 |
| **REQ-CORE-03** | Frontend must not be the source of truth; all data validation and integrity checks must be enforced at the backend/data layer. | **MUST** | Detailed Ref §19; PRD §17 |
| **REQ-CORE-04** | Prevent duplicate engines (e.g., no distinct "War Event" entity or separate "Character Journey Map" backend). | **CONSTRAINT** | Blueprint §9.3; 133-Point Audit §9–15 |

---

### B. Data & Entity Requirements

| ID | Requirement | Classification | Source |
| :--- | :--- | :--- | :--- |
| **REQ-DAT-01** | Support V1 Core Entities: `Character`, `Event`, `Location`, `Group`, `Relationship`, `FamilyRelationship`, `War`, `WarDay`, `Formation`, `Source`, `Claim`, `Evidence`. | **MUST** | Blueprint §11; PRD §6.1 |
| **REQ-DAT-02** | Provide unique, stable identifiers and human-readable slugs for canonical URL routing across all major entities. | **MUST** | Blueprint §11; PRD §12 |
| **REQ-DAT-03** | Support entity aliases, alternate names, and transliteration variations (e.g., Arjuna / Partha / Phalguna). | **MUST** | Blueprint §11; PRD §9.8 (SRCH-001) |
| **REQ-DAT-04** | Represent explicit epistemic data states: `Known`, `Unknown`, `Not Researched`, `Not Applicable`, `Conflicting`, and `Approximate`. | **MUST** | Blueprint §12; Detailed Ref §15; PRD §11; Rule 03 |
| **REQ-DAT-05** | Distinguish between a missing value, an unresearched field, and an affirmatively unknown historical attribute. | **MUST** | Detailed Ref §15; PRD §11 |
| **REQ-DAT-06** | Preserve extensible metadata payloads on all core entity records for future annotation without breaking baseline schemas. | **SHOULD** | Blueprint §11 |
| **REQ-DAT-07** | Defer future non-V1 entities (`Journey`, `DatasetVersion`, `EditorialNote`, `VariantAccount`) to V2/V3. | **CONSTRAINT** | Blueprint §9.1; PRD §6.2 |

---

### C. Knowledge Graph Requirements

| ID | Requirement | Classification | Source |
| :--- | :--- | :--- | :--- |
| **REQ-GRP-01** | Model relationships as **first-class data records** (`source_entity`, `target_entity`, `relationship_type`, `directionality`, `claim_id`, `metadata`). | **MUST** | Blueprint §11; PRD §6.3, §9.5 (REL-002) |
| **REQ-GRP-02** | Support bi-directional and directed traversal across entity types (`Character ↔ Character`, `Character ↔ Event`, `Event ↔ Location`, `Event ↔ WarDay`, etc.). | **MUST** | Blueprint §9.2; PRD §6.3 |
| **REQ-GRP-03** | Provide graph querying to support Focus mode sub-graphs (focused entity, direct connections, and second-degree relationships). | **MUST** | PRD §9.9 (FOC-002); Detailed Ref §7.1 |
| **REQ-GRP-04** | Support specialized family relationships (`FamilyRelationship`) navigable as generational trees over the graph data. | **MUST** | Blueprint §11; PRD §9.5 (REL-004) |
| **REQ-GRP-05** | Never infer or generate factual graph edges from co-occurrence, narrative proximity, or layout positions. | **CONSTRAINT** | Blueprint §12; PRD §6.3, §9.5 (REL-005); Rule 03 |

---

### D. Evidence & Provenance Requirements

| ID | Requirement | Classification | Source |
| :--- | :--- | :--- | :--- |
| **REQ-EVD-01** | Enforce the cross-cutting `Entity → Claim → Evidence → Source` provenance architecture across all facts, relationships, and events. | **MUST** | Blueprint §10; PRD §6.1, §9.10 (SRC-004) |
| **REQ-EVD-02** | Store source bibliographic metadata (`title`, `author`, `source_type`, `publication_info`, `identifier`, `url`, `summary`). | **MUST** | Blueprint §11; PRD §9.10 (SRC-001) |
| **REQ-EVD-03** | Store granular evidence locators (Parva, Adhyaya, Shloka/verse numbers, page, translation notes) and textual excerpts. | **MUST** | Blueprint §11; PRD §9.10 (SRC-001) |
| **REQ-EVD-04** | Support claim-level epistemic metadata including certainty levels and assessment annotations. | **MUST** | Blueprint §11; PRD §9.10 (SRC-005) |
| **REQ-EVD-05** | Represent conflicting accounts across differing editions or traditions as distinct linked claims rather than overwriting. | **MUST** | Blueprint §12; PRD §9.10 (SRC-005); Rule 03 |
| **REQ-EVD-06** | Clearly distinguish human source-derived claims from AI-generated or algorithmic inferences. | **MUST** | Blueprint §10; Detailed Ref §19; PRD §10 |

---

### E. Timeline Requirements

| ID | Requirement | Classification | Source |
| :--- | :--- | :--- | :--- |
| **REQ-TIM-01** | Store event chronology supporting exact sequence indices, date values, and explicit date precision attributes. | **MUST** | Blueprint §11; PRD §9.2 (TIM-001, TIM-007) |
| **REQ-TIM-02** | Support relative narrative sequencing when absolute calendar dates are unknown or disputed (no false precision). | **MUST** | Blueprint §12; PRD §9.2 (TIM-007); Rule 03 |
| **REQ-TIM-03** | Provide queries supporting level-of-detail aggregation (major era markers vs individual granular events) for progressive loading. | **SHOULD** | Blueprint §13.2; PRD §9.2 (TIM-008) |

---

### F. Character & Entity Requirements

| ID | Requirement | Classification | Source |
| :--- | :--- | :--- | :--- |
| **REQ-CHR-01** | Retrieve full character profiles with connected relationships, event participations, group memberships, and source claims. | **MUST** | PRD §9.3 (CHR-001–CHR-005) |
| **REQ-CHR-02** | Support lookup by canonical slug or alternate names/aliases. | **MUST** | PRD §9.3 (CHR-006) |
| **REQ-CHR-03** | Serve character portrait URLs where curated; return null/empty gracefully without placeholder fabrication. | **MUST** | Blueprint §12; PRD §11; Rule 03 |

---

### G. Location & Map Requirements

| ID | Requirement | Classification | Source |
| :--- | :--- | :--- | :--- |
| **REQ-LOC-01** | Store geographic locations with name, aliases, location type, summary, and optional latitude/longitude coordinates. | **MUST** | Blueprint §11; PRD §9.6 (MAP-001) |
| **REQ-LOC-02** | Handle unmapped/disputed locations gracefully (allow null coordinates; never invent coordinates to satisfy a map). | **CONSTRAINT** | Blueprint §12; PRD §9.6 (MAP-005); Rule 03 |
| **REQ-LOC-03** | Retrieve location-specific events, participating characters, and event-location sequences (derived from Event-Location data without a separate V1 Journey entity). | **MUST** | PRD §6.2, §9.6 (MAP-002, MAP-004); Blueprint §9.1 |
| **REQ-LOC-04** | Provide spatial query capabilities supporting map bounding boxes, clustering, and contextual layer filtering. | **SHOULD** | Blueprint §13.2; PRD §9.6 (MAP-006) |

---

### H. War Explorer Requirements

| ID | Requirement | Classification | Source |
| :--- | :--- | :--- | :--- |
| **REQ-WAR-01** | Store major war structures (`War` entity and ordered `WarDay` 1–18 subdivisions). | **MUST** | Blueprint §11; PRD §9.7 (WAR-001, WAR-002) |
| **REQ-WAR-02** | Connect individual battlefield occurrences to the shared `Event` model filtered by `war_id` and `war_day_id`. | **MUST** | Blueprint §9.3; PRD §6.4, §9.4 (EVT-008) |
| **REQ-WAR-03** | Retrieve progressive day-by-day payloads (events, commanders, fallen warriors, faction movements) without loading all 18 days at once. | **MUST** | Blueprint §13.2; PRD §9.7 (WAR-006) |
| **REQ-WAR-04** | Store battlefield formation data (`Formation`) linked to events and claims; treat visual geometry as conditional on data readiness. | **MUST** | Blueprint §11; PRD §9.7 (WAR-005) |

---

### I. Relationship Requirements

| ID | Requirement | Classification | Source |
| :--- | :--- | :--- | :--- |
| **REQ-REL-01** | Support diverse typed relationships (parent-child, sibling, marriage, teacher-student, alliance, rivalry, combatant, allegiance). | **MUST** | Detailed Ref §12; PRD §9.5 (REL-002) |
| **REQ-REL-02** | Store relationship directionality (directed, symmetric/mutual). | **MUST** | Blueprint §11 |
| **REQ-REL-03** | Link every disputed or significant relationship to its backing `claim_id`. | **MUST** | Blueprint §11; PRD §9.5 (REL-002) |

---

### J. Global Search Requirements

| ID | Requirement | Classification | Source |
| :--- | :--- | :--- | :--- |
| **REQ-SRC-01** | Index and search across all primary entity types (`Character`, `Event`, `Location`, `Group`, `War`, `Source`). | **MUST** | PRD §9.8 (SRCH-001) |
| **REQ-SRC-02** | Search matching on primary names, alternate names, epithets, aliases, and summaries. | **MUST** | Blueprint §11; PRD §9.8 (SRCH-001) |
| **REQ-SRC-03** | Return ranked results with explicit entity type identification and canonical route links. | **MUST** | PRD §9.8 (SRCH-002, SRCH-004) |
| **REQ-SRC-04** | Support fast prefix / autocomplete query suggestions for search-as-you-type. | **SHOULD** | Detailed Ref §12; PRD §9.8 |
| **REQ-SRC-05** | Provide entity type and contextual filtering within search results. | **MUST** | 133-Point Audit §15; PRD §9.8 |

---

### K. User State & Focus Backend Requirements

| ID | Requirement | Classification | Source |
| :--- | :--- | :--- | :--- |
| **REQ-STA-01** | Support stateless exploration: all primary views and canonical entity routes must be resolvable via standard URL parameters/routes without requiring active session state. | **MUST** | Detailed Ref §7.2, §7.3; PRD §12 |
| **REQ-STA-02** | User preference settings (theme, density, motion, text size) are client-managed in V1; backend must never let client settings modify canonical facts or provenance. | **CONSTRAINT** | Blueprint §12; PRD §9.11 (SET-007) |
| **REQ-STA-03** | User accounts, saved bookmarks, and custom notes are explicitly deferred to V2/V3. | **CONSTRAINT** | Detailed Ref §20.3; PRD §18.2 |

---

### L. Authentication & Authorization Requirements

| ID | Requirement | Classification | Source |
| :--- | :--- | :--- | :--- |
| **REQ-AUT-01** | Public exploration access requires **no authentication** (100% anonymous, read-only public access). | **MUST** | Detailed Ref §19.1; PRD §17 |
| **REQ-AUT-02** | Public user registration, public comments, and public wiki-style editing are strictly excluded from V1. | **CONSTRAINT** | Detailed Ref §19.1; PRD §17, §19 |
| **REQ-AUT-03** | Administrative/curation operations (data ingestion, dataset publishing) must be secured and restricted to authorized project owners. | **MUST** | Detailed Ref §19.2; PRD §17 |

---

### M. Media & Storage Requirements

| ID | Requirement | Classification | Source |
| :--- | :--- | :--- | :--- |
| **REQ-MED-01** | Store optimized references/URIs for visual assets (character portraits, symbolic icons, battlefield diagrams, map tiles). | **MUST** | Blueprint §11; Detailed Ref §27 |
| **REQ-MED-02** | Serve static cultural assets (SVGs, illustrations) via lightweight, cache-friendly storage. | **SHOULD** | Detailed Ref §27 |
| **REQ-MED-03** | Heavy 3D assets and video-game simulations are strictly prohibited from V1. | **CONSTRAINT** | Detailed Ref §20.5; PRD §19 |

---

### N. Data Ingestion & Curation Requirements

| ID | Requirement | Classification | Source |
| :--- | :--- | :--- | :--- |
| **REQ-ING-01** | Support structured ingestion of curated datasets (seed dataset from initial research documents). | **MUST** | Blueprint §14; PRD §20 |
| **REQ-ING-02** | Enforce schema validation and data quality gates before records enter the published dataset. | **MUST** | Blueprint §10.1; Detailed Ref §14.3; PRD §10 |
| **REQ-ING-03** | Ingestion pipeline must operate outside runtime user queries (curated offline/administrative ingestion pipeline). | **MUST** | Blueprint §10.1; 133-Point Audit §121; PRD §10 |
| **REQ-ING-04** | Support incremental addition of credible scholarly sources and evidence without requiring visualization redesign. | **MUST** | Blueprint §14; Detailed Ref §14.2, §22.3 |

---

### O. Performance & Scale Requirements

| ID | Requirement | Classification | Source |
| :--- | :--- | :--- | :--- |
| **REQ-PRF-01** | Support progressive disclosure payloads (lightweight summary cards first, full entity graphs and evidence on demand). | **MUST** | Blueprint §13.2; Detailed Ref §16; PRD §16 |
| **REQ-PRF-02** | Fast read latencies for high-frequency navigation queries (entity detail, timeline slices, search queries). | **MUST** | PRD §16 |
| **REQ-PRF-03** | Explicit numeric latency and throughput budgets: *Not explicitly specified in PRD; to be formalized in Block B9.* | **OPEN DECISION** | PRD §16, §24 |
| **REQ-PRF-04** | Enable caching of static and rarely-changing canonical graph entities at edge/API layers. | **SHOULD** | Detailed Ref §16; PRD §16 |

---

### P. Accessibility-Related Backend Requirements

| ID | Requirement | Classification | Source |
| :--- | :--- | :--- | :--- |
| **REQ-ACC-01** | All entity payloads must include clean text labels, summaries, and transcripts; meaning must never rely solely on color codes or binary imagery. | **MUST** | Blueprint §13.3; Detailed Ref §17; PRD §15 |
| **REQ-ACC-02** | Provide explicit textual alternatives for formation structures and spatial relationships when visual rendering is unsupported. | **MUST** | Blueprint §12; Detailed Ref §17; PRD §15 |

---

### Q. Security & Privacy Requirements

| ID | Requirement | Classification | Source |
| :--- | :--- | :--- | :--- |
| **REQ-SEC-01** | Prevent data tampering: read-only public endpoints must have zero ability to execute mutations on canonical datasets. | **MUST** | Detailed Ref §19; PRD §17 |
| **REQ-SEC-02** | Secure administrative endpoints and ingestion scripts using robust authentication and environment secret management. | **MUST** | PRD §17; Rule 03 |
| **REQ-SEC-03** | Never expose internal tokens, credentials, or private configuration in client responses or public repositories. | **CONSTRAINT** | PRD §17; Rule 03 |

---

### R. API & Integration Requirements

| ID | Requirement | Classification | Source |
| :--- | :--- | :--- | :--- |
| **REQ-API-01** | Expose entity retrieval capabilities (by ID, by canonical slug, with configurable relationship depth). | **MUST** | PRD §7.1, §9.3, §9.4 |
| **REQ-API-02** | Expose timeline slice and range query capabilities with level-of-detail filtering. | **MUST** | PRD §9.2 |
| **REQ-API-03** | Expose geographic location and bounding box query capabilities. | **MUST** | PRD §9.6 |
| **REQ-API-04** | Expose War Day progressive payload queries (Day overview, participants, events). | **MUST** | PRD §9.7 |
| **REQ-API-05** | Expose global search and autocomplete query capabilities with type filtering and pagination. | **MUST** | PRD §9.8 |
| **REQ-API-06** | Expose contextual evidence and citation retrieval for any given claim or entity. | **MUST** | PRD §9.10 |
| **REQ-API-07** | Maintain API style neutrality (REST vs GraphQL vs RPC) until Block B5 evaluation. | **CONSTRAINT** | Stage 1.0 Charter |

---

### S. Testing & Verification Requirements

| ID | Requirement | Classification | Source |
| :--- | :--- | :--- | :--- |
| **REQ-TST-01** | Backend must support automated unit and integration test suites validating schema constraints, graph traversal, and query accuracy. | **MUST** | Blueprint §14; PRD §22, §23; Rule 08 |
| **REQ-TST-02** | Validate representative seed dataset correctness against PRD acceptance criteria before dataset expansion. | **MUST** | Blueprint §14; PRD §20, §21 |
| **REQ-TST-03** | Enforce automated regression checks on graph queries and evidence linkage. | **SHOULD** | Rule 08 |

---

## 4. Backend Constraints

1. **Zero Fabrication**: The backend must never generate synthetic facts, coordinates, dates, relationships, or source citations to fill gaps.
2. **Read-Heavy Curation Model**: V1 is a 100% curated, read-heavy publication system. There is no public user-generated content, no public write API, and no social networking features.
3. **No Unapproved V2/V3 Leaks**: Features such as AI Q&A bots, battle simulators, 3D open worlds, collaborative editing, or public APIs are out of scope for V1 backend design.
4. **Technology Independence in Stage 1.0**: No code, ORM, database engine, or framework is selected during this planning stage.

---

## 5. Backend Decision Matrix

| Architectural Obligation | Why It Matters | Authoritative Source | Backend Implication | Target Stage 1 Block |
| :--- | :--- | :--- | :--- | :--- |
| **Unified Knowledge Graph** | Ensures cross-lens consistency across Timeline, Map, People, War, and Search. | Blueprint §1, §5; PRD §2 | Data model must support multi-entity graph nodes and typed edges. | **B2 (Data), B3 (Graph)** |
| **Evidence & Provenance Linkage** | Preserves academic trust, citations, uncertainty, and variant traditions. | Blueprint §10; PRD §6.1, §9.10 | Schema must model `Entity → Claim → Evidence → Source` relationships. | **B4 (Evidence)** |
| **Epistemic Data States** | Prevents false precision and conveys true historical certainty levels. | Blueprint §12; PRD §11; Rule 03 | Fields must support explicit unknown/approximate/conflicting states. | **B2 (Data), B4 (Evidence)** |
| **Global Multi-Entity Search** | Powers primary discovery across all entity types and aliases. | PRD §9.8; 133-Point Audit §15 | Search indexing across names, aliases, summaries, and types required. | **B6 (Search)** |
| **Progressive War Day Loading** | Prevents loading 18 days of heavy battlefield data at once. | Blueprint §13.2; PRD §9.7 | API must support day-by-day modular query payloads. | **B5 (API), B9 (Perf)** |
| **Curated Ingestion Pipeline** | Ensures only verified scholarly facts enter the production database. | Blueprint §10.1; PRD §10 | Ingestion scripts with strict validation schema gates required. | **B10 (Ingestion), B11 (Seed)** |
| **Public Read / Secure Admin** | Guarantees public accessibility while protecting dataset integrity. | Detailed Ref §19; PRD §17 | Separation of anonymous read endpoints from secured ingestion workflows. | **B7 (Auth & Permissions)** |
| **Stateless Canonical Routing** | Enables shareable, bookmarkable deep links without server session state. | Detailed Ref §7.3; PRD §12 | Fast slug-based entity and view query resolution. | **B2 (Data), B5 (API)** |

---

## 6. Requirement Traceability Matrix

| Requirement Domain | Key IDs | Authoritative Source Document |
| :--- | :--- | :--- |
| **Core Architecture & Layers** | REQ-CORE-01 – 04 | Blueprint §4, §5, §9; PRD §5, §8; 133-Point Audit §1–16 |
| **Data Entities & Epistemic States** | REQ-DAT-01 – 07 | Blueprint §11, §12; PRD §6.1, §11; Detailed Ref §15 |
| **Knowledge Graph & Relationships** | REQ-GRP-01 – 05 | Blueprint §9.2, §11; PRD §6.3, §9.5; Detailed Ref §4 |
| **Evidence, Claims & Sources** | REQ-EVD-01 – 06 | Blueprint §10, §11; PRD §9.10, §10; Detailed Ref §14 |
| **Timeline Chronology** | REQ-TIM-01 – 03 | Blueprint §11, §13.2; PRD §9.2; Detailed Ref §12 |
| **Characters & Entities** | REQ-CHR-01 – 03 | Blueprint §11; PRD §9.3; Detailed Ref §12 |
| **Locations & Geographic Map** | REQ-LOC-01 – 04 | Blueprint §11, §12; PRD §9.6; Detailed Ref §12 |
| **War & Battlefield Formations** | REQ-WAR-01 – 04 | Blueprint §9.3, §11; PRD §6.4, §9.7; Detailed Ref §12 |
| **Relationships & Family Trees** | REQ-REL-01 – 03 | Blueprint §11; PRD §9.5; Detailed Ref §12 |
| **Global Search & Autocomplete** | REQ-SRC-01 – 05 | PRD §9.8; Detailed Ref §12; 133-Point Audit §15 |
| **User State & Stateless URLs** | REQ-STA-01 – 03 | PRD §9.11, §12; Detailed Ref §7 |
| **Auth & Curation Access** | REQ-AUT-01 – 03 | PRD §17, §19; Detailed Ref §19 |
| **Media & Static Storage** | REQ-MED-01 – 03 | Blueprint §11; Detailed Ref §27; PRD §19 |
| **Data Ingestion & Quality Gates** | REQ-ING-01 – 04 | Blueprint §10.1, §14; PRD §10, §20; Detailed Ref §14.3 |
| **Performance & Caching** | REQ-PRF-01 – 04 | Blueprint §13.2; PRD §16; Detailed Ref §16 |
| **Accessibility Text Payloads** | REQ-ACC-01 – 02 | Blueprint §13.3; PRD §15; Detailed Ref §17 |
| **Security & Data Tampering** | REQ-SEC-01 – 03 | PRD §17; Detailed Ref §19 |
| **API Capabilities** | REQ-API-01 – 07 | PRD §7.1, §9.2–§9.10 |
| **Testing & Quality Assurance** | REQ-TST-01 – 03 | Blueprint §14; PRD §22, §23; Rule 08 |

---

## 7. Open Technical Decisions (To be resolved in B1–B12)

The following decisions are deliberately open in Stage 1.0 and will be evaluated in their designated blocks:

1. **Technology Stack & Runtime**: Backend programming language, execution runtime, and hosting infrastructure (*Block B1*).
2. **Database Engine**: Relational vs Graph vs Document vs Hybrid database architecture for entity and graph storage (*Block B1/B2*).
3. **Graph Storage & Traversal Engine**: Relational adjacency list vs native graph database vs in-memory indexed graph structures (*Block B3*).
4. **Search Engine & Indexing Technology**: Embedded search vs full-text search engine (Postgres FTS, Meilisearch, Elasticsearch, SQLite FTS) (*Block B6*).
5. **API Architecture & Protocol**: REST vs GraphQL vs tRPC / Type-safe RPC (*Block B5*).
6. **Authentication Mechanism for Curation**: API keys, JWT, or environment-secured administrative tooling (*Block B7*).
7. **Storage Solution for Media**: Cloud object storage (S3/GCS/Cloudflare R2) vs local/static CDN distribution (*Block B8*).
8. **Caching & Performance Strategy**: In-memory caching (Redis/internal), HTTP CDN caching, or static pre-generation (*Block B9*).
9. **Data Ingestion Format**: JSON/YAML/SQL seed file format and validation pipeline tooling (*Block B10/B11*).

---

## 8. Mapping to Stage 1 Work Blocks

| Block ID | Work Block Title | Governed Scope & Decisions to Resolve |
| :--- | :--- | :--- |
| **B1** | **Technology & Infrastructure** | Language runtime, framework, database engine, hosting environment. |
| **B2** | **Data Architecture** | Concrete relational/document schemas, entity types, fields, epistemic state modeling. |
| **B3** | **Knowledge Graph** | Edge modeling, bi-directional traversal patterns, graph indexing, Focus sub-graph retrieval. |
| **B4** | **Evidence & Provenance** | Granular citation schemas, locator models, certainty levels, conflicting tradition support. |
| **B5** | **API Architecture** | Endpoint design, protocol selection, request/response contracts, pagination, error schemas. |
| **B6** | **Search Architecture** | Search indexing strategy, alias/transliteration handling, ranking algorithms, autocomplete. |
| **B7** | **Authentication & Permissions**| Public read access rules, admin curation auth, environment secrets management. |
| **B8** | **Storage & Media** | Asset storage, image URL patterns, SVG/diagram asset serving. |
| **B9** | **Performance & Caching** | Response time budgets, query optimization, edge caching, progressive loading payloads. |
| **B10** | **Data Ingestion** | Ingestion pipeline scripts, validation gates, dataset parser and importer. |
| **B11** | **Seed Dataset** | Small representative seed dataset creation exercising all core entities and lenses. |
| **B12** | **Backend Testing & Verification**| Automated test suites (unit, integration, query performance, data integrity gates). |
| **B13** | **Master Synthesis & Gap Audit** | Comprehensive Backend Master Architecture Plan synthesis and review. |

---

## 9. Explicit Non-Goals for Stage 1.0

- **DO NOT** select a backend programming language, framework, database, or ORM.
- **DO NOT** write database migration files, SQL tables, or schema code.
- **DO NOT** create API endpoints, server routes, or controllers.
- **DO NOT** install npm, pip, or other package manager dependencies.
- **DO NOT** build seed datasets or execute data ingestion.
- **DO NOT** design frontend components or make frontend framework choices.
