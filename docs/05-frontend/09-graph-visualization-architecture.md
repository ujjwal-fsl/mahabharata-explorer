# Graph Visualization Architecture (Block F10)

## 1. Document Status & Purpose

- **Document Identifier**: Block F10
- **Status**: Approved Architectural Specification
- **Stage**: Stage 2 — Frontend Architecture
- **Location**: `docs/05-frontend/09-graph-visualization-architecture.md`
- **Upstream Dependencies**:
  - `docs/04-backend/02-data-architecture.md` (Stage 1 Block B2)
  - `docs/04-backend/03-knowledge-graph.md` (Stage 1 Block B3)
  - `docs/04-backend/04-evidence-and-provenance.md` (Stage 1 Block B4)
  - `docs/04-backend/05-api-architecture.md` (Stage 1 Block B5)
  - `docs/04-backend/09-performance-and-caching.md` (Stage 1 Block B9)
  - `docs/05-frontend/00-frontend-architecture-context.md` (Block F1)
  - `docs/05-frontend/01-technology-stack-and-build-architecture.md` (Block F2)
  - `docs/05-frontend/02-design-system-and-visual-grammar.md` (Block F3)
  - `docs/05-frontend/03-responsive-layout-and-application-shell-architecture.md` (Block F4)
  - `docs/05-frontend/04-state-management-and-client-side-data-architecture.md` (Block F5)
  - `docs/05-frontend/05-api-client-data-fetching-and-caching-architecture.md` (Block F6)
  - `docs/05-frontend/06-routing-navigation-and-deep-linking-architecture.md` (Block F7)
  - `docs/05-frontend/07-global-search-and-autocomplete-ux-architecture.md` (Block F8)
  - `docs/05-frontend/08-character-and-entity-views-architecture.md` (Block F9)
- **Downstream Consumers**:
  - `Block F11`: Family Lineage Tree Architecture
  - `Block F12`: Chronology & Timeline Architecture
  - `Block F13`: Geographic Map Architecture
  - `Block F14`: War & Tactical Vyuha Architecture
  - `Block F15`: Evidence, Citation & Provenance UX Architecture
  - `Block F16`: Internationalization, Typography & Accessibility Architecture
  - `Block F17`: Performance Budgets, Verification & Testing Architecture
  - `Stage 4`: Frontend Application Implementation

```
┌────────────────────────────────────────────────────────────────────────┐
│                   STAGE 2 ARCHITECTURE DEPENDENCY MAP                  │
├───────────────────────────────────┬────────────────────────────────────┤
│ UPSTREAM PREREQUISITES            │ CURRENT BLOCK & DOWNSTREAM SCOPE   │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Block B3: Knowledge Graph Model │ ► BLOCK F10: GRAPH VISUALIZATION   │
│ - Block B5: API Graph Endpoints   │   - F11: Family Lineage Tree       │
│ - Block F3: Visual Grammar        │   - F12: Timeline & Chronology     │
│ - Block F4: Viewport Classes      │   - F13: Geographic Map Lens       │
│ - Block F5: State Hierarchy       │   - F14: War & Tactical Vyuhas     │
│ - Block F6: API Fetching/Caching  │   - F15: Evidence Drawer UX        │
│ - Block F7: URL Grammar & Routing │   - F16: A11y & Typography Engine  │
│ - Block F8: Search Continuity     │   - F17: Testing & Budgets         │
│ - Block F9: Entity Views Launch   │                                    │
└───────────────────────────────────┴────────────────────────────────────┘
```

This document defines the complete frontend **architecture** for the Mahābhārata Explorer's interactive relationship graph lens (`/graph`, `/graph/:slug`). The graph is an exploratory visualization lens over the canonical backend knowledge graph established by Stage 1. It provides an intuitive, high-readability topological perspective on entity connections while adhering to the core project constitution: One Knowledge Graph, Modern First Visuals, Epistemic Rigor, Accessible Fallbacks, and Zero Data Fabrication.

---

## 2. Architectural Context & Foundational Principles

### 2.1 Pure Architectural Specification & Library Neutrality
Block F10 is an architectural specification only. It defines projection models, node/edge semantic mappings, interaction lifecycles, viewport responsiveness, accessible fallbacks, and rendering boundaries. It does **not**:
- Implement React components, hooks, or custom web components.
- Introduce concrete third-party graph rendering libraries or packages.
- Create application source code, package manifests, or bundle configurations.
- Introduce direct database queries, SQL CTEs, or backend schema mutations.

### 2.2 Foundational Principles
1. **One Graph, Many Lenses (Constitutional Principle 2, REQ-CORE-01)**: The interactive graph is a dynamic visualization projection of the authoritative backend knowledge graph (Block B3). The frontend does not create, maintain, or persist an independent graph database or duplicate relationship taxonomy.
2. **Zero Fabrication (Constitutional Principle 4 & 13, REQ-GRP-05)**: The graph never infers, synthesizes, or predicts relationships based on layout proximity, narrative co-occurrence, or node clustering. Every rendered edge maps strictly to an authoritative, claim-backed backend record. Missing relationships remain unrendered; uncertain relationships carry explicit epistemic visual badges.
3. **Bounded Focus Exploration ($D \le 2$)**: To maintain cognitive legibility, visual hierarchy, and payload efficiency, graph exploration is anchored to a focus entity and bounded to a maximum depth of two hops ($D \le 2$). Unconstrained global multi-thousand-node physics simulations are strictly prohibited.
4. **Stable & Deterministic Projection**: Given identical backend API data and filter states, the graph visual layout must resolve deterministically, avoiding disorienting node reshuffling, canvas jitter, or erratic force oscillations upon interaction.
5. **Accessible Equivalent Representation**: The graph experience must provide equivalent access to graph information and actions through an accessible semantic representation for users relying on screen readers, keyboard navigation, or reduced motion. Block F16 authoritatively governs concrete accessibility implementation, while Block F17 owns verification.

---

## 3. Scope of Block F10

### 3.1 In-Scope
- Purpose, UX role, and information architecture of the Graph Lens (`/graph`, `/graph/:slug`).
- Conceptual node and edge model projecting canonical B2/B3 entities and relationships into visual elements.
- Focus graph exploration model: focal node ($D_0$), first-degree neighbors ($D_1$), and bounded second-degree neighbors ($D_2$).
- Focus transition semantics: focal replacement, exploration expansion, and history navigation.
- Relationship-type presentation, directionality conventions, and edge multiplicity handling.
- Visual hierarchy, typography, and epistemic state presentation across nodes and edges.
- Multi-modal interaction architecture: pointer, touch, keyboard, and accessibility controls.
- Graph filtering, visual density management, and decluttering rules without data mutation.
- Layout architecture: conceptual placement models, stability invariants, and collision mitigation.
- Responsive adaptation across the four Block F4 viewport classes (Compact, Medium, Expanded, Wide).
- Semantic accessible representation and screen reader interaction tree.
- Evidence drawer trigger boundaries connecting graph elements to Block F15.
- State ownership distribution across URL (F7), Server Cache (F6), Lens State (F10), and UI State (F5).
- Performance boundaries: rendering class classification (DOM vs. SVG vs. Canvas/WebGL), frame rate goals, resource lifecycle, and payload budgets.
- Security and untrusted data handling for text labels, attributes, and URLs.

