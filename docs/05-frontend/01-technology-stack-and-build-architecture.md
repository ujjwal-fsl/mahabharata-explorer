# Technology Stack & Build Architecture (Block F2)

## 1. Architectural Context

This document establishes the **Technology Stack & Build Architecture** for the **Mahābhārata Explorer** frontend. It defines the foundational programming language, UI framework, client-side rendering model, build toolchain, type-safety architecture, dependency governance, and browser baseline for Stage 2.

```
┌────────────────────────────────────────────────────────────────────────┐
│                   STAGE 2 ARCHITECTURAL PLACEMENT                      │
├───────────────────────────────────┬────────────────────────────────────┤
│ UPSTREAM CONTRACTS & CONSTRAINTS  │ DOWNSTREAM FRONTEND SPECIFICATIONS │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Stage 1 Backend Architecture    │ - F3: Design System & Tokens       │
│   (B1 Infra, B2 Data, B3 Graph,   │ - F4: Responsive Layout & Shell    │
│    B4 Provenance, B5 API, B7 Auth,│ - F5: State Management             │
│    B8 Media, B9 Performance)      │ - F6: API Client & Data Fetching   │
│ - Block F1: Frontend Constitution │ - F7: Routing & Deep-Linking       │
│   (Principles, Lenses, Boundaries)│ - F10–F14: Visualizations          │
│                                   │ - F17: Testing & Budgets           │
└───────────────────────────────────┴────────────────────────────────────┘
```

### 1.1 Architectural Grounding & Decoupling
1. **Downstream of Stage 1 & Block F1**: F2 inherits all constitutional principles from [00-frontend-architecture-context.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/05-frontend/00-frontend-architecture-context.md) and the operational contracts established in Stage 1 ([05-api-architecture.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/05-api-architecture.md), [09-performance-and-caching.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/09-performance-and-caching.md)).
2. **Decoupled from PostgreSQL Internal Schemas**: The frontend interacts strictly with **backend API domain representations** exposed via HTTP/JSON. It does not import database models, query PostgreSQL directly, or assume database schema layout.
3. **HTTP Server Framework Neutrality**: The backend HTTP framework selection remains a Stage 4 implementation choice; the frontend relies exclusively on standard HTTP/REST contracts (`GET` requests, JSON payloads, RFC 7807 problem details).
4. **Pure Architectural Specification**: F2 does not create application source code, configure `package.json`, install NPM packages, or initialize build scripts.

---

## 2. Frontend Architectural Requirements

The technology stack must satisfy the requirements derived from the project constitution (PRD §8, §15, Blueprint §9, F1 §5):

| Requirement Area | Architectural Description & Scope | Classification |
| :--- | :--- | :--- |
| **Interactive Discovery** | Multi-lens exploration across 10 distinct lenses (Characters, Lineage, Graph, Timeline, Map, Wars, Vyuhas, Sources, Search, Focus). | **MUST** |
| **Rich Visualizations** | Smooth rendering of interactive network graphs, genealogical DAGs, chronological timelines, spatial viewports, and tactical SVG formations. | **MUST** |
| **Responsive Parity** | Parity across Desktop, Laptop, Tablet, and Mobile in both Portrait and Landscape orientations. | **MUST** |
| **Progressive Disclosure** | Progressive loading aligned with endpoint-specific backend payload targets defined by B9, requesting and rendering only the data needed for the active exploration context. | **MUST** |
| **Deep Linkability** | Stateless, URL-addressable navigation for all canonical entities, active lenses, filters, and Focus subgraphs. | **MUST** |
| **Accessibility (a11y)** | WCAG 2.1 AA compliance, semantic DOM elements, full keyboard navigation, screen reader support, and textual alternatives for complex visuals. | **MUST** |
| **Anonymous Read-Only** | 100% unauthenticated exploration with zero client-side secret storage or administrative mutation forms. | **MUST** |
| **Zero Fabrication** | Explicit UI rendering for epistemic uncertainty (`unknown`, `conflicting`, `approximate`, `unmapped`); no synthetic text or speculative artwork. | **MUST** |
| **Build Performance** | Fast development server startup, rapid HMR, optimized tree-shaking, and lazy route/visualization chunk splitting. | **SHOULD** |
| **Code Maintainability** | Strict type safety, clean modular boundaries, discoverable component architecture, and high debuggability for AI-assisted engineering. | **MUST** |

---

## 3. Technology Options & Framework Evaluation

