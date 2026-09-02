# State Management & Client-Side Data Architecture (Block F5)

## 1. Architectural Context

This document establishes the **State Management & Client-Side Data Architecture** for the **Mahābhārata Explorer** frontend. It defines the client-side state taxonomy, ownership boundaries, data lifecycle, derived state transformations, focus and depth synchronization ($D \le 2$), concurrency defense, and state persistence rules for Stage 2.

```
┌────────────────────────────────────────────────────────────────────────┐
│                   STAGE 2 ARCHITECTURAL PLACEMENT                      │
├───────────────────────────────────┬────────────────────────────────────┤
│ UPSTREAM CONTRACTS & CONSTRAINTS  │ DOWNSTREAM FRONTEND SPECIFICATIONS │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Stage 1 Backend Architecture    │ - F6: API Client & Data Fetching   │
│   (B2 Data, B3 Graph, B5 API,     │ - F7: Routing & Navigation UX      │
│    B9 Performance & Caching)      │ - F8: Global Search UX             │
│ - Block F1: Frontend Constitution │ - F9: Character & Entity Views     │
│   (5 State Categories, Boundaries)│ - F10–F14: Visualizations          │
│ - Block F2: Tech Stack (React SPA)│ - F15: Evidence & Provenance UX    │
│ - Block F3: Tripartite States     │ - F16: Accessibility & IAST        │
│ - Block F4: Shell & Context Panels│ - F17: Testing & Budgets           │
└───────────────────────────────────┴────────────────────────────────────┘
```

### 1.1 Pure Architectural Specification
Block F5 is strictly an architectural specification. It establishes data flow rules, ownership boundaries, and state evaluation. It does not write React store code, implement hooks, install state management packages, or configure browser storage.

---

## 2. State Architecture Constitution

The client-side state architecture is governed by 10 foundational principles:

