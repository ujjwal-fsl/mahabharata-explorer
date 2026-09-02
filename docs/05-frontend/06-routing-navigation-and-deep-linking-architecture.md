# Routing, Navigation & Deep-Linking Architecture (Block F7)

## 1. Architectural Context & Purpose

This document establishes the **Routing, Navigation & Deep-Linking Architecture** for the **Mahābhārata Explorer** frontend. It defines the client-side route hierarchy, canonical URL grammar, lens routing contracts, URL state serialization, navigation semantics across exploration lenses, focus management during transitions, deep-linking contracts, and routing error boundaries for Stage 2.

```
┌────────────────────────────────────────────────────────────────────────┐
│                   STAGE 2 ARCHITECTURAL PLACEMENT                      │
├───────────────────────────────────┬────────────────────────────────────┤
│ UPSTREAM CONTRACTS & CONSTRAINTS  │ DOWNSTREAM FRONTEND SPECIFICATIONS │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Stage 1 Backend Architecture    │ - F8: Global Search UX             │
│   (B2 Data, B3 Graph, B4 Provenance│ - F9: Character & Entity Views     │
│    B5 API, B7 Auth, B9 Perf/Cache)│ - F10: Knowledge Graph Canvas      │
│ - Block F1: Frontend Constitution │ - F11: Family Lineage Tree         │
│ - Block F2: Tech Stack (React SPA)│ - F12: Timeline Scrubber           │
│ - Block F3: Tripartite State Model│ - F13: Geographic Map Engine       │
│ - Block F4: Responsive Shell      │ - F14: War & Tactical Vyuhas       │
│ - Block F5: State Management Model│ - F15: Evidence & Provenance UX    │
│ - Block F6: API Client & Caching  │ - F16: Accessibility & IAST        │
│                                   │ - F17: Testing & Budgets           │
└───────────────────────────────────┴────────────────────────────────────┘
```

### 1.1 Pure Architectural Specification & Library Neutrality
Block F7 is strictly an architectural specification. It authoritatively establishes the canonical route grammar, path/query contracts, and navigation semantics. It does not write client routing code, instantiate router packages, install third-party navigation libraries, or configure web server redirect rules.

---

## 2. Core Architectural Principles

The routing and navigation architecture is governed by 10 foundational principles:

