# Technology & Infrastructure Architecture (Block B1)

## 1. Purpose

This document evaluates, compares, and establishes the foundational technology stack, runtime environment, data storage engine, and infrastructure direction for the **Mahābhārata Explorer** backend.

The goal of this architectural planning document is to establish a robust, maintainable, cost-effective, and performance-oriented foundation that directly fulfills the obligations defined in [00-backend-architecture-context.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/00-backend-architecture-context.md), adheres to the Five-Layer Architecture, supports the unified Knowledge Graph, and preserves strict data integrity.

---

## 2. Project Requirements & Architectural Obligations

The technology selection must satisfy the 76 backend requirements established in Stage 1.0. Key architectural drivers include:

1. **One Unified Knowledge Graph (REQ-CORE-01, REQ-GRP-01–05)**: Must support first-class typed edges, bi-directional traversal, Focus mode sub-graph queries (1st and 2nd degree depth), and family generational trees across 12 core entity types.
2. **Epistemic Provenance & Integrity (REQ-EVD-01–06, REQ-DAT-04–05)**: Must represent the full `Entity → Claim → Evidence → Source` relationship graph, including granular textual locators, certainty assessments, and conflicting variant accounts without data flattening.
3. **Multi-Lens Query Capabilities (REQ-TIM-01–03, REQ-LOC-01–04, REQ-WAR-01–04)**: Must support chronological range queries with variable precision, spatial bounding-box/coordinate queries with null fallbacks, and progressive modular War Day (1–18) payload loading.
4. **Global Multi-Entity Search (REQ-SRC-01–05)**: Fast search matching across entity titles, epithets, aliases, and transliteration variations with type filtering and prefix autocomplete.
5. **Read-Heavy, Zero-Fabrication Curation Model (REQ-AUT-01–03, REQ-ING-01–04)**: 100% public read-only anonymous access. Offline/administrative curation and schema validation pipeline. Zero public UGC or write traffic in V1.
6. **Operational Simplicity & Cost Discipline (Rule 02, Rule 04)**: Avoid unnecessary microservice fragmentation, multi-database synchronization overhead, or expensive specialized infrastructure unless strictly necessary.
7. **End-to-End Type Safety & Antigravity IDE Compatibility**: Enable unified typing across the monorepo (`packages/shared`, `apps/api`, `apps/web`) with fast local development, automated testing, and CI/CD pipelines.

---

## 3. Evaluation of Realistic Architectural Options

We evaluate three distinct, production-grade architectural options:

---

### Option 1: Unified TypeScript Stack with PostgreSQL (Monolithic / Shared-Kernel Architecture)

- **Language / Runtime**: TypeScript on Node.js (Active/Maintenance LTS at implementation time).
- **Backend Framework / API Layer**: Type-safe HTTP service (specific framework evaluated in B5).
- **Primary Database**: **PostgreSQL 16+** (Relational storage with JSONB support, Recursive Common Table Expressions for graph queries, and Full-Text Search capabilities).
- **Data Access Approach**: Type-safe SQL data-access layer / ORM (specific library evaluated in B2).
- **Graph Storage / Traversal**: Relational graph model (Adjacency List with typed `Relationship` join table, indexed Foreign Keys, and Recursive CTEs for multi-hop graph and family tree traversal; concrete query patterns to be validated in B3).
- **Search Engine**: PostgreSQL Full-Text Search with GIN indexing, with `pg_trgm` (trigram matching) as the likely extension for alias/transliteration fuzziness (concrete search architecture to be finalized in B6).
- **Media & Asset Handling**: Cloud object storage / static asset distribution with URLs stored in database records (hosting provider deferred to B8).

#### Strengths
- **Single Source of Truth**: One ACID-compliant database stores entities, graph edges, evidence chains, and search indexes without dual-write synchronization or distributed transaction hazards.
- **End-to-End TypeScript Type Safety**: Shared types between backend data models (`packages/shared`) and frontend exploration lenses eliminate contract drift.
- **Efficient Relational Graphing**: PostgreSQL recursive CTEs are architecturally well-suited to handle shallow 1–2 hop Focus sub-graphs and ancestral family trees for a bounded curated dataset without requiring a dedicated graph database.
- **Minimal Operational Burden**: Operates on standard managed PostgreSQL instances and local development environments without running multiple distributed clusters.
- **First-Class Antigravity IDE Integration**: Fast local execution, native TypeScript tooling, rapid testing with modern test runners, and clean CI compatibility.