```
┌────────────────────────────────────────────────────────────────────────┐
│                    STATE ARCHITECTURE PRINCIPLES                       │
├────────────────────────────────────────────────────────────────────────┤
│ 1. SINGLE SOURCE OF TRUTH: Every independently maintained state value  │
│    has one authoritative owner. Derived values may be computed from    │
│    multiple authoritative sources and must not become competing sources│
│    of truth.                                                           │
│ 2. ZERO UNNECESSARY DUPLICATION: Server data is never cloned into      │
│    parallel mutable client stores; derived views are computed on-demand│
│ 3. DERIVED OVER STORED: Filtered lists, view models, and subgraph      │
│    projections are derived via pure selectors rather than stored state.│
│ 4. STATELESS DEEP-LINKING: Any exploration state necessary to recreate │
│    a view (entity, lens, focus, depth, filters) belongs to URL state.  │
│ 5. EPHEMERAL LOCALITY: Transient UI interactions (hover, active focus, │
│    temporary inputs) remain strictly local to the rendering component. │
│ 6. ASYNCHRONOUS SERVER BOUNDARY: Server state is treated as an external│
│    read-only snapshot, never as locally mutable data.                  │
│ 7. STRICT CONCURRENT ISOLATION: High-frequency interaction state (zoom,│
│    scrubbing) must not cause re-rendering of parent shell layouts.     │
│ 8. FOCUS DEPTH INVARIANT: Graph and exploration focus state strictly   │
│    respects the backend architectural constraint D ≤ 2.                │
│ 9. LIFECYCLE SEPARATION: Application data lifecycle states are strictly│
│    separated from historical epistemic truth states (F3 §12).          │
│ 10. NO CLIENT-SIDE DATABASE OVERKILL: Relational integrity belongs to  │
│     the backend; the frontend consumes typed domain DTOs.              │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Five Canonical State Categories

In accordance with [00-frontend-architecture-context.md §8](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/05-frontend/00-frontend-architecture-context.md#L145), all client data is classified into five canonical categories:

```
┌────────────────────────────────────────────────────────────────────────┐
│                      FIVE CANONICAL STATE CATEGORIES                   │
├──────────────────┬─────────────────────────────────────────────────────┤
│ State Category   │ Scope, Characteristics & Behavioral Contract        │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **1. Server      │ Backend-originated domain and API representations   │
│ State**          │ (Entities, Relationships, Subgraphs, Citations).   │
│                  │ Read-only client snapshots; managed by API client   │
│                  │ and data-fetching layer (Block F6).                 │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **2. URL         │ Navigational and exploration state serializable to  │
│ State**          │ the URL (Active lens, entity identifier, focus node,│
│                  │ depth D=1/D=2, active filters). Managed by F7.      │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **3. Lens        │ Exploration state meaningful to a specific lens     │
│ State**          │ viewport (Timeline zoom scale, Map center/zoom,     │
│                  │ Graph node selection, War day stepper).             │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **4. UI          │ Transient interface interaction state (Drawer open/ │
│ State**          │ closed, modal visibility, hovercards, tooltips,     │
│                  │ local text input values). Scoped to component tree. │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **5. Client      │ User preference settings persisting across sessions │
│ Preferences**    │ (Light/Dark theme, IAST/Devanāgarī display script,  │
│                  │ UI density, motion reduction override).             │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 4. State Ownership Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                        STATE OWNERSHIP MATRIX                          │
├──────────────┬──────────────┬──────────────┬─────────────┬─────────────┤
│ Category     │ Authoritative│ Lifecycle    │ Persistence │ URL Sync    │
│              │ Owner        │ Scope        │ Strategy    │ Required?   │
├──────────────┼──────────────┼──────────────┼─────────────┼─────────────┤
│ **Server**   │ Backend API  │ Query/Cache  │ Server/Query│ No (Fetched │
│              │ (via F6)     │ Lifecycle    │ Lifecycle   │ via URL key)│
├──────────────┼──────────────┼──────────────┼─────────────┼─────────────┤
│ **URL**      │ Browser URL  │ Navigation   │ Navigation/ │ **Authoritative│
│              │ (via F7)     │ History      │ URL History │ for URL state│
├──────────────┼──────────────┼──────────────┼─────────────┼─────────────┤
│ **Lens**     │ Active Lens  │ Lens Session │ Session-    │ Context-    │
│              │ (F9–F14)     │ / Transition │ scoped      │ dependent   │
├──────────────┼──────────────┼──────────────┼─────────────┼─────────────┤
│ **UI**       │ Component /  │ Element      │ Ephemeral   │ **No**      │
│              │ Context (F4) │ Mount/Unmount│ (In-memory) │ (Prohibited)│
├──────────────┼──────────────┼──────────────┼─────────────┼─────────────┤
│ **Pref.**    │ User Client  │ Long-term    │ Reload-     │ **No**      │
│              │ Store        │ Client       │ Persistent  │ (Persistent)│
└──────────────┴──────────────┴──────────────┴─────────────┴─────────────┘
```

---

## 5. Source of Truth & Duplication Boundaries

To prevent state desynchronization across exploration lenses:

```
┌────────────────────────────────────────────────────────────────────────┐
│                     SOURCE OF TRUTH ARCHITECTURE                       │
├────────────────────────────────────────────────────────────────────────┤
│ 1. CANONICAL SERVER DATA: API DTOs cached by query layer (F6).         │
│    - Read-only; never modified directly by UI components.              │
├────────────────────────────────────────────────────────────────────────┤
│ 2. CANONICAL EXPLORATION PARAMETERS: The URL (F7).                     │
│    - Represents active entity and exploration depth using the URL       │
│      representation defined by Block F7.                               │
├────────────────────────────────────────────────────────────────────────┤
│ 3. DERIVED VIEW MODELS: On-demand pure selectors.                     │
│    - Combines Canonical Server Data + URL/Lens Parameters.             │
│    - Computes filtered subgraphs, localized badges, and lists.         │
└────────────────────────────────────────────────────────────────────────┘
```

### 5.1 Duplication Rules
1. **Entity Duplication Prohibited**: Full entity objects are never duplicated into separate "favorites" or "recent" client stores; stores reference entities strictly by **canonical slug** (e.g., `slug: "arjuna"`).
2. **No Client-Side Relational Database**: The frontend does not implement an in-memory SQL/relational engine. Graph traversals beyond the fetched subgraph are delegated to the backend REST API ([03-graph-architecture.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/03-graph-architecture.md)).

---

## 6. Server State Boundary & Data Fetching Hand-off

1. **Decoupled API Representations**: The frontend consumes typed API domain DTOs ([05-api-architecture.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/05-api-architecture.md)). It does not import PostgreSQL database schemas or assume direct database access.
2. **Immutability of Server Data**: Server responses are read-only client snapshots. Client operations (filtering, sorting, searching) operate on derived projections without mutating cached API objects.
3. **Data Fetching Hand-Off**: Concrete network mechanics, HTTP cache headers, `ETag` conditional revalidation (`If-None-Match`), and freshness/revalidation lifecycles are formally specified in Block **F6 (API Client, Data Fetching & Caching Architecture)**.

---

## 7. URL State Boundary & Deep-Linking Rules

URL state represents the reproducible exploration coordinates of the application:

```
┌────────────────────────────────────────────────────────────────────────┐
│                       URL STATE CRITERIA MATRIX                        │
├───────────────────────────────────┬────────────────────────────────────┤
│ BELONGS IN URL STATE              │ PROHIBITED FROM URL STATE          │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Active exploration lens         │ - Transient mouse hover coordinates│
│ - Canonical entity identifier     │ - Tooltip open/closed state        │
│ - Graph focus node & depth ($D$)  │ - Temporary input text typing      │
│ - Active timeline period / Parva  │ - Ongoing animation progress frames│
│ - Active war day index (1–18)     │ - Micro-scroll pixel positions     │
│ - Primary lens search & filters   │ - Ephemeral drawer drag positions  │
│ - Evidence drawer open reference  │ - Local UI button focus rings      │
└───────────────────────────────────┴────────────────────────────────────┘
```
*(Note: Final route schema, path parameters, and query serialization syntax are formally specified in Block **F7**).*

---

## 8. Lens State Architecture

Lens state manages viewport parameters specific to individual exploration lenses:

```
┌────────────────────────────────────────────────────────────────────────┐
│              ILLUSTRATIVE LENS STATE CATEGORIES (UX IN F9–F14)         │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Lens Domain      │ Illustrative Lens State Scope                       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Knowledge      │ Focused node ID, expanded cluster state, graph      │
│ Graph (F10)**    │ layout lock mode, visual physics stabilization.     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Family Tree    │ Root ancestral node, branch collapse/expand state,  │
│ Lineage (F11)**  │ generation depth filter.                            │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Timeline       │ Active Parva range, chronological zoom level,       │
│ Scrubber (F12)** │ selected event milestone ID.                        │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Geographic     │ Bounding box coordinates, active historical layer,  │
│ Map (F13)**      │ regional location cluster selection.                │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **War Explorer   │ Active battle day (1–18), selected duel/event ID,   │
│ & Vyuhas (F14)** │ active formation tactical phase.                    │
└──────────────────┴─────────────────────────────────────────────────────┘
```
*(Note: The above entries are illustrative categories only and do not define the authoritative internal state models of Blocks F9–F14. Those blocks retain authority over detailed lens-specific state and visualization mechanics).*

### 8.1 Lens State Synchronization Rules
1. **Isolated by Default**: Lens-specific visual settings (e.g., map zoom level, graph camera angle) do not leak into other lenses.
2. **Canonical Entity Continuity**: When switching lenses (e.g., from Character Profile to Timeline), the **active entity identifier** is preserved across the transition.

---

## 9. UI State Architecture & Contextual Panels

UI state controls transient visual surfaces in alignment with Block F4's application shell:

1. **Contextual Drawer / Panel State**: Tracks whether secondary surfaces (Evidence Drawer, Node Inspector) are open or closed, and which entity/claim they reference.
2. **Overlay & Modal State**: Tracks global search palette visibility and confirmation dialogs.
3. **Form & Filter Input State**: Buffers local search text before submission or debounced query dispatch.

---

## 10. Client Preferences Architecture

User preferences persist across browser sessions but do not alter exploration coordinates:

```
┌────────────────────────────────────────────────────────────────────────┐
│                      CLIENT PREFERENCE DOMAINS                         │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Preference       │ Architectural Role & Scope                          │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Theme**        │ Light Mode $\leftrightarrow$ Dark Mode toggle       │
│                  │ (transforms semantic CSS tokens, F3 §19).           │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Script / IAST**│ Primary transliteration script: IAST diacritics     │
│                  │ $\leftrightarrow$ Devanāgarī (details in F16).      │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **UI Density**   │ Standard $\leftrightarrow$ Compact density mode.    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Motion**       │ System default $\leftrightarrow$ Reduced motion     │
│                  │ override (`prefers-reduced-motion`, F3 §16).        │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 11. Derived State & Pure Selectors

