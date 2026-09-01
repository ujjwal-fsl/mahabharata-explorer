# Backend Testing & Verification Architecture (Block B12)

## 1. Purpose & Verification Principles

This document establishes the **Backend Testing & Verification Architecture** for the **Mahābhārata Explorer** backend. It defines the formal verification strategies, test levels, automated test suites, quality gates, and release-readiness criteria required to rigorously validate the backend architecture established across Blocks B1 through B11.

In accordance with project principles:
- **Verification Requirement (Rule 14, AGENTS.md §14)**: Do not declare a feature complete merely because it compiles or runs without immediate errors; implementation must be validated against its functional, referential, performance, and epistemic requirements.
- **Zero Fabrication (Rule 03, PRD §11)**: Automated tests must verify that the backend never synthesizes, normalizes away, or hallucinates historical facts, dates, coordinates, relationships, or citations.
- **Epistemic Honesty (Rule 03, B2 §5, B4 §6)**: Verification suites must explicitly test that genuine ambiguities—unmapped locations, disputed chronologies, conflicting scholarly traditions, and unknown parentage—are preserved and correctly surfaced.
- **Distinction Between Targets and Empirical Measurements (B9 §2)**: Latency and payload budgets established in B9 are architectural design targets to be benchmarked and validated during testing execution; B12 specifies the verification methodology and release criteria rather than claiming empirical results before test execution.

---

## 2. Verification Architecture Across Backend Subsystems (B1–B11)

```
┌────────────────────────────────────────────────────────────────────────┐
│                   BACKEND VERIFICATION MATRIX (B1–B11)                 │
├──────────────────┬──────────────────┬──────────────────────────────────┤
│ Subsystem / Area │ Authority Block  │ Core Verification Objective      │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ Database Schema  │ B1, B2           │ Relational constraints, types,   │
│                  │                  │ composite indexes, JSONB bounds. │
│ Knowledge Graph  │ B3               │ Traversal bounds (D≤2), symmetry,│
│                  │                  │ lineage DAG, zero co-occurrences.│
│ Provenance       │ B4               │ Entity/Edge→Claim→Evidence→Source│
│                  │                  │ chains, native locators, conflict│
│ API Contracts    │ B5               │ Standard envelopes, status codes,│
│                  │                  │ offset pagination bounds, slugs. │
│ Search Engine    │ B6               │ Trigram/FTS tiers, diacritics,   │
│                  │                  │ alias resolution, prefix suggest.│
│ Auth & Security  │ B7               │ Anonymous read, mutation blocking│
│                  │                  │ (405), secret isolation, CORS.   │
│ Storage & Media  │ B8               │ URI resolution, SVG XML sanitize,│
│                  │                  │ missing-asset monogram fallback. │
│ Perf & Caching   │ B9               │ All 8 p95 latency targets,       │
│                  │                  │ D2→D1 degradation, ETags, rate.  │
│ Ingestion Gates  │ B10              │ Hard blockers, soft warnings,    │
│                  │                  │ deterministic idempotency.       │
│ Seed Dataset     │ B11              │ Representative multi-lens scope, │
│                  │                  │ 6 epistemic states, graph DAG.   │
└──────────────────┴──────────────────┴──────────────────────────────────┘
```

---

### 2.1. Schema & Database Integrity Verification (B1, B2)
1. **Migration & Schema Verification**: Verify that database migrations construct all 12 canonical tables (`characters`, `events`, `locations`, `groups`, `wars`, `war_days`, `formations`, `sources`, `claims`, `evidence`, `relationships`, `family_relationships`) with exact column types, primary keys, foreign keys, and unique slug constraints.
2. **JSONB Schema Boundary Validation**: Ensure `metadata` columns adhere strictly to presentation/minor annotation bounds and never harbor unindexed canonical domain properties (B2 §4.10).
3. **Index Coverage Verification**: Verify via query execution plans (`EXPLAIN`) that all standard query patterns utilize configured B-tree and GIN indexes without unindexed sequential table scans.

---

### 2.2. Knowledge Graph Correctness Verification (B3)
1. **Traversal Depth & Bounding Tests**:
   - Verify that `/graph/focus/:type/:slug` defaults to $D=1$ ($\le 50$ nodes) and strictly enforces $D \le 2$ ($\le 100$ nodes).
   - Verify that traversal depth requests with $D > 2$ are rejected with `400 Bad Request`.
