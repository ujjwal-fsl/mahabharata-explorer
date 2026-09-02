# Frontend Architecture Context & Foundation (Block F1)

## 1. Purpose

This document establishes the foundational **Frontend Architecture Context & Constitution** for the **Mahābhārata Explorer** application. It establishes the architectural mission, core principles, responsibility boundaries, state models, exploration lenses, responsive and visualization strategies, accessibility standards, backend contract dependencies, requirement taxonomy, and the 17-block roadmap (F1 through F17) governing all frontend architectural specifications.

Stage 2 defines the frontend architecture only. In accordance with the fixed project roadmap:
- **Stage 0 (Antigravity Setup)**: Completed.
- **Stage 1 (Backend Architecture B1–B13)**: Completed, audited, and committed (`08e1057`).
- **Stage 2 (Frontend Architecture F1–F17)**: Active stage (Architecture only; no code implementation).
- **Stage 3 (Integration Architecture)**: Subsequent stage.
- **Stage 4 (Implementation)**: Subsequent stage.
- **Stage 5 (Validation)**: Subsequent stage.

---

## 2. Stage 2 Scope & Critical Boundaries

Stage 2 is strictly an **architectural specification stage**. It does not write application code, create components, initialize frameworks, install NPM packages, or implement styling.

```
┌────────────────────────────────────────────────────────────────────────┐
│                      STAGE 2 ARCHITECTURAL BOUNDARY                    │
├───────────────────────────────────┬────────────────────────────────────┤
│ IN SCOPE FOR STAGE 2 (F1–F17)     │ STRICTLY OUT OF SCOPE (DEFERRED)   │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Architectural Principles        │ - Application Code (React/Vue/etc.)│
│ - Component & Layout Hierarchy    │ - Component Implementation / JSX   │
│ - Design System & Visual Grammar  │ - CSS / Styling Code               │
│ - Multi-Lens Exploration UX       │ - Package Installation (`npm i`)   │
│ - State & URL Routing Contracts   │ - API Client / Fetch Code          │
│ - Graph & Map Viz Architecture    │ - Frontend Test Runner Execution   │
│ - Accessibility & Performance     │ - Production Build Configurations  │
└───────────────────────────────────┴────────────────────────────────────┘
```

---

## 3. Relationship to Stage 1 Backend Architecture

The frontend architecture is directly grounded in and constrained by the authoritative Stage 1 backend architectural baseline (`docs/04-backend/`):

1. **Authoritative Backend Contract**: The backend API ([05-api-architecture.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/05-api-architecture.md)) defines the exact JSON response envelopes (`{ "data": ... }`, `{ "data": [...], "pagination": ... }`), RFC 7807 problem details, HTTP status codes, and query parameter semantics. The frontend architecture consumes these API contracts without inventing alternative API shapes.
2. **One Unified Knowledge Graph (B2, B3)**: The frontend acts as a multi-lens exploration window over API domain representations derived from the unified 12-entity relational and graph model ([02-data-architecture.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/02-data-architecture.md), [03-knowledge-graph.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/03-knowledge-graph.md)).
3. **Four-Tier Provenance Model (B4)**: The frontend interface explicitly exposes the $\text{Entity/Edge} \rightarrow \text{Claim} \rightarrow \text{Evidence} \rightarrow \text{Source}$ hierarchy without flattening scholarly nuance.
4. **Performance & Payload Limits (B9)**: The frontend architecture is designed to operate within the 8 formal backend latency budgets ($p95 \le 30\text{--}100\text{ ms}$) and uncompressed payload targets ($\le 20\text{--}500\text{ KB}$), honoring the three-tier progressive disclosure model.
5. **Security & Read-Only Model (B7)**: The public frontend is an unauthenticated, 100% anonymous read-only explorer. No user mutation forms or administrative curation interfaces exist in the public client.

---

## 4. Frontend Architectural Mission