To avoid state desynchronization and memory bloat, derived data is calculated on-demand:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        DERIVED STATE PIPELINE                          │
├────────────────────────────────────────────────────────────────────────┤
│ [Cached Server Data (F6)] + [Active URL / Lens State (F7)]             │
│                                │                                       │
│                                ▼ (Pure Selector Function)              │
│               [Computed View Model / Filtered Subgraph]                │
│                                │                                       │
│                                ▼ (Render Output)                       │
│             [React UI Component / Visualization Canvas]                │
└────────────────────────────────────────────────────────────────────────┘
```

### 11.1 Memoization Principles
- Selectors perform structural memoization: if underlying server DTOs and filter criteria have not changed, cached view models are returned, bypassing expensive graph recalculations.

---

## 12. Entity Identity & Graph Focus Synchronization ($D \le 2$)

In accordance with Stage 1 Graph Architecture ([03-graph-architecture.md §4](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/03-graph-architecture.md#L120)):

```
┌────────────────────────────────────────────────────────────────────────┐
│                    EXPLORATION FOCUS STATE MODEL                       │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Focus Parameter  │ Architectural Constraint & Behavior                 │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `focus_entity`   │ Canonical slug of the active node                   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `depth` ($D$)    │ Strictly constrained to **$D=1$** or **$D=2$**.     │
│                  │ Enforces backend invariant $D \le 2$ (REQ-GRP-01).  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `active_filters` │ Relationship categories (kinship, alliance, etc.)   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `fallback_mode`  │ Backend Focus fallback handling ($D_2 \rightarrow   │
│                  │ D_1 \rightarrow 504$) reflected transparently in UI.│
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 13. State Lifecycle & Navigation Transitions

