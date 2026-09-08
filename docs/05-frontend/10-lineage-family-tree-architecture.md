# Lineage / Family Tree Architecture (Block F11)

## 1. Document Status & Purpose

- **Document Identifier**: Block F11
- **Status**: Approved Architectural Specification
- **Stage**: Stage 2 — Frontend Architecture
- **Location**: `docs/05-frontend/10-lineage-family-tree-architecture.md`
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
  - `docs/05-frontend/09-graph-visualization-architecture.md` (Block F10)
- **Downstream Consumers**:
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
│ - Block B2: FamilyRelationship DB │ ► BLOCK F11: LINEAGE / FAMILY TREE │
│ - Block B3: Kinship Exclusivity   │   - F12: Timeline & Chronology     │
│ - Block B5: Family API Endpoints  │   - F13: Geographic Map Lens       │
│ - Block F3: Visual Grammar        │   - F14: War & Tactical Vyuhas     │
│ - Block F4: Viewport Classes      │   - F15: Evidence Drawer UX        │
│ - Block F5: State Hierarchy       │   - F16: A11y & Typography Engine  │
│ - Block F6: API Fetching/Caching  │   - F17: Testing & Budgets         │
│ - Block F7: URL Grammar & Routing │                                    │
│ - Block F9: Entity Views Launch   │                                    │
│ - Block F10: Graph Visualization  │                                    │
└───────────────────────────────────┴────────────────────────────────────┘
```

This document establishes the frontend **architecture** for the Mahābhārata Explorer's Lineage and Family Tree visualization lens (`/lineage`, `/lineage/:slug`). Kinship, ancestry, and dynastic succession are foundational to epic narrative and knowledge exploration.

Block F11 defines the visual, structural, responsive, and accessible projection of genealogical relationships. In strict accordance with the project constitution, F11 treats family lineage not as an unconstrained generic network, but as a **structured, generational genealogical projection** governed by Zero Fabrication, Epistemic Honesty, and Accessible Equivalents: canonical parent-child lineage traversal forms the directed acyclic generational structure, while partner and union associations are non-entity visual connectors layered onto that projection.

---

## 2. Architectural Context & Foundational Principles

### 2.1 Pure Architectural Specification & Library Neutrality
Block F11 is an architectural specification only. It defines projection models, generational layout rules, union association semantics, epistemic state rendering, responsive transformations, and accessible semantic companions. It does **not**:
- Implement React components, hooks, or custom web elements.
- Select or mandate concrete third-party rendering or layout libraries.
- Introduce application source code, package dependencies, or bundle configurations.
- Introduce direct database queries, SQL CTEs, or backend schema changes.

### 2.2 Foundational Principles

#### 1. Lineage is a Generational Structure, Not a Generic Force Graph (Constitutional Principle 2 & 5)
A generic force-directed graph (Block F10) scatters nodes topologically based on organic edge attraction. A genealogical lineage tree requires strict temporal-generational orientation:
- Generational levels form an invariant visual axis (senior ancestors placed strictly upstream of descendants along the generational axis).
- Canonical parent-child traversal forms the underlying directed acyclic generational backbone, onto which partner/union connectors are layered without disrupting generational monotonicity.
- Force-directed organic node clustering, circular network layouts, and physics simulations are strictly prohibited for family trees.

#### 2. Zero Fabrication of Persons or Unions (Constitutional Principle 4 & 13, REQ-GRP-05)
Ancient texts frequently record incomplete lineage (e.g., a figure whose father is named but mother is unmentioned, or figures with unknown paternal origins). Under no circumstance may the frontend synthesize dummy character nodes (such as "Unknown Mother", "Unrecorded Wife", or "Unnamed Father") to force binary tree symmetry. Missing endpoints remain cleanly unrendered; structural junctions represent connections between known records only.

#### 3. Unions are Visual Associations, Not Entities (Block B2 & B3 Alignment)
The canonical data architecture defines entities (`Character`, `Location`, `Event`, etc.) and relationships (`FamilyRelationship`). There is no `Marriage` or `Union` entity table in B2. In F11, unions, marriages, and consort relationships are rendered as connecting associative junctions linking two or more characters, never as fabricated entity nodes with profile pages or synthetic slugs.

#### 4. Asymmetrical and Complex Unions are First-Class
The Mahābhārata and epic literature feature complex genealogical structures:
- **Polygynous Unions**: Figures with multiple partners across distinct genealogical branches.
- **Polyandrous Unions**: Figures with multiple partners sharing parental links.
- **Niyoga & Divine Progenitors**: Figures with both social/legal lineages and divine or biological progenitors attested by primary traditions.
- **Single-Parent Lines & Foundlings**: Figures recorded with a single parent or discovered origins.
F11 visualizes these structures accurately as attested by canonical data, without shoehorning them into simplistic nuclear-family assumptions.

#### 5. Accessible Equivalent Representation (Constitutional Principle 10)
Genealogical trees are complex visual diagrams. Users who cannot perceive visual spatial layouts must be provided with an equivalent, fully navigable hierarchical semantic representation. Block F16 authoritatively governs concrete ARIA attributes, keyboard traps, and screen-reader announcements; Block F17 validates accessibility conformance.

---

## 3. Scope of Block F11

### 3.1 In-Scope
- UX purpose and information architecture of the Lineage Lens (`/lineage`, `/lineage/:slug`).
- Conceptual data transformation projecting canonical `FamilyRelationship` rows into a generational DAG.
- Generational axis conventions: primary directionality, generational tier indexing, and lateral ordering.
- Visual representation of unions: single unions, multiple partners, and non-marital or divine parentage attested by canonical data.
- Parentage semantics: biological, social, divine, single-parent lineages, and half-sibling branches as supplied by backend claims.
- Generational depth conceptual controls and branch expansion/collapse.
- Disconnected lineages, multi-root dynastic overviews, and fragmented ancestral segments.
- Epistemic status rendering across all six canonical B2 states (`epistemic_status`: `known`, `unknown`, `not_researched`, `not_applicable`, `conflicting`, `approximate`).
- Lineage-specific filtering: generational depth, union visibility toggles, and direct-line emphasis.
- Responsive adaptation across the four Block F4 viewport classes (Compact, Medium, Expanded, Wide).
- Dual-mode accessibility model: visual generational DAG and semantic companion navigator.
- Evidence drawer trigger boundaries connecting lineage associations to Block F15.
- Conceptual URL state synchronization and deep-linking via Block F7 (`/lineage`, `/lineage/:slug`).
- Client state ownership across URL (F7), Server Cache (F6), Lineage Lens State (F11), and UI State (F5).
- Performance and rendering boundaries: conceptual tiers across Semantic DOM, SVG, Canvas/WebGL, or hybrid rendering.
- Security against malicious injection in ancestral labels, epithets, and deep links.

### 3.2 Out-of-Scope (Non-Goals)
- Implementing concrete layout algorithms, tree calculation engines, or selecting npm libraries (deferred to Stage 4).
- Free-form multi-entity topological relationship networks ($D \le 2$ general graph owned by Block F10).
- Chronological timeline tracks and Parva event sequences (owned by Block F12: Timeline & Chronology).
- Geographic distribution of kingdoms and territorial conquests (owned by Block F13: Geographic Map).
- Battlefield formation diagrams and tactical combat lines (owned by Block F14: War & Tactical Vyuhas).
- Primary textual citation rendering and verse-level evidence drawer content (owned by Block F15: Evidence Drawer).
- System-wide contrast tokens, keyboard event listeners, and global ARIA implementations (owned by Block F16).
- Quantitative performance budgets, benchmark runners, and bundle size limits (owned by Block F17).
- Native genealogical export file formats (e.g., GEDCOM, GraphML) or print rendering engines are outside the scope of F11.

---

## 4. Lineage Lens Role in the Explorer Ecosystem

The Lineage Lens provides the deep generational perspective of the Mahābhārata. While the Character View (Block F9) details an individual's personal life and direct kin, and the Graph Lens (Block F10) captures immediate multi-domain connections ($D \le 2$), the Lineage Lens visualizes the historical continuity of bloodlines, ancestries, and dynastic successions across generations:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   LINEAGE LENS ROLE & CONTEXTUAL BRIDGES               │
├───────────────────────────────────┬────────────────────────────────────┤
│ User Mental Model                 │ Exploration Objective              │
├───────────────────────────────────┼────────────────────────────────────┤
│ "How are two major figures        │ Shared ancestral lineage, common   │
│  genealogically related?"         │ forebears, and generational steps. │
├───────────────────────────────────┼────────────────────────────────────┤
│ "Who are the children of a figure │ Attributed offspring per union     │
│  and through which partners?"     │ partner without cross-conflation.  │
├───────────────────────────────────┼────────────────────────────────────┤
│ "What is the broader dynastic     │ Multi-generational lineage from    │
│  descent of a royal house?"       │ ancestral roots to descendants.    │
├───────────────────────────────────┼────────────────────────────────────┤
│ "What differing parentage accounts│ Competing or dual traditions       │
│  are recorded for this figure?"   │ visualized with evidence claims.   │
└───────────────────────────────────┴────────────────────────────────────┘
```