2. **Relationship Symmetry & Directionality Tests**:
   - Verify that relationship types declared symmetric in B3 (e.g., `allied_with` in B3 §4.1) project bidirectional adjacency in graph queries.
   - Verify that asymmetric directed relationships (e.g., `teacher_of`, `rival_of`, `combatant_in`) maintain strict directionality.
3. **Generational Lineage DAG Acyclicity**:
   - Verify that ancestry traversals in `family_relationships` detect and prevent circular reference loops.
4. **Zero Co-occurrence Inference Gate**:
   - Verify that no relationship edge is created merely because two entities co-occur in an event narrative, passage, or dataset. All published relationships must follow the canonical B3 model and satisfy required provenance rules (Rule 03, B3 §1).

---

### 2.3. Evidence & Provenance Verification (B4)
1. **Entity & Edge Claim Separation**:
   - Test entity factual claims separately from relationship/edge claims, verifying that claims attach to the correct subject entity (`subject_type`, `subject_id`) or relationship edge.
2. **Provenance Chain Completeness**:
   - Verify that every factual claim traces deterministically: $\text{Entity/Edge} \longrightarrow \text{Claim} \longrightarrow \text{Evidence} \longrightarrow \text{Source}$.
   - Reject orphaned claims (claims without `source_id`) or unlinked evidence (evidence without `claim_id`).
3. **Native Citation Locator Integrity**:
   - Verify that `Evidence.locator` strings preserve the source edition's native citation syntax verbatim without data loss or normalization corruption (B4 §5).
4. **Conflicting Tradition Preservation**:
   - Verify that competing scholarly traditions (e.g., Critical Edition vs. regional variants) remain separate claim records with `epistemic_status = 'conflicting'`, verifying they are never synthesized or averaged into a single record.

---

### 2.4. API Contract & Envelope Verification (B5)
1. **Response Envelope Compliance (B5 §6.1)**:
   - **Single Resource**: Verify that single-entity endpoints return `{ "data": { ... } }`.
   - **Collection Resource**: Verify that list endpoints return:
     ```json
     {
       "data": [ ... ],
       "pagination": {
         "limit": 20,
         "offset": 0,
         "total_count": 100,
         "has_more": true
       }
     }
     ```
   - **Error Resource (B5 §6.2)**: Verify that error responses conform strictly to RFC 7807 Problem Details:
     ```json
     {
       "type": "about:blank",
       "title": "Not Found",
       "status": 404,
       "detail": "Character with slug 'unknown-warrior' not found",
       "instance": "/api/v1/characters/unknown-warrior"
     }
     ```
2. **Status Code Accuracy**:
   - `200 OK` for successful queries; `304 Not Modified` for conditional ETag hits; `400 Bad Request` for invalid parameters; `404 Not Found` for nonexistent slugs; `405 Method Not Allowed` for public mutations; `429 Too Many Requests` for rate limits; `504 Gateway Timeout` for statement timeouts.
3. **Offset Pagination Guardrails (B5 §7.1)**:
   - Verify default `limit = 20`, maximum `limit <= 100`, correct `offset` calculations, accurate `total_count`, correct `has_more` boolean, and deterministic ordering across sequential offset pages.

---

### 2.5. Search, Diacritic & Autocomplete Verification (B6)
1. **Dual-Tier Search Mechanics**:
   - Verify Tier 1 Trigram fuzzy matching (`pg_trgm`) on names and aliases.
   - Verify Tier 2 Full-Text Search (`tsvector`) on summaries and evidence excerpts.
2. **Diacritic & Transliteration Normalization**:
   - Verify that searching unaccented queries (e.g., `Bhisma`) correctly matches IAST-accented records (e.g., `Bhīṣma`).
   - Verify that curated aliases (e.g., `Partha` $\rightarrow$ `Arjuna`, `Radheya` $\rightarrow$ `Karna`) resolve deterministically with `match_type = 'alias'`.
3. **Autocomplete Latency & Budget**:
   - Verify prefix query responses (`/search/suggest?q=...`) return lightweight suggestion cards within the $\le 20\text{ KB}$ payload budget.

---

### 2.6. Security, Authentication & Permission Boundary Verification (B7)
1. **Public Read Boundary**: Verify that 100% of public exploration routes operate anonymously without requiring credentials or session tokens.
2. **Public Mutation Blocking**: Verify that public HTTP mutation verbs (`POST`, `PUT`, `PATCH`, `DELETE`) on public endpoints immediately return `405 Method Not Allowed`.
3. **Secret Isolation & Safe Errors**: Verify that error responses never leak database connection strings, credentials, or internal stack traces in production mode.
4. **CORS Enforcement**: Verify that cross-origin access is origin-restricted according to B7; wildcard CORS with credentials is strictly prohibited.