The mission of the Mahābhārata Explorer frontend is to provide an **instantaneous, culturally resonant, multi-lens visual exploration experience** that allows curious readers, scholars, and students to navigate the vast interconnected epic—its heroes, genealogies, battles, geographical sites, timelines, and scholarly sources—with total epistemic honesty, visual elegance, and zero data fabrication.

---

## 5. Core Architectural Principles

```
┌────────────────────────────────────────────────────────────────────────┐
│                   FRONTEND CONSTITUTIONAL PRINCIPLES                   │
├────────────────────────────────────────────────────────────────────────┤
│ 1. KNOWLEDGE EXPLORER FIRST: An interactive discovery engine, not a    │
│    static encyclopedia or generic CRUD application.                    │
│ 2. ONE GRAPH, MANY LENSES: Every view (Character, Map, Timeline, War,  │
│    Graph) queries the same unified knowledge graph.                    │
│ 3. EVIDENCE-AWARE UI: Citations, Sanskrit verse locators, and variant  │
│    traditions are first-class visual elements, not hidden footnotes.   │
│ 4. ZERO FABRICATION: Missing data renders honestly as explicit null/   │
│    unknown states. No synthetic AI text or speculative placeholder art.│
│ 5. RESPONSIVE BY DESIGN: First-class parity across Mobile, Tablet,     │
│    Laptop, and Desktop in both Portrait and Landscape orientations.    │
│ 6. THREE-TIER PROGRESSIVE DISCLOSURE: Summary Card (<20KB) → Full     │
│    Profile (<100KB) → Granular Provenance (<150KB) on demand.          │
│ 7. PERFORMANCE-CONSCIOUS RENDERING: Virtualized lists, bounded graph   │
│    traversals (D≤2), and cancellation-aware network fetching.          │
│ 8. STATELESS DEEP LINKING: Meaningful exploration and navigation state │
│    is shareable via canonical URLs; transient UI state remains local.  │
│ 9. ACCESSIBILITY FIRST: Semantic HTML, full keyboard navigation, screen│
│    reader compatibility, high contrast, and non-visual alternatives.  │
│ 10. VISUAL RESTRAINT: Modern first, traditional second. Minimalist     │
│     flat design with subtle Sanātana essence; zero fake parchment.     │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 6. Frontend Responsibility Boundary

To maintain clear separation of concerns, the boundaries of frontend responsibility are strictly defined:

```
┌────────────────────────────────────────────────────────────────────────┐
│                     FRONTEND RESPONSIBILITY MATRIX                     │
├───────────────────────────────────┬────────────────────────────────────┤
│ WHAT BELONGS TO THE FRONTEND      │ WHAT DOES NOT BELONG TO FRONTEND   │
├───────────────────────────────────┼────────────────────────────────────┤
│ - UI Presentation & Component Tree│ - Canonical Truth Determination    │
│ - Interaction & Gestures (Touch)  │ - Source Verification / Curation   │
│ - Client-Side Exploration State   │ - Database Mutations / Ingestion   │
│ - URL Routing & Deep-Link State   │ - Authoritative Search Indexing    │
│ - Canvas/SVG/WebGL Visualization  │ - Backend Statement Timeouts       │
│ - Client Caching & Revalidation   │ - Bypassing Epistemic Statuses     │
│ - Accessibility & ARIA Semantics  │ - Fabrication of Missing Values    │
│ - Responsive Layout Adaptation    │ - Security / Auth Enforcement      │
│ - Loading, Error & Empty Views    │ - Administrative Publishing Tools  │
└───────────────────────────────────┴────────────────────────────────────┘
```

---

## 7. Unified Multi-Lens Exploration Model

The frontend does not build isolated applications for different topics. Instead, it implements a **Lens-Based Exploration Architecture** where the user smoothly transitions between different dimensional perspectives of the exact same underlying entity graph:

```
                               CANONICAL ENTITY
                     (e.g., Arjuna / Kurukshetra War)
                                     │
      ┌──────────────┬───────────────┼───────────────┬──────────────┐
      ▼              ▼               ▼               ▼              ▼