### 4.1 Lens Demarcation: Lineage (F11) vs. Graph (F10)
To avoid architectural overlap and maintain system clarity:

| Architectural Dimension | Graph Lens (Block F10) | Lineage Lens (Block F11) |
| :--- | :--- | :--- |
| **Primary Topology** | Topological network (undirected or mixed directed graph) | Generational Directed Acyclic Graph (DAG) |
| **Exploration Depth** | Strictly bounded radius ($D \le 2$ hops) | Generational depth bounded by generational lineage traversal |
| **Axis Constraints** | Free spatial layout; no invariant spatial axis | Strict generational axis (temporal hierarchy along one primary dimension) |
| **Entity Types** | Heterogeneous (Characters, Locations, Events, Groups, Wars) | Homogeneous (Characters only) |
| **Relationship Scope**| Canonical B2/B3 relationship types | Canonical B2/B3 kinship types from `FamilyRelationship` only |
| **Marriage / Unions** | Symmetric relationship edge | Explicit associative union junctions with attributed child branches |
| **Canonical Route** | `/graph`, `/graph/:slug` | `/lineage`, `/lineage/:slug` |

---

## 5. Canonical Family Data Boundary & Backend Consumption

### 5.1 Authoritative Backend Source (Block B2 & B3)
The Lineage Lens consumes kinship data strictly from the canonical backend models established in Stage 1:
- `characters` table (Block B2 §4.1): Character identity, canonical name, IAST diacritics, script representation, epithets, and gender.
- `family_relationships` table (Block B2 §4.6, Block B3 §10): Specialized character-to-character edges.
- `claims` table (Block B2 §4.11, Block B4): Backing epistemic justification for parentage, marriage, and variant lineage records.

