# API Client, Data Fetching & Caching Architecture (Block F6)

## 1. Architectural Context & Purpose

This document establishes the **API Client, Data Fetching & Caching Architecture** for the **Mahābhārata Explorer** frontend. It defines the client-side network boundary, request construction, response deserialization, query identity, cache lifecycle, conditional HTTP revalidation capabilities, concurrency protection, retry policies, rate-limit backpressure handling, and offset pagination for Stage 2.

```
┌────────────────────────────────────────────────────────────────────────┐
│                   STAGE 2 ARCHITECTURAL PLACEMENT                      │
├───────────────────────────────────┬────────────────────────────────────┤
│ UPSTREAM CONTRACTS & CONSTRAINTS  │ DOWNSTREAM FRONTEND SPECIFICATIONS │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Stage 1 Backend Architecture    │ - F7: Routing & Navigation UX      │
│   (B2 Data, B3 Graph, B4 Provenance│ - F8: Global Search UX             │
│    B5 API, B7 Auth, B9 Perf/Cache)│ - F9: Character & Entity Views     │
│ - Block F1: Frontend Constitution │ - F10–F14: Visualizations          │
│ - Block F2: Tech Stack (React SPA)│ - F15: Evidence & Provenance UX    │
│ - Block F3: Tripartite State Model│ - F16: Accessibility & IAST        │
│ - Block F4: Shell & Context Panels│ - F17: Testing & Budgets           │
│ - Block F5: State Management Model│                                    │
└───────────────────────────────────┴────────────────────────────────────┘
```

### 1.1 Pure Architectural Specification & Library Neutrality
Block F6 is strictly an architectural specification. It establishes data flow rules, protocol contracts, cache behavior, and error boundaries. It does not select or install third-party data-fetching libraries, write API client code, configure caching packages, or create mock servers.

---

## 2. Core Architectural Principles

The API client and data-fetching architecture is governed by 10 foundational principles:

```
┌────────────────────────────────────────────────────────────────────────┐
│                     API CLIENT CORE PRINCIPLES                         │
├────────────────────────────────────────────────────────────────────────┤
│ 1. BACKEND DOMAIN AUTHORITY: Backend REST API responses are the sole   │
│    authoritative source of truth for all domain entities and graphs.   │
│ 2. CACHE AS READ-ONLY OPTIMIZATION: Client cache entries are read-only │
│    snapshots and performance optimizations, not an independent database│
│ 3. DECOUPLED FROM POSTGRESQL: Frontend consumes typed HTTP API DTOs;   │
│    it has zero knowledge of database tables, joins, or SQL schemas.    │
│ 4. STRICT ZERO FABRICATION: Missing, unavailable, or failed network    │
│    responses are never synthesized or converted into verified data.    │
│ 5. LIFECYCLE VS. EPISTEMIC ISOLATION: Network lifecycle states (loading,│
│    success, error) remain strictly distinct from B2 epistemic states.  │
│ 6. BOUNDED FOCUS FETCHING: Graph requests enforce the invariant D ≤ 2  │
│    with transparent handling of backend D2 → D1 fallback and 504.     │
│ 7. CONCURRENCY & OBSOLESCENCE DEFENSE: Obsolete asynchronous responses │
│    must never overwrite state from a newer exploration context.        │
│ 8. HTTP REVALIDATION CAPABILITY: Supports protocol-level HTTP cache    │
│    headers and validators (such as ETags/Last-Modified) where provided.│
│ 9. BOUNDED RETRIES & NO RETRY STORMS: Bounded backoff limits prevent   │
│    client-driven cascade failures under network degradation.           │
│ 10. ANONYMOUS READ-ONLY ACCESS: Public exploration client is anonymous │
│     and read-only; administrative operations remain strictly outside.  │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 3. API Boundary & Domain Representation Model

The frontend interacts with the backend strictly across an HTTP REST boundary:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        API INTEGRATION BOUNDARY                        │
├────────────────────────────────────────────────────────────────────────┤
│ FRONTEND CLIENT (React SPA)                                            │
│   │                                                                    │
│   ▼ (HTTP GET / Standard REST Envelopes)                               │
│ BACKEND REST API ([05-api-architecture.md])                           │
│   ├── Single Entity Envelope: `{ data: T }`                            │
│   ├── Collection Envelope: `{ data: T[], pagination: OffsetPagination }│
│   ├── Error Envelope (RFC 7807): `{ type, title, status, detail, ... }`│
│   └── HTTP Caching: Protocol headers & conditional validators          │
└────────────────────────────────────────────────────────────────────────┘
```

### 3.1 DTO Ingestion & Semantic Decoupling
1. **Domain Representation Isolation**: The frontend maps incoming API payloads into typed view representations without assuming backend relational structure.
2. **Defensive Ingestion**: Additional or unexpected backend fields are safely ignored; missing optional fields default to typed `null` values without throwing deserialization errors.
3. **Immutability**: Deserialized responses are treated as frozen, read-only data structures.

---

## 4. API Client Layer Architecture

The API client abstracts transport execution while exposing clean asynchronous data streams to the Block F5 state architecture:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        API CLIENT LAYER MODEL                          │
├────────────────────────────────────────────────────────────────────────┤
│ 1. REQUEST CONSTRUCTION: Normalizes base URLs, path parameters, and    │
│    query parameters; sets protocol headers (e.g., `Accept: json`).     │
├────────────────────────────────────────────────────────────────────────┤
│ 2. TRANSPORT EXECUTION: Dispatches standard HTTP GET requests; manages │
│    timeouts and network abort signals.                                 │
├────────────────────────────────────────────────────────────────────────┤
│ 3. RESPONSE INTERPRETATION: Evaluates HTTP status codes (2xx Success,  │
│    304 Not Modified, 4xx Client Errors, 5xx Failures).                 │
├────────────────────────────────────────────────────────────────────────┤
│ 4. DESERIALIZATION & DOMAIN HANDLING: Parses JSON envelopes (`data`),  │
│    handles validation boundaries, and normalizes RFC 7807 problem      │
│    details upon encountering error responses.                          │
├────────────────────────────────────────────────────────────────────────┤
│ 5. CACHE INTEGRATION: Exposes normalized data to the client query      │
│    cache and notifies subscribing UI components.                       │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 5. Request Lifecycle Architecture

The client tracks the lifecycle of every network query through distinct states, cleanly separated from historical epistemic truth states:

```
┌────────────────────────────────────────────────────────────────────────┐
│                       REQUEST LIFECYCLE MODEL                          │
├──────────────┬─────────────────────────────────────────────────────────┤
│ State        │ Behavioral Meaning & UI Manifestation                   │
├──────────────┼─────────────────────────────────────────────────────────┤
│ **`idle`**   │ Initial state before request dispatch (un-fetched).     │
├──────────────┼─────────────────────────────────────────────────────────┤
│ **`loading`**│ Initial network dispatch; displaying geometric skeleton.│
├──────────────┼─────────────────────────────────────────────────────────┤
│ **`refresh`**│ Background revalidation active; displaying cached data  │
│              │ with an unobtrusive background activity indicator.      │
├──────────────┼─────────────────────────────────────────────────────────┤
│ **`success`**│ Valid payload received and cached; displaying view.     │
├──────────────┼─────────────────────────────────────────────────────────┤
│ **`empty`**  │ Valid 200 payload with 0 results; displaying empty state│
├──────────────┼─────────────────────────────────────────────────────────┤
│ **`error`**  │ Network or HTTP failure; displaying RFC 7807 alert card.│
└──────────────┴─────────────────────────────────────────────────────────┘
```

---

## 6. Query Identity & Cache-Key Architecture

Cache keys provide deterministic query identity for caching, deduplication, and revalidation:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        QUERY KEY ARCHITECTURE                          │
├────────────────────────────────────────────────────────────────────────┤
│ A query key is an abstract, structured, serializable identifier:       │
│   `[Resource_Category, Canonical_Identity, Normalized_Parameters]`     │
│                                                                        │
│ Query Identity Characteristics:                                        │
│   - Category: Identifies the resource domain (e.g., entity, graph).    │
│   - Canonical Identity: Entity slug or unique domain identifier.       │
│   - Normalized Parameters: Deterministically ordered filter parameters.│
└────────────────────────────────────────────────────────────────────────┘
```