```
┌────────────────────────────────────────────────────────────────────────┐
│                        STATE LIFECYCLE FLOW                            │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Lifecycle Event  │ State System Action                                 │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Initial Load / │ URL parameters parsed $\rightarrow$ Server data     │
│ Deep Entry**     │ queried via F6 $\rightarrow$ Shell mounts active lens│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Lens Switch**  │ URL path updates $\rightarrow$ Active entity ID is  │
│                  │ carried over; lens-specific canvas state resets.    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Entity Jump**  │ URL updates to new slug $\rightarrow$ Focus state   │
│                  │ updates $\rightarrow$ Server query executes for DTO.│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Back / Forward │ URL state changes $\rightarrow$ UI synchronizes     │
│ (Browser)**      │ instantly; cached server DTOs render immediately.   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Error Event**  │ RFC 7807 problem details stored in component error  │
│                  │ state $\rightarrow$ Shell error boundary activates. │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 14. State Consistency & Concurrency Invariant

To guarantee data integrity across rapid exploration interactions:
1. **Concurrency Invariant**: **Obsolete asynchronous results must not overwrite state associated with a newer exploration context.**
2. **Freshness & Revalidation**: When revalidating cached server data in the background, UI state displays the cached snapshot while an unobtrusive background indicator signals refreshing.
3. **Implementation Hand-Off**: Concrete request cancellation mechanics, sequence tags, and network cache coordination are implementation mechanisms owned by Block **F6** and Stage 4.

---

## 15. Data Lifecycle States vs. Epistemic Truth States

A strict conceptual boundary is maintained between application network lifecycle states and historical epistemic truth states:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   STATE CATEGORY DISAMBIGUATION                        │
├───────────────────────────────────┬────────────────────────────────────┤
│ DATA LIFECYCLE STATES (System)    │ EPISTEMIC TRUTH STATES (Domain B2) │
├───────────────────────────────────┼────────────────────────────────────┤
│ - `idle`: Initial un-fetched state│ - `known`: Core verified fact      │
│ - `loading`: Initial network fetch│ - `unknown`: Affirmatively absent  │
│ - `refreshing`: Background reval  │ - `not_researched`: Un-curated     │
│ - `success`: Valid payload cached │ - `not_applicable`: Inapplicable   │
│ - `empty`: 0 search/filter matches│ - `conflicting`: Disputed claims   │
│ - `error`: RFC 7807 HTTP failure  │ - `approximate`: Estimated date    │
└───────────────────────────────────┴────────────────────────────────────┘
```
*Rule*: An `unknown` parentage in the epic is a **valid domain value** (`epistemic = 'unknown'`) delivered inside a **successful HTTP response** (`lifecycle = 'success'`), never an application error.