### 5.2 Canonical Kinship Semantics
Kinship relationship types, endpoint semantics, directionality, inverse semantics, and traversal rules are consumed exactly as defined by the authoritative B2/B3 canonical family model. F11 does not enumerate, rename, normalize, or reinterpret canonical family relationship types.

Visual categories are described generically (parent-child links, spousal/partner associations, sibling groupings, and generational ascendants/descendants), while all canonical semantics remain strictly under B2/B3 authority.

### 5.3 API Consumption Contract (Block B5 & F6)
The Lineage Lens consumes kinship data via the canonical API and domain contracts established by Stage 1 (Blocks B5 and F6). F11 does not define or invent specific REST endpoint paths, query parameter specifications, or backend query traversal implementation details. Exact endpoint contracts and payload envelopes remain the authority of the API architecture, with F11 consuming canonical character kinship associations and multi-generational lineages as provided by the backend interface.

---

## 6. Family Tree Projection Model

The lineage projection engine transforms a set of flat character records and typed kinship edges into a structured generational visual model.

```
┌────────────────────────────────────────────────────────────────────────┐
│                      LINEAGE PROJECTION PIPELINE                       │
├────────────────────────────────────────────────────────────────────────┤
│ 1. INGEST CANONICAL RECORDS: Characters, Kinship Edges, Claims.        │
├────────────────────────────────────────────────────────────────────────┤
│ 2. RESOLVE GENERATIONAL LEVELS: Derive generational offset relative to │
│    focal entity through parent-child lineage traversal.                │
├────────────────────────────────────────────────────────────────────────┤
│ 3. CONSTRUCT UNION ASSOCIATIONS: Identify partner pairs and formal     │
│    spousal edges. Create non-entity Union Junctions linking partners.  │
├────────────────────────────────────────────────────────────────────────┤
│ 4. MAP CHILD ATTRIBUTIONS: Attach children to their specific union     │
│    junction (or single parent if other parent is unrecorded).          │
├────────────────────────────────────────────────────────────────────────┤
│ 5. ORDER LATERAL SIBLINGS: Sort siblings and branches deterministically│
│    (by narrative seniority where recorded, or deterministic slug sort).│
├────────────────────────────────────────────────────────────────────────┤
│ 6. EMIT VISUAL GENERATIONAL DAG: Output structured layout tiers.       │
└────────────────────────────────────────────────────────────────────────┘
```

### 6.1 Non-Entity Union Junctions
To represent marital unions and joint parentage without fabricating entity nodes:
- A **Union Junction** is a purely visual/layout construct. It has no character ID, no slug, and no profile page.
- It acts as a connector between partner nodes.
- Children born of that specific union descend directly from the Union Junction, establishing unambiguous parentage attribution.

### 6.2 The Zero-Fabrication Rule in Tree Projection
When a character has only one recorded parent in canonical records:
- **Rule**: Do **NOT** synthesize an "Unknown" spouse or "Unknown Mother/Father" node to populate a binary tree template.
- **Projection**: The child descends directly from the single known parent node via an individual parent-child line, bypassing any union junction.
- This preserves absolute epistemic truthfulness: if canonical records attest only a single parent, the explorer reflects precisely what the sources attest without placeholder entities.

---

## 7. Generation-Axis Architecture

### 7.1 Generational Axis Invariants
1. **Single Invariant Generational Axis**: Generational depth is mapped consistently along a single primary spatial axis.
   - The ordering of ancestors and descendants remains monotonic along that axis.
   - The active spatial orientation (e.g., vertical progression or horizontal flow) is a presentation choice that may adapt according to responsive viewport requirements and implementation layout needs, with vertical progression serving as a standard presentation default.
2. **Generational Equidistance**: Characters belonging to the same derived generational tier relative to the focal entity align to the same generational baseline coordinate along the primary axis.
3. **Monotonic Generational Direction**: Descendant nodes must never precede ancestor nodes along the directional flow of the generational axis.
4. **Cross-Generational Unions**: Where partners span different generational levels in canonical data, the layout resolves the connection without distorting the global generational tier alignment.

