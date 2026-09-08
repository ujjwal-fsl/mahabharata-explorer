# Testing, Performance & Validation Architecture (Block F17)

## 1. Document Status & Purpose

```
┌────────────────────────────────────────────────────────────────────────┐
│                          DOCUMENT ATTRIBUTION                          │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Document ID      │ docs/05-frontend/16-testing-performance-validation-  │
│                  │ architecture.md                                     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ Stage / Block    │ Stage 2 (Frontend Architecture) — Block F17 (FINAL) │
├──────────────────┼─────────────────────────────────────────────────────┤
│ Upstream Sources │ - Stage 1 Blocks B1–B13: Backend Architecture       │
│                  │ - Stage 2 Block F1: Frontend Architecture Context   │
│                  │ - Stage 2 Block F2: Technology & Build Architecture │
│                  │ - Stage 2 Block F3: Design System & Visual Grammar  │
│                  │ - Stage 2 Block F4: Layout & Application Shell      │
│                  │ - Stage 2 Block F5: State Management Architecture   │
│                  │ - Stage 2 Block F6: Fetching & Caching Architecture │
│                  │ - Stage 2 Block F7: Routing & Navigation            │
│                  │ - Stage 2 Block F8: Search Architecture             │
│                  │ - Stage 2 Block F9: Entity Views Architecture       │
│                  │ - Stage 2 Block F10: Graph Visualization Arch.      │
│                  │ - Stage 2 Block F11: Lineage / Family Tree Arch.    │
│                  │ - Stage 2 Block F12: Timeline & Chronology Arch.    │
│                  │ - Stage 2 Block F13: Geographic Map Architecture    │
│                  │ - Stage 2 Block F14: War & Tactical Vyuha Arch.     │
│                  │ - Stage 2 Block F15: Evidence & Provenance UI       │
│                  │ - Stage 2 Block F16: Accessibility Architecture     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ Downstream Deps  │ - Stage 4: Implementation Phase                     │
│                  │ - Stage 5: Empirical Validation & Acceptance        │
├──────────────────┼─────────────────────────────────────────────────────┤
│ Status           │ Draft Architectural Specification (Review Pending) │
└──────────────────┴─────────────────────────────────────────────────────┘
```

This document establishes the **Testing, Performance & Validation Architecture** for the **Mahābhārata Explorer** frontend. As the seventeenth and final block of Stage 2 (Frontend Architecture), Block F17 defines the authoritative verification model, quality-gate hierarchy, frontend performance boundaries, testing pyramid, accessibility verification methodology, responsive recomposition testing, visualization validation rules, zero-fabrication regression guards, and the formal handover criteria from Stage 2 through Stage 4 (Implementation) to Stage 5 (Empirical Validation).

In accordance with Constitutional Rule 14:
> *"Do not declare a feature complete merely because it compiles; implementation must eventually be tested against its requirements."*

Block F17 is an **architectural specification, not an empirical testing report or implementation test harness**. It specifies **WHAT** must be tested, validated, measured, and gated, the criteria governing release readiness, and the precise ownership boundaries separating backend targets, frontend architecture, implementation artifacts, and empirical validation outcomes.

---

## 2. Scope & Architectural Boundaries

### 2.1 Scope of Block F17
Block F17 governs:
1. **Frontend Testing Pyramid**: Structural organization of unit, component, integration, route, visualization, accessibility, end-to-end, and regression tests.
2. **Frontend Performance Architecture**: Measurement models, client performance categories, Core Web Vitals correlation, and attribution frameworks separating client rendering from backend latency.
3. **Bundle & Code-Splitting Validation**: Verification of F2 chunk isolation, route-level splitting, heavy visualization isolation, and dependency governance.
4. **Accessibility Verification Architecture**: Verification methodology for F16 WCAG 2.2 Level AA requirements, dual-presentation companions, keyboard operability, and focus hygiene.
5. **Routing & Deep-Link Validation**: Verification of F7 canonical route grammar, path identity vs. query modifier rules, not-found boundaries, and state reproducibility.
6. **Data, Epistemic & Provenance Validation**: Preservation of B2/B4 six epistemic states, four-tier provenance hierarchy, native locators, and zero-fabrication invariants.
7. **Visualization Validation**: Functional and structural validation of Graph (F10), Lineage (F11), Timeline (F12), Map (F13), and War/Vyuha (F14) against canonical data.
8. **Responsive & Reflow Validation**: Behavioral recomposition testing across F4 space-based viewport classes (Compact, Medium, Expanded, Wide).
9. **Security & Content-Safety Validation**: Untrusted URL parameter defense, data escaping, and public read-only boundary enforcement.
10. **CI / Quality-Gate Architecture**: Staged quality gates, failure criteria, and release-blocking policies.
11. **Stage 2 Handover & Closure**: Formal completion criteria for Stage 2 Frontend Architecture.

### 2.2 Out-of-Scope & Deferred Activities
In accordance with Stage 2 boundaries:
- **Zero Implementation Code**: F17 does not write test scripts, Jest/Vitest specs, Playwright test suites, or component mocks.
- **Zero Package Installations**: F17 does not install test runners, bundle analyzers, or CI tools.
- **Zero Empirical Claims**: F17 does not report benchmark scores, achieved Lighthouse numbers, or current WCAG audit results; all empirical measurements are conducted during Stage 5.
- **Zero Backend Target Redefinitions**: F17 consumes B9 backend performance targets as immutable upstream contracts and does not redefine them as frontend budgets.

---

## 3. Upstream Architectural Contracts & Invariants