### 6.1 Query Equivalence Rules
1. **Deterministic Parameter Normalization**: Query parameters are ordered deterministically to ensure that equivalent parameter combinations resolve to identical cache keys.
2. **Contextual Independence**: Transient UI states (e.g., drawer scroll position, hover coordinates) are strictly excluded from query keys.

---

## 7. Client-Side Query Cache Architecture

The client cache acts as a read-only performance buffer:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        CLIENT CACHE SEMANTICS                          │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Dimension        │ Architectural Policy & Semantic Definition          │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Ownership**    │ Owned by API Client Query Layer; read-only to UI.   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Entry Schema** │ Encapsulates query identity, backend-originated     │
│                  │ representation, lifecycle/freshness metadata, and   │
│                  │ protocol validation metadata where available.       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Read-Through** │ Queries check the cache:                            │
│                  │ - Hit (Fresh): Immediate return of representation.  │
│                  │ - Hit (Stale): Returns cached representation and    │
│                  │   triggers background conditional revalidation.     │
│                  │ - Miss: Dispatches initial network request.         │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Residency**    │ Cache residency, eviction, and concrete memory-     │
│                  │ management mechanisms are deferred to Stage 4.      │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 8. Freshness & Conditional HTTP Revalidation Architecture

The frontend supports protocol-level conditional HTTP revalidation capabilities:

```
┌────────────────────────────────────────────────────────────────────────┐
│                  CONDITIONAL REVALIDATION WORKFLOW                     │
├────────────────────────────────────────────────────────────────────────┤
│ 1. INITIAL QUERY: Client requests resource $\rightarrow$ Server returns│
│    `200 OK` with payload and validator metadata (e.g., ETag/Modified). │
├────────────────────────────────────────────────────────────────────────┤
│ 2. SUBSEQUENT QUERY (STALE): Client dispatches background revalidation │
│    attaching conditional validator headers (e.g., `If-None-Match`).    │
├────────────────────────────────────────────────────────────────────────┤
│ 3A. UNCHANGED DATA: Server returns `304 Not Modified` $\rightarrow$    │
│     Client updates freshness metadata; avoids view re-rendering.       │
├────────────────────────────────────────────────────────────────────────┤
│ 3B. UPDATED DATA: Server returns `200 OK` with new payload and metadata│
│     $\rightarrow$ Cache entry updates; subscribing views re-render.    │
└────────────────────────────────────────────────────────────────────────┘
```
*(Note: Conditional validators such as ETag and Last-Modified are supported protocol capabilities where provided by the backend; F6 does not make them mandatory universal implementation mandates for all endpoints).*

---

## 9. Request Deduplication & Concurrency Invariants

1. **In-Flight Deduplication**: Equivalent concurrent requests should be deduplicated where practical to avoid unnecessary duplicate network operations.
2. **Obsolescence Invariant**: **Obsolete asynchronous results must not overwrite state associated with a newer exploration context.** Concrete request cancellation, context tagging, and network coordination mechanisms are implementation details owned by Stage 4.

---

## 10. Retry, Failure & Error Architecture