### 7.2 Generational Model & Tier Derivation
Generation is a deterministic structural property derived from canonical parent-child lineage traversal relative to the active focal entity:
- **Focal Baseline**: The focal entity defines the relative generational baseline (offset 0).
- **Direct Ascendants**: Ascending traversals across parent edges derive successive ancestral tiers (negative generational offsets).
- **Direct Descendants**: Descending traversals across child edges derive successive descendant tiers (positive generational offsets).
- **Collateral Positions**: Sibling, spousal, and collateral figures are positioned relative to their genealogical generation where derivable from the canonical data model. F11 does not hard-code fixed generation assignments for relationship categories independent of data traversal.

---

## 8. Marriage & Union Architecture

Epic genealogy encompasses diverse marital and parental arrangements. The lineage architecture accommodates these structures natively based strictly on canonical data.

### 8.1 Multiple Partner Unions
When canonical records attest multiple spouses or partners for a character:
- Each union is represented as an independent union junction.
- Offspring born to each partner are grouped under that specific union junction.
- **Visual Invariant**: Children of one partner are never visually grouped with children of another partner. Half-siblings remain distinct while sharing the common parent link.

### 8.2 Joint and Complex Unions
When canonical records attest a figure with multiple partners sharing parental relationships:
- Each partner pairing forms an explicit union junction to which their respective offspring attach.
- Siblings and half-siblings maintain clear parentage attribution to their specific parental pairings without layout ambiguity.

### 8.3 Multiple Parentage Claims & Diverse Progenitors
Where ancient traditions or recensions record multiple parentages for a figure (such as biological vs. social/legal parentage, divine invocations, or adoptive parents):
- F11 must **never** choose between biological, social, legal, divine, adoptive, or other parentage interpretations, nor prioritize one parentage category over another as a structural spine.
- Canonical B2/B3 family data and the associated epistemic claim model determine what relationships exist and how they are classified.
- F11 presents all canonical parentage relationships attested by backend data, utilizing visual differentiation (such as distinct edge styling and prominent epistemic/provenance indicators) to distinguish differing parentage claims without conflating them or discarding variant traditions.
- Activating a parentage affordance connects directly to the canonical Block F15 evidence experience.

---

## 9. Parent, Child & Sibling Semantics

### 9.1 Sibling Relationships and Lateral Ordering
- **Canonical Sibling Semantics**: Sibling grouping, full-sibling distinctions, and half-sibling distinctions are derived strictly from canonical B2/B3 relationship records and must never be inferred solely from visual topology or layout adjacency.
- **Lateral Ordering Principle**: Within a sibling group, nodes are ordered along the lateral axis based on canonical seniority metadata where recorded in backend data. Where seniority is unrecorded or uncertain, ordering defaults to a deterministic presentation-only fallback (such as alphabetical sorting by canonical slug) to ensure visual layout stability.
- **Presentation Fallback Invariant**: A presentation-only fallback ordering exists solely to prevent visual layout flicker across renders; it must never be interpreted as implying birth order, seniority, chronology, or relationship semantics.

### 9.2 Incomplete Parentage & Asymmetric Records
Ancient genealogies frequently omit one parent or mythological origins:
- When only a father is recorded in canonical data, the child branch attaches directly to the father node.
- When only a mother is recorded in canonical data, the child branch attaches directly to the mother node.
- No placeholder nodes or empty boxes are rendered. Missing parentage is presented honestly as an epistemic reality of the record.

---

## 10. Ancestor & Descendant Exploration Model

### 10.1 Generational Depth vs. Graph Hops
In Block F10 (Graph Lens), exploration is strictly bounded to a radial topological distance of two hops ($D \le 2$). In Block F11 (Lineage Lens), exploration is structured along the **generational axis**:
- Exploration is bounded along the generational axis to maintain visual legibility and payload efficiency.
- Initial views display a bounded generational window relative to the focal entity, with user controls enabling generational expansion or contraction as supported by backend query capabilities.
- Unbounded global rendering of deep dynastic trees is avoided to maintain cognitive clarity and rendering responsiveness.

### 10.2 Branch Expansion & Collapse
To explore deep lineages without overwhelming the viewport:
- Characters with further unrendered ancestors or descendants display an interactive branch expansion indicator.
- Triggering an expansion dynamically loads or unfolds the connected branch along the generational axis.
- Collapsing a branch folds descendant or ancestor tiers under that node, maintaining stable coordinates for unaffected nodes.
- Expansion state is tracked locally in UI state and avoids jarring global layout oscillations.

---

## 11. Disconnected Branches, Multiple Roots & Incomplete Lineages

### 11.1 Lineage Overview (`/lineage`)
When accessed at the root route `/lineage` without a specific character slug:
- The view serves as the generic lineage exploration entry point and lineage overview.
- Content, featured dynastic entry points, and high-level lineage selections are determined dynamically by available canonical data and Stage 4 product implementation.
- Multiple lineage roots may be presented with clear visual separation, allowing inter-lineage marital connections to form explicit bridges when attested by canonical data.

### 11.2 Lineage Islands & Fragmented Records
Figures whose genealogical connections are isolated or fragmentary render as self-contained lineages:
- If no ancestral connection to larger lineages exists in canonical records, the lineage view honestly renders the isolated branch without fabricating connections.