Block F17 strictly consumes and preserves the authoritative architectural contracts established in Stage 1 and Blocks F1–F16:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   UPSTREAM CONTRACT CONSUMPTION MATRIX                 │
├──────────────────┬──────────────────┬──────────────────────────────────┤
│ Source Document  │ Contract Domain  │ F17 Architectural Preservation   │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Block B2**     │ Data Schema      │ 12 canonical entities; exact 6   │
│                  │ & Truth States   │ project-wide epistemic states.   │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Block B3**     │ Knowledge Graph  │ Bounded graph topology; D≤2      │
│                  │                  │ traversal limit; DAG kinship.    │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Block B4**     │ Provenance       │ Four-tier provenance chain;      │
│                  │ Hierarchy        │ Entity/Edge→Claim→Evidence→Source│
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Block B5**     │ API Protocol     │ Standard JSON envelopes; RFC 7807│
│                  │ & Pagination     │ errors; offset/limit pagination. │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Block B7**     │ Security Model   │ Anonymous read-only; 405 error   │
│                  │                  │ on mutations; no admin leakage.  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Block B8**     │ Storage & Media  │ Safe asset resolution; SVG XML   │
│                  │                  │ sanitization; monogram fallback. │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Block B9**     │ Performance &    │ 8 p95 latency budgets; D2→D1     │
│                  │ Caching Target   │ degradation; HTTP Cache-Control. │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Block F2**     │ Build & Tech     │ Vite 6+; TypeScript strict;      │
│                  │ Architecture     │ code-splitting chunk isolation.  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Block F4**     │ Shell & Layout   │ 4 viewport classes; contextual   │
│                  │                  │ surface taxonomy; reflow rules.  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Block F5**     │ State Hierarchy  │ URL authoritative state; server  │
│                  │                  │ cache; local UI / user prefs.    │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Block F6**     │ Fetch & Cache    │ Server lifecycle; query keys;    │
│                  │                  │ ETag revalidation; deduplication.│
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Block F7**     │ Routing & URL    │ Canonical route grammar; path ID │
│                  │                  │ vs query modifiers; history API. │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Block F8**     │ Global Search    │ 2-tier search; suggest debounce; │
│                  │                  │ combobox accessibility semantics.│
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Blocks F9–F14**│ Exploration      │ Entity facets; D≤2 focus graph;  │
│                  │ Lenses           │ kinship DAG; narrative timeline; │
│                  │                  │ map coordinate status; war tiers.│
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Block F15**    │ Evidence UI      │ 4-tier citation UI; neutral      │
│                  │                  │ missing excerpts; B4 origin tags.│
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Block F16**    │ Accessibility    │ WCAG 2.2 Level AA; dual-companion│
│                  │ Architecture     │ pattern; focus hygiene; parity.  │
└──────────────────┴──────────────────┴──────────────────────────────────┘
```

### 3.1 Preservation of B9 Backend Performance Targets
F17 maintains full traceability to Block B9's 8 server-side latency targets and payload budgets. F17 does not alter, redefine, or claim ownership over these server budgets:

1. **Canonical Entity Detail**: Server $p95 \le 50\text{ ms}$, payload $\le 100\text{ KB}$
2. **Autocomplete Suggestions**: Server $p95 \le 30\text{ ms}$, payload $\le 20\text{ KB}$
3. **Global Multi-Entity Search**: Server $p95 \le 100\text{ ms}$, payload $\le 150\text{ KB}$
4. **Timeline Slice / Chronology**: Server $p95 \le 75\text{ ms}$, payload $\le 250\text{ KB}$
5. **Map Spatial View (Bounding Box)**: Server $p95 \le 75\text{ ms}$, payload $\le 200\text{ KB}$
6. **War Day Tactical Occurrences**: Server $p95 \le 75\text{ ms}$, payload $\le 250\text{ KB}$
7. **Global Focus Subgraph ($D_1$)**: Server $p95 \le 100\text{ ms}$, payload $\le 300\text{ KB}$
8. **Global Focus Subgraph ($D_2$)**: Server $p95 \le 200\text{ ms}$, payload $\le 500\text{ KB}$

**Backend Safeguards**:
- Statement timeout: $1000\text{ ms}$ global read; $500\text{ ms}$ heavy graph traversal.
- Focus $D_2$ automatic degradation: Fallback to $D_1$ on $500\text{ ms}$ timeout; `504 Gateway Timeout` only if $D_1$ also fails.
- Rate limits: $300\text{ req/min}$ (navigation/entity), $120\text{ req/min}$ (search), $60\text{ req/min}$ (Focus $D_2$), $30\text{ req/min}$ (admin).

---

## 4. Frontend Testing Pyramid & Verification Levels

The frontend testing architecture is organized into a coherent multi-tiered pyramid. Each tier targets distinct failure modes, ensuring rapid developer feedback without relying excessively on heavyweight end-to-end tests:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        FRONTEND TESTING PYRAMID                        │
├────────────────────────────────────────────────────────────────────────┤
│                                 / \                                    │
│                                /   \                                   │
│                               / E2E \  ◄── System Journeys, Smoke      │
│                              /───────\                                 │
│                             /  Route  \  ◄── Deep Links, History, 404  │
│                            /───────────\                               │
│                           / Integration \  ◄── Query + State + Cache   │
│                          /───────────────\                             │
│                         /   Visual / A11y \  ◄── Companions, WCAG, F4  │
│                        /───────────────────\                           │
│                       /      Component      \  ◄── Facets, Pills, HUD  │
│                      /───────────────────────\                         │
│                     /          Unit           \  ◄── Canonical Models, │
│                    /───────────────────────────\     Slug, Parsing     │
└────────────────────────────────────────────────────────────────────────┘
```

### 4.1 Tier Responsibilities & Scope