### 3.2 Out-of-Scope (Non-Goals)
- Implementing concrete rendering algorithms, physics calculations, or selecting concrete npm packages (deferred to Stage 4).
- Genealogical tree traversal, generational row layouts, and deep multi-generational lineage rendering (owned by Block F11: Family Lineage Tree).
- Chronological timeline tracks, Parva scrubbers, and narrative event sequencing (owned by Block F12: Timeline & Chronology).
- Geographic coordinate mapping, GIS tile rendering, and cartographic projections (owned by Block F13: Geographic Map).
- Battlefield formation geometry, tactical troop arrays, and daily combat movements (owned by Block F14: War & Tactical Vyuhas).
- Detailed textual citation rendering, Critical Edition verse viewers, and claim inspection drawers (owned by Block F15: Evidence Drawer).
- System-wide contrast token math, global font-loading mechanics, and ARIA primitives (owned by Block F16).
- Setting formal frontend bundle-size budgets or automated visual regression test runners (owned by Block F17).
- Offline client storage engines or native graph export formats are outside the scope of F10.

---

## 4. Graph Role in the Explorer Ecosystem

The Graph Lens serves as the primary topological exploration vehicle within the Mahābhārata Explorer. While entity profiles (Block F9) offer deep vertical dives into biographical facts, the graph reveals the intricate web of personal, political, pedagogical, and martial connections across the epic:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   GRAPH LENS ROLE & CONTEXTUAL BRIDGES                 │
├───────────────────────────────────┬────────────────────────────────────┤
│ User Mental Model                 │ Exploration Objective             │
├───────────────────────────────────┼────────────────────────────────────┤
│ "How are these two figures        │ Inter-entity relational path &     │
│  connected?"                      │ intermediary alliances or kin.     │
├───────────────────────────────────┼────────────────────────────────────┤
│ "What is the social circle of     │ 1-hop and 2-hop radius around a    │
│  this warrior or sage?"           │ central protagonist or faction.    │
├───────────────────────────────────┼────────────────────────────────────┤
│ "Where do alliances cross         │ Visualizing cross-factional ties,  │
│  dynastic boundaries?"            │ teachers spanning both sides.      │
├───────────────────────────────────┼────────────────────────────────────┤
│ "Is this connection settled or    │ Direct visual inspection of        │
│  contested in the tradition?"     │ epistemic status/certainty on edge │
└───────────────────────────────────┴────────────────────────────────────┘
```

### 4.1 Distinct Lens Separation
To preserve architectural discipline, F10 maintains clear architectural ownership boundaries with other exploration lenses:
- **General Graph vs. Family Lineage (Block F11)**: The general graph visualizes social, political, teacher-student, and basic direct kinship ties in an unconstrained topological layout. Deep multi-generational descent, ancestor DAGs, and generational band alignment belong exclusively to Block F11.
- **General Graph vs. Timeline (Block F12)**: The graph represents relational topology regardless of time. Temporal event progression, Parva-by-Parva development, and war-day chronologies belong exclusively to Block F12.
- **General Graph vs. Geography (Block F13)**: While Location nodes may participate in the graph (e.g., indicating capital seats or realms), spatial distances, coordinate maps, and geographic terrains belong exclusively to Block F13.
- **General Graph vs. Tactical Vyuhas (Block F14)**: Martial affiliations and combat encounters appear on the graph as canonical relationships, but military formations, tactical unit geometry, and daily battle deployments belong exclusively to Block F14.

---

## 5. Graph Data Boundary & Backend Integration

### 5.1 Consumption of Established API Contracts
Block F10 consumes the public API endpoints established in Stage 1 Block B5 without introducing new endpoints or altering schema signatures:
- **Focus Subgraph Retrieval**: Consumes `GET /api/v1/graph/focus/:entity_type/:slug?d={1|2}` to fetch the bounded focus subgraph payload centered on the specified entity.
- **Relationship & Edge Inspection**: Graph inspection consumes relationship metadata available through the established Block B5 API representations. If additional relationship or evidence detail is required, retrieval follows the established Block F6 and Block B5 data-fetching contracts.

### 5.2 Server-State Caching & Lifecycle Integration (Block F6)
Graph data retrieval delegates strictly to the Block F6 API client:
- **Server-State Identity**: Graph server-state identity must incorporate the canonical focus identity and any parameters that materially change the requested server representation. Concrete cache-key serialization is owned by Block F6.
- **Lifecycle Integration**: Request deduplication, conditional revalidation via `ETag`, and RFC 7807 problem details handling are owned entirely by Block F6.
- **Data Boundary**: F10 components never manage raw `fetch` streams, HTTP headers, or network retries directly.

### 5.3 Public Offset Pagination Invariant
If a graph node possesses large collections of secondary associations exceeding canonical payload limits, the underlying sub-resources (such as event participant rosters or detailed claim lists) consume the established public offset pagination contract (`offset={n}&limit={m}`) established in Stage 1 Block B5 and Block F6. Graph endpoints do not invent cursor pagination contracts.

---

## 6. Canonical-to-Visual Projection Model

The graph visualization is a declarative projection transforming read-only domain models from Block B2/B3 into visual graph elements:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   CANONICAL-TO-VISUAL PROJECTION MODEL                 │
├───────────────────────────────────┬────────────────────────────────────┤
│ Canonical Backend Record (B2/B3)  │ Projected Visual Graph Element (F10)│
├───────────────────────────────────┼────────────────────────────────────┤
│ Canonical Entity (`Character`,    │ Visual Graph Node:                 │
│ `Group`, `Location`, `Event`)     │ - Geometric glyph & entity color token│
│                                   │ - Primary label (canonical / IAST) │
│                                   │ - Secondary badge (domain / role)  │
│                                   │ - Epistemic border / indicator     │
│                                   │ - Distance tier styling (D0/D1/D2) │
├───────────────────────────────────┼────────────────────────────────────┤
│ Canonical Relationship Record     │ Visual Graph Edge:                 │
│ (`Relationship`,                  │ - Connecting path / line           │
│  `FamilyRelationship`)            │ - Stroke pattern (solid, dash, split)│
│                                   │ - Directional marker (arrowhead)   │
│                                   │ - Relationship-type semantic badge │
│                                   │ - Epistemic glyph & claim trigger  │
├───────────────────────────────────┼────────────────────────────────────┤
│ Backing Scholarly Assertion       │ Provenance Affordance:             │
│ (`Claim`, `Evidence`, `Source`)   │ - Contextual claim indicator       │
│                                   │ - Block F15 Evidence Drawer trigger│
└───────────────────────────────────┴────────────────────────────────────┘
```

### 6.1 Strict Data Projection Invariants
1. **No Entity Invention**: Every node rendered on the canvas must correspond directly to a valid canonical entity ID and slug in the API payload. Purely decorative nodes, cluster centroids, or synthetic intermediary vertices are prohibited.
2. **No Edge Synthesis**: Every rendered edge must map to an explicit `Relationship` or `FamilyRelationship` ID. Structural associations (e.g., two characters participating in the same event) are never rendered as direct relational lines unless backed by an explicit canonical relationship record.
3. **Immutability**: Graph projection is strictly a read-only visual transformation. Filtering, collapsing, or hiding nodes within the client modifies visual presence only; it never alters, mutates, or deletes backend records.