---

## 12. Epistemic-State & Conflicting Traditions Presentation

Genealogy in ancient epic literature contains contested parentages, regional variations, and symbolic traditions. Lineage visualization embodies the project's core principle of Epistemic Honesty.

### 12.1 Presentation of the Six Canonical B2 Epistemic States
Every character node and kinship link displays its epistemic state via non-color-differentiated visual tokens, strictly matching the six canonical B2 epistemic states:

```
┌────────────────────────────────────────────────────────────────────────┐
│               EPISTEMIC STATE VISUAL GRAMMAR IN LINEAGE                │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Epistemic State  │ Presentation Requirement                            │
│ (Block B2 §6)    │                                                     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `known`          │ Standard visual treatment for the `known` state as  │
│                  │ defined by B2/F3; state meaning is not redefined by │
│                  │ F11. Non-color visual indicator.                    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `conflicting`    │ Non-color visual indicator distinguishing multiple  │
│                  │ canonical claims; dual branches rendered where      │
│                  │ attested by backend data.                           │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `approximate`    │ Non-color visual indicator distinguishing           │
│                  │ approximate status defined by B2/F3.                │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `unknown`        │ Non-color visual indicator distinguishing explicitly│
│                  │ unknown status defined by B2/F3.                    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `not_researched` │ Non-color visual indicator distinguishing unresearched│
│                  │ status defined by B2/F3.                            │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `not_applicable` │ Non-color visual indicator distinguishing           │
│                  │ not-applicable status defined by B2/F3.             │
└──────────────────┴─────────────────────────────────────────────────────┘
```

> **Visual Design Boundary**: F11 defines the semantic requirement that epistemic state must never rely on color alone. Block B2 authoritatively defines epistemic state meaning; Block F3 defines global visual design system tokens; Block F16/F17 govern accessibility and verification.

### 12.2 Conflicting Parentage Accounts
When different recensions or scholarly traditions assert conflicting parentage:
- The backend stores separate `FamilyRelationship` records, each carrying `epistemic_status = 'conflicting'` and an individual `claim_id`.
- The frontend renders both candidate parentage lines with distinct visual callouts and links to their backing claims.
- Activating either branch invokes the canonical Block F15 evidence experience, presenting the primary textual claims supporting each account.
- The UI never arbitrarily selects one tradition and discards the other.

---

## 13. Lineage Filtering Architecture

To enable clear analytical inquiry without altering underlying data, the Lineage Lens provides non-destructive client-side filters:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        LINEAGE FILTER CONTROLS                         │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Filter Dimension │ Conceptual Functionality                            │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Generational   │ Controls the upstream and downstream generational   │
│   Depth**        │ span rendered relative to the focal entity.         │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Union / Spouse │ Toggles display of marital partners and collateral  │
│   Visibility**   │ marital branches.                                   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Direct Lineage │ Highlights the direct lineage connecting the focus  │
│   Emphasis**     │ character to a selected forebear or descendant,     │
│                  │ visually subduing collateral branches.              │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Epistemic      │ Toggles visibility or emphasis of approximate or    │
│   Certainty**    │ conflicting parentage branches.                     │
└──────────────────┴─────────────────────────────────────────────────────┘
```

### 13.1 Non-Destructive Filtering Invariant
Filtering adjusts visibility, opacity, and layout compaction only. It never modifies cached API responses or deletes nodes from client-side data stores.

---

## 14. Responsive Recomposition Across Viewport Classes

Lineage trees expand along generational and lateral dimensions. Presentation adapts across the four Block F4 viewport classes without relying on arbitrary pixel thresholds:

```
┌────────────────────────────────────────────────────────────────────────┐
│             RESPONSIVE LINEAGE RECOMPOSITION ARCHITECTURE              │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Viewport Class   │ Layout Paradigm & Recomposition Strategy            │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Compact**      │ - Visual canvas adapts into a structured, vertical  │
│ (Handhelds /     │   **Generational Explorer** with expandable cards.  │
│  Small Screens)  │ - Ancestors above, focal character in center,       │
│                  │   descendants below. Optional spatial mini-map.     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Medium**       │ - 2D spatial canvas with active touch panning and   │
│ (Tablets /       │   pinch-to-zoom. Collateral branches default to     │
│  Foldables)      │   collapsed indicators to conserve space.           │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Expanded**     │ - Multi-generational DAG with visible partner       │
│ (Desktops /      │   branches and lateral sibling cohorts.             │
│  Laptops)        │ - Generational axis ruler and docked detail panel.  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Wide**         │ - Expansive multi-dynasty lineage canvas.           │
│ (Ultra-wide /    │ - Simultaneous side-by-side comparison of distinct  │
│  Multi-Monitor)  │   lineages with persistent evidence drawer sidebar. │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 15. Dual-Mode Accessibility & Semantic Navigator

Lineage trees are inherently visual diagrams that present barriers to screen-reader and keyboard users if implemented purely as visual canvases.