```
┌────────────────────────────────────────────────────────────────────────┐
│                      TESTING TIER RESPONSIBILITY                       │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Testing Tier     │ Architectural Scope & Target Invariants             │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **1. Unit Tests**│ Pure functions, URL serializers/deserializers (F7), │
│                  │ canonical slug normalizers, epistemic state checks  │
│                  │ (B2/F9), date/sequence index mappers (F12), helper  │
│                  │ utilities, and DTO-to-ViewModel transformers.       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **2. Component   │ Isolated UI component rendering, accessible naming, │
│    Tests**       │ slot projection, badge states, empty card fallbacks,│
│                  │ citation pills (F15), and focus ring visibility.    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **3. Visual &    │ Semantic companion parity (F16), WCAG 2.2 AA rule   │
│    Accessibility │ audits, keyboard focus containment/restoration,     │
│    Tests**       │ reduced-motion toggling, and F4 reflow behavior.    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **4. Integration │ Query hook lifecycles (F6), cache deduplication,    │
│    Tests**       │ cache revalidation flows, URL state syncing (F5),   │
│                  │ and multi-component state coordination.             │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **5. Route &     │ Canonical URL grammar execution (F7), deep-link     │
│    Navigation    │ hydration, path-identity 404 handling, query        │
│    Tests**       │ parameter normalization, and history back/forward.  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **6. End-to-End  │ Critical user journeys across lenses (Search ──►    │
│    (E2E) Tests** │ Character Profile ──► Graph Focus ──► Provenance).  │
│                  │ Verifies full browser rendering and shell mounting. │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 5. Frontend Performance Architecture

### 5.1 Separation of Performance Ownership
To avoid blurring system responsibilities, performance metrics are strictly categorized across architectural boundaries:

```
┌────────────────────────────────────────────────────────────────────────┐
│                     PERFORMANCE ATTRIBUTION MODEL                      │
├──────────────────┬──────────────────┬──────────────────────────────────┤
│ Metric Category  │ Governing Block  │ Boundary Definition              │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Backend Latency│ Block B9         │ Server processing time ($p95$).  │
│   & Payload**    │                  │ Network transmission to client.  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Frontend Fetch-│ Block F17        │ Time from network response start │
│   to-Render**    │ (Architecture)   │ to DOM paint / visual readiness. │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Interaction    │ Block F17        │ Input latency from user gesture  │
│   Responsiveness │ (Architecture)   │ to state update and frame paint. │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **End-to-End User│ Stage 5          │ Cumulative time from user click  │
│   Perceived Time**│ (Empirical)     │ to interactive rendered content. │
└──────────────────┴──────────────────┴──────────────────────────────────┘
```

### 5.2 Performance Budget Categories
F17 establishes three distinct budget classes:
- **Class A: Architectural Invariants (Fixed in Architecture)**: Structural rules that do not depend on hardware speed (e.g., $D \le 2$ graph depth limits, B5 offset/limit bounding).
- **Class B: Implementation-Dependent Baselines (Stage 4)**: Bundle footprints, virtualization activation thresholds, and component render profiles established and baselined during Stage 4 implementation.
- **Class C: Empirical Acceptance Budgets (Stage 5)**: Core Web Vitals thresholds (LCP, INP, CLS) and real-device frame stability evaluated against production hardware under Stage 5 validation.

### 5.3 Diagnostic Attribution Model
When performance degrades during testing, the frontend testing harness must isolate the root bottleneck using standard browser Performance APIs (`PerformanceObserver`, Navigation Timing, Resource Timing):

$$\text{Total User Latency} = T_{\text{DNS+TCP}} + T_{\text{TTFB}} (\text{B9 Backend}) + T_{\text{Download}} + T_{\text{Parse+Eval}} + T_{\text{Render}}$$

The test architecture must distinguish:
1. **Slow Backend**: $T_{\text{TTFB}}$ exceeds B9 budget $\rightarrow$ Backend/database defect.
2. **Slow Network**: $T_{\text{Download}}$ high for small payload $\rightarrow$ Environmental/network constraint.
3. **Client Processing Bottleneck**: $T_{\text{Parse+Eval}}$ high $\rightarrow$ JavaScript execution overhead or un-split bundle.
4. **Client Rendering Bottleneck**: $T_{\text{Render}}$ high $\rightarrow$ Excessive DOM depth, non-virtualized lists, or costly re-renders.

---

## 6. Bundle & Code-Splitting Validation

In alignment with Technology & Build Architecture (Block F2 §6):

### 6.1 Chunk Isolation Boundaries
The build architecture enforces strict chunk isolation to prevent initial load bloat:
1. **Core Shell Chunk**: Contains the application shell, layout frames, routing core, base theme tokens, and global search trigger.
2. **Lens Route Chunks**: Each primary exploration lens (Character Profile, Timeline, War Explorer) is packaged in an isolated lazy chunk loaded on demand.
3. **Heavy Visualization Chunks**: Heavy visualization engines are segregated into dedicated dynamic chunks, ensuring text/profile exploration does not eagerly load visualization dependencies.

### 6.2 Automated Bundle Regression Validation
Stage 4 build pipelines must implement automated bundle analysis to verify:
- **Chunk Isolation**: No heavy visualization library is bundled into the core entry chunk.
- **Duplicate Dependency Detection**: Verification that shared dependencies (utilities, date mappers) are deduplicated into common vendor chunks.
- **Accidental Eager Loading**: Automated detection of direct, non-dynamic imports of lazy lens modules.
- **Baseline Regression Gating**: Relative size comparisons against established Stage 4 baselines; unexpected bundle growth triggers CI quality-gate warnings.

---

## 7. Accessibility Verification Architecture

In alignment with Accessibility Architecture (Block F16):

### 7.1 Normative Verification Baseline
The frontend testing architecture verifies compliance against **WCAG 2.2 Level AA**. F17 defines the verification criteria; product compliance is evaluated empirically in Stage 5.

### 7.2 Verification Facets & Automated Audits
1. **Automated Accessibility Scans**:
   - Automated rule scanners integrated into component and E2E test runs to detect missing accessible names, invalid ARIA roles, color contrast failures, and duplicate IDs.
2. **Keyboard Operability Verification**:
   - Automated testing verifies that every interactive control, interactive lens navigation/control, search suggestion, and citation pill can be reached via `Tab` / `Shift+Tab`.
   - Interactive controls must be operable using the keyboard interaction appropriate to their semantic control type, consistent with F16.
   - Verification that no keyboard traps exist across any application view or modal overlay.
3. **Focus Management & Containment**:
   - Tests verify that modal dialogs and other explicitly modal overlays must contain focus according to F16; non-modal contextual surfaces must preserve appropriate keyboard navigation and focus continuity.
   - Tests verify that dismissing an overlay returns focus deterministically to the invoking trigger element.
   - Tests verify that client-side route navigation moves focus to the primary content heading or landmark container without stranded focus.
4. **Dual-Presentation Parity Verification**:
   - Verification that for every graphical visualizer (F10–F14), an accessible semantic companion exists in the DOM.
   - Tests verify that the accessible companion derives from the exact same canonical query payload as the graphical view.
5. **Reduced-Motion Verification**:
   - Tests verify that when `prefers-reduced-motion` is simulated, non-essential motion, transitions, and animations are suppressed or effectively eliminated, consistent with Block F16.

---

## 8. Routing, Navigation & Deep-Link Validation

In alignment with Routing & Navigation Architecture (Block F7):

### 8.1 Path Identity vs. Query Modifier Invariants
The testing architecture strictly enforces the F7 routing contract:
1. **Path Identity Invariant**:
   - Path segments represent immutable resource identity (`/characters/:slug`, `/wars/:war_slug/day/:day`).
   - Invalid path slugs must **never** be silently clamped or redirected to a random entity; they must resolve deterministically to standard `404 Not Found` views.
2. **Query Modifier Invariant**:
   - Query parameters represent exploration modifiers (`?d=2`, `?q=search_term`, `?rel=kinship`, `?offset=20`).
   - Bounded parameters are validated: $d > 2$ is clamped to $d=2$; unsupported query parameters are pruned deterministically.
3. **Canonical Normalization**:
   - Slugs must be strictly lowercase kebab-case.
   - Default query parameters (e.g., `d=1`, `offset=0`) are omitted from serialized URLs.
   - Query keys are serialized in deterministic alphabetical order to ensure URL uniqueness.

### 8.2 Deep-Link Reproducibility
Automated tests verify that navigating directly to any canonical deep link hydrates the exact exploration view, entity focus, active facet, and backing provenance state without requiring prior navigation history.

---

## 9. Data Fetching, Caching & State Validation

In alignment with Blocks F5 and F6:

### 9.1 Server State Lifecycle Verification
Tests verify client handling across all query lifecycle states (Block F6 §5):
- **`loading`**: Loading skeletons preserve intended layout structure without avoidable reflow; user cannot trigger duplicate simultaneous queries.
- **`success` / `ready`**: Canonical data renders accurately in accordance with view models.
- **`empty`**: Valid payload with zero results renders clean domain-specific empty states.
- **`error`**: RFC 7807 problem details render clean user-facing error notices with retry actions.
- **Revalidation & Freshness**: Per Block F6, background cache revalidation updates the UI smoothly without clearing active content or degrading interactive responsiveness.

### 9.2 Request Deduplication & Race Condition Guards
1. **Deduplication Tests**: Verify that multiple identical components requesting the same canonical entity emit only one physical HTTP request.
2. **Obsolete-Request Protection**: Verify that when a user rapidly changes exploration focus ($A \rightarrow B \rightarrow C$), an earlier delayed response for $A$ cannot overwrite the active view for $C$.
3. **Cache Key Determinism**: Verify that query keys strictly mirror canonical URL parameters and resource identifiers per F6 §4.

---

## 10. Visualization Validation Architecture

In alignment with Exploration Lens Blocks F10 through F14 and Accessibility Block F16:

### 10.1 Multi-Lens Verification Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                   VISUALIZATION VERIFICATION MATRIX                    │
├──────────────────┬──────────────────┬──────────────────────────────────┤
│ Exploration Lens │ Governing Block  │ Core Verification Objectives     │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Relationship   │ Block F10        │ Traversal bounded at D≤2; node   │
│   Graph**        │                  │ and edge counts match canonical  │
│                  │                  │ payload; symmetric edges align.  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Lineage /      │ Block F11        │ Generational flow derived from   │
│   Family Tree**  │                  │ parent-child data; zero invented │
│                  │                  │ intermediate generational nodes. │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Timeline /     │ Block F12        │ Events ordered by sequence_index;│
│   Chronology**   │                  │ narrative progression preserved; │
│                  │                  │ zero fabricated calendar dates.  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Geographic     │ Block F13        │ Canonical coordinate_status      │
│   Map**          │                  │ respected; unmapped locations    │
│                  │                  │ rendered in non-spatial directory│
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **War & Tactical │ Block F14        │ 3-tier fidelity respected; zero  │
│   Vyuhas**       │                  │ fabricated troop numbers, army   │
│                  │                  │ sizes, or metric battle geometry.│
└──────────────────┴──────────────────┴──────────────────────────────────┘
```