---

## 7. Focus Graph & Bounded Exploration Model ($D \le 2$)

### 7.1 The $D \le 2$ Hard Constraint
In strict accordance with Stage 1 Block B3 §9 and PRD §9.9 (REQ-GRP-01), the exploration depth from the active focal entity is bounded to **$D \le 2$**.

```
                           ┌─────────────────┐
                           │   D2 Neighbor   │
                           │ (Secondary Kin) │
                           └────────┬────────┘
                                    │
                           ┌────────┴────────┐
                           │   D1 Neighbor   │
                           │ (Direct Ally)   │
                           └────────┬────────┘
                                    │
                           ┌────────┴────────┐
                           │     FOCAL       │
                           │     NODE        │
                           │     (D0)        │
                           └────────┬────────┘
                                    │
                           ┌────────┴────────┐
                           │   D1 Neighbor   │
                           │ (Direct Teacher)│
                           └─────────────────┘
```

### 7.2 Semantic Definitions of Traversal Depths
- **Focal Entity ($D_0$)**:
  - The single root entity around which the current graph exploration is organized.
  - Centrally anchored in the canvas composition.
  - Distinguished by maximum visual scale, prominent high-contrast typographic treatment, and distinct focal ring.
- **First-Degree Neighbors ($D_1$)**:
  - Immediate entities connected to the focal node via direct 1-hop relationships (`source_id = focal_id` or `target_id = focal_id`).
  - Rendered at standard visual scale with full node labels, relationship badges, and epistemic indicators.
- **Second-Degree Neighbors ($D_2$)**:
  - Entities connected to $D_1$ nodes but having no direct relationship to the $D_0$ focal entity.
  - Visualized only when the exploration depth is set to $D=2$.
  - Rendered with de-emphasized visual weight (smaller diameter, muted typography, simplified labels) to preserve cognitive focus on the primary radius.
  - Constrained by the backend deterministic ceiling ($N \le 50$ total nodes per B3 §9) to prevent canvas saturation.
- **Depth Beyond 2 ($D > 2$)**:
  - **Strictly Prohibited in V1**. The UI provides no controls, gestures, or URL parameters to expand traversal to $D=3$ or beyond. Traversal to more distant entities occurs by shifting the focal anchor ($D_0$).

### 7.3 Focus Transition Semantics: Replacement vs. Additive Models
The Mahābhārata Explorer adopts a **Focal Replacement Model** as its default exploration paradigm:
1. **Focal Replacement (Default)**: Clicking a neighbor node ($D_1$ or $D_2$) and choosing "Make Focus" re-anchors the graph:
   - The selected node transitions smoothly to the central $D_0$ position.
   - The route updates via Block F7 (`/graph/:slug`).
   - A new bounded subgraph ($D \le 2$) is requested from the server via Block F6.
   - Nodes outside the new 2-hop radius are cleanly transitioned out of the viewport.
2. **Local Neighborhood Pinning (Secondary Inspection)**:
   - Clicking a node without making it the focal entity toggles its **Selected State**.
   - Selection opens the contextual inspector panel and highlights its incident edges, without displacing the central $D_0$ anchor or requesting a new subgraph.
3. **Reset to Initial Focal State**:
   - A reset action may restore the current graph to its initial focal presentation, including the applicable default viewport and lens-state settings, without requiring unnecessary network refetching.

---

## 8. Node Architecture

### 8.1 Entity Typology & Visual Encoding
Visual graph nodes project canonical entities defined in Block B2/B3. To maintain an accessible, modern-first visual language without relying solely on color (Block F3 §4), entities are distinguished through a combination of **shape geometry, non-color iconography, and semantic border tokens**:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        NODE VISUAL TYPOLOGY                            │
├──────────────┬──────────────────┬──────────────────┬───────────────────┤
│ Entity Type  │ Base Geometry    │ Semantic Icon    │ Visual Role       │
├──────────────┼──────────────────┼──────────────────┼───────────────────┤
│ **Character**│ Circle / Disc    │ Person silhouette│ Individual epic   │
│              │                  │ / helmet glyph   │ figure            │
├──────────────┼──────────────────┼──────────────────┼───────────────────┤
│ **Group**    │ Rounded Hexagon  │ Shield / banner  │ Dynasty, clan, or │
│              │                  │ emblem           │ military army     │
├──────────────┼──────────────────┼──────────────────┼───────────────────┤
│ **Location** │ Diamond / Lozenge│ Geographic pin / │ Capital, realm, or│
│              │                  │ mountain glyph   │ sacred tirtha     │
├──────────────┼──────────────────┼──────────────────┼───────────────────┤
│ **Event**    │ Rounded Rectangle│ Chrono marker /  │ Pivotal historical│
│              │                  │ flame glyph      │ occurrence        │
└──────────────┴──────────────────┴──────────────────┴───────────────────┘
```

### 8.2 Node Anatomical Structure
Every rendered node is composed of four standardized visual zones:
1. **Core Glyph Zone**: Houses the geometric shape and entity type icon.
2. **Label Zone**:
   - Primary line: Canonical entity name formatted with appropriate typographic diacritics (e.g., *Arjuna*, *Karṇa*).
   - Secondary line (when space/zoom permits): Primary alias, patronymic, or dynastic role (e.g., *Pārtha*, *Sūtaputra*).
3. **Epistemic Badge Zone**: Non-color indicator displaying epistemic status (`epistemic_status`) or certainty if the entity itself carries contested or approximate status (Block F3 §5).
4. **Degree Indicator Zone**: Subtly indicates connected neighbor count, providing progressive disclosure of connection density.

### 8.3 Node State Semantics
Nodes transition dynamically through distinct, mutually exclusive interaction states:
- **`default`**: Standard presentation reflecting entity type, distance tier ($D_0/D_1/D_2$), and epistemic status.
- **`hovered`**: Cursor positioned over node. Node displays prominent visual elevation, subtle scale emphasis, and a transient tooltip revealing concise entity context.
- **`focused`**: Node receives keyboard focus via spatial or semantic navigation. Encircled by a high-contrast, accessible focus ring conforming to Block F3/F16 design tokens.
- **`selected`**: Node activated via pointer or keyboard. Node displays active selection emphasis; contextual inspector panel populates; connected edges highlight.
- **`dimmed`**: Node is unrelated to the currently selected element or is excluded by an active filter. Node opacity is perceptually de-emphasized and secondary labels are suppressed to minimize visual clutter.
- **`loading_skeleton`**: Placeholder node geometry displayed during initial layout construction or focus transitions.

---

## 9. Edge Architecture
 
### 9.1 Canonical Relationship Semantics & Directional Conventions
Relationship types, endpoint semantics, and directionality are consumed exactly as represented by the authoritative B2/B3 canonical model. F10 does not enumerate, rename, normalize, or reinterpret the canonical relationship vocabulary:
- **Directed Relationships**: Rendered with an appropriate directional visual marker (such as a terminal arrowhead) pointing unambiguously from source to target. Visual orientation and presentation may adapt to the current graph view without altering canonical relationship semantics or meaning.
- **Symmetric Relationships**: Rendered with a non-directional visual treatment (such as a neutral, bidirectional stroke) indicating mutual equivalence.
- **Relationship Labels**: Edge labels and relationship badges are rendered directly from canonical backend records. F10 does not rename or invent relationship types.

### 9.2 Directional Rendering Conventions
- **Directed Edges**: An arrowhead glyph is placed at the target node, pointing unambiguously from source to target along the directed relationship path. F10 renders canonical relationship semantics as supplied by B2/B3, while visual orientation and labeling adapt to the current graph view without changing canonical relationship meaning.
- **Symmetric Edges**: Rendered as a neutral, bidirectional line without directional arrowheads, or with subtle symmetrical terminals, indicating mutual equivalence.

### 9.3 Edge Multiplicity & Superimposed Connections
In the Mahābhārata, two entities frequently share multiple distinct relationships (e.g., individuals who are both mentors and later allies or commanders; cousins who share both kinship and martial rivalry).
1. **No Silent Edge Dropping**: The visualization must never discard canonical relationships to simplify drawing. All edges returned by the API must remain accessible.
2. **Multi-Edge Rendering Strategy**:
   - When 2 or 3 distinct relationships exist between the same pair of nodes, edges are rendered as **parallel curved arcs (multi-lane splines)** with distinct radial curvature, preventing visual collision.
   - When $> 3$ relationships connect the same node pair, the paths collapse into a single **Bundled Multi-Relation Edge** carrying a discrete multiplicity badge (e.g., `[3 Relations]`). Clicking the bundle opens the contextual inspector displaying all constituent relationships with their respective Claims.
3. **Conflicting Accounts Multiplicity**: When competing traditions assert conflicting facts (e.g., conflicting accounts of who slew a warrior, Block B3 §5.2), distinct edge lines are rendered side-by-side, styled with the canonical **`conflicting` split-stroke visual token** (Block F3 §5) and linking to distinct Claims in Block F15.

---

## 10. Graph Visual Hierarchy & Density Management

To prevent the common "hairball" failure of network visualizations, Block F10 enforces a rigorous visual hierarchy governed by exploration depth and semantic relevance:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        VISUAL WEIGHT HIERARCHY                         │
├───────────────────┬──────────────┬──────────────┬──────────────────────┤
│ Element Tier      │ Scale / Size │ Text Weight  │ Visual Emphasis      │
├───────────────────┼──────────────┼──────────────┼──────────────────────┤
│ **Focal ($D_0$)** │ Maximum      │ Semibold     │ Solid / High         │
│                   │ Prominence   │ Primary IAST │ Contrast Anchor Ring │
├───────────────────┼──────────────┼──────────────┼──────────────────────┤
│ **Direct ($D_1$)**│ Standard     │ Medium       │ Primary Canvas       │
│                   │ Baseline     │ Canonical    │ Contrast             │
├───────────────────┼──────────────┼──────────────┼──────────────────────┤
│ **Extended ($D_2$)│ De-emphasized│ Regular      │ Muted Tone           │
│                   │ Compact      │ Simplified   │ De-emphasized Stroke │
├───────────────────┼──────────────┼──────────────┼──────────────────────┤
│ **Dimmed / Filtered│ Receded     │ Suppressed   │ Perceptually         │
│                   │              │              │ Ghosted              │
└───────────────────┴──────────────┴──────────────┴──────────────────────┘
```