### 15.1 The Dual-Mode Architecture
Block F11 mandates a **Dual-Mode Presentation Model**:
1. **Interactive Visual Canvas**: Optimized for spatial navigation, generational overviews, and visual discovery.
2. **Accessible Semantic Companion**: A fully structured, screen-reader accessible hierarchical HTML representation (e.g., nested semantic lists with proper generational headings, landmark regions, and kinship relationship statements).

```
┌────────────────────────────────────────────────────────────────────────┐
│                   DUAL-MODE ACCESSIBILITY STRUCTURE                    │
├───────────────────────────────────┬────────────────────────────────────┤
│ Visual Generational Canvas        │ Accessible Semantic Companion      │
│ (Spatial Visual View)             │ (Screen-Reader / Keyboard Tree)    │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Visual layout coordinates       │ - `<nav aria-label="Lineage">`     │
│ - Connecting lines and junctions  │ - `<ol>` nested generational tiers │
│ - Spatial pan and zoom            │ - Explicit semantic parent, child, │
│ - Visual hover and focus states   │   partner, and sibling statements  │
│                                   │ - Fully keyboard-traversable nodes │
│                                   │ - Screen-reader expansion feedback │
└───────────────────────────────────┴────────────────────────────────────┘
```

### 15.2 Boundary with Block F16 and Block F17
- **F11 Ownership**: Defines the semantic companion model, generational hierarchy representation, and equivalence requirements.
- **F16 Ownership**: Authoritatively defines concrete ARIA roles (`role="tree"`, `role="treeitem"`, `aria-expanded`), roving tabindex implementations, focus traps, and screen-reader announcements.
- **F17 Ownership**: Defines automated accessibility verification tests, screen-reader test scripts, and WCAG 2.2 Level AA quality gates.

---

## 16. Evidence & Provenance Integration (Block F15 Boundary)

Every genealogical connection is subject to scholarly textual grounding.

### 16.1 Evidence Affordance Boundary
- Character nodes and kinship connecting lines feature an unobtrusive evidence affordance.
- Activating the evidence affordance invokes the canonical **Block F15 Evidence Drawer** for the relevant canonical claim or relationship.
- F11 does not prescribe the internal event dispatch mechanism or payload format, nor does it render inline textual verses or footnotes inside the tree canvas. All citation inspection is strictly delegated to Block F15.

---

## 17. Routing, Navigation & Deep-Linking Integration (Block F7)

The Lineage Lens conforms to the canonical route taxonomy established in Block F7 §4:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        CANONICAL LINEAGE ROUTES                        │
├──────────────────────────┬─────────────────────────────────────────────┤
│ Route Path Pattern       │ Exploration Perspective                     │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/lineage`               │ Lineage Exploration Entry                   │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/lineage/:slug`         │ Ancestral & Descendant Lineage DAG          │
└──────────────────────────┴─────────────────────────────────────────────┘
```

### 17.1 Conceptual URL State & Parameter Alignment
- F11 identifies conceptual URL-worthy lineage exploration state (e.g., active focal entity, generational span, partner visibility, and direct-line emphasis).
- **F7 Ownership Boundary**: Block F7 alone authoritatively owns whether and how client state is serialized into URL query parameters. F11 does not create, define, or enforce any query parameter contracts. All parameter naming, serialization formats, validation, and defaults remain strictly under Block F7 authority and Stage 4 coordination.

### 17.2 Cross-Lens Deep-Linking
Every character node in the lineage view includes seamless navigation bridges to other exploration lenses:
- **Entity Profile**: Link to `/characters/:slug`
- **Focus Graph**: Link to `/graph/:slug`
- **Chronological Timeline**: Link to `/timeline/:slug`
- **Re-Anchor Lineage**: Link to `/lineage/:slug` (shifts the active focal character of the lineage tree)

---

## 18. Lifecycle, Loading, Empty & Error States

The Lineage Lens manages asynchronous states with complete epistemic honesty:

```
┌────────────────────────────────────────────────────────────────────────┐
│                       LINEAGE LIFECYCLE STATES                         │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Lifecycle State  │ Visual & Semantic Behavior                          │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Initial        │ Generational skeleton tiers display placeholder     │
│   Loading**      │ cards with shimmer animation along the generational │
│                  │ axis. Layout preserves stable generational heights. │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Incremental    │ When expanding an ancestor or descendant branch,    │
│   Expansion**    │ only the target branch displays localized loading;  │
│                  │ existing nodes remain stable and fully interactive. │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Empty Kinship  │ When an entity record returns no associated family  │
│   Result**       │ relationships, the UI distinguishes between:        │
│                  │ - No kinship relationships currently returned       │
│                  │ - Kinship explicitly `unknown` in canonical sources │
│                  │ - Kinship marked `not_researched` (in progress)     │
│                  │ - Kinship marked `not_applicable`                   │
│                  │ - Kinship marked `conflicting`                      │
│                  │ Avoids falsely claiming the corpus is complete.     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Lineage Not    │ Display standard 404 entity view with search        │
│   Found**        │ recommendations, conforming to Block F7 error specs.│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Network / API  │ Inline non-destructive retry banner allowing user   │
│   Error**        │ to re-attempt data fetch without losing local state.│
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 19. Performance & Rendering Boundaries