```
┌────────────────────────────────────────────────────────────────────────┐
│                     ROUTING CORE PRINCIPLES                            │
├────────────────────────────────────────────────────────────────────────┤
│ 1. URL AS SINGLE TRUTH FOR EXPLORATION: In accordance with F5, the URL │
│    is the authoritative source of truth for all URL-representable      │
│    exploration coordinates (entity, lens, focus depth, filters).       │
│ 2. PATH FOR IDENTITY, QUERY FOR MODIFIERS: Canonical entity identity   │
│    and structural lenses belong to path segments; exploration filters, │
│    depth ($D$), and pagination belong strictly to query parameters.    │
│ 3. STATELESS DEEP-LINKING REPRODUCIBILITY: All URL-representable       │
│    exploration state must be reproducible from the canonical URL;      │
│    transient UI state is not required to survive refresh or share.     │
│ 4. ZERO TRANSIENT UI ENCODING: Ephemeral UI states (hover, tooltip,   │
│    micro-scroll, drag positions) are strictly prohibited from URLs.    │
│ 5. VIEWPORT-INDEPENDENT URLS: Exploration URLs represent semantic data │
│    coordinates independently of device form factor or layout classes. │
│ 6. DETERMINISTIC CANONICALIZATION: URL parameters are sorted and       │
│    normalized; default values and unsupported parameters are pruned to │
│    guarantee deterministic, clean, and collision-free URLs.            │
│ 7. STRICT BOUNDED FOCUS ($D \le 2$): Graph and focus routing enforce   │
│    the backend invariant $D \le 2$ (REQ-GRP-01); $D > 2$ is clamped.   │
│ 8. OFFSET PAGINATION CONTRACT: Public collection endpoints consume the │
│    standard offset pagination contract ($offset$, $limit$) from B5/F6. │
│ 9. ENTITY CONTINUITY ACROSS LENSES: Navigating between exploration     │
│    lenses preserves the active entity subject across the transition.   │
│ 10. UNTRUSTED INPUT RESILIENCE: All URL paths and parameters are       │
│     treated as untrusted user input and defensively validated.         │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Client-Side Routing Architecture & Model

In alignment with the Client-Side Single Page Application (SPA) architecture established in Block F2 ([01-technology-stack-and-build-architecture.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/05-frontend/01-technology-stack-and-build-architecture.md)):

```
┌────────────────────────────────────────────────────────────────────────┐
│                     CLIENT SPA ROUTING ARCHITECTURE                    │
├────────────────────────────────────────────────────────────────────────┤
│ 1. HTML5 HISTORY API: Leverages standard `pushState` and `replaceState`│
│    for clean paths (`/characters/arjuna`) without hash fragments (`#`).│
├────────────────────────────────────────────────────────────────────────┤
│ 2. TOP-LEVEL SHELL INTEGRATION: The application shell (Block F4)       │
│    remains mounted across route transitions; only the primary workspace│
│    frame and contextual inspection panels update their active views.   │
├────────────────────────────────────────────────────────────────────────┤
│ 3. ROUTE DECOUPLING FROM DATA FETCHING: The router parses navigation   │
│    coordinates and updates URL state (Block F5); data fetching and     │
│    caching are delegated cleanly to the query layer (Block F6).        │
└────────────────────────────────────────────────────────────────────────┘
```

### 3.1 Route Hierarchy
The application route space is partitioned into four structural tiers:
1. **Root / Discovery Route**: The gateway portal providing entry into the epic.
2. **Structural Lens Routes**: Top-level exploration lenses providing domain-specific perspectives over the unified knowledge graph.
3. **Canonical Entity Routes**: Subject-centric routes focused on individual characters, events, places, or factions.
4. **System & Error Routes**: Not-found (`404`), network failure, and maintenance fallback views.

---

## 4. Canonical Route Grammar & Path Hierarchy

Block F7 establishes the authoritative URL grammar for the Mahābhārata Explorer:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        CANONICAL ROUTE TAXONOMY                        │
├──────────────────────────┬─────────────────────────────────────────────┤
│ Route Path Pattern       │ Exploration Perspective & Semantic Scope    │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/`                      │ Root Discovery Portal & Exploration Hub     │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/characters`            │ Character Directory & Index Lens            │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/characters/:slug`      │ Character Biography, Attributes & Profile   │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/graph`                 │ Global Knowledge Graph Overview             │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/graph/:slug`           │ Entity-Centric Focus Subgraph ($D \le 2$)   │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/lineage`               │ Universal Dynasty & Lineage Overview        │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/lineage/:slug`         │ Ancestral & Descendant Lineage DAG          │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/relationships`         │ Relationship Category Matrix & Directory    │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/relationships/:slug`   │ Entity Relational Network & Kinship Lens    │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/timeline`              │ Epic Chronology & Parva Sequence Explorer   │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/timeline/:slug`        │ Entity-Centric Chronological Milestones     │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/geography`             │ Ancient Indian Epic Map & Regional Index    │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/geography/:slug`       │ Historical Location / Kingdom Detail View   │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/factions`              │ Dynasties, Clans & Alliances Overview       │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/factions/:slug`        │ Faction Profile, Members & Allegiances      │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/wars`                  │ Wars & Conflicts Index                      │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/wars/:war_slug`        │ Conflict Summary & Narrative Overview       │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/wars/:war_slug/day/:day`│ Kurukshetra War Daily Tactical Breakdown   │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/vyuhas`                │ Military Formations Directory               │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/vyuhas/:slug`          │ Tactical Battle Formation (Vyuha) Detail    │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/search`                │ Global Scholarly Search Results View        │
└──────────────────────────┴─────────────────────────────────────────────┘
```

---

## 5. Parameter Semantics: Path vs. Query String

To guarantee clean, intuitive, and bookmarkable URLs, state attributes are strictly segregated between path segments and query strings:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   PATH VS. QUERY ATTRIBUTION MATRIX                    │
├───────────────────────────────────┬────────────────────────────────────┤
│ PATH SEGMENTS (Identity & Lens)   │ QUERY PARAMETERS (View Modifiers)  │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Canonical Entity Slug (`:slug`) │ - Focus Graph Depth (`d=1`, `d=2`) │
│ - Exploration Lens Name           │ - Search Query String (`q=...`)    │
│ - War Identifier (`:war_slug`)    │ - Epistemic Status (`status=...`)  │
│ - Discrete War Day Index (`:day`) │ - Chronological Parva (`parva=18`) │
│                                   │ - Collection Offset (`offset=50`)  │
│                                   │ - Collection Limit (`limit=50`)    │
│                                   │ - Relationship Filter (`rel=...`)  │
│                                   │ - Active Tab Identifier (`tab=...`)│
│                                   │ - Evidence Reference (`claim=...`) │
└───────────────────────────────────┴────────────────────────────────────┘
```