### 10.2 Semantic Companion Equivalence Testing
Automated tests verify that for every visual projection:
- The accessible semantic companion exposes the exact same entities, relationships, events, and citations present in the visual view.
- Companion elements provide identical interactive affordances (focusing nodes, opening profiles, inspecting evidence).
- Zero secondary or divergent data sources are consumed by either view.

---

## 11. Data Integrity, Epistemic States & Provenance Validation

### 11.1 The Six Epistemic States Invariant
The frontend consumes the exact six project-wide epistemic states defined in Block B2 §6 and B4 §6:
1. `known`
2. `conflicting`
3. `approximate`
4. `unknown`
5. `not_researched`
6. `not_applicable`

**Strict Test Invariants**:
- The frontend must **never** add a seventh project-wide state.
- Automated tests verify that UI rendering does not mutate epistemic states:
  - `conflicting` must never be rendered as undisputed fact.
  - `approximate` must never be displayed as exact.
  - `unknown` must never be hidden or reported as non-existent.
  - `not_researched` must never be displayed as false.
- Categorical separation between `epistemic_status` (truth state), `certainty` (canonical certainty rating), and `provenance_origin` (B4 classification) must be verified.

### 11.2 Four-Tier Provenance Hierarchy Verification
In alignment with Block F15:
- Tests verify the full provenance trail:
  $$\text{Entity / Edge} \longrightarrow \text{Claim} \longrightarrow \text{Evidence} \longrightarrow \text{Source}$$