Lineage structures vary widely in size, from small direct-family clusters to large multi-generational dynastic segments.

### 19.1 Rendering Strategy Spectrum (Library-Neutral)
The implementation may select semantic DOM, structured SVG, Canvas/WebGL, or hybrid rendering strategies based on topological complexity, accessibility requirements, device capability, and performance verification:
1. **Semantic DOM / CSS Grid Tier**: Well-suited for Compact viewports, linear generational explorers, and low-density trees. Provides direct DOM accessibility and zero overhead.
2. **Structured SVG Tier**: Well-suited for Medium and Expanded viewports with moderate generational depth. Provides scalable vector precision, native event handling, and accessible SVG structure.
3. **Canvas / WebGL / Hybrid Tier**: Well-suited for wide-canvas multi-dynasty views with high node density, offloading drawing operations to maintain smooth interaction.

### 19.2 Quantitative Verification Boundary
F11 defines conceptual rendering tiers and architectural invariants only. Fixed element-count thresholds, frame rate targets, and bundle size budgets are strictly owned and verified by **Block F17**.

---

## 20. Security & Untrusted Data Protections

1. **Safe Text Ingestion**: All character names, epithets, and genealogical notes from backend payloads are treated as untrusted data and rendered strictly via safe DOM text nodes or sanitized framework properties. Raw HTML injection is strictly prohibited.
2. **Deep-Link Slug Sanitization**: Slugs received from URL parameters (`/lineage/:slug`) are validated against canonical alphanumeric kebab-case patterns before initiating backend requests, mitigating script injection or path traversal attempts.

---

## 21. Cross-Lens Transitions

Navigation between the Lineage Lens and other exploration lenses preserves user exploration continuity:
- **Entity View $\leftrightarrow$ Lineage**: Selecting "View Lineage" from a character profile opens `/lineage/:slug` centered on that character. Returning navigates directly back to `/characters/:slug`.
- **Lineage $\leftrightarrow$ Focus Graph**: A character node in the lineage view offers a "View in Graph" action, transitioning smoothly to `/graph/:slug` with the focus preserved.
- **Lineage $\leftrightarrow$ Timeline**: Navigating from a character in lineage opens the canonical Timeline lens for that character, preserving applicable supported exploration context.

---

## 22. State Ownership Distribution

State management strictly follows the hierarchy established in Block F5:

```
┌────────────────────────────────────────────────────────────────────────┐
│                       STATE OWNERSHIP HIERARCHY                        │
├──────────────────┬──────────────────┬──────────────────────────────────┤
│ State Scope      │ Governing Layer  │ Managed State Attributes         │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **URL State**    │ Block F7 Router  │ Canonical route path (`/lineage`,│
│                  │                  │ `/lineage/:slug`) and parameters │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Server Cache** │ Block F6 Query   │ Cached Character, Kinship, and   │
│                  │ Client           │ Claim API response payloads      │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Lens State**   │ Lineage Lens     │ Expanded/collapsed branch IDs,   │
│                  │ State            │ active highlighting paths        │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Transient UI** │ Local Component  │ Pan/zoom canvas matrix, tooltip  │
│                  │ State            │ coordinates, hover highlights    │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Preferences**  │ Block F5 Store   │ Global display preferences       │
│                  │                  │ (e.g., text size scaling)        │
└──────────────────┴──────────────────┴──────────────────────────────────┘
```

---