To select the core UI framework and application model, three prominent modern frontend ecosystems and two primary rendering architectures were evaluated against the specific demands of the Mahābhārata Explorer:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   FRONTEND ECOSYSTEM EVALUATION MATRIX                 │
├───────────────────┬──────────────┬──────────────┬──────────────────────┤
│ Evaluation Factor │ React 19+    │ Vue 3+       │ Svelte 5 / Kit       │
├───────────────────┼──────────────┼──────────────┼──────────────────────┤
│ Visualization     │ **Broad &    │ Strong       │ Moderate             │
│ Ecosystem (D3/Viz)│ Mature**     │ (Good wrapper│ (Manual DOM/canvas   │
│                   │ (Extensive adapters for D3, Cytoscape, MapLibre, React Flow)│ support, smaller ecosystem)│ lifecycle binding needed)│
├───────────────────┼──────────────┼──────────────┼──────────────────────┤
│ Component Model & │ **Declarative**│ Template +   │ Runes / Compiler     │
│ Complex State     │ Unidirectional, predictable state reflow │ Reactive proxy model│ Fine-grained signals │
├───────────────────┼──────────────┼──────────────┼──────────────────────┤
│ TypeScript Depth  │ **First-Class**│ Excellent    │ Good                 │
│ & Static Safety   │ Native TSX, complete inference across props/generics│ TSX support, template type-checking│ Svelte-check compiler│
├───────────────────┼──────────────┼──────────────┼──────────────────────┤
│ Accessibility &   │ **Extensive**│ Strong       │ Strong               │
│ ARIA Primitives   │ (Radix, React Aria, floating-ui, Headless UI)│ (Radix-Vue, Headless UI)│ (Bits UI, Melt UI)   │
├───────────────────┼──────────────┼──────────────┼──────────────────────┤
│ AI-Assisted       │ **High       │ High         │ Moderate             │
│ Development (AGY) │ Suitability**│ Template syntax and SFC multi-blocks│ Rapid compiler syntax evolution (Svelte 4→5)|
│                   │ Predictable patterns, deep static typing, clear modularity│              │                      │
└───────────────────┴──────────────┴──────────────┴──────────────────────┘
```

### 3.1 Application Architecture: SPA vs. SSR / Hybrid
- **Server-Side Rendering (SSR / Next.js / Nuxt / SvelteKit)**:
  - *Evaluation*: Adds substantial server infrastructure complexity (Node.js runtime hosting, potential server hydration mismatches with complex canvas/WebGL viewports, duplicate server caching tiers). Because the Mahābhārata Explorer is an **anonymous, interactive visual exploration client** querying a standalone backend REST API with edge HTTP caching, SSR adds operational overhead without commensurate benefit.
- **Single-Page Application (SPA / Static Client Bundle)**:
  - *Evaluation*: Well aligned with the application's architecture for client-heavy interactive exploration with rich canvas/SVG rendering, client-side route transitions, and static CDN-friendly distribution. Canonical entity pages achieve fast client-side navigation after the initial application shell is loaded, consuming HTTP-cached backend REST API endpoints.
  - *Decision*: **Client-Side SPA Architecture**.

### 3.2 Framework Selection
- **Selected Framework**: **React 19+ (TypeScript)**.
- **Rationale**:
  1. *Broad and Mature Visualization Ecosystem*: React provides an extensive ecosystem of battle-tested bindings and lifecycle adapters for complex canvas, WebGL, SVG, and graph rendering engines (Cytoscape, D3, MapLibre, React Flow).
  2. *Accessible UI Primitives*: Broad availability of headless ARIA primitive libraries (e.g., Radix UI, Floating UI) ensures accessible dropdowns, dialogs, drawers, and tooltips without fighting framework wrappers.
  3. *Unidirectional Data Flow*: Predictable unidirectional rendering minimizes state tearing across multi-pane exploration layouts (e.g., synchronizing a timeline scrubber with a map viewport).
  4. *Suitability for AI-Assisted Engineering*: Standard TSX component patterns offer strong code clarity, static typing, and modular discoverability within the Google Antigravity environment.

---

## 4. Recommended Frontend Stack

```
┌────────────────────────────────────────────────────────────────────────┐
│                     FRONTEND TECHNOLOGY STACK                          │
├──────────────────────────┬─────────────────────────────────────────────┤
│ Core Layer               │ Selected Technology                         │
├──────────────────────────┼─────────────────────────────────────────────┤
│ UI Framework             │ **React 19+**                               │
│ Programming Language     │ **TypeScript 5.x** (Strict Mode)            │
│ Application Architecture │ **Client-Side Single-Page Application (SPA)**│
│ Build Engine & Bundler   │ **Vite 6+** (ESBuild + Rollup)              │
│ Module System            │ **Native ECMAScript Modules (ESM)**         │
│ Styling Architecture     │ *Specification Deferred to Block F3*        │
│ State Management         │ *Specification Deferred to Block F5*        │
│ API Client & Fetching    │ *Specification Deferred to Block F6*        │
│ Routing Engine           │ *Specification Deferred to Block F7*        │
│ Visualization Engines    │ *Specification Deferred to Blocks F10–F14*  │
│ Test Runner & Benchmarks │ *Specification Deferred to Block F17*       │
└──────────────────────────┴─────────────────────────────────────────────┘
```

---

## 5. TypeScript Architecture & Type-Safety Boundaries

TypeScript is the mandatory language for all frontend development. The type system enforces conceptual separation between transport contracts, domain models, and visualization structures:

```
┌────────────────────────────────────────────────────────────────────────┐
│                       TYPE-SAFETY ARCHITECTURE                         │
├────────────────────────────────────────────────────────────────────────┤
│ 1. TRANSPORT / API CONTRACT TYPES (e.g., API DTOs)                     │
│    - Directly mirrors B5 REST API response envelopes and DTOs.         │
│    - `{ data: T }`, `{ data: T[], pagination: OffsetPagination }`     │
│    - Models epistemic status enums: 'known'|'unknown'|'conflicting'|...│
├────────────────────────────────────────────────────────────────────────┤
│ 2. DOMAIN / APPLICATION VIEW MODELS (e.g., View Models)                │
│    - Clean client-side representations optimized for UI rendering.     │
│    - Computed helpers (e.g., display name with IAST diacritics,        │
│      monogram fallback strings, localized role badges).                │
├────────────────────────────────────────────────────────────────────────┤
│ 3. VISUALIZATION ENGINE TYPES (e.g., Viz Coordinates & Graph Models)   │
│    - Canvas/SVG coordinates, graph node/edge layouts, bounding boxes,  │
│      viewport transformation matrices.                                 │
└────────────────────────────────────────────────────────────────────────┘
```
*(Note: Concrete file paths and directory structures for types are illustrative and will be established during Stage 4 implementation).*

### 5.1 Strictness Principles
1. **Strict Type Checking**: Mandatory `strict: true`, `noImplicitAny: true`, `strictNullChecks: true`, and `exactOptionalPropertyTypes: true`.
2. **Epistemic Nullability**: Fields that may be unresearched or unknown in the backend model (e.g., coordinates, birth order, parentage) must be explicitly typed as `T | null` or include explicit epistemic discriminant flags.
3. **No Database Leakage**: Database-internal primary key conventions or raw table definitions are never imported into frontend code; only typed API DTOs are consumed.
4. **Runtime Boundary Defense**: TypeScript types provide compile-time verification; runtime boundary validation (safe JSON parsing, RFC 7807 error status checking) protects client state from malformed network responses.

---

## 6. Build Tooling & Asset Pipeline

The build architecture utilizes **Vite 6+**, providing an optimized developer experience and production output:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        VITE BUILD ARCHITECTURE                         │
├───────────────────────────────────┬────────────────────────────────────┤
│ DEVELOPMENT ENVIRONMENT           │ PRODUCTION BUNDLE PIPELINE         │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Native ESM-based dev server     │ - Multi-chunk Rollup bundling      │
│ - Instant Hot Module Replacement  │ - Route-based code splitting       │
│ - Fast TypeScript transpilation   │ - Heavy viz chunk isolation        │
│   via ESBuild                     │ - Tree-shaking of unused utilities │
│ - Proxy mapping to backend API    │ - Content-hashed asset output      │
└───────────────────────────────────┴────────────────────────────────────┘
```