- Tests verify that native source locators (e.g., chapter/verse markers) are preserved exactly as supplied by the backend without synthetic normalization.
- Competing claims must remain distinguishable and must not be silently merged into an invented consensus.

---

## 12. Zero-Fabrication Regression Guards

Constitutional Rule 04 mandates zero fabrication across all surfaces:
> *"Never fabricate: facts, dates, coordinates, relationships, source claims, certainty."*

### 12.1 Regression Guard Architecture
F17 establishes automated regression guards to prevent accidental introduction of fabricated data:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   ZERO-FABRICATION REGRESSION GUARDS                   │
├───────────────────────────────────┬────────────────────────────────────┤
│ REGRESSION GUARD TARGET           │ VERIFICATION MECHANISM             │
├───────────────────────────────────┼────────────────────────────────────┤
│ 1. Missing Data Presentation      │ Verify that missing attributes     │
│    (Illustrative)                 │ render neutrally without inferred  │
│                                   │ rationales or synthetic claims.    │
├───────────────────────────────────┼────────────────────────────────────┤
│ 2. Unrecorded Kinship Links       │ Verify that unrecorded lineage ties│
│    (Illustrative)                 │ do not synthesize phantom nodes.   │
├───────────────────────────────────┼────────────────────────────────────┤
│ 3. Tactical Metrics               │ Verify that battle formations omit │
│    (Illustrative)                 │ troop counts and exact metrics     │
│                                   │ unless canonical data provides it. │
├───────────────────────────────────┼────────────────────────────────────┤
│ 4. Unmapped Geographic Sites      │ Verify that unmapped sites are not │
│    (Illustrative)                 │ assigned speculative coordinates.  │
├───────────────────────────────────┼────────────────────────────────────┤
│ 5. Test Fixture Isolation         │ Synthetic software-test fixtures   │
│                                   │ must be explicitly isolated from   │
│                                   │ canonical/production content paths │
│                                   │ and must never be serialized,      │
│                                   │ published, or rendered as          │
│                                   │ authoritative project data.        │
└───────────────────────────────────┴────────────────────────────────────┘
```

---

## 13. Responsive & Reflow Validation Architecture

In alignment with Layout & Application Shell Architecture (Block F4):

### 13.1 Space-Based Viewport Testing
Rather than testing arbitrary device models, tests validate behavioral recomposition across the four F4 space-based viewport classes:
- **Compact**: Bottom navigation rail; slide-overs reflow to bottom sheets; single-column facet stacking.
- **Medium**: Collapsible sidebar navigation; modal contextual panels; two-column adaptive grid.
- **Expanded**: Persistent sidebar navigation; docked side-by-side inspection drawer; multi-column workspace.
- **Wide**: Expanded multi-pane studio layout; persistent provenance inspector.

*(Note: In accordance with Block F4 §4, viewport classes define behavioral layout composition rather than rigid pixel breakpoint thresholds; quantitative CSS breakpoint values are selected and verified during Stage 4 implementation).*

### 13.2 Reflow & Scalability Invariants
Automated visual tests verify:
- **400% Zoom Reflow**: At 400% zoom / equivalent narrow viewport conditions (evaluated at 320 CSS pixels width), content remains usable without loss of information or functionality and without avoidable two-dimensional scrolling.
- **200% Text Zoom**: Typography scales without clipping, overlapping, or truncated text containers.
- **Gesture Parity**: Single-pointer alternatives exist for all touch-drag or pinch-zoom interactions.

---

## 14. Security & Content-Safety Validation

In alignment with Backend Security Architecture (Block B7) and Media Architecture (Block B8):

### 14.1 Frontend Security Boundaries
1. **Untrusted URL Defense**: All query parameters and route path slugs are validated against parameter-specific constraints governed by Blocks F7 and F8 before state consumption. Search query inputs (`q`) permit bounded Unicode and IAST characters, while path slugs and bounded modifiers are validated against their defined canonical vocabularies.
2. **Plain-Text Escaping**: Sanskrit excerpts, translated passages, and user query strings rendered into DOM or ARIA labels are treated strictly as text, preventing Cross-Site Scripting (XSS).
3. **SVG & Media Sanitization**: Tests verify that rendered external SVG and visual media conform to the safe-asset and XML sanitization requirements established by Block B8 §6.2.
4. **Public Read-Only Boundary**: Tests verify that the public client provides no mutation controls and does not issue mutation requests; backend mutation behavior remains governed by Block B7.

---

## 15. Error, Empty, Loading & Partial-Data Validation

In alignment with Blocks F6 and F9:

### 15.1 Viewport State Verification Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                   VIEWPORT STATE VERIFICATION MATRIX                   │
├──────────────────┬─────────────────────────────────────────────────────┤
│ State Type       │ Required Verification Behavior                      │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **1. Loading**   │ Accessible status announcement; loading placeholders│
│                  │ preserve intended layout structure and avoid        │
│                  │ avoidable content reflow.                           │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **2. Ready**     │ Complete canonical view model rendered accurately.  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **3. Not Found   │ Clear 404 message; recovery links to exploration    │
│    (404)**       │ lenses; no unhandled routing exceptions.            │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **4. Network     │ Offline / timeout message; manual retry trigger;    │
│    Failure**     │ cached data preserved if available per F6 caching.  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **5. Rate Limit  │ Respects `429 Too Many Requests`; renders cooldown  │
│    (429)**       │ indicator per `Retry-After` header.                 │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **6. Partial     │ Renders available attributes. Absent information    │
│    Data**        │ must be rendered neutrally unless a canonical       │
│                  │ epistemic status is supplied by the authoritative   │
│                  │ data contract. The frontend must not infer an       │
│                  │ epistemic status solely from absence.               │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 16. Browser & Platform Validation Architecture

### 16.1 Supported Platform Baseline
The validation architecture establishes baseline target platform families for verification:
- **Chromium Family**: Google Chrome, Microsoft Edge (Desktop & Mobile).
- **Gecko Family**: Mozilla Firefox (Desktop).
- **WebKit Family**: Apple Safari (macOS & iOS).

### 16.2 Assistive Technology Verification Coverage
Screen-reader interoperability verification covers the primary platform combinations:
- NVDA with Firefox / Chrome (Windows).
- JAWS with Chrome / Edge (Windows).
- VoiceOver with Safari (macOS & iOS).
- TalkBack with Chrome (Android).

*(Note on Lifecycle Boundaries: Platform families and assistive technology categories are architecturally defined in Stage 2. Concrete browser and screen-reader versions and specific test-runner tooling are selected in Stage 4 test harnesses. Empirical cross-browser compatibility and assistive technology certification occur exclusively in Stage 5).*

---

## 17. CI / Quality-Gate Architecture

F17 specifies a staged, 10-gate quality pipeline required to validate release readiness:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        STAGED QUALITY GATES                            │
├──────┬────────────────────────┬────────────────────────────────────────┤
│ Gate │ Quality Gate Domain    │ Blocking Failure Criteria              │
├──────┼────────────────────────┼────────────────────────────────────────┤
│ **1**│ Static & Type Checking │ Any TypeScript compile or lint error.  │
├──────┼────────────────────────┼────────────────────────────────────────┤
│ **2**│ Unit Test Suite        │ Any unit test failure; zero coverage   │
│      │                        │ regressions on core normalizers.       │
├──────┼────────────────────────┼────────────────────────────────────────┤
│ **3**│ Component Test Suite   │ Any component rendering failure.       │
├──────┼────────────────────────┼────────────────────────────────────────┤
│ **4**│ Integration Suite      │ Any query lifecycle or cache failure.  │
├──────┼────────────────────────┼────────────────────────────────────────┤
│ **5**│ Routing & Deep-Link    │ Any route grammar or 404 failure.      │
├──────┼────────────────────────┼────────────────────────────────────────┤
│ **6**│ Visualization Parity   │ Any mismatch between visual and        │
│      │                        │ accessible companion datasets.         │
├──────┼────────────────────────┼────────────────────────────────────────┤
│ **7**│ Accessibility Audit    │ Any automated WCAG 2.2 Level AA rule   │
│      │                        │ violation or missing accessible name.  │
├──────┼────────────────────────┼────────────────────────────────────────┤
│ **8**│ Zero-Fabrication Guard │ Any synthesized fact, unrecorded       │
│      │                        │ parentage, or inferred missing cause.  │
├──────┼────────────────────────┼────────────────────────────────────────┤
│ **9**│ Security & Content     │ Any unsanitized SVG, XSS pattern, or   │
│      │                        │ accidental mutation trigger.           │
├──────┼────────────────────────┼────────────────────────────────────────┤
│**10**│ Empirical Validation   │ Release certification conducted in     │
│      │ (Stage 5)              │ Stage 5 across real hardware/browsers. │
└──────┴────────────────────────┴────────────────────────────────────────┘
```