The API client categorizes failures into retryable and non-retryable classes:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        FAILURE TAXONOMY & POLICY                       │
├──────────────────┬──────────────────┬──────────────────────────────────┤
│ Failure Class    │ HTTP Codes       │ Client Policy & UX Manifestation │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Non-Retryable  │ `404 Not Found`, │ Immediate failure; render inline │
│ Client Error**   │ `400 Bad Request`│ error banner or empty entity card│
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Rate Limited** │ `429 Too Many    │ Respect rate-limit window; block │
│                  │ Requests`        │ automatic retries; render notice.│
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Transient      │ `502`, `503`,    │ Bounded backoff retry policy;    │
│ Server Error**   │ Network Timeout  │ avoids retry storms; then alert. │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Focus Timeout**│ `504 Gateway     │ Invariant: Focus operation timed │
│                  │ Timeout`         │ out. Render explicit retry card. │
└──────────────────┴─────────────────────────────────────────────────────┘
```

### 10.1 RFC 7807 Error Ingestion
When the backend returns an error envelope (`application/problem+json`), the client parses `type`, `title`, `status`, and `detail` to render human-readable, context-aware error messages without leaking database stack traces.

---

## 11. Pagination & Incremental Fetching Architecture

Public collection pagination follows the offset-pagination contract established by B5 ([05-api-architecture.md §7.1](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/05-api-architecture.md#L242)). F6 does not introduce or expose an alternative cursor-pagination contract. Backend query-execution optimizations remain an internal backend concern.

All public collection endpoints utilize the standardized offset-pagination envelope:

```
┌────────────────────────────────────────────────────────────────────────┐
│                      OFFSET PAGINATION ENVELOPE                        │
├────────────────────────────────────────────────────────────────────────┤
│ `{`                                                                    │
│   `"data": [ ... ],`                                                   │
│   `"pagination": {`                                                    │
│     `"offset": 0,`                                                     │
│     `"limit": 50,`                                                     │
│     `"total_count": 342,`                                              │
│     `"has_more": true`                                                 │
│   `}`                                                                  │
│ `}`                                                                    │
└────────────────────────────────────────────────────────────────────────┘
```
- **Incremental Data Append**: Successive pages (`offset = 50, 100, ...`) are appended to derived collection views without mutating cached page blocks.

---

## 12. Backend Payload Compatibility Targets (B9 Alignment)

The frontend architecture remains strictly compatible with the **endpoint-specific backend payload targets** established in Stage 1 ([09-performance-and-caching.md §5](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/09-performance-and-caching.md#L140)):

```
┌────────────────────────────────────────────────────────────────────────┐
│             INHERITED BACKEND B9 PAYLOAD COMPATIBILITY TARGETS         │
├──────────────────────────┬─────────────────────────────────────────────┤
│ Endpoint Category        │ Inherited Backend B9 Target Payload Limit   │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Entity Detail            │ Target: $\le 100\text{ KB}$                 │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Autocomplete             │ Target: $\le 20\text{ KB}$                  │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Global Search            │ Target: $\le 150\text{ KB}$                 │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Timeline                 │ Target: $\le 250\text{ KB}$                 │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Map bbox                 │ Target: $\le 200\text{ KB}$                 │
├──────────────────────────┼─────────────────────────────────────────────┤
│ War Day                  │ Target: $\le 250\text{ KB}$                 │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Focus D1                 │ Target: $\le 300\text{ KB}$                 │
├──────────────────────────┼─────────────────────────────────────────────┤
│ Focus D2                 │ Target: $\le 500\text{ KB}$                 │
└──────────────────────────┴─────────────────────────────────────────────┘
```
*(Note: These are inherited backend design targets and compatibility constraints from B9, NOT new frontend performance budgets and NOT redefined by Block F6. Formal frontend performance budgets are governed exclusively by Block **F17**).*

---

## 13. Focus Graph Fetching & Fallback Integration

Focus graph fetching adheres to the invariants established in B3, B5, and B9:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   FOCUS GRAPH FETCHING INTEGRATION                     │
├────────────────────────────────────────────────────────────────────────┤
│ 1. BOUNDED DEPTH: Requests enforce $D=1$ or $D=2$ (REQ-GRP-01).        │
│ 2. BACKEND FALLBACK ($D_2 \rightarrow D_1$): If the backend degrades a │
│    $D=2$ request to $D=1$ due to node density timeout, the client      │
│    receives the $D=1$ payload with a fallback indicator banner.        │
│ 3. TIMEOUT ERROR (504): If the operation times out completely, the     │
│    client renders an error state; it NEVER fabricates a fake graph.    │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 14. Endpoint-Class Fetching Strategies

Block F6 establishes the high-level semantic fetching characteristics across endpoint categories:

1. **Entity / Detail Representations**:
   - Suitable for cache reuse where the representation remains valid. Detailed entity-view composition belongs to Block **F9**.
2. **Autocomplete Representations**:
   - Short-lived, request-oriented data. Detailed interaction and debounce behavior belong to Block **F8**.
3. **Global Search Representations**:
   - Query-specific collection representations; offset pagination follows backend contracts. Detailed search UX belongs to Block **F8**.
4. **Timeline Representations**:
   - Chronological sequence representations reusable according to cache policy. Detailed chronology and chunking behavior belong to Block **F12**.
5. **Spatial / Map Representations**:
   - Request-specific spatial bounding-box representations reusable according to cache policy. Detailed spatial partitioning belongs to Block **F13**.
6. **Focus Graph Representations**:
   - Bounded subgraph representations strictly respecting $D \le 2$ and B6/B9 fallback constraints. Detailed visualization rendering belongs to Block **F10**.

---

## 15. Rate Limits & Backpressure Handling

The API client respects backend rate limits established in Stage 1 ([05-api-architecture.md §7](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/05-api-architecture.md#L180)):
- Navigation / Entity Profiles: $300\text{ req/min}$
- Autocomplete / Search: $120\text{ req/min}$
- Focus Subgraphs ($D=2$): $60\text{ req/min}$
- Administrative Operations: $30\text{ req/min}$

*Client Backpressure Behavior*: Upon receiving `429 Too Many Requests`, the client pauses automatic revalidation, displays a rate-limit notice, and resumes queries once the backpressure window clears.

---

## 16. Offline & Network Degradation Behavior

1. **Graceful Network Degradation**: Previously cached representations may remain usable during network degradation according to cache and freshness policies.
2. **No False Certainty**: The UI clearly communicates that stale cached data cannot reflect real-time updates until network connectivity is restored. Block F6 does not introduce an offline-first architecture.

---

## 17. Security & Trust Boundary Architecture

1. **Public Read-Only Boundary**: The public exploration client is anonymous, read-only, and exposes zero public mutation workflows. Administrative operations remain strictly outside the public exploration client.
2. **Untrusted Data Principle**: Server-provided content is treated as untrusted data; arbitrary backend content is never evaluated as executable script.
3. **Media & Vector Safety**: Visual media and SVG assets adhere to the backend XML sanitization boundaries established in Stage 1 ([08-storage-and-media.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/08-storage-and-media.md)).
4. **Zero Client Secrets**: Zero private API keys, cryptographic secrets, or administrative tokens exist within public client bundles.

---

## 18. Upstream Dependency & Downstream Ownership Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                     DOWNSTREAM OWNERSHIP MATRIX                        │
├────────────────────────────────────────┬───────────────────────────────┤
│ Architectural Domain                   │ Authoritative Target Block    │
├────────────────────────────────────────┼───────────────────────────────┤
│ Client State Ownership & Store Model   │ **Block F5**                  │
│ Route Grammar, URL Schema & Router     │ **Block F7**                  │
│ Global Search Modal & Command UX       │ **Block F8**                  │
│ Character Profile Layouts & Tabs       │ **Block F9**                  │
│ Knowledge Graph Visualization Engine   │ **Block F10**                 │
│ Family Lineage Tree DAG Layout         │ **Block F11**                 │
│ Timeline Scrubber & Chronology Engine  │ **Block F12**                 │
│ Geographic Map Visualization Engine    │ **Block F13**                 │
│ Tactical Battle Formations (Vyuhas)    │ **Block F14**                 │
│ Evidence Drawer Citation & Text UX     │ **Block F15**                 │
│ Accessibility Architecture & IAST/Dev  │ **Block F16**                 │
│ Formal Frontend Performance Budgets    │ **Block F17**                 │
│ Concrete Package Setup & Client Hooks  │ **Stage 4 (Implementation)**  │
└────────────────────────────────────────┴───────────────────────────────┘
```