### 6.1 Code Splitting & Chunk Isolation Strategy
To maintain responsive initial page loads and satisfy B9 performance guidelines:
1. **Core Application Shell**: Bundles the main navigation shell, global search modal, and base typography into a minimal initial entry chunk.
2. **Route-Level Splitting**: Each primary exploration lens (Character Profile, War Explorer, Timeline) is packaged as an independent, lazy-loaded chunk loaded on demand upon navigation.
3. **Heavy Visualization Isolation**: Heavy rendering libraries (Graph canvas engines, MapLibre GL, D3 tree layout modules) are segregated into dedicated dynamic chunks, ensuring users visiting text/profile views do not download unused visualization binaries.

---

## 7. Rendering Architecture & DOM/Canvas Boundaries

To preserve accessibility, responsiveness, and rendering performance, the frontend establishes a three-tier rendering architecture. Concrete assignments of specific lenses to rendering tiers represent preferred architectural directions; final rendering engine selections remain under the authority of Blocks F10–F14:

```
┌────────────────────────────────────────────────────────────────────────┐
│                       THREE-TIER RENDERING MODEL                       │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Rendering Tier   │ Architectural Technology & Preferred Scope          │
├──────────────────┼─────────────────────────────────────────────────────┤
│ 1. Semantic DOM  │ Standard HTML5 elements (`<article>`, `<nav>`,      │
│    (Standard UI) │ `<button>`, `<aside>`). Handles layout shells, text,│
│                  │ character profiles, search results, forms, dialogs. │
│                  │ Fully accessible to screen readers and keyboard.    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ 2. Vector SVG    │ Scalable vector graphics. Preferred direction for   │
│    (Diagrams)    │ battlefield Vyuha diagrams, iconography, badges,    │
│                  │ and focused genealogical sub-trees (authority F11/14│
├──────────────────┼─────────────────────────────────────────────────────┤
│ 3. Canvas/WebGL  │ High-performance 2D/3D hardware-accelerated canvas. │
│    (Dense Viz)   │ Preferred direction for complex force-directed      │
│                  │ Knowledge Graphs and interactive spatial maps       │
│                  │ (authority F10/13), with accessible DOM fallbacks.  │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 8. Visualization Technology Selection Criteria

Blocks **F10 through F14** will specify concrete visualization engines. Block F2 establishes the architectural evaluation criteria governing those selections:

| Evaluation Criterion | Architectural Requirement & Standard |
| :--- | :--- |
| **Lifecycle Interoperability** | Must cleanly synchronize with React lifecycle (mount, update, unmount) without memory leaks or phantom DOM nodes. |
| **Responsive Scalability** | Viewport changes should avoid unnecessary full graph recomputation and should preserve existing layout state where technically feasible. |
| **Touch & Gesture Support** | Must natively support multi-touch pinch-to-zoom, pan, drag, and tap on mobile/tablet viewports. |
| **Accessibility Fallback** | Must provide an accessible DOM representation (e.g., semantic table or nested list) for screen readers. |
| **Bundle Footprint** | Must be modular and tree-shakeable to avoid inflating client bundles. |
| **Physics Stability** | Force simulation engines must stabilize rapidly to prevent continuous battery drain and visual jitter. |

*Concrete Library Selection Status (Under Authority of Dedicated Blocks)*:
- **F10 (Knowledge Graph)**: Deferred to Block F10 (Evaluating Cytoscape.js, D3-force, React Flow).
- **F11 (Family Lineage DAG)**: Deferred to Block F11 (Evaluating D3-hierarchy, Dagre).
- **F12 (Timeline Scrubber)**: Deferred to Block F12 (Evaluating SVG/Canvas timeline virtualizers).
- **F13 (Geographic Map)**: Deferred to Block F13 (Evaluating MapLibre GL, Leaflet).
- **F14 (Battlefield Vyuhas)**: Deferred to Block F14 (Evaluating React SVG renderers with B8 asset links).

---

## 9. Dependency & Package Governance

To avoid dependency bloat and supply-chain vulnerabilities, frontend architecture enforces disciplined package management:

```
┌────────────────────────────────────────────────────────────────────────┐
│                     PACKAGE GOVERNANCE PRINCIPLES                      │
├────────────────────────────────────────────────────────────────────────┤
│ 1. NATIVE PLATFORM FIRST: Use standard browser APIs (Fetch, URLSearchParams,│
│    Intl, ResizeObserver) before reaching for third-party utilities.    │
│ 2. ZERO RUNTIME BLOAT: Avoid heavy monolithic utility suites (e.g., Lodash,│
│    Moment.js). Use modern JS or lightweight, tree-shakeable libraries. │
│ 3. ADAPTER PATTERN: Wrap major third-party engines (graph, map, state) │
│    behind local application interfaces to allow painless replacement.  │
│ 4. PERMISSIVE LICENSING: Prefer permissively licensed dependencies such │
│    as MIT, Apache-2.0, and BSD variants; dependencies with other       │
│    licenses require explicit project-level review.                     │
│ 5. RIGID DEPENDENCY AUDITING: Lockfile determinism and automated vulnerability│
│    scanning enforced in CI/CD pipeline.                                │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 10. Routing & Deep-Linking Boundary