[Character View] [Lineage Tree] [Timeline Event] [Spatial Map] [Focus Graph]
 (Biographical)   (Kinship DAG)   (Chronological)  (Geographic)  (Network D≤2)
      │              │               │               │              │
      └──────────────┴───────────────┼───────────────┴──────────────┘
                                     ▼
                        EVIDENCE & CITATION DRAWER
                       (BORI CE Locators & Verses)
```

---

## 8. Exploration Lenses Taxonomy

The frontend architecture organizes user exploration into 10 primary lenses. 

> [!NOTE]
> The path patterns listed below (e.g., `/characters/:slug`) are **illustrative conceptual examples only**. Final route syntax, route hierarchies, URL parameter contracts, and navigation semantics are formally specified in Block **F7 (Routing & Deep-Link Architecture)**.

1. **Character Explorer (e.g., `/characters/:slug`)**: Biographical summaries, aliases/epithets, role tags, and curated portraits (with semantic monogram fallbacks).
2. **Family Lineage Explorer (e.g., `/characters/:slug/family`)**: Interactive multi-generational parent-child DAGs, sibling groups, and spousal links.
3. **General Relationship Explorer (e.g., `/relationships`)**: Typed non-familial network discovery (alliances, rivalries, mentorships).
4. **Timeline & Chronology Explorer (e.g., `/timeline`, `/events/:slug`)**: Chronological sequence ordering (`sequence_index`), Parva subdivisions (1–18), and milestone Level-of-Detail (LOD) zoom.
5. **Geographic Map Explorer (e.g., `/map`, `/locations/:slug`)**: Interactive viewport map with bounding box queries, mapped pins, and unmapped site indicators.
6. **Dynasties & Factions Explorer (e.g., `/groups/:slug`)**: Dynastic trees (Kuru, Yadava, Panchala) and factional war allegiances.
7. **War Explorer (e.g., `/wars/:slug`, `/wars/:slug/days/:day`)**: Partitioned day-by-day battlefield narratives, commanders, fallen heroes, and tactical occurrences.
8. **Battlefield Vyuhas Explorer (e.g., `/formations/:slug`)**: Tactical formation descriptions, SVG diagrams, event links, and accessibility text.
9. **Global Search & Autocomplete (e.g., `/search`, header overlay)**: Instant dual-tier fuzzy/FTS search, transliteration matching, and type filtering.
10. **Global Focus Graph (e.g., `/graph/focus/:type/:slug`)**: Ego-centric interactive network graphs bounded to depth $D=1$ ($\le 50$ nodes) and $D=2$ ($\le 100$ nodes).

---

## 9. Foundational State Model

The frontend state architecture separates data into distinct, predictable lifecycle tiers without prematurely selecting a specific state library:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        FRONTEND STATE HIERARCHY                        │
├──────────────────┬─────────────────────────────────────────────────────┤
│ State Category   │ Description & Architectural Scope                   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ 1. Server State  │ Cached API responses, ETags, offset pagination      │
│                  │ metadata, and query loading/error status (read-only)│
│ 2. URL State     │ Canonical route, shareable query params (filters,   │
│                  │ active lens, search query) for deep-linking.        │
│ 3. Lens State    │ Active exploration filters, timeline LOD zoom level, │
│                  │ map bounding box coordinates, and graph node focus. │
│ 4. UI State      │ Transient modal visibility, drawer open/close,      │
│                  │ hover tooltips, and temporary dropdown states.      │
│ 5. Client Prefs  │ Local-only accessibility settings (theme, reduced   │
│                  │ motion, text size) stored in localStorage.          │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 10. Responsive Architecture Principles

In accordance with constitutional Rule 09 and Rule 06:
1. **Multi-Device Parity**: The full knowledge graph and all 10 exploration lenses must be fully accessible across Mobile (phone), Tablet, Laptop, and Desktop.
2. **Adaptive Layout Patterns**:
   - *Desktop/Laptop*: Multi-pane side-by-side exploration (e.g., Map on left, Event Drawer on right; Graph in center, Evidence Panel on right).
   - *Tablet*: Collapsible side-drawers and responsive canvas views.
   - *Mobile (Phone)*: Stacked views, bottom-sheet cards, full-screen search overlays, and swipeable day-by-day war navigators.
3. **Touch vs. Pointer Interactions**: Large, accessible touch targets ($\ge 44 \times 44\text{ px}$) on mobile/tablet; rich hovercards and right-click context menus on desktop.
4. **Orientation Adaptability**: Smooth layout reflow between Portrait (reading/vertical timeline) and Landscape (wide map/graph visualization).

---

## 11. Graphical & Visualization Architecture Principles

Complex visual representations are central to the exploration experience:

1. **Knowledge Graph Visualizations**:
   - Bounded ego-network rendering respecting B3/B9 constraints ($D \le 2$).
   - High-contrast nodes with clear glyphs/labels.
   - Layout stability preventing visual disorientation during exploration.
2. **Family Lineage Visualizations**:
   - Tree/DAG generational flow (top-to-bottom or left-to-right).
   - Clear visual differentiation between biological, adoptive, and spiritual parentage.
3. **Geographic Map Visualizations**:
   - Bounded viewport coordinate rendering.
   - Distinct clustering markers for dense regional clusters (e.g., Kurukshetra battlefield).
   - Non-spatial fallback panel for unmapped locations.
4. **Battlefield Vyuha Visualizations**:
   - Responsive vector SVG diagrams delivered via B8 asset hosting.
   - Synchronized textual tactical descriptions and accessible SVG metadata.

---

## 12. URL & Deep-Link Architecture Principles

In accordance with [05-api-architecture.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/05-api-architecture.md) and **REQ-STA-01**:
1. **Canonical Entity URLs**: Every canonical entity possesses a permanent, human-readable deep link based on its unique slug.
2. **Exploration State in Query Parameters**: Meaningful navigational and exploration state that benefits from persistence or sharing (active lens, depth, war day, search query, filters) should be representable through canonical URLs.
3. **Transient UI State Kept Local**: Temporary interaction states (such as hover states, tooltip popups, dropdown toggles, or animation progress) remain purely local and are never written to the URL.
4. **Stateless Navigation**: Refreshing the browser or sharing a URL reconstructs the identical exploration view without relying on server-side session cookies.

---

## 13. Loading, Error, Empty, and Epistemic Conflict States

Every visual lens and component must handle all standard operational and epistemic states:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        VIEWPORT STATE TAXONOMY                         │
├───────────────────────────────────┬────────────────────────────────────┤
│ OPERATIONAL UI STATES             │ EPISTEMIC & TRUTH STATES           │
├───────────────────────────────────┼────────────────────────────────────┤
│ 1. Loading: Skeleton place-cards  │ 1. Known: Standard undisputed card │
│ 2. Empty: Clean empty-state prompt│ 2. Approximate: Date/span badge    │
│ 3. Not Found (404): Safe recovery │ 3. Unknown: Explicit "Unknown" tag │
│ 4. Rate Limited (429): Retry timer│ 4. Conflicting: Multi-claim toggle │
│ 5. Server Error (504): Fallback UI│ 5. Unmapped: Graceful pin exclusion│
└───────────────────────────────────┴────────────────────────────────────┘
```