---

## 19. Architectural Decision Record (ADR)

| Decision ID | Architectural Decision | Chosen Approach | Alternatives Evaluated | Rationale & Trade-offs | Owner | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **ADR-FE-31** | API Client Boundary | **Decoupled REST DTO Ingestion**| Direct SQL, GraphQL | Enforces clean decoupling from PostgreSQL and backend framework neutrality. | F6 | **DECIDED** |
| **ADR-FE-32** | Query Identity Model | **Abstract Tuple Query Keys**   | Raw URL strings, random hash | Provides deterministic query matching and deduplication across views. | F6 | **DECIDED** |
| **ADR-FE-33** | Revalidation Capability | **HTTP Conditional Revalidation**| Periodic blind polling | Supports protocol-level validators (ETag/Modified) where provided by backend.| F6 | **DECIDED** |
| **ADR-FE-34** | Concurrency Safety | **Obsolescence Protection Invariant**| Unchecked async resolution | Invariant ensures stale asynchronous results never overwrite newer state.| F6 | **DECIDED** |
| **ADR-FE-35** | Public Offset Pagination | **B5 Public Offset Pagination Contract** | Public cursor pagination | Consumes established B5 public offset-pagination contract; does not expose an alternative cursor-based public API contract. | F6 | **DECIDED** |
| **ADR-FE-36** | Focus Error Handling | **Explicit 504 Failure Display** | Synthetic fallback generation | Upholds zero-fabrication principle; never synthesizes fake subgraphs. | F6 | **DECIDED** |