#### Weaknesses
- Deep graph path-finding (>5 hops or arbitrary network-wide clustering) is less declarative in SQL than in dedicated graph query languages like Cypher (though such deep queries are outside V1 scope).
- Typo tolerance and fuzzy ranking require intentional trigram threshold tuning compared to dedicated search appliances.

#### Fit Against Requirements
- **Graph Suitability**: **HIGH** (Provides the required structural foundation for REQ-GRP-01–05; concrete indexing and queries to be validated in B3).
- **Search Suitability**: **HIGH** (Expected to satisfy REQ-SRC-01–05 via GIN trigram indexes for multilingual transliterations and aliases; to be formalized in B6).
- **Evidence & Provenance**: **EXCELLENT** (Relational integrity ensures foreign keys across Entity, Claim, Evidence, and Source never orphan).
- **Ingestion & Curation**: **EXCELLENT** (Transactional bulk inserts and type-safe schema validation gates).
- **Performance & Scaling**: **HIGH** (Expected to meet V1 exploration needs with low query latency; formal latency budgets and benchmarks deferred to B9/B12).
- **Antigravity Compatibility**: **EXCELLENT** (Single monorepo, zero background daemon friction).
- **Major Risks**: Risk of inefficient SQL traversal if indexes are not properly designed (mitigated by formal query validation in B3).

---

### Option 2: Python Backend with PostgreSQL & External Search Engine (FastAPI + SQLAlchemy + Meilisearch)

- **Language / Runtime**: Python 3.11+.
- **Backend Framework**: FastAPI (Asynchronous Python ASGI).
- **Primary Database**: PostgreSQL (Relational).
- **Data Access / ORM**: SQLAlchemy 2.0 (async) / SQLModel + Alembic migrations.
- **Graph Storage / Traversal**: Relational tables with async SQLAlchemy queries and in-memory graph libraries (e.g., NetworkX) for analysis.
- **Search Engine**: Standalone **Meilisearch** instance for typo-tolerant search.
- **Media & Asset Handling**: Cloud Object Storage / CDN.

#### Strengths
- **Data Science & NLP Ecosystem**: Strong library support for future textual analysis and research pipelines.
- **FastAPI Framework Design**: Automatic OpenAPI generation, asynchronous request handling, and Pydantic validation.
- **Dedicated Typo-Tolerant Search**: Meilisearch provides high-quality out-of-the-box search ranking.

#### Weaknesses
- **Language Boundary Disconnect**: Backend (Python) and Frontend (TypeScript) cannot share native type definitions directly, requiring intermediate OpenAPI generation pipelines to prevent interface drift.
- **Operational Dual-System Overhead**: Running a separate search service alongside PostgreSQL introduces synchronization pipelines, index re-building jobs, and increased operational surface area.
- **Heavier Local Footprint**: Requires managing Python virtual environments alongside Node.js in the monorepo workspace.

#### Fit Against Requirements
- **Graph Suitability**: **MEDIUM-HIGH** (Relational queries backed by Python graph tools).
- **Search Suitability**: **EXCELLENT** (Meilisearch provides high-quality search ranking).
- **Evidence & Provenance**: **HIGH** (Relational foreign key integrity in PostgreSQL).
- **Ingestion & Curation**: **EXCELLENT** (Python scripts excel at data ingestion).
- **Performance & Scaling**: **HIGH** (FastAPI async is fast; search is decoupled).
- **Antigravity Compatibility**: **MEDIUM** (Requires managing dual language runtimes in local workspace).
- **Major Risks**: Dual-write drift between PostgreSQL and external search engine; monorepo type fragmentation.

---

### Option 3: Dedicated Polyglot Stack (Node.js + Neo4j Graph DB + PostgreSQL + Elasticsearch)