---

## 14. Accessibility Foundation (Rule 10, PRD §15)

Accessibility is an architectural requirement embedded across all frontend blocks:
1. **Semantic HTML5 & WAI-ARIA**: Proper landmark roles (`<nav>`, `<main>`, `<aside>`), ARIA live regions for dynamic search/filter updates, and explicit ARIA labels on all graphical nodes.
2. **Keyboard Navigation & Focus Management**: 100% of interactive features (graph navigation, map panning, timeline scrubbing, evidence drawers) must be operable via keyboard (`Tab`, `Enter`, `Escape`, Arrow keys).
3. **Non-Visual Representation**: Complex visual diagrams (maps, family trees, vyuhas) must provide equivalent structured text tables or list alternatives.
4. **Visual Contrast & Reduced Motion**: WCAG 2.1 AA compliance (contrast ratio $\ge 4.5:1$ for normal text); respect for user's `prefers-reduced-motion` preferences by disabling canvas physics and animated transitions.

---

## 15. Performance & Rendering Foundation (Rule 09, B9 Alignment)

To deliver an instantaneous exploration experience and operate strictly within backend budgets:
1. **Three-Tier Payload Consumption**: Initial page loads request Tier 1 Summary Cards ($\le 20\text{ KB}$); Tier 2 Profiles ($\le 100\text{ KB}$) are loaded on navigation; Tier 3 Provenance ($\le 150\text{ KB}$) is loaded on demand when opening the Evidence Drawer.
2. **List & Table Virtualization**: Long lists (e.g., extensive event timelines, multi-result search listings) utilize windowed/virtualized DOM rendering to support smooth, responsive scrolling across large collections.
3. **Bounded Graph Rendering**: Frontend graph rendering must respect the bounded graph results and Focus depth constraints ($D \le 2$) defined by B3/B9, targeting smooth interaction without viewport congestion (formal frontend rendering budgets to be specified in Block F17).
4. **Request Cancellation & Deduplication**: Fast autocomplete typing cancels previous in-flight HTTP requests to prevent race conditions and unnecessary network overhead.