- **Blocking Policy**: Gates 1 through 9 are **hard automated blockers** in CI. Any failure rejects the build.
- **Non-Blocking Warnings**: Minor bundle growth warnings or non-critical styling variations produce advisory notices reviewed before release.
- **Deferred Empirical Gating**: Gate 10 is executed exclusively during Stage 5.

---

## 18. End-to-End Traceability Matrix

Block F17 maintains complete requirement traceability across the entire engineering pipeline:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   END-TO-END TRACEABILITY PIPELINE                     │
├────────────────────────────────────────────────────────────────────────┤
│ STAGE 1 (B1–B13) ──► STAGE 2 (F1–F16) ──► BLOCK F17 ──► STAGE 4 ──► STAGE 5│
│ Backend API/Data    Frontend Architecture Verification  Implementation Empirical │
└────────────────────────────────────────────────────────────────────────┘
```

```
┌────────────────────────────────────────────────────────────────────────┐
│                   TRACEABLE ARCHITECTURAL INVARIANTS                   │
├──────────────────┬──────────────────┬──────────────────────────────────┤
│ Architectural    │ Upstream Source  │ Block F17 Verification Mapping   │
│ Invariant        │ Contract         │                                  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Zero           │ Rule 04; B4 §1;  │ Section 12: Automated regression │
│   Fabrication**  │ F1 §5; F16 §27   │ guards against synthetic facts.  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Six Epistemic  │ B2 §6; B4 §6;    │ Section 11: Validation of exact  │
│   States**       │ F9 §4; F16 §11   │ six states without UI mutation.  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Four-Tier      │ B4 §2; F15 §4;   │ Section 11: End-to-end provenance│
│   Provenance**   │ F16 §14          │ chain verification in DOM.       │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Bounded Focus  │ B3 §6; B9 §2;    │ Section 8 & 10: Strict D≤2 route │
│   Traversal**    │ F7 §2; F10 §3    │ clamping and companion parity.   │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Offset         │ B5 §6; F6 §6;    │ Section 8: Public offset/limit   │
│   Pagination**   │ F7 §2            │ contract validation.             │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **WCAG 2.2 AA    │ Rule 10; PRD §9; │ Section 7: Automated audits,     │
│   Accessibility**│ F16 §2           │ keyboard, focus, companion tests.│
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Space-Based    │ F4 §4; F16 §24   │ Section 13: Recomposition tests  │
│   Reflow**       │                  │ across Compact/Med/Exp/Wide.     │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **B9 Performance │ B9 §2            │ Section 3 & 5: Preservation of   │
│   Preservation** │                  │ server latency & payload targets.│
└──────────────────┴──────────────────┴──────────────────────────────────┘
```

---

## 19. Comprehensive Ownership Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                        SYSTEM OWNERSHIP MATRIX                         │
├──────────────────────────┬──────────────────┬──────────────────────────┤
│ Architectural Domain     │ Primary Owner    │ Governing Document       │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ Backend / API Systems    │ Stage 1 (B1–B13) │ docs/04-backend/*        │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ Frontend Constitution    │ Block F1         │ 00-frontend-context.md   │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ Platform & Build Arch.   │ Block F2         │ 01-technology-stack.md   │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ Design System & Tokens   │ Block F3         │ 02-design-system.md      │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ Shell & Responsive Arch. │ Block F4         │ 03-responsive-layout.md  │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ State Management Arch.   │ Block F5         │ 04-state-management.md   │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ Data Fetching & Cache    │ Block F6         │ 05-api-client-caching.md │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ Routing & Deep-Linking   │ Block F7         │ 06-routing-navigation.md │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ Search Information Arch. │ Block F8         │ 07-search-autocomplete.md│
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ Entity Views IA          │ Block F9         │ 08-entity-views.md       │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ Graph Visualization Arch.│ Block F10        │ 09-graph-visualization.md│
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ Lineage DAG Arch.        │ Block F11        │ 10-lineage-family-tree.md│
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ Timeline Chronology Arch.│ Block F12        │ 11-timeline-chronology.md│
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ Geographic Map Arch.     │ Block F13        │ 12-geographic-map.md     │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ War & Tactical Vyuha Arch│ Block F14        │ 13-war-tactical-vyuha.md │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ Evidence & Provenance IA │ Block F15        │ 14-evidence-provenance.md│
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ Accessibility Arch.      │ Block F16        │ 15-accessibility-arch.md │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ **Testing & Validation** │ **Block F17**    │ **16-testing-perf-arch** │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ Implementation Phase     │ Stage 4          │ Application source code  │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ Empirical Validation     │ Stage 5          │ Empirical test reports   │
└──────────────────────────┴──────────────────┴──────────────────────────┘
```