## 23. Architectural Ownership Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                     ARCHITECTURAL OWNERSHIP MATRIX                     │
├────────────────────────────┬─────────────┬─────────────────────────────┤
│ Architectural Concern      │ Owner Block │ Boundary Description        │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Canonical Kinship Schema   │ Block B2/B3 │ DB tables, FKs, CTE queries │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Kinship API Endpoints      │ Block B5    │ REST contracts & envelopes  │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Lineage Visual DAG Model   │ Block F11   │ Generational layout rules   │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ General Relationship Graph │ Block F10   │ 2-hop topological network   │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Lineage Route & URL State  │ Block F7    │ Route grammar & parameters  │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Evidence Drawer Citation   │ Block F15   │ Textual claims & citations  │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Concrete A11y & ARIA       │ Block F16   │ Concrete screen-reader code │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Performance Budgets & Tests│ Block F17   │ Quantitative verification   │
└────────────────────────────┴─────────────┴─────────────────────────────┘
```

---

## 24. Architectural Decision Records (ADRs)

### ADR-F11-01: Lineage as a Generational DAG, Not a Force Graph
- **Context**: Visualizing family trees with generic force-directed graph algorithms results in generational disorder where descendants can appear above ancestors and marriages are indistinguishable from alliances.
- **Decision**: Architect family lineage as a structured, generational Directed Acyclic Graph (DAG) with an invariant generational axis.
- **Consequences**: Guarantees intuitive dynastic comprehension and genealogical clarity.

### ADR-F11-02: Zero Fabrication of Unrecorded Spouses or Parents
- **Context**: Standard genealogical software assumes strict binary trees (every person must have two parents) and synthesizes "Unknown" dummy cards when records are incomplete.
- **Decision**: Prohibit the creation of synthetic dummy entities. Render single-parent lines directly to their known parent.
- **Consequences**: Enforces absolute epistemic honesty and honors the ancient textual reality of epic sources.

### ADR-F11-03: Visual Union Junctions Over Entity-Level Marriages
- **Context**: Marriages could be modeled either as first-class entity nodes or as visual connectors between characters.
- **Decision**: Treat unions strictly as associative visual junctions, aligning with backend models where marriage is an edge, not a row in an entity table.
- **Consequences**: Prevents polluting the entity graph with non-character profile routes while enabling clear attribution of children to specific partner pairings.

### ADR-F11-04: Dual-Mode Accessibility with Semantic Companion Navigator
- **Context**: Visual DAG canvases are difficult or impossible to interpret via screen readers and keyboard navigation alone.
- **Decision**: Require an accessible semantic companion representation presenting full hierarchical generational trees alongside the visual canvas. Concrete ARIA and announcements are owned by F16; verification is owned by F17.
- **Consequences**: Fully complies with Constitutional Principle 10 (Accessibility as a First-Class Requirement).

### ADR-F11-05: Strict Boundary Between Generational Lineage and Topological Graph
- **Context**: Overlap between the general relationship graph (`/graph`) and the family tree (`/lineage`) risks duplicate implementation and confused UX.
- **Decision**: Strictly isolate general $D \le 2$ multi-entity relationships to Block F10, while dedicating Block F11 exclusively to generational kinship DAGs.
- **Consequences**: Clean separation of concerns, reusable components, and distinct user mental models.

---

## 25. Requirement Traceability Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                   REQUIREMENT TRACEABILITY MATRIX                      │
├───────────────────┬──────────────────┬─────────────────────────────────┤
│ Upstream Req ID   │ Source Document  │ Block F11 Architectural Mapping │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REQ-CORE-01**   │ Constitution;    │ Section 2.2, Section 5: Unified │
│                   │ Blueprint §1     │ kinship data consumed from B2/B3│
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REQ-CORE-02**   │ Constitution;    │ Section 4.1, Section 23: Strict │
│                   │ Rule 02          │ demarcation between F10 and F11.│
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REQ-GRP-05**    │ Constitution;    │ Section 2.2, Section 6.2: Zero  │
│                   │ Rule 03; B3 §7   │ dummy entity fabrication.       │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REL-004**       │ PRD §9.5; B3 §10 │ Section 6, Section 7: Dedicated │
│                   │                  │ generational family tree DAG.   │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **SRC-005**       │ PRD §9.10; B3 §5 │ Section 12: Conflicting parentage│
│                   │                  │ and multi-claim visualization.  │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **NAV-001**       │ PRD §9.12; F7 §4 │ Section 17: Canonical routes    │
│                   │                  │ `/lineage`, `/lineage/:slug`.   │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **A11Y-001**      │ PRD §9.13; F3 §4 │ Section 15: Dual-mode semantic  │
│                   │                  │ companion representation.       │
└───────────────────┴──────────────────┴─────────────────────────────────┘
```

---

## 26. F11 Exit Criteria & Acceptance Checklist

- [x] Lineage lens purpose, UX role, and distinct boundary against F10 established.
- [x] Read-only projection model defined transforming `FamilyRelationship` rows into a generational DAG.
- [x] Strict generational axis invariants (monotonic generational flow along a consistent axis) defined.
- [x] Marriages and unions architected as visual associative junctions without fabricating entity nodes.
- [x] Complex marital and parental arrangements formally addressed without choosing between traditions.
- [x] Parent, child, and sibling semantics specified with zero synthetic "Unknown" dummy entities.
- [x] Generational exploration model defined along generational axis and distinguished from F10's $D \le 2$ limit.
- [x] Lineage overview and disconnected lineage handling specified generically without hard-coded lists.
- [x] Epistemic status states (`epistemic_status`) strictly match the six canonical B2 states with non-color indicators per Block F3.
- [x] Conflicting parentage accounts visualized with distinct claim triggers to Block F15.
- [x] Lineage filtering architecture defined conceptually without data mutation.
- [x] Responsive recomposition mapped across the four Block F4 viewport classes.
- [x] Dual-mode accessibility model (Visual DAG + Accessible Companion Navigator) architected with F16/F17 demarcation.
- [x] Canonical routes `/lineage` and `/lineage/:slug` preserved; query parameters left for Stage 4 / F7 coordination.
- [x] High-frequency interaction state isolated from URL/storage per Block F5.
- [x] Performance architecture defines conceptual rendering tiers (DOM, SVG, Canvas/WebGL) with budgets deferred to F17.
- [x] Security and untrusted data protections specified for text rendering and deep links.
- [x] Zero application source code, UI packages, or component files were introduced.
- [x] Zero Stage 1 backend documents or Blocks F1–F10 documents were modified.