---

## 16. Security Foundation (B7 Alignment)

1. **Client is Not a Trust Boundary**: The frontend assumes all data received from the network is validated at the backend layer.
2. **Zero Secret Storage**: No administrative tokens, API keys, or database connection strings are bundled into frontend client assets.
3. **XSS Prevention & SVG Sanitization**: All rich text and descriptions are rendered using standard safe text bindings; SVG diagrams from the backend are strictly validated against B8 sanitization standards before injection.
4. **Origin-Restricted Networking**: Frontend network requests communicate solely with approved backend API origins.

---

## 17. Architectural Quality Attributes

| Quality Attribute | Architectural Implementation Strategy |
| :--- | :--- |
| **Epistemic Honesty** | Preserves variant claims, unmapped coordinates, and explicit unknown states without data fabrication. |
| **Responsiveness** | Responsive layout reflow across Mobile, Tablet, Laptop, and Desktop. |
| **Accessibility** | WCAG 2.1 AA compliance, full keyboard navigability, and screen reader semantic alternatives. |
| **Performance** | Smooth rendering and interaction, virtualized scrolling, and adherence to B9 payload/latency budgets (formal frontend performance budgets to be specified in Block F17). |
| **Deep-Linkability**| Stateless canonical URL routing for all entities, lenses, filters, and Focus subgraphs. |
| **Visual Elegance** | Modern, minimalist flat design with restrained Sanātana aesthetic; zero faux-parchment noise. |

---

## 18. Backend ↔ Frontend Architectural Dependency Matrix