### 5.1 Architectural Rule
- **Path Invariant**: Path segments establish **WHAT** resource is being inspected.
- **Query Invariant**: Query parameters establish **HOW** that resource is currently projected, filtered, sliced, or traversed.

---

## 6. Lens Route Specifications & Parameter Contracts

F7 defines the authoritative route hierarchy, parameter locations, serialization, and canonicalization contracts for all lenses. Specific modifier values listed below illustrate the routing pattern; final internal vocabularies and visualization state belong authoritatively to Blocks **F8 through F15**:

### 6.1 Character Lens (`/characters`, `/characters/:slug`)
- **Path Parameter**: `:slug` — Canonical lowercase slug of the character (e.g., `arjuna`, `karna`, `bhishma`).
- **Query Parameters**:
  - `tab`: Active profile tab (illustrative vocabulary: `bio`, `relationships`, `events`, `lineage`; owned by F9). Default: `bio` (omitted when default).
  - `claim`: Canonical claim ID for contextual evidence drawer opening (e.g., `claim=clm_01f8`; owned by F15).

### 6.2 Global Focus Graph Lens (`/graph`, `/graph/:slug`)
- **Path Parameter**: `:slug` — Canonical root entity slug for the focus subgraph.
- **Query Parameters**:
  - `d`: Exploration depth integer. **Constrained strictly to `1` or `2`** (REQ-GRP-01). Default: `1` (omitted when `d=1`).
  - `rel`: Comma-separated relationship filter list (illustrative vocabulary: `kinship`, `alliance`, `guru_shishya`, `rivalry`; owned by F10).

### 6.3 Family Lineage Lens (`/lineage`, `/lineage/:slug`)
- **Path Parameter**: `:slug` — Anchor ancestor or central figure for the lineage tree.
- **Query Parameters**:
  - `generations`: Generational span integer ($1 \le g \le 5$; owned by F11). Default: `2`.
  - `spouses`: Boolean toggle (`true`/`false`) to show/hide marital branches (owned by F11). Default: `true`.

### 6.4 Timeline & Chronology Lens (`/timeline`, `/timeline/:slug`)
- **Path Parameter**: Optional `:slug` to filter chronological milestones involving an entity.
- **Query Parameters**:
  - `parva`: Parva index integer (`1` to `18`; owned by F12). Default: all Parvas.
  - `importance`: Minimum event significance threshold ($1 \le i \le 5$; owned by F12). Default: `3`.
  - `offset`, `limit`: Bounded public offset pagination parameters.

