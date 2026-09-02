# Performance & Caching Architecture (Block B9)

## 1. Purpose

This document establishes the **Performance & Caching Architecture** for the **Mahābhārata Explorer** backend. It specifies the measurable latency and payload budgets, query optimization strategies, database indexing standards, HTTP and edge caching models, progressive disclosure patterns, rate limiting controls, and degradation behaviors required to deliver an instantaneous, responsive exploration experience across all lenses.

In accordance with project principles:
- **One Knowledge Graph, Responsive Multi-Lens Discovery (Rule 01, REQ-PRF-01, REQ-PRF-02)**: The unified knowledge graph must serve diverse exploration lenses (Characters, Timeline, Map, Wars, Graph Focus, Search) with consistent, low-latency data delivery without creating specialized data silos.
- **Zero Fabrication & Information Integrity (Rule 03, PRD §11)**: Performance optimizations, caching layers, and payload shedding must **never** truncate, distort, or fabricate epistemic certainty states, variant traditions, or citation provenance.
- **System Reuse & Bounded Dataset Planning Assumption (Rule 02, Rule 04, REQ-PRF-04)**: For V1, the canonical dataset is scoped as a bounded, curated corpus (with a planning assumption of $\le 100\text{ MB}$ total structured data). The performance architecture maximizes PostgreSQL's native buffer pool and standard HTTP/edge caching, avoiding the operational complexity of mandatory distributed cache clusters (e.g., Redis) unless justified by future scale.

---

## 2. Measurable Architectural Performance Budgets (REQ-PRF-03)

To resolve **REQ-PRF-03** ("Explicit numeric latency and throughput budgets"), the backend establishes the following **architectural design budgets**. These figures represent engineering targets against which implementation must be validated during Block B12 benchmarking, rather than empirical measurements.

```
┌────────────────────────────────────────────────────────────────────────┐
│                   V1 ARCHITECTURAL PERFORMANCE BUDGETS                 │
├───────────────────────────────────┬──────────────────┬─────────────────┤
│ Query / Operation Category        │ Target Latency   │ Max Payload Size│
│                                   │ (p95 at Server)  │ (Uncompressed)  │
├───────────────────────────────────┼──────────────────┼─────────────────┤
│ 1. Canonical Entity Detail        │ $\le 50\text{ ms}$ │ $\le 100\text{ KB}$│
│ 2. Autocomplete Suggestions       │ $\le 30\text{ ms}$ │ $\le 20\text{ KB}$ │
│ 3. Global Multi-Entity Search     │ $\le 100\text{ ms}$│ $\le 150\text{ KB}$│
│ 4. Timeline Slice / Chronology    │ $\le 75\text{ ms}$ │ $\le 250\text{ KB}$│
│ 5. Map Spatial View (Bounding Box)│ $\le 75\text{ ms}$ │ $\le 200\text{ KB}$│
│ 6. War Day Tactical Occurrences   │ $\le 75\text{ ms}$ │ $\le 250\text{ KB}$│
│ 7. Global Focus Subgraph ($D_1$)  │ $\le 100\text{ ms}$│ $\le 300\text{ KB}$│
│ 8. Global Focus Subgraph ($D_2$)  │ $\le 200\text{ ms}$│ $\le 500\text{ KB}$│
└───────────────────────────────────┴──────────────────┴─────────────────┘
```

> **Verification Note**: All target latencies represent server-side execution time under nominal load. Formal empirical benchmarking and load testing are deferred to Block B12 (Backend Testing & Verification).

---

## 3. Database Query & Indexing Architecture

Because V1 is a read-heavy, low-mutation workload, query execution is optimized through specialized indexing, deterministic query shapes, and bounded join depths:

```
                                INCOMING QUERY
                                       │
                ┌──────────────────────┴──────────────────────┐
                │                                             │
                ▼ (Single Entity Lookup)                      ▼ (Multi-Node / Graph Lookup)
      ┌────────────────────┐                       ┌─────────────────────┐
      │  B-Tree Index Scan │                       │ Parameterized CTE / │
      │  (Slug / ID PK)    │                       │ Filtered Subquery   │
      └─────────┬──────────┘                       └──────────┬──────────┘
                │                                             │
                ▼                                             ▼
      ┌──────────────────────────────────────────────────────────────────┐
      │                   POSTGRESQL SHARED BUFFER POOL                  │
      │   (Primary in-memory cache mechanism for bounded V1 workload)    │
      └──────────────────────────────────────────────────────────────────┘
```