### 10.1 Density Management & Decluttering Rules
1. **Adaptive Label Disclosure**:
   - At baseline zoom, focal ($D_0$) and primary ($D_1$) labels are prioritized for complete rendering. Extended ($D_2$) labels render conditionally based on available local space and overlap avoidance.
   - During zoomed-out exploration, extended labels collapse into compact glyphs, preserving focal and direct neighbor legibility.
   - Hovering or focusing on any node immediately forces its label and primary identifier to the foreground regardless of viewport zoom level.
2. **Collision Avoidance Principle**: Visual node boundaries, touch surfaces, and label containers must maintain adequate spatial clearance. Layout solvers must enforce collision buffers to prevent illegible overlapping of adjacent glyphs and typography.
3. **Transparent Omission Notification**: If client-side filters (e.g., hiding kinship ties) cause certain nodes to be omitted from the visual layout, the UI must render an explicit status notification (e.g., *"Kinship connections hidden by active filter"*). Content is never silently vanished.

---

## 11. Multi-Modal Interaction Architecture

The graph supports fluid exploration across pointer, touch, and keyboard modalities without making any single input method mandatory.

```
┌────────────────────────────────────────────────────────────────────────┐
│                      INTERACTION MODALITY MATRIX                       │
├──────────────────┬──────────────────┬──────────────────────────────────┤
│ User Action      │ Pointer / Mouse  │ Touch Gesture / Mobile           │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Select Node**  │ Single Left Click│ Single Tap                       │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Re-Anchor ($D_0$)│ Double Click or │ Contextual Menu Action:          │
│                  │ Inspector Action │ "Explore as Focus"               │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Pan Canvas**   │ Click-and-Drag   │ Single/Two-Finger Drag           │
│                  │ on background    │ (with scroll-lock guard)         │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Zoom Viewport**│ Mouse Wheel /    │ Pinch-to-Zoom Gesture            │
│                  │ UI +/- Buttons   │ & UI +/- Buttons                 │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Inspect Edge** │ Click Edge Line  │ Tap Edge Touch Target / Pill     │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Inspect Claim**│ Click Evidence   │ Tap Evidence Trigger             │
│                  │ Trigger Pill     │ (Dispatches F15 Drawer)          │
└──────────────────┴──────────────────┴──────────────────────────────────┘
```

### 11.1 Progressive Disclosure Interaction Lifecycle
Exploration follows a predictable three-step disclosure flow:
1. **Glance (Canvas Observation)**: User observes overall topological cluster, node types, and edge lines.
2. **Inspection (Single Activation)**: User clicks/taps or keyboard-selects a node or edge. The element highlights; an inspector panel surfaces entity summary, attributes, and relationship lists.
3. **Navigation (Deep Engagement)**: User activates a secondary action from the inspector panel:
   - "Set as Graph Focus" $\rightarrow$ re-anchors the graph to this entity ($D_0$).
   - "Open Entity Profile" $\rightarrow$ navigates to canonical entity view (Block F9, `/characters/:slug`).
   - "View Backing Evidence" $\rightarrow$ opens the Block F15 Evidence Drawer for the specific claim.
   - "Explore in Another Lens" $\rightarrow$ launches Lineage (F11), Timeline (F12), or Map (F13).

### 11.2 Edge Touch Target Augmentation
Because rendered edge strokes may be slender, mobile and touch interfaces must augment edges with an adequate interaction hit area conforming to Block F3/F4 touch ergonomics, ensuring effortless touch selection without precision penalties.

---

## 12. Keyboard Navigation & Accessible Interaction Model

Dense visual canvases often present significant accessibility barriers for keyboard-only users. Block F10 establishes an architectural requirement for **Dual-Mode Graph Accessibility**:
- **Spatial Canvas Navigation**: Enables keyboard users to traverse visually connected nodes along connecting edges or focus landmarks.
- **Accessible Semantic Representation**: Exposes graph information, neighbor groupings, and meaningful graph actions without requiring visual canvas interaction.
- **Stage 2 / Stage 4 Demarcation**: Block F10 establishes the requirement for accessible graph information and action equivalence; Block F16 owns concrete accessibility, keyboard, DOM, ARIA, focus-management, and screen-reader implementation; Block F17 validates the result.