### 6.5 Geographic Map Lens (`/geography`, `/geography/:slug`)
- **Path Parameter**: Optional `:slug` of an ancient kingdom, capital, sacred tirtha, or battlefield.
- **Query Parameters**:
  - `region`: Historical region filter (owned by F13).
  - `category`: Location classification filter (owned by F13).

### 6.6 War & Daily Tactical Lens (`/wars/:war_slug`, `/wars/:war_slug/day/:day`)
- **Path Parameters**:
  - `:war_slug`: Conflict identifier (e.g., `kurukshetra`).
  - `:day`: Integer day index ($1 \le \text{day} \le 18$).
- **Query Parameters**:
  - `phase`: Tactical battle phase (owned by F14).
  - `event`: Specific duel or battle event identifier (owned by F14).

### 6.7 Battlefield Vyuha Lens (`/vyuhas`, `/vyuhas/:slug`)
- **Path Parameter**: `:slug` — Formation identifier (e.g., `krauncha`, `chakravyuha`, `padma`).
- **Query Parameters**:
  - `commander`: Canonical slug of the commanding marshal who arrayed the formation (owned by F14).

### 6.8 Global Scholarly Search (`/search`)
- **Query Parameters**:
  - `q`: Search query string supporting standard Unicode and IAST characters (owned by F8).
  - `category`: Entity filter domain (owned by F8).
  - `status`: Epistemic certainty filter (owned by F8).
  - `offset`, `limit`: Bounded public offset pagination parameters.

---

## 7. URL State Serialization, Normalization & Canonicalization

To guarantee URL shareability and prevent cache fragmentation:

```
┌────────────────────────────────────────────────────────────────────────┐
│                     URL CANONICALIZATION PIPELINE                      │
├────────────────────────────────────────────────────────────────────────┤
│ 1. LOWERCASE PATH SLUGS: Slugs are strictly normalized to lowercase    │
│    kebab-case (e.g., `/characters/Arjuna` $\rightarrow$ `/characters/arjuna`).│
├────────────────────────────────────────────────────────────────────────┤
│ 2. ALPHABETICAL QUERY SORTING: Query keys are serialized in            │
│    deterministic alphabetical order (`?d=2&rel=kinship`).              │
├────────────────────────────────────────────────────────────────────────┤
│ 3. DEFAULT VALUE OMISSION: Parameters matching default values are      │
│    stripped from the URL (`d=1` is omitted, leaving `/graph/karna`).   │
├────────────────────────────────────────────────────────────────────────┤
│ 4. UNSUPPORTED PARAMETER PRUNING: Unsupported or unknown query keys do │
│    not participate in application state and are pruned from the URL.   │
├────────────────────────────────────────────────────────────────────────┤
│ 5. REPLACEMENT ON NORMALIZATION: If an incoming URL deviates from      │
│    canonical format, the router executes `history.replaceState` to    │
│    normalize the browser location bar without adding history entries.  │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 8. Defensiveness & Defensive Parameter Handling

A strict architectural distinction is maintained between **representation normalization** and **semantic resource identity**:

```
┌────────────────────────────────────────────────────────────────────────┐
│             NORMALIZATION VS. RESOURCE IDENTITY BOUNDARY               │
├───────────────────────────────────┬────────────────────────────────────┤
│ REPRESENTATION NORMALIZATION      │ SEMANTIC RESOURCE IDENTITY         │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Changing casing (`/Arjuna` $\to$│ - Path identifiers define resource │
│   `/arjuna`)                      │   identity.                        │
│ - Reordering query parameters     │ - Semantically invalid path values │
│ - Stripping default parameters    │   (e.g., `/wars/.../day/25` or     │
│ - Clamping bounded exploration    │   `/characters/nonexistent`) MUST  │
│   modifiers (e.g., $D > 2 \to 2$) │   NOT be silently converted into   │
│ - Pruning unknown query parameters│   different entities or days.      │
│                                   │ - Invalid identities resolve to a  │
│                                   │   valid `404 / Not Found` outcome. │
└───────────────────────────────────┴────────────────────────────────────┘
```

### 8.1 Defensive Modifier Handling Matrix
Bounded exploration modifiers in query strings are handled defensively:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   PARAMETER SANITIZATION & CLAMPING                    │
├──────────────────┬──────────────────┬──────────────────────────────────┤
│ Parameter        │ Received Value   │ Defensive Behavior               │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ Depth (`d`)      │ `d=3` or `d=99`  │ Clamped to maximum `d=2`         │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ Depth (`d`)      │ `d=-1` or `d=abc`│ Reset to default `d=1` (omitted) │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ Offset (`offset`)│ `offset=-10`     │ Clamped to `offset=0` (omitted)  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ Limit (`limit`)  │ `limit=500`      │ Clamped to maximum `limit=100`   │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ Unknown Params   │ `foo=bar`        │ Pruned during canonicalization   │
└──────────────────┴──────────────────┴──────────────────────────────────┘
```