---

## 20. Architecture vs. Empirical Verification

To maintain strict epistemological clarity:

```
┌────────────────────────────────────────────────────────────────────────┐
│               ARCHITECTURE VS. EMPIRICAL VERIFICATION                  │
├───────────────────────────────────┬────────────────────────────────────┤
│ WHAT ARCHITECTURE DEFINES (STAGE 2│ WHAT EMPIRICAL TESTING VERIFIES    │
│ & BLOCK F17)                      │ (STAGE 5)                          │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Testing strategies and levels.  │ - Measured runtime frame rates.    │
│ - Quality-gate rules and failure  │ - Empirical bundle sizes in KB.    │
│   conditions.                     │ - Measured real-device Core Web    │
│ - Invariants that must be proven. │   Vitals (LCP, INP, CLS).          │
│ - Target conformance standards    │ - Certified screen-reader audits   │
│   (WCAG 2.2 Level AA).            │   with human assistive users.      │
│ - Diagnostic attribution models.  │ - Cross-browser certification      │
│ - Zero-fabrication regression     │   results on physical hardware.    │
│   rules.                          │ - Final product release acceptance.│
└───────────────────────────────────┴────────────────────────────────────┘
```

---

## 21. Architectural Decision Records (ADRs)

### ADR-F17-01: Multi-Tiered Testing Pyramid Over End-to-End Exclusivity
- **Context**: Relying exclusively on end-to-end tests for rich SPAs leads to slow CI runs, flaky test executions, and delayed feedback.
- **Decision**: Adopt a multi-tiered pyramid where unit and component tests validate logic, state, and accessibility rules, reserving E2E tests for critical user journeys and smoke validation.
- **Consequences**: Accelerates developer feedback cycles, ensures comprehensive coverage, and isolates root-cause failures rapidly.

### ADR-F17-02: Diagnostic Performance Attribution Separation
- **Context**: Client applications frequently misattribute backend or network delays as rendering bugs, and vice versa.
- **Decision**: Formalize the diagnostic attribution model separating backend TTFB (governed by B9), network transit, and client fetch-to-render latency.
- **Consequences**: Ensures that frontend optimizations target client bottlenecks without masking backend database performance issues.

### ADR-F17-03: Zero-Fabrication Automated Regression Gates
- **Context**: Well-intentioned UI improvements may inadvertently synthesize placeholder dates, assumed kinship ties, or invented explanations for missing data.
- **Decision**: Establish automated regression tests verifying that missing data renders neutrally, canonical epistemic states are never mutated, and test fixtures remain isolated.
- **Consequences**: Guarantees absolute epistemic honesty and academic credibility across all frontend releases.

### ADR-F17-04: Strict Separation of Architecture from Empirical Measurement
- **Context**: Software architecture documents sometimes falsely claim compliance or benchmark numbers before code has been written.
- **Decision**: Explicitly declare that Block F17 establishes specifications, quality gates, and methodologies; all empirical benchmarking and compliance claims belong to Stage 5.
- **Consequences**: Eliminates false claims of readiness and maintains rigorous engineering integrity.

---

## 22. Traceable Requirements