---

### 2.7. Storage & Media Security Verification (B8)
1. **Asset Reference Resolution**: Verify that all asset URIs resolve against the environment-configured asset host (`ASSET_BASE_URL`).
2. **Missing Asset Graceful Fallback**: Verify that characters with `portrait_url = NULL` return clean semantic monograms/placeholders rather than broken UI images or synthetic AI portraits.
3. **Staged SVG XML Sanitization Verification**:
   - Verify that staged/ingested SVG files within the trusted curation pipeline containing `<script>`, `onload`, `javascript:`, or external XML entity definitions are intercepted and rejected before publication.
   - Preserve B7's zero-public-upload boundary (tests verify the ingestion boundary, not public file uploads).

---

### 2.8. Performance, Latency & Caching Verification (B9)
1. **Benchmarking Across All 8 Operation Categories**:
   - **1. Canonical Entity Detail**: Target $p95 \le 50\text{ ms}$, payload $\le 100\text{ KB}$.
   - **2. Autocomplete Suggestions**: Target $p95 \le 30\text{ ms}$, payload $\le 20\text{ KB}$.
   - **3. Global Multi-Entity Search**: Target $p95 \le 100\text{ ms}$, payload $\le 150\text{ KB}$.
   - **4. Timeline Slice / Chronology**: Target $p95 \le 75\text{ ms}$, payload $\le 250\text{ KB}$.
   - **5. Map Spatial View (Bounding Box)**: Target $p95 \le 75\text{ ms}$, payload $\le 200\text{ KB}$.
   - **6. War Day Tactical Occurrences**: Target $p95 \le 75\text{ ms}$, payload $\le 250\text{ KB}$.
   - **7. Global Focus Subgraph ($D_1$)**: Target $p95 \le 100\text{ ms}$, payload $\le 300\text{ KB}$.
   - **8. Global Focus Subgraph ($D_2$)**: Target $p95 \le 200\text{ ms}$, payload $\le 500\text{ KB}$.
2. **Deterministic Focus Traversal Degradation Flow**:
   - Verify that when a $D=2$ focus query exceeds its $500\text{ ms}$ timeout, the system executes the bounded $D=1$ fallback query.
   - Verify that if $D=1$ succeeds, it returns the standard B5 $D=1$ envelope.
   - Verify that if $D=1$ also times out, the API returns standard RFC 7807 `504 Gateway Timeout`.
3. **HTTP Cache Revalidation & Invalidation**:
   - Verify `ETag` generation and `304 Not Modified` on unchanged conditional requests (`If-None-Match`).
   - Verify that publication of a new dataset release version changes `ETag` values and triggers fresh `200 OK` deliveries.
4. **Rate Limiting Enforcement**:
   - Verify that exceeding sliding-window IP thresholds triggers `429 Too Many Requests` with a valid `Retry-After` header.

---

### 2.9. Ingestion Pipeline & Quality Gate Verification (B10)
1. **Hard Quality Gate Enforcement**: Verify that the ingestion pipeline immediately fails and halts publication when encountering:
   - Schema syntax errors
   - Dangling foreign keys
   - Duplicate entity slugs
   - Invalid polymorphic pairs
   - Generational lineage cycles
   - Invalid directionality on symmetric relationships (B3)
   - Unlinked claims or empty evidence locators
2. **Deterministic Idempotency**: Verify that re-running ingestion on identical seed input performs zero duplicate insertions.

---

### 2.10. Seed Dataset Multi-Lens Verification (B11)
Verify that the representative seed dataset thoroughly exercises all major V1 exploration capabilities:
1. **Character Detail**: Rich biographical summaries, role tags, and portrait/monogram states.
2. **Family Lineage**: Branching 4-generation parent-child DAGs and spousal relationships.
3. **General Relationships**: Non-familial directed edges (alliances, rivalries, mentorships).
4. **Timeline / Chronology**: Chronological sequence ordering (`sequence_index`) and Parva mappings.
5. **Locations / Geography**: Mapped coordinates (bounding box queries) and unmapped sites.
6. **Groups / Dynasties**: Dynasty hierarchies and factional allegiances.
7. **Wars & WarDays**: Partitioned tactical occurrences (Days 1, 13, 14, 18).
8. **Formations (Vyuhas)**: Tactical descriptions, event links, and SVG diagram references.
9. **Provenance & Citations**: Entity $\rightarrow$ Claim $\rightarrow$ Evidence $\rightarrow$ Source chains and native locators.
10. **Search & Aliases**: Prefix suggestions, diacritic stripping, and alias lookup.
11. **Global Focus Graph**: Hub nodes and bounded $D_1/D_2$ traversal networks.
12. **Six Epistemic States**: Representation of `known`, `approximate`, `unknown`, `conflicting`, `not_researched`, and `not_applicable`.