---

## 13. Filtering & Visual Density Architecture

### 13.1 Filter Dimensions & Controlled Vocabularies
Graph filtering enables users to reduce visual complexity by selectively revealing or hiding graph elements. Filtering operates across structured dimensions derived from canonical Stage 1 data:
- **Entity Domain**: Filtering by canonical entity types (`Character`, `Group`, `Location`, `Event`).
- **Relationship Category**: Filtering by relationship groupings derived from canonical B2/B3 types (such as kinship, alliance, mentorship, rivalry, and martial connections).
- **Epistemic Status & Certainty**: Filtering by canonical epistemic states (`epistemic_status`: `known`, `conflicting`, etc.) or canonical claim certainty levels (`certainty`: `established`, `disputed`, etc.).
- **Exploration Depth**: Restricting visualization to immediate neighbors ($D=1$) or extended network ($D=2$).

### 13.2 Visual Hiding vs. Layout Exclusion vs. Data Mutation
Block F10 establishes strict architectural definitions for how filtering affects client state:
- **Visual Dimming (Soft Filter)**: Elements that do not match the filter remain in the layout to preserve overall structural stability, but their visual presence is de-emphasized and labels are suppressed.
- **Layout Exclusion (Hard Filter)**: When a category is explicitly excluded, excluded elements are omitted from the active layout composition, allowing remaining nodes to utilize canvas space efficiently.
- **Strict Anti-Mutation Invariant**: Filtering is **never** sent as a destructive mutation to the backend. The underlying cached API payload retains all nodes and edges; filtering is executed entirely as a declarative client-side projection over the cached server state (Block F5/F6).

---

## 14. Layout Architecture & Spatial Stability

### 14.1 Conceptual Layout Principles
Block F10 governs the layout invariants necessary for readable, stable topological exploration while deferring the exact layout algorithm and computational solver to Stage 4:
1. **Focus-Centered Composition**: The active focal entity ($D_0$) is visually anchored as the central subject of the composition.
2. **Perceptible Depth Hierarchy**: First-degree ($D_1$) and second-degree ($D_2$) relationships remain visually distinguished through spatial grouping and hierarchical weight.
3. **Readable Relationships & Collision Mitigation**: Node positions and edge paths must maintain adequate clearance to prevent illegible overlapping of glyphs, labels, and parallel connectors.
4. **Layout Deferral**: The exact algorithmic layout strategy (e.g., radial tree layout, constrained force-directed simulation, circular sectoring, or pre-computed topological packing) is deferred to Stage 4 implementation.

### 14.2 Spatial Stability & Anti-Jitter Invariants
1. **Deterministic Presentation**: Given identical backend API data and filter settings, the graph layout must resolve deterministically, avoiding disorienting node reshuffling, canvas jitter, or random node flipping across reloads.
2. **Perceptually Appropriate Transitions**: When switching filters or toggling $D=2$, existing nodes smoothly interpolate from their current positions to their new positions. Abrupt visual teleportation or jarring canvas flashes are prohibited.
3. **Reduced Motion Support**: When the user has enabled the system-wide reduced motion preference (`prefers-reduced-motion: reduce`), animated kinetic transitions are bypassed: layouts resolve immediately to their stable coordinates without oscillation (Block F3 §7).

---

## 15. Responsive Architecture across Viewport Classes

The graph lens adapts its visual composition across the four space-based viewport classes defined in Block F4 §4:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   RESPONSIVE RECOMPOSITION MATRIX                      │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Viewport Class   │ Layout Paradigm & Interaction Adaptation            │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Wide**         │ Multi-column studio: Full-screen graph canvas;      │
│ (F4 Wide Class)  │ persistent left-hand filter & legend rail;          │
│                  │ persistent right-hand entity & claim inspector;     │
│                  │ simultaneous display of companion semantic list.    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Expanded**     │ Persistent two-pane workspace: Full graph canvas;   │
│ (F4 Expanded     │ collapsible left filter panel; persistent right-hand│
│  Class)          │ contextual inspector panel.                         │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Medium**       │ Adaptive canvas overlay: Edge-to-edge canvas;       │
│ (F4 Medium Class)│ compact floating control pill for filters;          │
│                  │ contextual inspector slides over as a semi-sheet    │
│                  │ from the right on node selection.                   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Compact**      │ Single-column stacked stream: Full-viewport canvas  │
│ (F4 Compact      │ with gesture controls; selection opens a bottom-    │
│  Class)          │ sheet inspector; prominent toggle to switch between │
│                  │ Canvas and Semantic List view.                      │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 16. Accessibility Architecture (Block F16 Integration)

In strict accordance with Constitutional Principle 10 (Accessibility as a First-Class Requirement):

### 16.1 Accessible Semantic Representation & Equivalence
A visual canvas element alone cannot satisfy non-visual exploration. Therefore, the Graph Lens requires an accessible semantic representation:
1. **Semantic Graph Companion**: Alongside visual graph rendering, an accessible semantic representation exposes all graph information, relationship groupings, and interaction triggers without requiring visual canvas interaction.
2. **Informational & Action Equivalence**: The semantic representation must allow users relying on screen readers or non-pointer inputs to perceive focal entity details, browse first-degree and second-degree connections, inspect backing claims, and re-anchor the graph.
3. **Programmatically Perceivable State Changes**: Graph selections, depth changes, and filter changes must be perceivable through the accessible representation, while concrete announcement mechanisms belong to Block F16.
4. **F16/F17 Authority**: Block F16 owns concrete accessibility, keyboard, DOM, ARIA, focus-management, and screen-reader implementation; Block F17 validates the resulting accessibility conformance.

### 16.2 Contrast & Non-Color Discrimination
- All text labels and indicators must maintain adequate contrast against the canvas background across both Light and Dark themes, complying with Block F3 tokens.
- Information is never conveyed through color alone: all entity types, relationship classes, and epistemic states combine color with geometric shapes, borders, stroke patterns, and textual badges.

---

## 17. Epistemic-State Presentation in Graph Visualizations