---

## 16. State Granularity & Rendering Isolation

To prevent rendering bottlenecks on dense visualization canvases:
1. **Isolated Subscriptions**: High-frequency interactive states (e.g., timeline scrubbing position, canvas cursor coordinates) are isolated in local component state or localized hooks; they do not trigger re-renders of the application shell header or navigation rail.
2. **Workspace Render Boundary**: Shell layout frames (F4) remain structurally decoupled from internal lens re-renders.

---

## 17. State Management Technology Evaluation

To implement the client-side state architecture, prominent state management paradigms were evaluated:

```
┌────────────────────────────────────────────────────────────────────────┐
│                    STATE TECHNOLOGY EVALUATION                         │
├───────────────────┬──────────────────┬───────────────┬─────────────────┤
│ Evaluation Factor │ Modular Store    │ React Context │ Centralized Redux│
│                   │ (e.g., Zustand)  │ + useReducer  │ (RTK / Monolith)│
├───────────────────┼──────────────────┼───────────────┼─────────────────┤
│ Performance &     │ **Strong**       │ Potential     │ Good            │
│ Granular Selectors│ Fine-grained component subscriptions | Context re-renders all consumers | Selectors supported |
├───────────────────┼──────────────────┼───────────────┼─────────────────┤
│ TypeScript Depth  │ **Good fit**     │ Good fit      │ Complex         │
│ & Inference       │ Clean type inference with minimal boilerplate | Native TSX | Heavy action typing |
├───────────────────┼──────────────────┼───────────────┼─────────────────┤
│ Bundle Footprint  │ **Lower          │ Lower         │ Higher          │
│                   │ complexity**     │ complexity    │ complexity      │
├───────────────────┼──────────────────┼───────────────┼─────────────────┤
│ Antigravity AI    │ **Good fit**     │ Moderate      │ Moderate        │
│ Inspectability    │ Explicit modular stores, clear hooks | Nested context tree | Action/reducer boilerplate |
└───────────────────┴──────────────────┴───────────────┴─────────────────┘
```

### 17.1 Recommended State Architecture Approach
- **Recommended Architectural Approach**: **Lightweight Modular Client-State Architecture with Fine-Grained Subscriptions**:
  1. **Server State**: Managed by dedicated query cache (Block F6).
  2. **URL State**: Managed by Router & URL Search Params (Block F7).
  3. **Global Client State & Preferences**: Lightweight, modular store with fine-grained selectors.
  4. **Transient UI State**: React standard component state (`useState`, `useRef`).
- *Status*: Concrete library selection (e.g., Zustand, Jotai) and package installation are non-binding illustrative examples deferred to Stage 4 implementation.

---