- **Language / Runtime**: TypeScript (Node.js) / Go.
- **Backend Framework**: Express / Fastify / NestJS.
- **Primary Database**: **Neo4j** (Native Property Graph Database) for Knowledge Graph + **PostgreSQL** for tabular/evidence data + **Elasticsearch** for search.
- **Data Access**: Neo4j JavaScript Driver (Cypher query language) + ORM for PostgreSQL.
- **Graph Storage / Traversal**: Native Labeled Property Graph (LPG) queried via Cypher.
- **Search Engine**: Elasticsearch cluster.

#### Strengths
- **Native Graph Database Capabilities**: Arbitrary deep graph path traversal, graph algorithms, and expressive Cypher queries.
- **Scholarly Graph Exploration**: Suited for enterprise-scale semantic networks with millions of unconstrained connections.

#### Weaknesses
- **Extreme Operational Complexity**: Managing three separate distributed database systems (Neo4j, Postgres, Elasticsearch) for a V1 curated knowledge base of bounded size is severe architectural over-engineering.
- **High Financial & Hosting Burden**: Neo4j and Elasticsearch clusters require substantial recurring infrastructure resources.
- **Severe Dual-Write & Consistency Risks**: Synchronizing entities between Postgres, Neo4j, and Elasticsearch creates race conditions, partial failure states, and data drift that directly threaten Rule 03 (Data Integrity).
- **Violates Project Governance**: Directly violates Rule 02 (Mandatory System Reuse / No Duplicate Engines) and Rule 04 (No Speculative Infrastructure "Just in Case").

#### Fit Against Requirements
- **Graph Suitability**: **EXCELLENT** (Native graph engine).
- **Search Suitability**: **EXCELLENT** (Elasticsearch).
- **Evidence & Provenance**: **MEDIUM** (Fragmented across relational evidence tables and graph nodes).
- **Ingestion & Curation**: **POOR** (Requires writing complex multi-system synchronization pipelines).
- **Performance & Scaling**: **HIGH** (Scales to massive enterprise datasets).
- **Antigravity Compatibility**: **POOR** (Requires running multiple heavyweight container services locally).
- **Major Risks**: Architectural collapse under operational burden; dual-write data inconsistency.

---

## 4. Comparative Decision Matrix

| Evaluation Dimension | Weight | Option 1: Unified TypeScript + PostgreSQL | Option 2: Python (FastAPI) + PostgreSQL + Meili | Option 3: Polyglot (Node + Neo4j + Postgres + ES) |
| :--- | :--- | :--- | :--- | :--- |
| **Unified Knowledge Graph (1-2 hop Focus/Lens traversal)** | **Critical** | **HIGH** (Relational recursive CTEs, zero sync overhead) | **HIGH** (PostgreSQL + Pydantic) | **EXCELLENT** (Native Cypher queries) |
| **Evidence & Provenance Integrity** | **Critical** | **EXCELLENT** (ACID FKs across Entity/Claim/Evidence) | **EXCELLENT** (ACID Relational) | **MEDIUM** (Fragmented multi-DB sync) |
| **Data-Driven Multi-Lens Support (Time/Map/War)** | **High** | **EXCELLENT** (Single unified data model) | **EXCELLENT** (Unified data model) | **MEDIUM-HIGH** (Multi-system query orchestration) |
| **Global Multi-Entity Search** | **High** | **HIGH** (Postgres FTS + Trigram GIN indexes) | **EXCELLENT** (Dedicated Meilisearch) | **EXCELLENT** (Elasticsearch) |
| **End-to-End Type Safety (Monorepo)** | **High** | **EXCELLENT** (Shared TS types in `packages/shared`) | **MEDIUM** (Requires OpenAPI generator) | **MEDIUM** (Requires complex multi-driver typing) |
| **Operational Simplicity & Cost** | **High** | **EXCELLENT** (Single managed DB, zero sync overhead) | **MEDIUM** (2 database instances to maintain) | **POOR** (3 distributed clusters, high cost) |
| **Ingestion Pipeline Simplicity** | **High** | **EXCELLENT** (Single transactional ingestion script) | **HIGH** (Python ingestion scripts) | **POOR** (Complex 3-way synchronization) |
| **Antigravity Developer Experience** | **High** | **EXCELLENT** (Lightweight, instant startup, fast tests) | **MEDIUM** (Dual runtime in workspace) | **POOR** (Heavy Docker orchestration) |
| **Architectural Discipline & Rule Compliance** | **Critical** | **EXCELLENT** (Zero over-engineering, fully satisfies V1) | **HIGH** (Good, but slight tooling split) | **FAIL** (Severe over-engineering, violates Rule 02/04) |
| **OVERALL EVALUATION** | — | **WINNER (RECOMMENDED)** | **VIABLE ALTERNATIVE** | **REJECTED (OVER-ENGINEERED)** |