Block B2 establishes the authoritative six epistemic states; Block F3 establishes their visual tokens; Block F10 governs their specific application to graph nodes and edges:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   GRAPH EPISTEMIC PRESENTATION RULES                   │
├──────────────────┬──────────────────┬──────────────────────────────────┤
│ Epistemic State  │ Node Treatment   │ Edge Treatment                   │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **`known`**      │ Solid perimeter, │ Solid stroke line, standard      │
│                  │ default opacity  │ directional terminal glyph       │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **`conflicting`**│ Split dual-tone  │ Forked/dual parallel stroke line,│
│                  │ border badge     │ split-color token, claim badges  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **`approximate`**│ Dotted perimeter │ Dotted stroke line, tilde (`~`)  │
│                  │ ring             │ relationship label prefix        │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **`unknown`**    │ Dashed border,   │ Dashed stroke line, open glyph;  │
│                  │ question glyph   │ zero fabricated details          │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **`not_researched`│ Low-contrast    │ Muted dotted stroke; explicitly  │
│                  │ dotted border    │ marked as pending scholarship    │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **`not_applicable`│ Diagonal hash   │ Ghosted/dimmed stroke with null  │
│                  │ badge            │ indicator; rendered per F3 tokens│
└──────────────────┴──────────────────┴──────────────────────────────────┘
```

---

## 18. Evidence & Provenance Trigger Boundaries (Block F15)

The graph lens visualizes connections but does not render exhaustive textual critical apparatuses. Detailed citation viewing delegates strictly to **Block F15 (Evidence, Citation & Provenance UX)**:

```
┌────────────────────────────────────────────────────────────────────────┐
│                  EVIDENCE TRIGGER BOUNDARY ARCHITECTURE                │
├────────────────────────────────────────────────────────────────────────┤
│ 1. GRAPH EDGE / NODE INSPECTION                                        │
│    - User clicks an edge or selects a relationship in the inspector.   │
│    - Inspector displays relationship summary and evidence pill:        │
│      [Evidence: 3 Citations ↗]                                         │
├────────────────────────────────────────────────────────────────────────┤
│ 2. DISPATCH EVIDENCE TRIGGER                                           │
│    - Clicking the evidence pill dispatches an invocation event to      │
│      Block F15, passing `{ claim_id: "clm_...", edge_id: "rel_..." }`. │
├────────────────────────────────────────────────────────────────────────┤
│ 3. DRAWER ACTIVATION (F15)                                             │
│    - Block F15 opens the persistent/overlay Evidence Drawer.           │
│    - URL updates with deep-link parameter (`?claim=clm_...`) per F7.   │
│    - Critical Edition shlokas and translations render in F15.          │
│    - Graph canvas remains visible and interactive in the background.   │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 19. Navigation & Deep-Link State Integration (Block F7)

The graph lens strictly conforms to the canonical route grammar established by Block F7 §4 and §6.2:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        GRAPH URL STATE CONTRACT                        │
├──────────────────┬─────────────────────────────────────────────────────┤
│ URL Component    │ Architectural Role & Canonical Representation       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Route Path**   │ `/graph` (Graph exploration entry point / overview  │
│                  │ lens) or                                            │
│                  │ `/graph/:slug` (Entity-centric bounded focus graph) │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Depth Query**  │ `?d=1` (default, omitted) or `?d=2`                 │
│                  │ Strict constraint: $d \in \{1, 2\}$ (REQ-GRP-01)    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Filters**      │ `?rel=...`                                          │
│                  │ Relationship filters may expose user-facing         │
│                  │ groupings derived from canonical B2/B3 relationship │
│                  │ data. Exact filter vocabulary and serialization are │
│                  │ coordinated with Block F7 and finalized during      │
│                  │ Stage 4 implementation.                             │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Evidence**     │ `?claim=:claim_id`                                  │
│                  │ Contextual evidence drawer activation (Block F15)   │
└──────────────────┴─────────────────────────────────────────────────────┘
```

### 19.1 URL Normalization Invariants
- Slugs are normalized to lowercase kebab-case (e.g., `/graph/Arjuna` $\rightarrow$ `/graph/arjuna`).
- Unsupported query parameters are silently discarded per Block F7 §7.
- Values of `d` outside `1` or `2` are canonicalized to `d=1` without crashing or throwing unhandled errors.

---

## 20. Lifecycle, Loading, Empty & Error States

The graph lens handles network and state transitions through explicit, predictable UI lifecycle representations:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   GRAPH VIEW LIFECYCLE MATRIX                          │
├───────────────────┬────────────────────────────────────────────────────┤
│ State             │ Presentation Behavior & User Experience            │
├───────────────────┼────────────────────────────────────────────────────┤
│ **Initial Load**  │ Initial loading provides a stable graph-region     │
│                   │ placeholder that minimizes layout shift and        │
│                   │ communicates loading state.                        │
├───────────────────┼────────────────────────────────────────────────────┤
│ **Ready / Active**│ Deterministically laid out focus graph ($D \le 2$) │
│                   │ with interactive controls and inspector panel.     │
├───────────────────┼────────────────────────────────────────────────────┤
│ **Not Found (404) │ Clean 404 notice indicating the requested entity   │
│ (Invalid Slug)**  │ does not exist; provides links to Global Search    │
│                   │ (`/search`) and the relevant entity directory/index│
├───────────────────┼────────────────────────────────────────────────────┤
│ **Network Error** │ Inline error card over canvas; preserves cached    │
│                   │ data if available; provides explicit "Retry" CTA.  │
├───────────────────┼────────────────────────────────────────────────────┤
│ **Empty Relations │ Clear, authentic empty state: "No direct           │
│ (Isolated Entity) │ relationships recorded in current corpus"; zero    │
│                   │ synthetic or decorative placeholder nodes.         │
├───────────────────┼────────────────────────────────────────────────────┤
│ **Filter Zero**   │ Notice: "No connections match active filters";     │
│                   │ one-click action to "Clear All Filters".           │
└───────────────────┴────────────────────────────────────────────────────┘
```

---

## 21. Performance Architecture & Rendering Boundaries

### 21.1 Three-Tier Rendering Classification
In alignment with Block F2 §4, the frontend establishes a three-tier rendering strategy for graphics:
1. **Semantic DOM Tier**: Used for the application shell, control rails, inspector panels, and the accessible companion navigation tree.
2. **Structured SVG Tier**: Architecturally suitable for structured, lower-density graph presentations where native DOM event binding, vector crispness at arbitrary zoom scales, and seamless integration with CSS styling tokens are advantageous.
3. **Canvas / WebGL Tier**: Architecturally suitable for dense visualization scenarios, high-frequency kinetic transformations, or complex visual layering where eliminating DOM node overhead preserves frame-rate stability.

> **Stage 4 Implementation Boundary**: Block F10 establishes the architectural requirement that the rendering layer must handle bounded subgraphs ($D \le 2$, respecting Block B3 §9 payload limits) with smooth, responsive interaction and predictable layout stability. The concrete selection between an optimized SVG pipeline or a Canvas/WebGL engine is deferred to Stage 4 implementation without selecting third-party libraries in this specification. Formal numeric performance budgets and automated verification belong authoritatively to Block F17.

### 21.2 Lifecycle & Disposal of Graph Resources
To prevent memory leaks during extended user exploration sessions:
- Canvas render loops, animation frames, and layout timers must register automatic cleanup hooks upon component unmount.
- Spatial indexing structures used for interaction hit-testing must be cleanly disposed of when re-anchoring to a new focal entity.
- Interaction resources and event subscriptions must be scoped and cleaned up with the graph lifecycle to prevent leaks and stale handlers.

---

## 22. Security & Untrusted Data Considerations