### 3.1. Indexing Standards Across Canonical Tables
1. **Primary Key & Slug B-Tree Indexes**:
   - Unique B-tree indexes on `(slug)` for all slug-bearing entities: `characters`, `events`, `locations`, `groups`, `wars`, `formations`, `sources`.
   - Primary key B-tree indexes on `(id)` for all 12 canonical tables: `characters`, `events`, `locations`, `groups`, `wars`, `war_days`, `formations`, `sources`, `claims`, `evidence`, `relationships`, `family_relationships`.
2. **Foreign Key & Relationship Composite Indexes**:
   - Composite B-tree indexes on polymorphic and typed relationship endpoints: `relationships(source_entity_type, source_entity_id)`, `relationships(target_entity_type, target_entity_id)`, `family_relationships(character_id, relative_id)`, `war_days(war_id, day_number)`.
   - Relational association table indexing: `event_participants(event_id, character_id)` to optimize participant and event-character lookups.
3. **Chronological & Spatial Composite Indexes**:
   - Timeline: `events(sequence_index, id)` for deterministic chronology scans.
   - Spatial: `locations(latitude, longitude)` where coordinates are not null.
4. **Search Trigram & Full-Text GIN Indexes**:
   - GIN trigram indexes on `name`, `title`, and `array_to_string(alternate_names, ' ')` (Block B6).
   - GIN full-text indexes on `events(summary)`, `claims(claim_text)`, and `evidence(excerpt)`.

### 3.2. Query Anti-Patterns & Execution Safeguards
- **Unbounded Deep Graph Recursion**: Recursive CTEs without strict depth limits ($D > 2$) or cycle detection are prohibited.
- **N+1 Entity Fetching**: Application routes must use single-query joined projections or batch `IN (...)` lookups.
- **API Collection Bounds vs. Internal Safeguards**: Public API collection endpoints enforce bounded pagination (`limit <= 100`, B5 §7.1), while internal database queries employ explicit `LIMIT` clauses and statement timeouts to prevent unbounded scans.

---

## 4. Lens-Specific Performance Strategies

### 4.1. Global Focus Subgraph Performance ($D_0, D_1, D_2$)
- **Bounded Exploration**: Requests default to $D=1$ ($\text{limit} \le 50$ connected nodes). $D=2$ traversal is optional and capped at $100$ total nodes (B3 §7.3).
- **Early Termination & Timeouts**: Graph queries enforce a database statement timeout of $500\text{ ms}$ to prevent runaway execution on dense hub nodes (e.g., Krishna, Arjuna).

### 4.2. Timeline & Chronology Slicing (REQ-TIM-03)
- **Level of Detail (LOD) Projections**: High-level timeline overviews query major milestone events (`importance >= 4`), while granular sub-events are loaded on demand during zoom/pan interactions.
- **Sequential Query Optimization**: Timeline queries should use the chronological `(sequence_index, id)` ordering and appropriate indexing to support deterministic sequential retrieval while preserving the public offset-pagination contract defined by B5.

### 4.3. Geographic Map & Spatial Bounding-Box Queries (REQ-LOC-04)
- **Bounding Box Slicing**: Queries filter by viewport bounds (`WHERE latitude BETWEEN :min_lat AND :max_lat AND longitude BETWEEN :min_lng AND :max_lng`).
- **Graceful Null Coordinate Handling**: Unmapped locations (`latitude IS NULL`) are excluded from map spatial queries without throwing query errors or returning fabricated coordinates (Rule 03).

### 4.4. War Day Tactical Progressive Loading (REQ-WAR-03)
- **Isolated Day Payloads**: War events, commanders, and casualties are queried day-by-day (`WHERE war_id = :war_id AND day_number = :day_number`).
- **Mega-Payload Prevention**: The backend never returns all 18 days of the Kurukshetra War in a single unpaginated request.