| Backend Architecture Block | Frontend Architectural Impact & Interface Contract |
| :--- | :--- |
| **B1 Technology & Infra** | Consumes HTTP/JSON API served by Node.js backend. |
| **B2 Data Architecture** | Consumes API representations derived from the 12 canonical entity types and 6 epistemic states defined by B2. |
| **B3 Knowledge Graph** | Consumes polymorphic edge models, symmetric pairings, and $D \le 2$ Focus traversal subgraphs. |
| **B4 Evidence & Provenance** | Renders Four-Tier Provenance drawers, native citation locators, and multi-claim conflict toggles. |
| **B5 API Architecture** | Implements standard response envelope parsing (`{ data, pagination }`), status handling, and RFC 7807 errors. |
| **B6 Search Architecture** | Consumes dual-tier search (`pg_trgm` + `tsvector`), prefix suggest (`/search/suggest`), and alias rankings. |
| **B7 Auth & Permissions** | Preserves 100% anonymous public read exploration and handles `405 Method Not Allowed` gracefully. |
| **B8 Storage & Media** | Resolves asset URIs via `ASSET_BASE_URL`, renders sanitized SVGs, and provides monogram fallbacks. |
| **B9 Performance & Caching**| Aligns with 8 latency budgets, progressive disclosure tiers, and HTTP `ETag` conditional caching. |
| **B10 Ingestion Architecture**| Assumes published dataset adheres to two-tier quality gates and referential integrity. |
| **B11 Seed Dataset Strategy**| Exercises all 10 exploration lenses using representative seed dataset scope ($25-40$ characters). |
| **B12 Backend Testing** | Validates API contracts and integration payloads consumed by frontend views. |
| **B13 Master Synthesis** | Adheres to consolidated backend decision, risk, and constraint registers. |

---

## 19. Stage 2 Frontend Architecture Roadmap (Blocks F1–F17)