Graph visualization must enforce rigorous defense-in-depth against malicious or malformed content:
1. **Safe Text Label Injection**: All entity names, epithets, relationship labels, and biographical snippets received from API endpoints are treated as untrusted data. Labels rendered within SVG `<text>` elements or Canvas `fillText` routines must be rendered as raw text strings, completely bypassing HTML parsing or `innerHTML` interpretation to prevent Cross-Site Scripting (XSS).
2. **Safe Deep-Link Navigation**: Entity slugs and URLs parsed from graph payloads must strictly validate against the alphanumeric kebab-case pattern (`^[a-z0-9-]+$`) before being interpolated into navigation routes. Graph-derived navigation targets must resolve only to validated internal application routes. External or protocol-relative URLs must not be accepted as untrusted graph navigation destinations; legitimate external source/media URLs remain governed by the canonical B8/F15 resource-handling rules.
3. **Resource Consumption Caps**: Bounded exploration depth ($D \le 2$) and strict server payload ceilings ($N \le 50$ nodes) protect the client browser from denial-of-service via adversarial graph structures or infinite cyclic payloads.

---

## 23. Cross-Lens Transitions & Navigation Continuity

The graph lens is an integral part of the interconnected exploration loop, providing fluid entry and exit points to other lenses. Transitions hand off the active entity as contextual subject to the destination lens, which authoritatively owns its lens-specific state and parameters:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        CROSS-LENS BRIDGES                              │
├───────────────────┬────────────────────────────────────────────────────┤
│ Target Lens       │ Transition Mechanism & Contextual Transfer         │
├───────────────────┼────────────────────────────────────────────────────┤
│ **Entity View**   │ Inspector CTA: "View Full Profile" $\rightarrow$   │
│ (Block F9)        │ Graph $\rightarrow$ Entity view with current entity│
│                   │ as subject (`/characters/:slug`).                  │
├───────────────────┼────────────────────────────────────────────────────┤
│ **Lineage Tree**  │ Inspector CTA: "Explore Family Lineage" $\rightarrow$│
│ (Block F11)       │ Graph $\rightarrow$ Lineage lens with current entity│
│                   │ as contextual root (`/lineage/:slug`).             │
├───────────────────┼────────────────────────────────────────────────────┤
│ **Timeline**      │ Inspector CTA: "View Event Milestones" $\rightarrow$│
│ (Block F12)       │ Graph $\rightarrow$ Timeline lens with current     │
│                   │ entity as contextual filter (`/timeline/:slug`).   │
├───────────────────┼────────────────────────────────────────────────────┤
│ **Geographic Map**│ Inspector CTA: "View Locations" $\rightarrow$      │
│ (Block F13)       │ Graph $\rightarrow$ Geographic Map lens with current│
│                   │ entity as contextual subject (`/geography/:slug`). │
├───────────────────┼────────────────────────────────────────────────────┤
│ **Global Search** │ User executes search via Block F8 autocomplete;    │
│ (Block F8)        │ selecting a result directly opens `/graph/:slug`.  │
└───────────────────┴────────────────────────────────────────────────────┘
```

---

## 24. State Ownership Distribution

State governance strictly complies with the five-layer state model defined in Block F5:

```
┌────────────────────────────────────────────────────────────────────────┐
│                     GRAPH STATE GOVERNANCE MATRIX                      │
├──────────────┬──────────────┬──────────────────────────────────────────┤
│ State Layer  │ Authoritative│ Concrete State Attributes                │
│              │ Owner        │                                          │
├──────────────┼──────────────┼──────────────────────────────────────────┤
│ **1. Server  │ Backend API  │ Normalized Subgraph DTO: nodes array,    │
│ State**      │ (via F6)     │ edges array, claim references. Read-only.│
├──────────────┼──────────────┼──────────────────────────────────────────┤
│ **2. URL     │ Browser URL  │ Focal slug (`:slug`), depth (`d=1|2`),   │
│ State**      │ (via F7)     │ active filter list (`rel=...`), active   │
│              │              │ claim drawer (`claim=...`). Shareable.   │
├──────────────┼──────────────┼──────────────────────────────────────────┤
│ **3. Lens    │ Active Graph │ Selected node ID, selected edge ID,      │
│ State**      │ Component    │ canvas zoom scale, canvas pan offset $(x,y)$,│
│              │ (Block F10)  │ companion view toggle (Canvas vs List).  │
├──────────────┼──────────────┼──────────────────────────────────────────┤
│ **4. UI      │ Ephemeral    │ Hovered node ID, active drag coordinate, │
│ State**      │ Component    │ tooltip visibility, filter menu open.    │
├──────────────┼──────────────┼──────────────────────────────────────────┤
│ **5. Client  │ User Client  │ Display script (IAST / Devanāgarī),      │
│ Preferences**│ Store (F5)   │ Theme (Light / Dark), Reduced Motion.    │
└──────────────┴──────────────┴──────────────────────────────────────────┘
```

*High-Frequency Interaction Invariant*: Canvas pan coordinates $(x, y)$, zoom multipliers, pointer hover states, and drag positions are strictly categorized as **Lens/UI State**. They are **never** synchronized to the browser URL or persisted to long-term storage, preventing history stack thrashing and performance degradation.

---

## 25. Architectural Ownership Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                     ARCHITECTURAL OWNERSHIP MATRIX                     │
├──────────────────┬──────────────────┬──────────────────────────────────┤
│ Architectural    │ Authoritative    │ Clear Demarcation & Boundary     │
│ Concern          │ Block Owner      │                                  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ Canonical Graph  │ Block B3         │ Owns database schemas, recursive │
│ Truth            │                  │ CTEs, and server-side integrity. │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ Visual Grammar   │ Block F3         │ Owns semantic color tokens,      │
│                  │                  │ typography scales, and glyphs.   │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ Responsive Grid  │ Block F4         │ Owns 4 viewport classes (Compact,│
│                  │                  │ Medium, Expanded, Wide).         │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ State Hierarchy  │ Block F5         │ Owns 5-tier state separation.    │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ Network & Cache  │ Block F6         │ Owns HTTP fetching, caching,     │
│                  │                  │ deduplication, and revalidation. │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ Routing & URL    │ Block F7         │ Owns canonical URL grammar,      │
│                  │                  │ parsing, and canonicalization.   │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ Search UX        │ Block F8         │ Owns autocomplete suggestions &  │
│                  │                  │ global `/search` results.        │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ Entity Views     │ Block F9         │ Owns `/characters/:slug` and     │
│                  │                  │ entity biographical facets.      │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Graph Lens**   │ **Block F10**    │ **Owns `/graph`, `/graph/:slug`, │
│                  │                  │ topological projection & UX.**   │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ Lineage DAG      │ Block F11        │ Owns generational descent trees  │
│                  │                  │ and multi-generational kin DAGs. │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ Timeline         │ Block F12        │ Owns chronological tracks and    │
│                  │                  │ Parva sequence scrubbers.        │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ Geographic Map   │ Block F13        │ Owns ancient GIS coordinates     │
│                  │                  │ and cartographic tile layers.    │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ Tactical Vyuhas  │ Block F14        │ Owns military formation geometry │
│                  │                  │ and daily tactical battlefield.  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ Evidence Drawer  │ Block F15        │ Owns primary citation apparatus, │
│                  │                  │ shloka views, and Claim details. │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ A11y Primitives  │ Block F16        │ Owns system-wide ARIA guidelines │
│                  │                  │ and screen reader standards.     │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ Formal Budgets   │ Block F17        │ Owns numeric bundle thresholds   │
│                  │                  │ and automated testing runners.   │
└──────────────────┴──────────────────┴──────────────────────────────────┘
```

---

## 26. Architectural Decision Records (ADRs)