---

## 5. Progressive Disclosure Architecture (REQ-PRF-01)

To minimize network transfer and client memory footprint, the backend implements **Three-Tier Progressive Disclosure**:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   THREE-TIER PROGRESSIVE DISCLOSURE                    │
├───────────────────────────────────┬────────────────────────────────────┤
│ TIER 1: LIGHTWEIGHT SUMMARY CARD  │ TIER 2: COMPLETE ENTITY DETAIL     │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Slug, Name, Role, Image URI     │ - Full biography, typed attributes │
│ - Immediate rendering (< 20 KB)   │ - Connected relationship list      │
│ - Returned by search/lists/links  │ - Filtered event participations    │
├───────────────────────────────────┴────────────────────────────────────┤
│ TIER 3: GRANULAR EVIDENCE & PROVENANCE (ON DEMAND)                     │
│ - Detailed shloka citations, Sanskrit excerpts, scholarly assessments  │
│ - Loaded on-demand when user opens Evidence Drawer / Citation Modal    │
└────────────────────────────────────────────────────────────────────────┘
```

1. **Tier 1 (Summary Card)**: Returned in search results, map pins, and graph hovercards ($\le 20\text{ KB}$).
2. **Tier 2 (Canonical Profile)**: Returned when navigating to `/characters/:slug`, `/events/:slug`, etc. ($\le 100\text{ KB}$).
3. **Tier 3 (Granular Provenance)**: Loaded on demand via `/entities/:entity_type/:slug/provenance` or `/relationships/:id/evidence` when expanding citations ($\le 150\text{ KB}$).

---

## 6. Caching Architecture & Infrastructure Evaluation

We evaluate caching infrastructure options for the V1 deployment:

### 6.1. Evaluation of Caching Infrastructure

| Approach | Description | Evaluation & Tradeoffs | V1 Architectural Status |
| :--- | :--- | :--- | :--- |
| **Option 1: PostgreSQL Buffer Pool + HTTP/Edge Caching** | Relies on PostgreSQL `shared_buffers` as primary database caching mechanism, paired with HTTP `Cache-Control` / `ETag` headers at the edge. | **Pros**: Zero operational complexity, zero cache synchronization bugs, zero extra infrastructure cost.<br>**Cons**: Does not share application-level memory across multi-instance clusters (unnecessary for V1). | **SELECTED V1 ARCHITECTURE** |
| **Option 2: Dedicated Distributed Redis Cache Cluster** | External Redis cluster caching serialized JSON API responses. | **Pros**: Sub-millisecond cache hits across multi-server clusters.<br>**Cons**: High operational complexity, extra infrastructure dependency, cache invalidation overhead. Violates Scope Discipline for a small curated dataset. | **REJECTED FOR V1 (DEFERRED TO V2/V3 SCALE)** |
| **Option 3: In-Process LRU Memory Cache (Node.js)** | In-memory `lru-cache` inside Node.js backend process. | **Pros**: Fast local hits.<br>**Cons**: Memory duplication across worker processes; potential stale data if not carefully bounded. | **OPTIONAL LOCAL OPTIMIZATION** |

---

### 6.2. Selected Caching Architecture: Edge + Database-Level Caching

The backend adopts a **Dual-Layer Caching Strategy**:

1. **Layer 1: PostgreSQL Shared Buffers**:
   - Acts as the primary database-level caching mechanism expected to be highly effective for the bounded V1 workload. Exact buffer pool sizing and cache residency are deployment-dependent and will be verified during B12 benchmarking.
2. **Layer 2: HTTP & Edge Caching**:
   - API endpoints emit deterministic `ETag` and `Cache-Control` headers.
   - Reverse proxies or CDN edge caches serve unauthenticated public read requests.

### 6.3. HTTP Cache-Control Standards

| Endpoint Category | Cache-Control Header | Revalidation Strategy |
| :--- | :--- | :--- |
| **Canonical Entity Detail (`/characters/:slug`, etc.)** | `public, max-age=3600, stale-while-revalidate=86400` | Strong `ETag` validation |
| **Global Focus Subgraph (`/graph/focus/...`)** | `public, max-age=3600, stale-while-revalidate=86400` | Strong `ETag` validation |
| **Search Autocomplete (`/search/suggest?q=...`)** | `public, max-age=1800, stale-while-revalidate=3600` | Weak `ETag` |
| **Static Versioned Assets (`/assets/...`)** | `public, max-age=31536000, immutable` (B8 §7.1) | Content-hashed URL |
| **Administrative Endpoints (`/api/v1/admin/*`)** | `private, no-cache, no-store, must-revalidate` | No caching permitted |

---

## 7. Cache Invalidation & Comprehensive Versioning Strategy

Because V1 uses a curated batch-publishing model (REQ-ING-03, B7 §4.1):
1. **Dataset Release Versioning & ETag Generation**:
   - The backend tracks a dataset release version identifier (e.g., release tag or commit hash) representing the current state of canonical entities, relationships, claims, evidence, and epistemic statuses.
   - Changes to canonical facts, relationship edges, source claims, evidence passages, or field-level epistemic statuses (`epistemic_status`, `coordinate_status`, `chronology_status`) update this release identifier, ensuring new `ETag` values are generated for affected resources.
2. **Conditional Revalidation & Edge Invalidation**:
   - Clients and intermediate caches issuing conditional HTTP requests (via `If-None-Match`) receive `304 Not Modified` when the cached representation remains current, and `200 OK` with fresh data when a new dataset release version has modified the `ETag`.
   - Concrete purge mechanisms for reverse proxies or CDN edge layers depend on the deployed hosting platform and are deferred to deployment/infrastructure planning.

---

## 8. Rate Limiting, Throttling & Abuse Protection

Although public exploration is read-only and unauthenticated (B7 §3.1), rate limiting protects the database from resource exhaustion and scraping abuse:

```
┌────────────────────────────────────────────────────────────────────────┐
│            INITIAL ARCHITECTURAL RATE LIMIT BUDGETS (PER IP)           │
├───────────────────────────────────┬────────────────────────────────────┤
│ Endpoint Category                 │ Initial Architectural Target       │
├───────────────────────────────────┼────────────────────────────────────┤
│ Public Navigation & Entity Detail │ $300\text{ requests / minute}$     │
│ Autocomplete & Search Queries     │ $120\text{ requests / minute}$     │
│ Focus Subgraph Traversal ($D_2$)  │ $60\text{ requests / minute}$      │
│ Administrative Endpoints          │ $30\text{ requests / minute}$      │
└───────────────────────────────────┴────────────────────────────────────┘
```

> **Target Note**: These rate limit figures are **initial architectural starting targets**, not empirically validated limits. Formal load testing and threshold adjustments are deferred to Block B12 verification.

- **Throttling Response**: When a client exceeds the threshold, the API responds with standard `429 Too Many Requests` (RFC 6585) containing a `Retry-After: <seconds>` header.
- **IP Extraction**: Client IP is extracted safely from trusted proxy headers (`X-Forwarded-For`) configured at deployment.

---

## 9. Graceful Degradation & Failure Safeguards

To maintain system stability and prevent cascading database failures under heavy query loads:

1. **Statement Timeouts**:
   - Global read query timeout: $1000\text{ ms}$.
   - Heavy graph traversal timeout: $500\text{ ms}$.
2. **Deterministic Focus Traversal Degradation Flow**:
   - If a $D=2$ Focus traversal exceeds its $500\text{ ms}$ timeout, the backend automatically attempts the bounded $D=1$ fallback query.
   - If the $D=1$ fallback completes successfully, the backend returns that $D=1$ result using the standard [05-api-architecture.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/05-api-architecture.md) response envelope without introducing non-standard response fields.
   - If the $D=1$ fallback also cannot complete within the applicable execution budget, the API returns the standard RFC 7807-compatible `504 Gateway Timeout` error response (B7 §8.1).
3. **Safe Error Envelopes**:
   - Unhandled query timeouts return standard RFC 7807 error envelopes (`504 Gateway Timeout`) without leaking connection strings or backend stack traces.

---

## 10. Observability & Performance Monitoring Requirements

In preparation for Block B12 verification:
1. **Structured Request Logging**:
   - Server logs execution duration (`duration_ms`), route pattern, HTTP status, and client IP for every request.
2. **Slow Query Logging**:
   - PostgreSQL logs any query taking $> 100\text{ ms}$ for automated inspection during development and testing.
3. **Synthetic Load Verification**:
   - Verification of the performance budgets defined in Section 2 is executed via synthetic load tests in Block B12 before production deployment.

---

## 11. Requirement Traceability Matrix

| Performance Requirement | Source Document | Implemented Architectural Mechanism |
| :--- | :--- | :--- |
| **Progressive Disclosure Payloads** | Context §4.O (REQ-PRF-01); Blueprint §13.2 | Section 5: Three-tier disclosure (Summary card $\rightarrow$ Profile $\rightarrow$ Provenance). |
| **Fast Navigation Latency** | Context §4.O (REQ-PRF-02); PRD §16 | Section 2, Section 3: B-tree slug lookups, PostgreSQL buffer pool. |
| **Explicit Numeric Budgets** | Context §4.O (REQ-PRF-03); PRD §16 | Section 2: Established formal latency ($\le 50\text{--}100\text{ ms}$) and payload ($\le 100\text{--}500\text{ KB}$) budgets. |
| **Static & Graph Entity Caching** | Context §4.O (REQ-PRF-04); Detailed Ref §16 | Section 6: HTTP `Cache-Control`, `ETag`, and edge caching architecture. |
| **Timeline LOD & Slicing** | Context §4.E (REQ-TIM-03); PRD §9.2 | Section 4.2: Milestone LOD filtering and deterministic sequential query optimization under the public offset-pagination contract. |
| **Spatial Bounding Box Queries** | Context §4.G (REQ-LOC-04); PRD §9.6 | Section 4.3: Viewport coordinate filtering with graceful null handling. |
| **War Day Progressive Loading** | Context §4.H (REQ-WAR-03); PRD §9.7 | Section 4.4: Day-by-day partitioned queries on Kurukshetra War days. |
| **Stateless URLs & Integrity** | Context §4.K (REQ-STA-01); Rule 03 | Section 1, Section 6: Stateless URL resolution without compromising truth states. |

---

## 12. Decisions Resolved & Deferred

### Decisions Resolved in Block B9:
1. **RESOLVED B9-01**: Established **Architectural Performance & Payload Budgets** for all major query categories.
2. **RESOLVED B9-02**: Adopted **Database-Native + Edge HTTP Caching** (PostgreSQL shared buffers + `ETag`/`Cache-Control`), eliminating mandatory Redis infrastructure for V1.
3. **RESOLVED B9-03**: Established the **Three-Tier Progressive Disclosure Model** (Summary $\rightarrow$ Entity $\rightarrow$ Provenance).
4. **RESOLVED B9-04**: Formulated **Lens-Specific Query Bounds** (LOD timeline, spatial bounding box, day-by-day war loading, $D \le 2$ graph focus limits).
5. **RESOLVED B9-05**: Defined **Initial Public Rate Limiting Budgets** (sliding-window IP limits to prevent scraping exhaustion).
6. **RESOLVED B9-06**: Defined **Deterministic Graceful Degradation Flow** (D2 timeout $\rightarrow$ D1 attempt $\rightarrow$ standard 504 Gateway Timeout).

### Decisions Deferred to Subsequent Blocks:
1. **In-Process Cache Implementation Details (`lru-cache` vs memory map)** → *Deferred to Stage 4 (Implementation)*.
2. **Concrete CDN Vendor & Edge Purge Configuration** → *Deferred to Deployment / Hosting Setup*.
3. **Empirical Latency Benchmarking & Load Testing Verification** → *Deferred to Block B12 (Backend Testing)*.
4. **Distributed In-Memory Cache (Redis Cluster)** → *Deferred to V2/V3 Scale Roadmap*.