The 17-block frontend architecture roadmap establishes a rigorous, sequential dependency flow:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   STAGE 2 FRONTEND ARCHITECTURE ROADMAP                │
├───────┬──────────────────────────────────┬─────────────────────────────┤
│ Block │ Title                            │ Primary Architectural Focus │
├───────┼──────────────────────────────────┼─────────────────────────────┤
│ **F1**│ Architecture Context (This Doc)  │ Constitution, scope & matrix│
│ **F2**│ Tech Stack & Build Architecture  │ Framework, tooling, bundler │
│ **F3**│ Design System & Visual Grammar   │ Typography, tokens, palette │
│ **F4**│ Responsive Layout Architecture   │ Shell, breakpoints, drawers │
│ **F5**│ State Management Architecture    │ Client state, cache, store  │
│ **F6**│ API Client & Data Fetching       │ Data fetching, envelopes    │
│ **F7**│ Routing & Deep-Link Architecture │ URL schema, params, history │
│ **F8**│ Global Search & Autocomplete UX  │ Suggest dropdown, filters   │
│ **F9**│ Character & Entity Views UX      │ Profile cards, roles, tags  │
│ **F10**│ Knowledge Graph Visualization   │ Force layout, D≤2 canvas    │
│ **F11**│ Family Lineage Tree Viz         │ Multi-gen DAG tree layout   │
│ **F12**│ Timeline & Chronology UX        │ Sequence zoom, Parva LOD    │
│ **F13**│ Geographic Map Explorer UX      │ Viewport pins, bbox, layers │
│ **F14**│ War Explorer & Vyuha Diagrams   │ 18-day battle, SVG vyuhas   │
│ **F15**│ Evidence & Provenance Drawer    │ Shloka locators, conflicts  │
│ **F16**│ Accessibility & Internationalize│ WCAG AA, IAST, screen-reader│
│ **F17**│ Frontend Testing & Verification │ Performance, budgets, tests │
└───────┴──────────────────────────────────┴─────────────────────────────┘
```

---

## 20. Preliminary Frontend Requirement Taxonomy

Requirements in Stage 2 follow a standardized, traceable categorization taxonomy:

- **`REQ-FE-FND-xx`**: Foundational frontend principles, single-page app contracts, and constitutional rules.
- **`REQ-FE-UI-xx`**: Design system, visual tokens, typographic scale, and aesthetic restraint.
- **`REQ-FE-NAV-xx`**: Shell layout, sidebars, responsive drawers, and navigation hierarchies.
- **`REQ-FE-STA-xx`**: Client state, server-cache hydration, and URL parameter synchronization.
- **`REQ-FE-NET-xx`**: API client, response envelope handling, ETag revalidation, and error boundaries.
- **`REQ-FE-LNS-xx`**: Exploration lens-specific capabilities (Characters, Wars, Formations, Lineage).
- **`REQ-FE-VIS-xx`**: Interactive graphical engines (Knowledge Graph, Lineage DAG, Map, Timeline).
- **`REQ-FE-EVD-xx`**: Evidence drawer, citation locator rendering, and conflicting tradition toggles.
- **`REQ-FE-ACC-xx`**: WCAG 2.1 AA compliance, keyboard navigation, and screen reader semantics.
- **`REQ-FE-PRF-xx`**: Client-side rendering performance, virtualization, and frame-rate budgets.
- **`REQ-FE-TST-xx`**: Frontend testing architecture, component testing, and visual regression.

---

## 21. Explicitly Deferred Frontend Decisions

To maintain architectural discipline, the following concrete implementation choices are intentionally deferred to subsequent Stage 2 blocks or Stage 4:

1. **Frontend Framework & Version** (e.g., React 19 vs. Vue 3 vs. Svelte 5) $\rightarrow$ *Deferred to Block F2*.
2. **Build Tool & Bundler** (e.g., Vite vs. Next.js vs. Astro) $\rightarrow$ *Deferred to Block F2*.
3. **Component Styling Architecture** (e.g., Tailwind CSS vs. CSS Modules vs. Vanilla Extract) $\rightarrow$ *Deferred to Block F3*.
4. **Specific State Management Library** (e.g., Zustand vs. Jotai vs. Redux Toolkit) $\rightarrow$ *Deferred to Block F5*.
5. **Data Fetching / Server-State Library** (e.g., TanStack Query vs. SWR) $\rightarrow$ *Deferred to Block F6*.
6. **Graph Visualization Engine** (e.g., Cytoscape.js vs. D3-force vs. React Flow) $\rightarrow$ *Deferred to Block F10*.
7. **Tree Layout Algorithm / Library** (e.g., D3-hierarchy vs. Dagre) $\rightarrow$ *Deferred to Block F11*.
8. **Map Rendering Engine** (e.g., MapLibre GL vs. Leaflet vs. OpenLayers) $\rightarrow$ *Deferred to Block F13*.
9. **Frontend Test Framework** (e.g., Vitest + Testing Library vs. Playwright) $\rightarrow$ *Deferred to Block F17*.

---

## 22. Architectural Constraints & Non-Goals

1. **Zero Public Mutation UI**: The frontend contains zero forms or controls for user account registration, user logins, comments, or wiki-style edits.
2. **Zero Fictional Art / Hallucinated Text**: Characters without portraits render clean semantic monograms; missing descriptions are never filled by generative AI.
3. **Zero 3D Open-World / Battle Simulators**: Complex 3D game engines and video simulations are strictly out of scope for V1.
4. **No Desktop-Only Lock-In**: No visual exploration lens may be designed exclusively for desktop; responsive adaptations must be specified for all screens.

---

## 23. F1 Exit Criteria

Block F1 is complete when:
- [x] Frontend responsibility boundaries are formally defined and separated from backend duties.
- [x] Backend dependency boundaries (B1–B13) are mapped to frontend interfaces.
- [x] The Unified Multi-Lens Exploration model and all 10 lenses are accounted for.
- [x] Foundational state, responsive, visualization, accessibility, performance, and security principles are established.
- [x] Zero-fabrication and epistemic honesty rules are explicitly enforced.
- [x] The 17-block Stage 2 roadmap (F1–F17) is established with clear dependency sequencing.
- [x] Deferred technology decisions are explicitly catalogued.
- [x] Preliminary requirement taxonomy is formalized.
- [x] Zero application implementation code, components, or package dependencies have been introduced.
- [x] Zero Stage 1 backend architecture documents have been modified.