## 18. Upstream Dependency & Downstream Ownership Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                     DOWNSTREAM OWNERSHIP MATRIX                        │
├────────────────────────────────────────┬───────────────────────────────┤
│ Architectural Domain                   │ Authoritative Target Block    │
├────────────────────────────────────────┼───────────────────────────────┤
│ API Client, Data Fetching & Caching    │ **Block F6**                  │
│ Route Grammar, URL Schema & Router     │ **Block F7**                  │
│ Global Search Modal & Autocomplete UX  │ **Block F8**                  │
│ Character Profile Layouts & Tabs       │ **Block F9**                  │
│ Knowledge Graph Canvas Engine & Physics│ **Block F10**                 │
│ Family Lineage Tree DAG Layout         │ **Block F11**                 │
│ Timeline Scrubber & Chronology Engine  │ **Block F12**                 │
│ Geographic Map Visualization Engine    │ **Block F13**                 │
│ Tactical Battle Formations (Vyuhas)    │ **Block F14**                 │
│ Evidence Drawer Citation & Text UX     │ **Block F15**                 │
│ Accessibility Architecture & IAST/Dev  │ **Block F16**                 │
│ State-Related Performance Budgets      │ **Block F17**                 │
│ Concrete Store Setup & Package Install │ **Stage 4 (Implementation)**  │
└────────────────────────────────────────┴───────────────────────────────┘
```

---

## 19. Architectural Decision Record (ADR)

| Decision ID | Architectural Decision | Chosen Approach | Alternatives Evaluated | Rationale & Trade-offs | Owner | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **ADR-FE-25** | State Taxonomy | **5 Canonical F1 Categories**| Flat monolithic store, 2-tier local/global | Explicitly distinguishes server, URL, lens, UI, and preference lifecycles. | F5 | **DECIDED** |
| **ADR-FE-26** | Server Data Model | **Read-Only Client Snapshots**| Client-side relational database | Prevents state duplication; lets backend remain authoritative for graph data. | F5 | **DECIDED** |
| **ADR-FE-27** | Exploration Navigation | **URL as Authoritative Source for URL-Representable Exploration State**| Redux router, session storage | Guarantees deep-linkability and browser back/forward fidelity. | F5 | **DECIDED** |
| **ADR-FE-28** | Focus Depth Boundary | **Strict $D \le 2$ Invariant** | Unbounded client graph expansion | Aligns with backend B3 constraint; prevents browser memory exhaustion. | F5 | **DECIDED** |
| **ADR-FE-29** | Derived Data Flow | **Pure Selectors & Memoization**| Redundant synchronized stores | Eliminates state desynchronization and minimizes re-render overhead. | F5 | **DECIDED** |
| **ADR-FE-30** | State Management Model | **Lightweight Modular Client State**| Heavy monolithic Redux, raw Context | Low complexity, fine-grained selectors, clear Antigravity code structure. | F5 | **DECIDED** |

---

## 20. Requirement Traceability Matrix

| Requirement / Principle | Source Document | Implementing Block F5 Section | Coverage Status |
| :--- | :--- | :--- | :--- |
| **Knowledge Explorer First** | F1 §5.1, PRD §2 | §2 (Principles), §5 (Source of Truth) | **SATISFIED** |
| **One Unified Graph, Many Lenses**| F1 §5.2, B3 §1 | §8 (Lens State), §12 (Focus Synchronization) | **SATISFIED** |
| **Evidence-Aware UI** | F1 §5.3, B4 §1 | §3 (Server State), §7 (Citation URL State) | **SATISFIED** |
| **Zero Data Fabrication** | F1 §5.4, Rule 03 | §15 (Data Lifecycle vs Epistemic States) | **SATISFIED** |
| **Stateless Deep Linking** | F1 §5.8, B5 §5 | §4 (Ownership Matrix), §7 (URL Criteria) | **SATISFIED** |
| **Graph Focus Depth ($D \le 2$)** | B3 §4, REQ-GRP-01| §12 (Focus State Model), §19 (ADR-FE-28) | **SATISFIED** |
| **Responsive Shell Alignment**| F4 §2, §9 | §9 (UI State), §16 (Rendering Isolation) | **SATISFIED** |
| **Performance-Aware State** | F2 §6, B9 §2 | §11 (Derived Pipeline), §16 (Granularity) | **SATISFIED** |

---

## 21. F5 Exit Criteria Checklist

- [x] State architecture constitution and 10 core principles are formally defined.
- [x] Five canonical state categories (Server, URL, Lens, UI, Client Preferences) are strictly maintained from F1.
- [x] State ownership matrix is specified across lifecycle, persistence, and URL synchronization.
- [x] Source of truth rules prohibit client database duplication.
- [x] Server state boundaries decouple frontend from direct database access.
- [x] URL state boundaries define criteria for URL-worthy state versus prohibited ephemeral data without concrete route syntax.
- [x] Lens state boundaries ensure isolation while preserving active entity focus continuity (with illustrative categorization).
- [x] UI and Client Preference state architectures are established.
- [x] Derived state pipeline and memoization principles are formalized.
- [x] Entity references use stable slugs; graph focus depth strictly enforces $D \le 2$.
- [x] Data lifecycle states (loading/success/error) are explicitly separated from epistemic truth states (known/unknown).
- [x] State granularity rules prevent high-frequency interactions from re-rendering shell layouts.
- [x] State management technology is evaluated with an architectural recommendation.
- [x] Downstream ownership matrix across F6–F17 and Stage 4 is explicitly catalogued.
- [x] Zero application source code, stores, hooks, or package installations were introduced.
- [x] Zero Stage 1 backend documents, Block F1, Block F2, Block F3, or Block F4 documents were modified.