---

## 20. Requirement Traceability Matrix

| Requirement / Principle | Source Document | Implementing Block F6 Section | Coverage Status |
| :--- | :--- | :--- | :--- |
| **Backend REST Alignment** | B5 §3, B9 §6 | §3 (API Boundary), §8 (Conditional Revalidation)| **SATISFIED** |
| **Zero Data Fabrication** | F1 §5.4, Rule 03 | §2 (Principles), §13 (Focus Fallbacks) | **SATISFIED** |
| **Focus Depth ($D \le 2$)** | B3 §4, REQ-GRP-01| §13 (Focus Graph Fetching) | **SATISFIED** |
| **Offset Pagination Contract**| B5 §7.1, F1 §8 | §11 (Pagination Architecture) | **SATISFIED** |
| **Exact B9 Payload Alignment**| B9 §5 | §12 (Backend Payload Compatibility Targets)    | **SATISFIED** |
| **Rate-Limit Backpressure** | B5 §7 | §15 (Rate Limits & Backpressure) | **SATISFIED** |
| **RFC 7807 Error Ingestion** | B5 §6 | §10.1 (RFC 7807 Error Ingestion) | **SATISFIED** |
| **State Separation Integrity**| F5 §15 | §5 (Lifecycle vs Epistemic States) | **SATISFIED** |

---

## 21. F6 Exit Criteria Checklist

- [x] API client boundary and REST DTO domain representation model are formalized.
- [x] Library-neutral query key architecture and deterministic equivalence rules are defined.
- [x] Client cache semantics (read-through, freshness metadata, residency hand-off) are specified.
- [x] Conditional HTTP revalidation capability is established without mandatory universal mandates.
- [x] Request deduplication and concurrency obsolescence invariants are defined.
- [x] Bounded retry policy and RFC 7807 problem details ingestion are formalized.
- [x] Public offset-pagination contract is preserved without exposing an alternative public cursor-pagination contract.
- [x] Exact inherited B9 backend payload targets are accurately documented as compatibility constraints.
- [x] Focus graph fetching rules ($D \le 2$, $D_2 \rightarrow D_1$ fallback, 504 timeout error) are enforced.
- [x] Backend rate-limit constraints ($300/120/60/30\text{ req/min}$) and backpressure handling are defined.
- [x] Zero application source code, API client hooks, or package installations were introduced.
- [x] Zero Stage 1 backend documents or Blocks F1–F5 documents were modified.