Client-side routing architecture enables seamless exploration while preserving browser history and shareability:

1. **Client-Side History Routing**: Leverages HTML5 History API (`pushState`/`replaceState`) for clean, human-readable URLs without hash fragments (`#`).
2. **Reload & Direct Navigation Resilience**: Static hosting servers (or development proxies) must configure single-page fallback routing (rewriting non-file requests to `index.html`).
3. **Stateless URL Exploration**: As established in F1 §12, all meaningful exploration parameters (selected character, timeline zoom, active lens, war day, search query) serialize to the URL, while transient interaction states (tooltips, hovercards) remain purely local.
4. **Contract Scope**: The specific routing library selection and concrete route hierarchy are formally specified in Block **F7 (Routing, Navigation & Deep-Linking Architecture)**.

---

## 11. API & Data Boundary Architecture

The frontend communicates with the backend exclusively through HTTP REST contracts:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        API INTEGRATION BOUNDARY                        │
├────────────────────────────────────────────────────────────────────────┤
│ FRONTEND APPLICATION (React SPA)                                       │
│   │                                                                    │
│   ▼ (HTTP GET / Standard REST Envelopes)                               │
│ BACKEND REST API ([05-api-architecture.md])                           │
│   ├── Response Envelope: `{ data: ... }`                               │
│   ├── Collection Envelope: `{ data: [...], pagination: { offset, ... }}`│
│   ├── Error Envelope: RFC 7807 `{ type, title, status, detail }`       │
│   └── Caching: `ETag`, `Cache-Control: public, no-cache`               │
└────────────────────────────────────────────────────────────────────────┘
```

1. **Envelope Handling**: Client data layers transparently consume `{ data: ... }` payloads and handle offset pagination metadata (`limit`, `offset`, `total_count`, `has_more`).
2. **Conditional Revalidation & Caching**: The frontend architecture must remain compatible with backend HTTP caching, `ETag` conditional revalidation, and backend Focus fallback semantics ($D_2 \rightarrow D_1 \rightarrow 504$).
3. **Error Boundary Integration**: Non-2xx responses parse RFC 7807 problem details to render context-sensitive error alerts, rate-limit timers (`429`), or Focus fallback indicators.
4. **Data-Fetching Responsibility**: Concrete data-fetching libraries, caching lifecycles, ETag storage mechanics, request deduplication, and revalidation policies are formally evaluated and specified in Block **F6 (API Client, Data Fetching & Caching Architecture)**.

---

## 12. Environment & Configuration Architecture

Environment configuration isolates runtime parameters across development, staging, and production:

1. **Build-Time Public Variables**: Environment variables exposed to the browser must use the standard `VITE_` prefix (e.g., `VITE_API_BASE_URL`, `VITE_ASSET_BASE_URL`).
2. **Zero Embedded Secrets**: Browser client bundles are fundamentally public. **Zero secrets, API private keys, or administrative tokens may exist in frontend configuration or source code.**
3. **Asset URL Resolution**: Static visual assets (portraits, diagrams, icons) resolve paths dynamically against `VITE_ASSET_BASE_URL`, ensuring seamless asset loading whether hosted locally during development or via a global CDN in production.

---

## 13. Browser & Platform Baseline

The frontend is designed around modern evergreen web browsers:

1. **Target Browser Families**: Modern Chromium (Chrome, Edge), Firefox, and WebKit (Safari on macOS and iOS), and modern mobile browsers.
2. **Baseline Web Platform Standards**: Modern ECMAScript (ES2022+), CSS Grid, Flexbox, Canvas 2D, WebGL 2.0, dynamic viewport height units (`dvh`), and modern DOM APIs (`ResizeObserver`, `AbortController`).
3. **Interaction Modalities**: Comprehensive support for touch gestures (mobile/tablet), pointer interactions (mouse/trackpad on desktop), and keyboard navigation across all platforms.
4. **Validation Scope**: The formal browser compatibility test matrix and empirical testing protocols will be defined in Block **F17** and validated during Stage 5.

---

## 14. Frontend Security Architecture

Frontend security enforces strict defense-in-depth principles:

1. **Untrusted Client Environment**: The frontend operates under the principle that the browser environment is untrusted; all data received from the network is validated before consumption.
2. **Cross-Site Scripting (XSS) Prevention**: React's native JSX escaping prevents HTML injection. Any raw SVG rendering (e.g., battlefield formations) is strictly constrained to assets originating from the verified `ASSET_BASE_URL` after backend XML sanitization ([08-storage-and-media.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/08-storage-and-media.md)).
3. **Safe URL & External Link Handling**: All scholarly external citation URLs render with `target="_blank"` and `rel="noopener noreferrer"`.
4. **Information Leakage Prevention**: Production error boundaries and logging middleware sanitize error messages to ensure backend database errors or internal stack traces are never surfaced in the UI.

---

## 15. Performance Architecture Strategies

In alignment with Stage 1 performance requirements and Block F1 principles, the frontend implements proactive performance strategies:

1. **Lens & Route-Based Code Splitting**: Minimizes initial bundle size by deferring non-essential exploration lenses until requested.
2. **DOM Virtualization**: Virtualizes large search-result lists and timelines to ensure consistent UI responsiveness.
3. **State Isolation**: High-frequency interactive states (e.g., timeline scrubbing, mouse hover coordinates) are isolated in local component state to prevent unnecessary re-rendering of parent layout trees.
4. **Network Request Cancellation**: In-flight autocomplete and search requests utilize `AbortController` to cancel stale requests when the user continues typing.
5. **Formal Budgets**: Formal frontend numeric budgets, bundle size limits, and frame-rate targets will be established in Block **F17 (Frontend Testing, Verification & Performance Budget Architecture)**.

---

## 16. Testability & Development Experience

The frontend stack is architected for comprehensive testability:

1. **Component Isolation**: UI components are structured with clean prop interfaces, enabling isolated unit testing without requiring live backend instances.
2. **Mock API Boundary**: Standard mock service handlers replicate B5 response envelopes and RFC 7807 error structures for deterministic local development and automated testing.
3. **Visual Regression Readiness**: Headless DOM structures and stable SVG element identifiers support visual regression and snapshot testing.
4. **Test Architecture Specification**: Formal test runner selection (e.g., Vitest + React Testing Library vs. Playwright) is defined in Block **F17**.

---

## 17. Google Antigravity Development Compatibility

The technology stack is structured for effective AI-assisted development in Google Antigravity:

1. **Predictable Architecture**: Standard React 19 + TypeScript + Vite project patterns follow clear, modular component and lens organizations, maximizing code discoverability.
2. **Strong Static Typing**: Deep TypeScript interfaces enable Antigravity to perform exact compile-time refactoring, type checking, and boundary verification across all lenses.
3. **Modular Component Isolation**: Small, single-responsibility components and explicit adapter interfaces allow Antigravity subagents to execute targeted modifications with minimal context thrashing.
4. **Zero Esoteric Metaprogramming**: Avoiding complex compiler transforms, proprietary macros, or opaque magic ensures all code remains readable, inspectable, and maintainable by both human engineers and AI agents.

---

## 18. Architectural Decision Record (ADR)

| Decision ID | Architectural Decision | Chosen Approach | Alternatives Evaluated | Rationale & Trade-offs | Owner / Block | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **ADR-FE-01** | Core UI Framework | **React 19+** | Vue 3, Svelte 5 | Broad and mature visualization ecosystem, mature headless ARIA primitives, strong Antigravity suitability. | F2 | **DECIDED** |
| **ADR-FE-02** | Programming Language | **TypeScript 5.x (Strict)** | JavaScript (ESNext) | Mandatory compile-time type safety across complex graph, timeline, and API representations. | F2 | **DECIDED** |
| **ADR-FE-03** | Application Model | **Client-Side SPA** | SSR (Next.js), SSG (Astro) | Eliminates server hydration mismatches on heavy canvas/SVG viewports; well aligned with static CDN + REST API. | F2 | **DECIDED** |
| **ADR-FE-04** | Build Tool & Bundler | **Vite 6+ (Rollup/ESBuild)** | Webpack, Turbopack | Fast HMR in dev, optimized Rollup chunk splitting, native ESM tooling. | F2 | **DECIDED** |
| **ADR-FE-05** | Styling Architecture | *Deferred to Block F3* | Tailwind CSS, CSS Modules | To be evaluated based on visual grammar, design tokens, and theme constraints. | F3 | **DEFERRED** |
| **ADR-FE-06** | State Management | *Deferred to Block F5* | Zustand, Jotai, Redux | To be evaluated based on lens coordination, cache needs, and state complexity. | F5 | **DEFERRED** |
| **ADR-FE-07** | API & Fetching Client | *Deferred to Block F6* | TanStack Query, SWR, Fetch | To be evaluated based on caching, ETag support, and request deduplication. | F6 | **DEFERRED** |
| **ADR-FE-08** | Routing Engine | *Deferred to Block F7* | React Router, TanStack Router | To be evaluated based on deep-linking, type-safe URL search params, and nested layout support. | F7 | **DEFERRED** |
| **ADR-FE-09** | Graph Viz Engine | *Deferred to Block F10* | Cytoscape.js, D3-force | To be evaluated based on force layout stability, touch gestures, and node performance. | F10 | **DEFERRED** |
| **ADR-FE-10** | Map Viz Engine | *Deferred to Block F13* | MapLibre GL, Leaflet | To be evaluated based on vector tiles, spatial clustering, and mobile performance. | F13 | **DEFERRED** |
| **ADR-FE-11** | Test Framework | *Deferred to Block F17* | Vitest + RTL, Playwright | To be evaluated in the dedicated frontend testing architecture block. | F17 | **DEFERRED** |

---

## 19. Requirement Traceability Matrix

| Requirement / Principle | Source Document | Implementing Block F2 Section | Coverage Status |
| :--- | :--- | :--- | :--- |
| **Knowledge Explorer First** | F1 §5.1, PRD §2 | §3 (SPA + React), §7 (Three-Tier Rendering) | **SATISFIED** |
| **One Unified Graph, Many Lenses**| F1 §5.2, B3 §1 | §2 (Requirements), §6 (Lens Code Splitting) | **SATISFIED** |
| **Evidence-Aware UI** | F1 §5.3, B4 §1 | §2 (Requirements), §5 (Provenance Types) | **SATISFIED** |
| **Zero Data Fabrication** | F1 §5.4, Rule 03 | §2 (Requirements), §5 (Epistemic Nullability) | **SATISFIED** |
| **Responsive by Design** | F1 §5.5, Rule 09 | §2 (Requirements), §13 (Browser Baseline) | **SATISFIED** |
| **Progressive Disclosure** | F1 §5.6, B9 §5 | §2 (Requirements), §15 (Data & Bundle Minimization)| **SATISFIED** |
| **Stateless Deep Linking** | F1 §5.8, B5 §5 | §10 (Routing Boundary), §18 (ADR-FE-08) | **SATISFIED** |
| **Accessibility First** | F1 §5.9, Rule 10 | §2 (Requirements), §7 (Semantic DOM Tier) | **SATISFIED** |
| **Visual Restraint** | F1 §5.10, Rule 11 | §4 (Tech Stack), §18 (ADR-FE-05) | **SATISFIED** |
| **Backend REST & ETag Alignment**| B5 §3, B9 §6 | §11 (API & Data Boundary) | **SATISFIED** |
| **Anonymous Read Security** | B7 §3 | §14 (Security Architecture) | **SATISFIED** |

---

## 20. F2 Exit Criteria Checklist

- [x] The frontend technology stack is explicitly justified (React 19+, TypeScript, Vite 6+, SPA model).
- [x] Framework alternatives (Vue 3, Svelte 5) and application architectures (SPA vs. SSR) were evaluated with concrete trade-offs.
- [x] TypeScript architecture establishes separation between API transport DTOs, domain view models, and viz types.
- [x] Build architecture establishes Vite bundling, route-based code splitting, and visualization chunk isolation.
- [x] Rendering architecture establishes boundaries across Semantic DOM, Vector SVG, and Canvas/WebGL tiers without preempting F10–F14.
- [x] Visualization selection criteria are established without prematurely deciding Blocks F10–F14.
- [x] Dependency governance, browser baseline, security boundaries, and performance strategies are defined.
- [x] Architectural Decision Record (ADR) explicitly catalogs decided choices and formally deferred items.
- [x] Traceability to F1 constitutional principles and Stage 1 backend constraints is fully documented.
- [x] Zero application source code, package installations, or configuration files were introduced.
- [x] Zero Stage 1 backend documents or F1 documents were modified.