```
┌────────────────────────────────────────────────────────────────────────┐
│                       TRACEABLE REQUIREMENTS LIST                      │
├───────────────┬──────────────────┬─────────────────────────────────────┤
│ Requirement ID│ Source Document  │ Architectural Description           │
├───────────────┼──────────────────┼─────────────────────────────────────┤
│ **REQ-TST-01**│ Constitutional   │ Multi-tiered testing pyramid        │
│               │ Rule 14; PRD §16 │ covering unit to E2E levels.        │
├───────────────┼──────────────────┼─────────────────────────────────────┤
│ **REQ-TST-02**│ Block B9; F17 §5 │ Frontend performance attribution and│
│               │                  │ diagnostic separation model.        │
├───────────────┼──────────────────┼─────────────────────────────────────┤
│ **REQ-TST-03**│ Block F2 §6      │ Chunk isolation and bundle          │
│               │                  │ regression validation architecture. │
├───────────────┼──────────────────┼─────────────────────────────────────┤
│ **REQ-TST-04**│ Block F16 §2     │ Automated WCAG 2.2 Level AA and     │
│               │                  │ companion parity verification.      │
├───────────────┼──────────────────┼─────────────────────────────────────┤
│ **REQ-TST-05**│ Block F7 §2, §4  │ Routing canonicalization, 404, and  │
│               │                  │ deep-link reproducibility testing.  │
├───────────────┼──────────────────┼─────────────────────────────────────┤
│ **REQ-TST-06**│ Block F5, F6     │ Server state lifecycle and query    │
│               │                  │ deduplication validation.           │
├───────────────┼──────────────────┼─────────────────────────────────────┤
│ **REQ-TST-07**│ Blocks F10–F14   │ Multi-lens visualization validation │
│               │                  │ against canonical data contracts.   │
├───────────────┼──────────────────┼─────────────────────────────────────┤
│ **REQ-TST-08**│ Blocks B2, B4, F15│ 6 epistemic states and 4-tier       │
│               │                  │ provenance hierarchy preservation.  │
├───────────────┼──────────────────┼─────────────────────────────────────┤
│ **REQ-TST-09**│ Constitutional   │ Automated zero-fabrication          │
│               │ Rule 04; B4 §1   │ regression guard suites.            │
├───────────────┼──────────────────┼─────────────────────────────────────┤
│ **REQ-TST-10**│ F17 §17          │ 10-tier quality-gate architecture   │
│               │                  │ enforcing release readiness.        │
└───────────────┴──────────────────┴─────────────────────────────────────┘
```

---

## 23. Stage 2 Closure & Stage 4/5 Handover

With the completion of **Block F17**, **Stage 2 (Frontend Architecture)** reaches full architectural closure:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        STAGE 2 CLOSURE MASTERPLAN                      │
├────────────────────────────────────────────────────────────────────────┤
│ Blocks F1 through F17 constitute the complete, authoritative, and      │
│ reconciled Frontend Architecture for the Mahābhārata Explorer.         │
├────────────────────────────────────────────────────────────────────────┤
│ 1. COMPLETE ARCHITECTURAL SPECIFICATION                                │
│    - All 17 frontend blocks (F1–F17) are fully architected.            │
│    - Zero unresolved architectural contradictions exist across blocks. │
├────────────────────────────────────────────────────────────────────────┤
│ 2. STRICT NON-IMPLEMENTATION INVARIANT                                 │
│    - Stage 2 defines architecture only. No application source code,    │
│      components, CSS files, or package dependencies have been created. │
├────────────────────────────────────────────────────────────────────────┤
│ 3. HANDOVER TO STAGE 4 (IMPLEMENTATION)                                │
│    - Stage 4 will implement application code in strict adherence to   │
│      the Stage 1 (B1–B13) and Stage 2 (F1–F17) specifications.         │
│    - Concrete library versions, CSS classes, test runners, and DOM     │
│      markup wiring are realized during Stage 4.                        │
├────────────────────────────────────────────────────────────────────────┤
│ 4. HANDOVER TO STAGE 5 (EMPIRICAL VALIDATION)                          │
│    - Stage 5 executes the verification test suites, benchmark runs,    │
│      Lighthouse audits, and screen-reader certifications defined in F17│
│    - Final acceptance is granted only upon empirical verification.     │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 24. Acceptance Criteria

```
┌────────────────────────────────────────────────────────────────────────┐
│                      ACCEPTANCE CRITERIA STATUS                        │
├────────────────────────────────────────────────────────────────────────┤
│ Note: Architecture defined below; final acceptance pending review.     │
└────────────────────────────────────────────────────────────────────────┘
```

- [x] Architecture defined; final acceptance pending review: Complete testing pyramid architected covering unit, component, integration, route, visualization, accessibility, and E2E tiers.
- [x] Architecture defined; final acceptance pending review: B9 backend performance targets preserved exactly without frontend ownership confusion.
- [x] Architecture defined; final acceptance pending review: Frontend performance attribution model established separating backend TTFB, network, and client render latency.
- [x] Architecture defined; final acceptance pending review: F2 chunk isolation and bundle regression validation architecture defined.
- [x] Architecture defined; final acceptance pending review: WCAG 2.2 Level AA accessibility verification and semantic companion equivalence testing architected.
- [x] Architecture defined; final acceptance pending review: F7 routing invariants (path identity 404s, query modifier clamping, normalization) validated.
- [x] Architecture defined; final acceptance pending review: F5/F6 server state lifecycle, cache deduplication, and obsolete-request protection validated.
- [x] Architecture defined; final acceptance pending review: F10–F14 visualization validation defined against canonical data contracts.
- [x] Architecture defined; final acceptance pending review: Exact six epistemic states and four-tier provenance hierarchy verified without UI mutation.
- [x] Architecture defined; final acceptance pending review: Automated zero-fabrication regression guards established against synthetic facts.
- [x] Architecture defined; final acceptance pending review: Responsive reflow validation architected across F4 Compact, Medium, Expanded, and Wide classes.
- [x] Architecture defined; final acceptance pending review: Frontend security, data escaping, and public read-only boundary validation defined.
- [x] Architecture defined; final acceptance pending review: 10-tier quality-gate architecture defined with clear blocking vs. advisory policies.
- [x] Architecture defined; final acceptance pending review: Supported browser families and assistive technology testing coverage specified.
- [x] Architecture defined; final acceptance pending review: Complete traceability matrix connecting B1–B13, F1–F16, F17, Stage 4, and Stage 5.
- [x] Architecture defined; final acceptance pending review: Stage 2 closure statement established with formal handover criteria.
- [x] Architecture defined; final acceptance pending review: Zero application source code, UI packages, or concrete component libraries introduced.
- [x] Architecture defined; final acceptance pending review: Zero Stage 1 backend documents or Blocks F1–F16 documents modified.