---

## 3. Cross-Block Regression Test Matrix

To prevent cross-block architectural regressions, integration test suites must execute the following cross-subsystem verification assertions:

| Subsystem Pair | Architectural Interface | Regression Objective & Verification Assertion |
| :--- | :--- | :--- |
| **B2 $\longleftrightarrow$ B5** | Data Model to API Contract | Assert that all canonical entity attributes serialize to exact B5 JSON response schemas without missing fields or unmapped types. |
| **B3 $\longleftrightarrow$ B4** | Graph Edges to Provenance | Assert that all relationship edges and factual claims resolve to valid Evidence and Source records without dangling references. |
| **B2 $\longleftrightarrow$ B6** | Data Model to Search Indexes | Assert that canonical names, titles, and `alternate_names` arrays correctly populate `pg_trgm` and `tsvector` indexes. |
| **B3 $\longleftrightarrow$ B10** | Graph Rules to Ingestion Gates | Assert that the ingestion pipeline rejects asymmetric alliance edges or circular generational parentage before database commit. |
| **B4 $\longleftrightarrow$ B10** | Provenance Rules to Ingestion Gates | Assert that the ingestion pipeline rejects claims without sources or evidence records without native locators. |
| **B2 $\longleftrightarrow$ B8** | Media References to Asset Origin | Assert that all canonical `portrait_url` and diagram URIs resolve against `ASSET_BASE_URL` and missing assets fall back to monograms. |
| **B5 $\longleftrightarrow$ B9** | API Contracts to Performance & Cache | Assert that API responses emit valid `ETag` and `Cache-Control` headers, and respond within latency budgets under nominal load. |
| **B11 $\longleftrightarrow$ B2–B10** | Seed Dataset to Unified Architecture | Assert that the representative seed dataset successfully exercises all canonical entities, API routes, quality gates, and epistemic states. |

---

## 4. Zero-Fabrication Negative-Test Coverage

To ensure strict compliance with **Rule 03 (Zero Fabrication)** and **PRD §11**, the testing architecture specifies dedicated negative verification test suites:

```
┌────────────────────────────────────────────────────────────────────────┐
│               ZERO-FABRICATION NEGATIVE TEST COVERAGE                  │
├────────────────────────────────────────────────────────────────────────┤
│ 1. CO-OCCURRENCE REJECTION: Assert that two characters appearing in the│
│    same event text do NOT automatically generate a relationship edge.  │
│ 2. CHRONOLOGY FABRICATION REJECTION: Assert that events with relative  │
│    ordering but unknown calendar dates are NOT assigned fake dates.    │
│ 3. COORDINATE FABRICATION REJECTION: Assert that unmapped locations    │
│    retain latitude/longitude = NULL with coordinate_status = 'unmapped'│
│ 4. CONFLICT SYNTHESIS REJECTION: Assert that conflicting traditions are│
│    NOT averaged or merged into a single synthetic claim.               │
│ 5. PORTRAIT FABRICATION REJECTION: Assert that characters without      │
│    curated portraits return portrait_url = NULL (no speculative AI art)│
│ 6. CITATION INTEGRITY REJECTION: Assert that modified or fabricated    │
│    shloka locators fail provenance validation.                         │
│ 7. EPISTEMIC STATE FIDELITY: Assert that 'unknown', 'not_researched',  │
│    and 'not_applicable' states are NEVER converted into factual claims.│
└────────────────────────────────────────────────────────────────────────┘
```

---

## 5. CI/CD Quality Gates & Release-Readiness Criteria

Release readiness is evaluated against explicit, deterministic verification gates:

```
┌────────────────────────────────────────────────────────────────────────┐
│                    RELEASE-READINESS CLASSIFICATION                    │
├───────────────┬────────────────────────────────────────────────────────┤
│ Level         │ Verification Scope & Mandatory Release Gates           │
├───────────────┼────────────────────────────────────────────────────────┤
│ **MUST**      │ - All unit, schema, and migration tests pass.          │
│ (Blocking)    │ - All integration route and B5 envelope tests pass.    │
│               │ - All B10 Hard Quality Gates pass on seed dataset.     │
│               │ - Zero public mutation vulnerabilities (405 enforced). │
│               │ - RFC 7807 compliant error envelopes on all failures.  │
│               │ - Zero cyclic lineage in ancestry structures.          │
│               │ - Staged SVG XML sanitization blocks malicious content.│
│               │ - Zero-fabrication negative test suites 100% pass.     │
├───────────────┼────────────────────────────────────────────────────────┤
│ **SHOULD**    │ - Performance benchmarks satisfy B9 p95 latency targets│
│ (Target)      │   under nominal local/staging execution.               │
│               │ - Payload sizes remain within B9 uncompressed budgets. │
│               │ - Rate limiting properly throttles burst traffic.      │
├───────────────┼────────────────────────────────────────────────────────┤
│ **DEFERRED**  │ - Multi-region global CDN edge load testing.           │
│ (Post-V1)     │ - Distributed cluster failover / multi-master tests.   │
│               │ - Large-scale production dataset fuzz testing.         │
└───────────────┴────────────────────────────────────────────────────────┘
```

---

## 6. Requirement Traceability Matrix

| Verification Requirement | Source Document | Implemented Verification Subsystem |
| :--- | :--- | :--- |
| **Comprehensive Test Coverage** | AGENTS.md Rule 14; PRD §16 | Section 2: Full verification matrix across B1–B11. |
| **Zero Fabrication Enforcement**| AGENTS.md Rule 03; PRD §11 | Section 2.2, Section 2.3, Section 4: Negative test coverage. |
| **Epistemic States Testing** | Context §4.N; B2 §5; B11 §5 | Section 2.10, Section 4: Six epistemic certainty states. |
| **Provenance Integrity** | B4 §2; PRD §9.10 | Section 2.3: Entity/Edge $\rightarrow$ Claim $\rightarrow$ Evidence $\rightarrow$ Source. |
| **API Envelope & Error Tests** | B5 §6; PRD §17 | Section 2.4: Standard single/collection envelopes and RFC 7807. |
| **Search & Diacritic Tests** | B6 §4; PRD §9.8 | Section 2.5: Trigram, FTS, unaccent, and alias tests. |
| **Security & Mutation Blocking**| B7 §3, §4; PRD §17 | Section 2.6: Anonymous read, 405 mutation rejection, CORS. |
| **Media URI & SVG Sanitization**| B8 §5, §7 | Section 2.7: Configured host resolution and XML sanitization. |
| **Performance Budgets & Fallback**| B9 §2, §9; PRD §16 | Section 2.8: All 8 p95 latency targets, payloads, D2 $\rightarrow$ D1 fallback. |
| **Ingestion Quality Gates** | B10 §4; Context §4.N | Section 2.9: Hard/soft quality gates and idempotency tests. |
| **Cross-Block Regression** | Context §4.N; Rule 02 | Section 3: Cross-Block Regression Matrix. |

---

## 7. Decisions Resolved & Deferred

### Decisions Resolved in Block B12:
1. **RESOLVED B12-01**: Established the **Subsystem Verification Architecture** covering all 11 preceding architectural blocks.
2. **RESOLVED B12-02**: Formulated the **Cross-Block Regression Test Matrix** validating inter-block contracts.
3. **RESOLVED B12-03**: Defined the **Dedicated Zero-Fabrication Negative Test Suite**.
4. **RESOLVED B12-04**: Formulated **Performance Target Benchmarking Protocols** across all 8 B9 operation categories.
5. **RESOLVED B12-05**: Established the **MUST / SHOULD / DEFERRED Release-Readiness Criteria**.

### Decisions Deferred to Implementation (Stage 4):
1. **Specific Test Runner & Framework Selection (e.g., Vitest / Jest / Supertest)** → *Deferred to Stage 4 (Backend Implementation)*.
2. **Concrete Synthetic Load Testing Tooling (e.g., k6 / Autocannon)** → *Deferred to Stage 4 (Backend Implementation)*.
3. **CI/CD Pipeline YAML Workflow Configuration** → *Deferred to Deployment / Infrastructure Setup*.