### ADR-F10-01: Graph Visualization as a Read-Only Projection of Canonical Backend Truth
- **Context**: Visual graph tools frequently construct local client-side caches that infer relationships from narrative text or layout proximity.
- **Decision**: The interactive graph is strictly a declarative, read-only projection of the authoritative backend knowledge graph (Block B3). The frontend never creates, infers, or persists synthetic edges.
- **Consequences**: Guarantees zero data fabrication. Simplifies client state by eliminating dual-source-of-truth discrepancies.

### ADR-F10-02: Bounded Exploration Model Anchored to $D \le 2$
- **Context**: Unbounded network graphs ("render all 5,000 characters at once") consistently degrade into illegible "hairballs" with severe WebGL/DOM performance costs.
- **Decision**: Graph exploration is strictly centered on an active focal entity ($D_0$) and bounded to a maximum depth of two hops ($D \le 2$). Traversal to distant figures is achieved by shifting the focal anchor.
- **Consequences**: Preserves cognitive clarity and readability. Keeps payload and rendering costs strictly within predictable performance envelopes ($\le 50$ nodes).

### ADR-F10-03: Dual-Mode Accessibility via Accessible Semantic Companion Representation
- **Context**: Highly interactive visual canvases are fundamentally opaque to screen readers and difficult to navigate via standard keyboard tabs.
- **Decision**: Mandate an accessible semantic companion representation presenting the complete graph information, groupings, and actions, alongside spatial canvas traversal. F16 authoritatively owns concrete DOM structure, ARIA roles, focus management, and screen reader announcements; F17 validates accessibility conformance.
- **Consequences**: Ensures that users relying on screen readers or non-pointer inputs have equivalent access to graph exploration without degrading visual canvas immersion.

### ADR-F10-04: Focal Replacement as the Primary Navigation Transition
- **Context**: Users exploring network graphs can either accumulate an ever-expanding web (additive model) or navigate from focal center to focal center (replacement model).
- **Decision**: Adopt the Focal Replacement model as the primary navigation paradigm. Clicking a neighbor and choosing "Make Focus" re-centers the graph, requests the bounded 2-hop neighborhood for the new focus, and updates the URL.
- **Consequences**: Keeps visual density strictly bounded. Integrates seamlessly with browser history and deep-linking (`/graph/:slug`).

### ADR-F10-05: Non-Color Differentiated Node and Edge Semantics
- **Context**: Relying exclusively on color hues to distinguish entity types or relationship categories fails accessibility standards for color-blind users.
- **Decision**: All nodes combine distinct geometric silhouettes with iconography; all edges combine distinct stroke patterns with textual badges and arrowheads, consuming canonical B2/B3 relationship types without redefining them.
- **Consequences**: Full visual legibility across all theme modes and color-vision variations.

### ADR-F10-06: Strict Delegation of Evidence Inspection to Block F15
- **Context**: Graph edges represent claims backed by ancient texts, temptingly leading to inline verse rendering within canvas tooltips.
- **Decision**: Graph edges display compact evidence trigger affordances (e.g., `[Evidence: 2 Claims ↗]`) that dispatch directly to the Block F15 Evidence Drawer, avoiding duplicate citation rendering architectures.
- **Consequences**: Clean separation of concerns. Consistent scholarly apparatus across all application lenses.

### ADR-F10-07: Library-Neutral Rendering Boundary Strategy
- **Context**: Committing to a specific third-party visualization library or prescribing rigid element counts in an architectural specification prematurely constrains implementation.
- **Decision**: Formally classify the rendering boundary across Semantic DOM, Structured SVG, and Canvas/WebGL tiers based on interaction density and structural characteristics, leaving concrete package selection to Stage 4 and formal performance verification to Block F17.
- **Consequences**: Preserves architectural purity, library neutrality, and long-term maintainability.

---

## 27. Requirement Traceability Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                   REQUIREMENT TRACEABILITY MATRIX                      │
├───────────────────┬──────────────────┬─────────────────────────────────┤
│ Upstream Req ID   │ Source Document  │ Block F10 Architectural Mapping │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REQ-CORE-01**   │ Constitution;    │ Section 2.2, Section 6: Unified │
│                   │ Blueprint §1     │ graph projection across lenses. │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REQ-CORE-02**   │ Constitution;    │ Section 4.1, Section 25: Strict │
│                   │ Rule 02          │ lens demarcation & system reuse.│
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REQ-GRP-01**    │ PRD §9.5, §9.9;  │ Section 7.1, Section 19: Bounded│
│                   │ B3 §9            │ depth $D \le 2$ hard limit.     │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REQ-GRP-05**    │ Constitution;    │ Section 2.2, Section 6.1: Zero  │
│                   │ Rule 03; B3 §7   │ edge synthesis or fabrication.  │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REL-002**       │ PRD §9.5; B3 §5  │ Section 9.1: Controlled         │
│                   │                  │ relationship taxonomy & arrows. │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **FOC-001–005**   │ PRD §9.9; B3 §9  │ Section 7: Global focus query   │
│                   │                  │ projection & re-anchoring.      │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **SRC-005**       │ PRD §9.10; B3 §5 │ Section 9.3, Section 17: Multi- │
│                   │                  │ claim conflicting edge display. │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **NAV-001**       │ PRD §9.12; F7 §4 │ Section 19: Canonical URL route │
│                   │                  │ `/graph/:slug` and parameters.  │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **A11Y-001**      │ PRD §9.13; F3 §4 │ Section 12, Section 16: Accessible│
│                   │                  │ semantic companion representation│
└───────────────────┴──────────────────┴─────────────────────────────────┘
```

---

## 28. F10 Exit Criteria & Acceptance Checklist

- [x] Graph purpose, UX role, and distinct boundary against F11–F14 established.
- [x] Read-only projection model defined transforming B2/B3 entities into visual elements.
- [x] Hard exploration depth limit ($D \le 2$) preserved and formally defined ($D_0, D_1, D_2$).
- [x] Focus graph re-anchoring semantics (Focal Replacement vs. Local Pinning) specified.
- [x] Node visual typology, geometry, states, and non-color indicators established.
- [x] Edge taxonomy consumes canonical B2/B3 relationships with multiplicity/curvature rules defined.
- [x] Density management, collision prevention, and adaptive label disclosure established.
- [x] Dual-mode accessibility model (Spatial traversal + Accessible Companion representation) architected with F16/F17 demarcation.
- [x] Multi-modal interaction (mouse, touch hit targets, keyboard) fully addressed without arbitrary numeric thresholds.
- [x] Epistemic status states (`epistemic_status`) mapped to non-color differentiated visual tokens per Block F3.
- [x] Evidence trigger boundaries established with Block F15 Evidence Drawer.
- [x] Route grammar and query parameter synchronization strictly aligned with Block F7.
- [x] Responsive recomposition mapped across the four Block F4 viewport classes.
- [x] High-frequency interaction state isolated from URL/storage per Block F5.
- [x] Performance architecture defines DOM/SVG/Canvas rendering tiers without selecting libraries.
- [x] Security and untrusted data protections specified for text rendering and deep links.
- [x] Zero application source code, UI packages, or component files were introduced.
- [x] Zero Stage 1 backend documents or Blocks F1–F9 documents were modified.