---

## 9. Deep-Linking & State Reproduction Contracts

The routing architecture defines the following deep-linking contracts:

1. **State Reproducibility Guarantee**: All **URL-representable exploration state** (entity identity, lens perspective, focus depth, active filters, search query, and collection offset) must be reproducible from the canonical URL.
2. **Transient UI State Exclusion**: Ephemeral interface states (e.g., hover states, tooltips, animation frame progress, drag positions, and micro-scroll pixel offsets) are not URL-representable and are not required to survive browser refresh or link sharing.
3. **Contextual Panels**: Secondary contextual surfaces (e.g., the Evidence Drawer) survive refresh and link sharing **only** when their state is explicitly designated as URL-representable (e.g., via the canonical `claim` query parameter).
4. **Browser History Navigation**: Browser `Back` and `Forward` buttons execute state restoration across URL-representable exploration coordinates without losing filter contexts.

---

## 10. Navigation Semantics & Lens Transitions

When navigating across the application:

```
┌────────────────────────────────────────────────────────────────────────┐
│                      NAVIGATION TRANSITION MODEL                       │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Navigation Type  │ State Transition Behavior & Rules                   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Entity-to-     │ URL path updates to new slug; viewport resets to    │
│ Entity Jump**    │ top; query filters reset to defaults.               │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Lens           │ Path switches to target lens; **active entity slug  │
│ Transition**     │ is preserved** (e.g., `/characters/karna` $\rightarrow$  │
│                  │ `/graph/karna`); lens-specific query params reset.  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Filter / Depth │ Updates query string via `history.pushState`;       │
│ Mutation**       │ triggers localized canvas update; zero shell reload.│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Contextual     │ Attaches `?claim=id` to URL; opens Evidence Drawer  │
│ Panel Toggle**   │ (Block F4/F15) without dismounting primary canvas.  │
└──────────────────┴─────────────────────────────────────────────────────┘
```

### 10.1 Avoiding State Leakage
- Lens-specific parameters (e.g., timeline `parva` or map `region`) are automatically pruned when transitioning to an unrelated lens (e.g., `/graph`).

---

## 11. Route-Level Data & Loading Boundaries