---

## 5. Architectural Recommendation

### Recommended Architecture: **Option 1 — Unified TypeScript Backend with PostgreSQL**

```
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                                   MONOREPO ROOT                                         │
│                                                                                         │
│   ┌───────────────────────────┐                      ┌──────────────────────────────┐   │
│   │        apps/web           │                      │          apps/api            │   │
│   │ (Exploration Lenses / UI) │                      │ (TypeScript API & Services)  │   │
│   └─────────────┬─────────────┘                      └──────────────┬───────────────┘   │
│                 │                                                   │                   │
│                 │            ┌─────────────────────────┐            │                   │
│                 └───────────►│     packages/shared     │◄───────────┘                   │
│                              │ (Domain Types & Schemas)│                                │
│                              └─────────────────────────┘                                │
│                                           │                                             │
│                                           ▼                                             │
│                              ┌─────────────────────────┐                                │
│                              │       PostgreSQL        │                                │
│                              │  - Relational Graph     │                                │
│                              │  - Evidence & Sources   │                                │
│                              │  - Trigram Search (GIN) │                                │
│                              └─────────────────────────┘                                │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

#### Core Components of the Recommended Architecture:
1. **Language & Runtime**: **TypeScript** on **Node.js** (Active/Maintenance LTS at implementation time).
2. **Database Engine**: **PostgreSQL 16+** as the single unified primary database.
3. **Data Access Approach**: **Type-safe SQL data-access layer** with schema-driven migrations (exact library selection deferred to Block B2).
4. **Knowledge Graph Traversal Engine**: **Relational Adjacency Graph** (indexed foreign keys, join tables, and recursive CTEs; concrete relationship schema and traversal queries to be modeled in Block B3).
5. **Search Architecture Direction**: **PostgreSQL Full-Text Search with `pg_trgm`** for multi-entity name matching, alias resolution, and prefix autocomplete (concrete indexing and query tuning to be formalized in Block B6).
6. **Ingestion & Validation Engine**: Type-safe validation scripts leveraging shared schemas (`packages/shared`) to validate seed datasets before database insertion (tooling deferred to Block B10).
7. **Asset Storage Strategy**: Static CDN / Object Storage with URLs referenced in database records (hosting provider deferred to Block B8).

---

## 6. Rationale: Why This Fits THIS Project Specifically

1. **Alignment with V1 Bounded Scale**:
   - The Mahābhārata epic encompasses hundreds of key characters, thousands of events, dozens of locations, and 18 war days. This represents an intensely interconnected, but structurally bounded, curated knowledge base.
   - For this dataset scale, an indexed PostgreSQL relational graph is expected to satisfy all V1 shallow traversal requirements (1–2 hop Focus sub-graphs and ancestral trees) cleanly and efficiently without the overhead of a specialized graph database.
2. **Elimination of Dual-Write Corruption Hazards (Rule 03 Compliance)**:
   - Polyglot database setups (e.g., storing entities in Postgres and graph edges in Neo4j) introduce synchronization lag and consistency risks. A single PostgreSQL database guarantees transactional integrity across Entities, Claims, Evidence, and Sources.
3. **Monorepo Type Coherence**:
   - Defining entity models and graph contracts in `packages/shared` allows backend services and frontend exploration lenses to share identical TypeScript interfaces, preventing contract divergence.
4. **Developer Ergonomics & Infrastructure Simplicity**:
   - PostgreSQL runs effortlessly in local development, integrates cleanly with Antigravity tools, and requires minimal operational maintenance.

---

## 7. Trade-off Analysis

### What We Gain
- **Architectural Coherence**: A unified technology paradigm across the entire monorepo.
- **Bulletproof Integrity**: Relational foreign keys and ACID transactions enforce the `Entity → Claim → Evidence → Source` hierarchy.
- **Agility & Simplicity**: Straightforward local testing and rapid migration workflows without multi-service orchestration.
- **Cost Discipline**: Minimal hosting overhead and operational surface area.

### What We Give Up
- **Native Cypher Query Language**: Multi-hop path queries are written in SQL CTEs rather than graph-specific query languages (though sufficient for V1 scope).
- **Out-of-the-Box Typo Ranking**: PostgreSQL trigram search requires intentional index configuration and tuning compared to dedicated search appliances.

### What Would Make Us Reconsider in the Future (V2/V3 Triggers)
1. If the knowledge base expands into an open multi-edition scholarly corpus with tens of millions of comparative textual nodes requiring arbitrary-depth path-finding algorithms (*reconsider dedicated Graph DB in V3*).
2. If search query complexity exceeds fuzzy alias matching and requires distributed natural language indexing or vector semantic embeddings (*reconsider dedicated search engine in V2/V3*).
3. If public collaborative editing is introduced requiring multi-master replication and live collaborative locks (*reconsider in V3*).

---

## 8. Explicit Decisions Made in Block B1

1. **DECISION B1-01**: **TypeScript on Node.js** (Active/Maintenance LTS at implementation time) is selected as the primary backend runtime.
2. **DECISION B1-02**: **PostgreSQL 16+** is selected as the unified primary database for entities, relationships, evidence/provenance, and search.
3. **DECISION B1-03**: **Relational Graph Architecture** (Adjacency model + Recursive CTEs) is selected as the knowledge graph direction, avoiding a dedicated graph database for V1.
4. **DECISION B1-04**: **PostgreSQL Full-Text Search with `pg_trgm`** is selected as the primary search direction for V1.
5. **DECISION B1-05**: Dedicated graph databases (Neo4j), external search clusters (Elasticsearch), and microservice architectures are **REJECTED for V1** as unjustified over-engineering.

---

## 9. Deferred Decisions (Mapped to Blocks B2–B12)

The following detailed implementation decisions are deliberately deferred:

- **B2 (Data Architecture)**: Concrete database table structures, column datatypes, UUID conventions, explicit epistemic state enum definitions, and selection of the specific type-safe ORM/data-access library.
- **B3 (Knowledge Graph)**: Concrete relationship join table schema, indexing strategy, recursive CTE query templates, and family tree traversal query validation.
- **B4 (Evidence & Provenance)**: Schema modeling for Parva/Adhyaya/Shloka locators, certainty scales, and conflicting claim resolution structures.
- **B5 (API Architecture)**: Selection of HTTP framework (Fastify vs Express vs Next.js Route Handlers) and API protocol (REST vs tRPC).
- **B6 (Search Architecture)**: SQL search ranking weights, trigram similarity threshold tuning, and autocomplete index structures.
- **B7 (Authentication & Permissions)**: Administrative token/key management and curation endpoint security mechanisms.
- **B8 (Storage & Media)**: Specific cloud storage provider / CDN selection and media URL reference conventions.
- **B9 (Performance & Caching)**: Formal response time budgets, benchmark targets, query caching rules, and payload size optimization.
- **B10 (Data Ingestion)**: Ingestion script tooling, JSON/YAML seed parser, and Zod schema validation pipelines.
- **B11 (Seed Dataset)**: Concrete seed dataset content and test entity verification records.
- **B12 (Backend Testing & Verification)**: Test framework selection, integration test harness, and empirical query latency benchmarks.

---

## 10. Architectural Risk Assessment & Mitigations

| Risk | Impact | Likelihood | Mitigation Strategy |
| :--- | :--- | :--- | :--- |
| **Recursive SQL Query Inefficiency** | Medium | Low | Restrict traversal depth to 2 hops in V1 Focus queries; enforce composite B-tree indexes on `(source_entity_id, relationship_type)` and `(target_entity_id)`; validate queries in B3. |
| **Trigram Search Performance on Large Tables** | Low | Low | Utilize GIN indexes (`gin_trgm_ops`) on indexed title/alias columns; paginate search results strictly; validate in B6. |
| **Schema Migration Drift** | High | Low | Enforce declarative type-safe migrations checked in Git and executed via automated CI verification gates. |
| **Ingestion Validation Failure** | High | Low | Implement pre-ingestion schema validation using Zod/TypeScript to halt invalid records before database insertion. |