Routing coordinates coordinate with the Block F6 data layer across clear architectural boundaries:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   ROUTING VS. DATA-FETCHING BOUNDARY                   │
├───────────────────────────────────┬────────────────────────────────────┤
│ ROUTING RESPONSIBILITY (Block F7) │ DATA FETCHING RESPONSIBILITY (F6)  │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Parse path and query parameters │ - Receive normalized query keys    │
│ - Validate and clamp coordinates  │ - Check client query cache         │
│ - Update URL State (Block F5)     │ - Dispatch background HTTP request │
│ - Select active lens component    │ - Manage conditional ETags / 304   │
│ - Manage scroll & focus placement │ - Normalize RFC 7807 problem errors│
└───────────────────────────────────┴────────────────────────────────────┘
```

### 11.1 Route-Level Status Mapping
- **Loading State**: When a route transition initiates, the shell displays skeleton placeholders while Block F6 loads cached or remote payloads.
- **Not Found (`404`)**: If an entity slug or invalid path index (e.g., `/wars/kurukshetra/day/25`) fails resolution, the router renders a dedicated "Resource Not Found" view without dismantling shell landmarks.

---

## 12. Accessibility & Browser Focus Management

To comply with Block F16 accessibility standards:
1. **Focus Reset on Navigation**: Upon route navigation, focus is programmatically shifted to the primary content container (`<main id="main-content">`) or the top-level route `<h1>` heading.
2. **Page Title Synchronization**: Every route update synchronizes `document.title` to a semantic format: `"{Entity / Lens Name} — Mahābhārata Explorer"`.
3. **Screen Reader Announcement**: A visually hidden `aria-live="polite"` route announcer alerts screen readers of navigation changes.

---

## 13. Responsive & Viewport Compatibility

In accordance with Block F4 ([03-responsive-layout-and-application-shell-architecture.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/03-responsive-layout-and-application-shell-architecture.md)):
1. **Viewport Independence**: URLs never encode device classes (`compact`, `medium`, `expanded`, `wide`) or mobile drawer toggle states.
2. **Universal Shareability**: A URL generated on a smartphone renders seamlessly on a desktop monitor and vice versa, adapting solely through responsive CSS layout rules.

---

## 14. Security & Trust Boundary

1. **Parameter-Specific Validation**: All URL inputs are validated according to declared parameter types:
   - **Canonical Slugs**: Validated against the project slug grammar (lowercase alphanumeric kebab-case: `^[a-z0-9-]+$`).
   - **Numeric Modifiers**: Validated against integer range constraints ($D \in \{1, 2\}$, offsets $\ge 0$, limits $1\text{--}100$).
   - **Free-Text Search Query (`q`)**: Validated via standard URL decoding; supports full Unicode including IAST transliteration diacritics and Devanāgarī scripts, subject to bounded length limits ($q \le 200\text{ characters}$) and input sanitization.
2. **Zero Route Authorization Secrets**: Because the public explorer is anonymous and read-only (Block F6 §17), routing enforces zero client-side authentication guards or private token checks.
3. **No Dynamic Code Evaluation**: Route parameters are never evaluated using `eval()`, dynamic script tags, or unescaped HTML injections.

---

## 15. Upstream Dependency & Downstream Ownership Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                     DOWNSTREAM OWNERSHIP MATRIX                        │
├────────────────────────────────────────┬───────────────────────────────┤
│ Architectural Domain                   │ Authoritative Target Block    │
├────────────────────────────────────────┼───────────────────────────────┤
│ Client State Ownership & Store Model   │ **Block F5**                  │
│ API Client, Network & Caching Layer    │ **Block F6**                  │
│ Route Grammar, URL Schema & Navigation │ **Block F7 (This Document)**  │
│ Global Search Modal & Command UX       │ **Block F8**                  │
│ Character Profile Layouts & Tabs       │ **Block F9**                  │
│ Knowledge Graph Visualization Engine   │ **Block F10**                 │
│ Family Lineage Tree DAG Layout         │ **Block F11**                 │
│ Timeline Scrubber & Chronology Engine  │ **Block F12**                 │
│ Geographic Map Visualization Engine    │ **Block F13**                 │
│ Tactical Battle Formations (Vyuhas)    │ **Block F14**                 │
│ Evidence Drawer Citation & Text UX     │ **Block F15**                 │
│ Detailed Accessibility Implementation  │ **Block F16**                 │
│ Performance Budgets & Bundle Audits    │ **Block F17**                 │
│ Router Package Selection & Code Setup  │ **Stage 4 (Implementation)**  │
└────────────────────────────────────────┴───────────────────────────────┘
```

---

## 16. Architectural Decision Record (ADR)

| Decision ID | Architectural Decision | Chosen Approach | Alternatives Evaluated | Rationale & Trade-offs | Owner | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **ADR-FE-37** | Routing Model | **Standard HTML5 History API** | Hash routing (`/#/`) | Provides clean, bookmarkable, shareable Web URLs compatible with SPA architecture, avoiding hash fragments. | F7 | **DECIDED** |
| **ADR-FE-38** | State Attribution | **Path for Identity, Query for Modifiers**| Everything in path, everything in query | Clear separation between canonical entity identity and transient filters. | F7 | **DECIDED** |
| **ADR-FE-39** | Canonicalization | **Deterministic Alphabetical Serialization**| Arbitrary parameter ordering | Prevents duplicate cache entries and ensures consistent shareable links.| F7 | **DECIDED** |
| **ADR-FE-40** | Focus Depth Boundary | **Defensive Clamping ($D \le 2$)** | Unbounded depth query | Enforces backend B3/F5 invariant; rejects $D > 2$ at the routing boundary.| F7 | **DECIDED** |
| **ADR-FE-41** | Pagination Routing | **Offset Query Parameters** | Cursor token in URL | Consumes standard B5 offset pagination (`offset`, `limit`) without cursors.| F7 | **DECIDED** |
| **ADR-FE-42** | Cross-Lens Continuity | **Entity Slug Preservation** | Reset to root on switch | Allows seamless perspective shifts (e.g., Profile $\rightarrow$ Graph $\rightarrow$ Timeline). | F7 | **DECIDED** |

---

## 17. Requirement Traceability Matrix

| Requirement / Principle | Source Document | Implementing Block F7 Section | Coverage Status |
| :--- | :--- | :--- | :--- |
| **Stateless Deep-Linking** | F1 §5.8, B5 §5 | §2 (Principles), §9 (Deep-Linking Contracts) | **SATISFIED** |
| **URL State Authority** | F5 §4, §7 | §2 (Principles), §5 (Path vs Query Matrix) | **SATISFIED** |
| **Focus Depth ($D \le 2$)** | B3 §4, REQ-GRP-01| §6.2 (Graph Lens), §8 (Defensive Handling)     | **SATISFIED** |
| **Public Offset Pagination** | B5 §7.1, F6 §11 | §5 (Query Attribution), §6.8 (Search), §16 (ADR-FE-41)| **SATISFIED** |
| **Responsive Shell Decoupling**| F4 §2, §9 | §3 (Shell Integration), §13 (Viewport Independence)| **SATISFIED** |
| **Zero Data Fabrication** | Rule 03, F1 §5.4 | §8 (Sanitization), §11.1 (Not Found 404 Mapping)| **SATISFIED** |
| **Accessibility Focus Standard**| F1 §5.10, F16 | §12 (Accessibility & Focus Management) | **SATISFIED** |

---

## 18. F7 Exit Criteria Checklist

- [x] Canonical URL grammar and route taxonomy are formally defined across all exploration lenses.
- [x] Clear architectural separation between path identity and query exploration modifiers is established.
- [x] URL state contract preserves Block F5's rule that URL state is authoritative for exploration coordinates.
- [x] Transient UI state and responsive device/layout state are strictly barred from URLs.
- [x] Deep-linking reproducibility is guaranteed for all URL-representable state without requiring transient UI survival.
- [x] Cross-lens navigation semantics and entity slug preservation rules are defined.
- [x] Focus graph routing strictly enforces and clamps depth to $D \le 2$.
- [x] Public collection routes strictly consume offset pagination (`offset`, `limit`) without cursor pagination.
- [x] URL parameter validation is parameter-specific; unknown query parameters are pruned during canonicalization.
- [x] Downstream ownership matrix across F8–F17 and Stage 4 is explicitly catalogued.
- [x] Zero application source code, router package installations, or implementation files were introduced.
- [x] Zero Stage 1 backend documents or Blocks F1–F6 documents were modified.
