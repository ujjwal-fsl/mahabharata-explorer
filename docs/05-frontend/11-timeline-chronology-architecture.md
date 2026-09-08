# Chronology & Timeline Architecture (Block F12)

## 1. Document Status & Purpose

- **Document Identifier**: Block F12
- **Status**: Approved Architectural Specification
- **Stage**: Stage 2 — Frontend Architecture
- **Location**: `docs/05-frontend/11-timeline-chronology-architecture.md`
- **Upstream Dependencies**:
  - `docs/04-backend/02-data-architecture.md` (Stage 1 Block B2)
  - `docs/04-backend/03-knowledge-graph.md` (Stage 1 Block B3)
  - `docs/04-backend/04-evidence-and-provenance.md` (Stage 1 Block B4)
  - `docs/04-backend/05-api-architecture.md` (Stage 1 Block B5)
  - `docs/04-backend/06-search-architecture.md` (Stage 1 Block B6)
  - `docs/04-backend/09-performance-and-caching.md` (Stage 1 Block B9)
  - `docs/04-backend/10-data-ingestion-architecture.md` (Stage 1 Block B10)
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
  - `docs/05-frontend/10-lineage-family-tree-architecture.md` (Block F11)
- **Downstream Consumers**:
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
│ - Block B2: Event & Sequence DB   │ ► BLOCK F12: CHRONOLOGY & TIMELINE │
│ - Block B3: Narrative Traversal   │   - F13: Geographic Map Lens       │
│ - Block B5: API Event Endpoints   │   - F14: War & Tactical Vyuhas     │
│ - Block F3: Visual Grammar        │   - F15: Evidence Drawer UX        │
│ - Block F4: Viewport Classes      │   - F16: A11y & Typography Engine  │
│ - Block F5: State Hierarchy       │   - F17: Testing & Budgets         │
│ - Block F6: API Fetching/Caching  │                                    │
│ - Block F7: URL Grammar & Routing │                                    │
│ - Block F9: Entity Views Launch   │                                    │
│ - Block F10: Graph Visualization  │                                    │
│ - Block F11: Family Lineage DAG   │                                    │
└───────────────────────────────────┴────────────────────────────────────┘
```

This document establishes the frontend **architecture** for the Mahābhārata Explorer's Chronology and Timeline visualization lens (`/timeline`, `/timeline/:slug`). Chronology in epic narrative spans deep ancestral eras, foundational narrative episodes, court assemblies, wanderings and exile periods, diplomatic missions, multi-day structured sequences, and post-conflict aftermaths.

Block F12 defines the visual, structural, responsive, and accessible projection of these temporal sequences. In strict accordance with the project constitution, F12 treats timeline visualization not as a conventional Gregorian calendar application, but as a **structured narrative-chronological exploration lens** governed by Zero Fabrication, Epistemic Honesty, and Accessible Equivalents: relative narrative sequence and epistemic status (`epistemic_status`) are projected without synthesizing arbitrary historical calendar dates or artificial chronological precision.

---

## 2. Architectural Context & Foundational Principles

### 2.1 Pure Architectural Specification & Library Neutrality
Block F12 is an architectural specification only. It defines projection models, chronological axis representations, sequence grouping rules, epistemic state rendering, responsive transformations, and accessible semantic companions. It does **not**:
- Implement React components, hooks, or custom web elements.
- Select or mandate concrete third-party timeline, graphics, or visualization libraries.
- Introduce application source code, package dependencies, or bundle configurations.
- Introduce direct database queries, SQL CTEs, or backend schema changes.

### 2.2 Foundational Principles

#### 1. Knowledge Explorer First, Not a Generic Calendar (Constitutional Principle 1)
Epic narrative time is structured primarily through narrative sequence, relative ordering, episodic intervals, and generational continuity. The Timeline Lens is designed for scholarly exploration of epic events, their interrelationships, and participating entities, rather than forcing narrative incidents into fixed modern calendar grids.

#### 2. Chronological Ordering is Not Automatically Absolute Dating (Constitutional Principle 4 & 13)
The architecture rigorously distinguishes between:
- Absolute or astronomical dates where canonically cataloged with backing scholarly claims.
- Approximate or relative chronological ordering (`date_precision = 'approximate' | 'relative'`).
- Undated or floating narrative episodes.
- Competing or conflicting chronological claims between recensions and scholarly traditions.
Under no circumstance may the frontend synthesize calendar dates, solar years, reign durations, or chronological certainty where canonical records attest only relative sequence or narrative order.

#### 3. Preserving Inherent Chronological Uncertainty (Rule 03 & Epistemic Honesty)
When two events lack canonical ordering data or their relative sequence is contested:
- The UI must preserve that epistemic reality rather than inventing an arbitrary linear sequence.
- Events sharing identical sequence metrics or lacking definitive ordering are rendered as contemporaneous, clustered, or parallel tracks, accompanied by prominent epistemic indicators.

#### 4. Multiple Competing Chronological Traditions are First-Class
Different recensions and scholarly traditions propose differing sequences or historical horizons for epic events:
- Competing accounts are preserved as parallel or tagged chronological tracks anchored to distinct claims.
- F12 never arbitrarily selects one scholarly theory or tradition as "the" authoritative timeline at the expense of others.

#### 5. Accessible Equivalent Representation (Constitutional Principle 10)
Temporal canvases and chronological scrollers can present severe barriers to screen-reader and non-pointer users. The timeline experience mandates an equivalent, fully navigable hierarchical semantic representation. Block F16 authoritatively governs concrete ARIA attributes, keyboard traps, and screen-reader announcements; Block F17 validates accessibility conformance.

---

## 3. Scope of Block F12

### 3.1 In-Scope
- UX purpose and information architecture of the Timeline Lens (`/timeline`, `/timeline/:slug`).
- Conceptual data transformation projecting canonical `Event`, `EventParticipant`, and associated entity records into a chronological visualization.
- Chronological representation models: relative narrative sequencing, absolute dates where available, approximate intervals, and undated episodes.
- Sequence axis conventions: primary directionality, temporal grouping, and non-linear spacing.
- Visual architecture: chronological tracks, sequence bands, event cards, uncertainty spans, and parallel tradition branches.
- Density management, temporal clustering, and progressive disclosure for long narrative arcs.
- Focal-character chronology: contextual event filtering centered on an individual entity (`/timeline/:slug`).
- Event selection and inspection architecture: participants, location context, martial context, epistemic status, and evidence affordances.
- Epistemic-state rendering using the project-wide canonical B2 vocabulary where applicable, while respecting the field-level state subset defined by B2.
- Competing chronological traditions presentation without harmonization or data loss.
- Non-destructive client-side filtering: entity participation, event classification, geographic location, and epistemic state.
- Responsive recomposition across the four Block F4 viewport classes (Compact, Medium, Expanded, Wide).
- Dual-mode accessibility model: visual chronological canvas and semantic companion timeline navigator.
- Evidence drawer trigger boundaries connecting chronological claims to Block F15.
- Conceptual URL state synchronization and deep-linking via Block F7 (`/timeline`, `/timeline/:slug`).
- Client state ownership across URL (F7), Server Cache (F6), Timeline Lens State (F12), and UI State (F5).
- Performance and rendering boundaries: conceptual tiers across Semantic DOM, SVG, Canvas/WebGL, or hybrid rendering.
- Security against malicious injection in event titles, summaries, and deep links.

### 3.2 Out-of-Scope (Non-Goals)
- Implementing concrete layout algorithms, timeline rendering loops, or selecting npm libraries (deferred to Stage 4).
- Free-form multi-entity topological relationship networks ($D \le 2$ general graph owned by Block F10).
- Multi-generational genealogical trees and kinship DAGs (owned by Block F11: Family Lineage Tree).
- Geographic coordinate mapping, spatial bounds, and cartographic projections (owned by Block F13: Geographic Map).
- Battlefield formation geometry, tactical troop arrays, and daily combat movements (owned by Block F14: War & Tactical Vyuhas).
- Primary textual citation rendering and verse-level evidence drawer content (owned by Block F15: Evidence Drawer).
- System-wide contrast tokens, keyboard event listeners, and global ARIA implementations (owned by Block F16).
- Quantitative performance budgets, benchmark runners, and bundle size limits (owned by Block F17).
- Native calendar export file formats (e.g., iCalendar, ICS) or print layout engines are outside the scope of F12.

---

## 4. Timeline Lens Role in the Explorer Ecosystem

The Timeline Lens provides the chronological and narrative perspective of the Mahābhārata. While the Character View (Block F9) details an individual's attributes and direct associations, the Graph Lens (Block F10) captures immediate multi-domain topologies ($D \le 2$), and the Lineage Lens (Block F11) visualizes generational descent, the Timeline Lens articulates how narrative episodes unfold over time:

```
┌────────────────────────────────────────────────────────────────────────┐
│                  TIMELINE LENS ROLE & CONTEXTUAL BRIDGES               │
├───────────────────────────────────┬────────────────────────────────────┤
│ User Mental Model                 │ Exploration Objective              │
├───────────────────────────────────┼────────────────────────────────────┤
│ "In what sequence did these       │ Relative narrative progression,    │
│  major epic episodes occur?"      │ sequence indices, and milestones.  │
├───────────────────────────────────┼────────────────────────────────────┤
│ "What key events did this figure  │ Entity chronological trajectory    │
│  participate in over their life?" │ filtered across narrative phases.  │
├───────────────────────────────────┼────────────────────────────────────┤
│ "Where does an event sit within   │ Macro-narrative context (e.g.,     │
│  the broader epic structure?"     │ narrative phase, campaign, or era).│
├───────────────────────────────────┼────────────────────────────────────┤
│ "What events are contested in     │ Divergent sequence traditions and  │
│  chronological sequence?"         │ approximate episodic intervals.    │
└───────────────────────────────────┴────────────────────────────────────┘
```

### 4.1 Lens Demarcation Matrix
To avoid architectural overlap and maintain system clarity across Stage 2 lenses:

| Architectural Dimension | Graph Lens (F10) | Lineage Lens (F11) | Timeline Lens (F12) | Geographic Lens (F13) | War Lens (F14) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Primary Organizing Axis** | Free spatial topology | Generational hierarchy | Temporal / Narrative sequence | Spatial coordinates (Lat/Long) | Tactical formation / War day |
| **Core Entity Focus** | Heterogeneous network | Characters only | Events & EventParticipants | Locations & Regions | Wars, WarDays, Formations |
| **Primary Traversal** | Radial distance ($D \le 2$) | Generational offsets | Chronological sequence indices | Spatial proximity & regions | Daily tactical progression |
| **Canonical Route** | `/graph`, `/graph/:slug` | `/lineage`, `/lineage/:slug` | `/timeline`, `/timeline/:slug` | `/geography`, `/geography/:slug` | `/wars/:war_slug`, `/vyuhas/:slug` |

---

## 5. Canonical Chronology Data Boundary & Backend Consumption

### 5.1 Authoritative Backend Source (Block B2 & B3)
The Timeline Lens consumes chronological data strictly from canonical Stage 1 models:
- `events` table (Block B2 §4.2): Event identity, title, summary, `date_value`, `date_precision`, `chronology_status`, `sequence_index`, `location_id`, `war_id`, `war_day_id`, and `metadata`.
- `event_participants` table (Block B2 §4.2, Block B3 §8): Character participation links, roles, and character metadata.
- `locations` table (Block B2 §4.3): Associated geographic landmarks and kingdoms where events took place.
- `wars` and `war_days` tables (Block B2 §4.7, §4.8): Structural campaign containment for martial chronologies.
- `claims` table (Block B2 §4.11, Block B4): Epistemic claims and source citations backing event occurrence and chronological placement.

### 5.2 Canonical Chronological Attributes
Chronological attributes are consumed exactly as defined by the authoritative B2 canonical model:
- `sequence_index`: Numeric ordering value for relative chronological sorting.
- `date_value`: Textual chronological descriptor (e.g., relative era markers, narrative phases, or absolute scholarly propositions where cataloged).
- `date_precision`: Categorical precision indicator (`exact`, `approximate`, `relative`, `unknown`).
- `chronology_status`: Epistemic status classification (`epistemic_status`) consumed from the authoritative B2 model. While Block B2 §5.1 defines the project-wide six-state vocabulary (`known`, `unknown`, `not_researched`, `not_applicable`, `conflicting`, `approximate`), individual epistemic fields support only the semantically applicable subset established by B2. F12 consumes the field-level states actually permitted by B2 for `events.chronology_status` and does not define, expand, or alter the allowed state set.

> **Architectural Invariant**: F12 does not create, rename, or invent chronological attributes. All sequence evaluations, precision states, and epistemic statuses are derived deterministically from canonical backend data.

### 5.3 API Consumption Contract (Block B5 & F6)
The Timeline Lens consumes event and chronological data via the canonical API and domain contracts established by Stage 1 (Blocks B5 and F6). F12 does not define or invent specific REST endpoint paths, query parameter specifications, or backend query traversal logic. Exact endpoint contracts, filtering parameters, and response envelopes remain the authority of the API architecture, with F12 consuming canonical event collections and participant records as provided by the backend interface.

---

## 6. Chronological Representation Model

A fundamental challenge of epic literature is that narrative time cannot be mapped onto a uniform linear calendar. F12 architects a multi-tiered chronological representation model supporting diverse temporal structures:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   CHRONOLOGICAL REPRESENTATION TIERS                   │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Temporal State   │ Projection Strategy & Visual Semantics              │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Absolute /     │ Positioned along a scaled temporal baseline where   │
│   Astronomical** │ astronomical or historical year proposals exist as  │
│                  │ claim-backed assertions. Visual metric scale.       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Relative       │ Positioned along an ordinal narrative axis driven   │
│   Sequence**     │ by `sequence_index`. Spacing represents sequence    │
│                  │ progression rather than fixed calendar durations.   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Approximate    │ Rendered with flexible interval bounds (e.g.,       │
│   Intervals**    │ shaded sequence spans or bracketed bands) indicating│
│                  │ uncertainty in start, duration, or conclusion.      │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Contemporaneous│ Positioned in parallel lateral tracks or clustered  │
│   / Clustered**  │ cohorts sharing a common sequence baseline.         │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Undated /      │ Grouped within broad narrative phases or contextual │
│   Floating**     │ containers without asserting precise sequence steps.│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Conflicting    │ Rendered as distinct parallel chronological tracks, │
│   Traditions**   │ allowing competing recension timelines to coexist.  │
└──────────────────┴─────────────────────────────────────────────────────┘
```

### 6.1 Ordinal vs. Metric Temporal Spacing
- **Ordinal Spacing (Default)**: In narrative sections where duration is unrecorded, spacing between events is normalized or dynamically weighted by visual density, rather than implying empty centuries or arbitrary day gaps.
- **Metric Spacing**: Where explicit durations or consecutive daily sequences exist in canonical data (e.g., structured multi-day campaigns), spacing reflects proportional intervals.

---

## 7. Timeline Projection Pipeline

The timeline projection engine transforms flat event records, participant associations, and claims into a structured chronological visualization.

```
┌────────────────────────────────────────────────────────────────────────┐
│                      TIMELINE PROJECTION PIPELINE                      │
├────────────────────────────────────────────────────────────────────────┤
│ 1. INGEST CANONICAL RECORDS: Events, Participants, Locations, Claims.  │
├────────────────────────────────────────────────────────────────────────┤
│ 2. RESOLVE ORDERING METRICS: Sort events by canonical `sequence_index`.│
│    Isolate events with identical indices or missing sequence data.     │
├────────────────────────────────────────────────────────────────────────┤
│ 3. CONSTRUCT TEMPORAL CLUSTERS: Group contemporaneous or closely       │
│    spaced events to prevent overlapping card collisions.               │
├────────────────────────────────────────────────────────────────────────┤
│ 4. IDENTIFY COMPETING TRACKS: Partition events carrying conflicting    │
│    chronological claims into distinct parallel visual tracks.          │
├────────────────────────────────────────────────────────────────────────┤
│ 5. ATTACH CONTEXTUAL ANCHORS: Link participating characters, locations,│
│    and evidence affordances to each projected event node.              │
├────────────────────────────────────────────────────────────────────────┤
│ 6. EMIT CHRONOLOGICAL VISUAL MODEL: Output structured sequence bands.  │
└────────────────────────────────────────────────────────────────────────┘
```

### 7.1 The Zero-Fabrication Rule in Timeline Projection
- Under no circumstance may the projection engine invent intermediate events to bridge gaps between narrative episodes.
- If the time elapsed between two consecutive events is unmentioned in canonical sources, the timeline does not display an estimated duration or synthetic date range.

---

## 8. Ordering Semantics & Display Hierarchy

F12 strictly distinguishes between canonical sequence, chronological dates, relative ordering, and presentation fallbacks:

```
┌────────────────────────────────────────────────────────────────────────┐
│                     CHRONOLOGICAL ORDERING SEMANTICS                   │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Ordering Type    │ Authority, Source & Invariant Boundary              │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Canonical      │ Authoritative numeric order (`sequence_index`)      │
│   Sequence**     │ supplied by backend data for timeline projection.   │
│                  │ Governs relative sorting; does not establish        │
│                  │ absolute historical dating, does not silently       │
│                  │ resolve conflicting chronological claims, and must  │
│                  │ not be treated as a substitute for evidence.        │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Chronological  │ Descriptive or absolute label (`date_value`)        │
│   Date / Label** │ accompanied by `date_precision`. Informational only;│
│                  │ does not supersede `sequence_index`.                │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Relative       │ Episodic containment (e.g., events within a named   │
│   Containment**  │ narrative phase, campaign, or episodic section).    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Presentation   │ Deterministic tie-breaker (e.g., alphabetical sort  │
│   Fallback**     │ by event slug) used solely to prevent layout flicker│
│                  │ when `sequence_index` values are identical.         │
└──────────────────┴─────────────────────────────────────────────────────┘
```

> **Sequence Index Authority Boundary**: While `sequence_index` provides canonical narrative ordering supplied by the backend for timeline projection, it does not establish absolute historical dating, does not silently resolve conflicting chronological claims, and must not be treated as a substitute for chronological evidence.

> **Presentation Fallback Invariant**: A presentation-only fallback ordering exists solely to guarantee deterministic UI rendering across browser sessions. It must never be presented as indicating historical sequence, narrative priority, or chronological order.

---

## 9. Timeline Visual Architecture

The visual architecture balances scholarly rigor with modern visual clarity:

```
┌────────────────────────────────────────────────────────────────────────┐
│                      TIMELINE VISUAL ARCHITECTURE                      │
├────────────────────────────────────────────────────────────────────────┤
│ [Macro Chronology Scrubber / Overview Minimap]                         │
│   ├── Narrative phase indicators & density histogram                  │
│   └── Draggable viewport window for long chronological arcs            │
├────────────────────────────────────────────────────────────────────────┤
│ [Primary Chronological Track]                                          │
│   ├── Temporal Axis (Monotonic sequence line with milestone markers)   │
│   ├── Period & Phase Header Bands (Contextual narrative eras)          │
│   ├── Event Cards:                                                     │
│   │     ├── Event Title & Canonical Identifier                         │
│   │     ├── Chronological Descriptor & Precision Badge                 │
│   │     ├── Geographic Location Pill (with map link)                   │
│   │     ├── Participating Characters (with profile links)              │
│   │     ├── Non-Color Epistemic Status Indicator                       │
│   │     └── Evidence Affordance ([Evidence ↗] triggering F15)          │
│   └── Uncertainty Spans (Visual brackets indicating floating bounds)   │
├────────────────────────────────────────────────────────────────────────┤
│ [Competing Chronological Tracks]                                       │
│   └── Parallel sequence tracks for conflicting scholarly traditions    │
└────────────────────────────────────────────────────────────────────────┘
```

### 9.1 Axis Orientation Invariant
1. **Single Invariant Chronological Axis**: Sequence progression is mapped consistently along a single primary spatial axis.
2. **Monotonic Progression**: Events with higher sequence indices must never precede events with lower sequence indices along the directional flow of the axis.
3. **Presentation Orientation**: Vertical progression (top-to-bottom) is the standard presentation default, while horizontal progression (left-to-right) may be selected according to viewport class and implementation requirements.

---

## 10. Dense Timeline Handling & Progressive Disclosure

Epic chronologies can encompass hundreds of closely spaced events interspersed with long narrative pauses. The architecture provides robust density management:

### 10.1 Multi-Scale Progressive Disclosure
1. **Macro Level (Narrative Phases & Periods)**: High-level overview displaying major narrative divisions, cumulative event densities, and epochal transitions.
2. **Meso Level (Episodic Clusters)**: Events sharing common narrative themes or brief episodic spans grouped into collapsible sequence clusters (e.g., displaying an aggregate card: *"Cluster: Multiple Events"*).
3. **Micro Level (Individual Event Nodes)**: Fully expanded event cards displaying complete participant rosters, location links, descriptive summaries, and evidence affordances.

### 10.2 Anti-Collision & Cluster Unfolding
- When multiple events share identical or near-identical sequence coordinates, the visual engine arranges cards in alternating lateral positions or stacks them in a clear ordinal cohort.
- Activating a cluster smoothly unfolds the constituent events along the primary axis without disorienting layout shifts.

---

## 11. Focal-Character Chronology (`/timeline/:slug`)

When accessed with an entity slug parameter (`/timeline/:slug`), the Timeline Lens adapts into an **Entity-Centric Chronological Lens**:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   FOCAL-CHARACTER TIMELINE ADAPTATION                  │
├───────────────────────────────────┬────────────────────────────────────┤
│ Global Timeline (`/timeline`)     │ Focal Timeline (`/timeline/:slug`) │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Universal epic narrative arc    │ - Chronological milestones of the  │
│ - All canonical events rendered   │   specified focal character        │
│ - Global narrative phase context  │ - Events where entity participates │
│ - Macro dynastic progression      │   are emphasized and highlighted   │
│                                   │ - Non-participating global events  │
│                                   │   subdued or collapsed into bounds │
│                                   │ - Contextual biographical link to  │
│                                   │   Character Profile (`/characters`)│
└───────────────────────────────────┴────────────────────────────────────┘
```

### 11.1 Contextual Retention Invariant
The focal-character timeline does not erase the broader epic timeline; it visually contextualizes the individual's milestones against the macro-narrative backbone, ensuring that the user understands when an event occurred relative to the wider narrative context.

---

## 12. Event Detail & Interaction Architecture

Selecting an individual event card exposes its full structured context through in-situ inspection (e.g., detail panel or contextual inspection view) without requiring a full-page navigation away from the chronological sequence. F12 does not create a dedicated event route; `/timeline/:slug` is strictly the F7-defined entity-centric timeline route. If navigation to an entity is appropriate, the inspection interface bridges directly to canonical entity routes established by F7.

```
┌────────────────────────────────────────────────────────────────────────┐
│                        EVENT DETAIL INSPECTION                         │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Detail Zone      │ Contextual Information & Functional Affordances     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Header**       │ Event title, canonical slug, narrative phase, and   │
│                  │ non-color epistemic status badge.                   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Chronology**   │ Display of `date_value`, `date_precision`, and      │
│                  │ sequence metrics as attested in canonical data.     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Narrative**    │ Curated scholarly summary of the event.             │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Participants** │ Roster of participating characters with explicit    │
│                  │ role badges and links to `/characters/:slug`.       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Location**     │ Associated location link to Geographic Lens         │
│                  │ (`/geography/:slug`) when canonical location exists.│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Martial**      │ Associated conflict link to War Lens (`/wars/:war_slug`)│
│                  │ when canonical war entity exists.                   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Evidence**     │ Evidence affordance invoking the Block F15 Evidence │
│                  │ Drawer for the backing claim(s).                    │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 13. Chronological Uncertainty & Epistemic-State Presentation

Temporal statements in ancient literature carry varying degrees of historical certainty. F12 embodies the project's core principle of Epistemic Honesty by strictly utilizing the six canonical B2 epistemic states (`epistemic_status`):

```
┌────────────────────────────────────────────────────────────────────────┐
│              EPISTEMIC STATE VISUAL GRAMMAR IN TIMELINE                │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Epistemic State  │ Presentation Requirement                            │
│ (Block B2 §6)    │                                                     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `known`          │ Standard visual treatment for the `known` state as  │
│                  │ defined by B2/F3; state meaning is not redefined by │
│                  │ F12. Non-color visual indicator.                    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `conflicting`    │ Non-color visual indicator distinguishing multiple  │
│                  │ competing claims; distinct competing claims or      │
│                  │ parallel tracks may be visually distinguished;      │
│                  │ state meaning is not redefined by F12.              │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `approximate`    │ Non-color visual indicator using the standard       │
│                  │ treatment defined by B2/F3; state meaning is not    │
│                  │ redefined by F12.                                   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `unknown`        │ Non-color visual indicator using the standard       │
│                  │ treatment defined by B2/F3; state meaning is not    │
│                  │ redefined by F12.                                   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `not_researched` │ Non-color visual indicator using the standard       │
│                  │ treatment defined by B2/F3; state meaning is not    │
│                  │ redefined by F12.                                   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `not_applicable` │ Non-color visual indicator using the standard       │
│                  │ treatment defined by B2/F3; state meaning is not    │
│                  │ redefined by F12.                                   │
└──────────────────┴─────────────────────────────────────────────────────┘
```

> **Visual Design Boundary**: F12 defines the semantic requirement that epistemic state must never rely on color alone. Block B2 authoritatively defines epistemic state meaning; Block F3 defines global visual design system tokens; Block F16/F17 govern accessibility and verification.

---

## 14. Conflicting Chronology & Competing Traditions

When different recensions, astronomical models, or scholarly traditions assert conflicting sequences or dates for events:
- For events with conflicting chronological records supplied by the canonical backend data architecture, the frontend renders competing accounts as **Parallel Chronological Tracks** or clearly tagged branch tracks.
- Each track clearly displays its scholarly tradition or textual recension source as provided by the data model.
- Activating a branch or event affordance invokes the canonical Block F15 evidence experience, exposing the primary text citations and arguments supporting each tradition.
- The UI never arbitrarily selects one chronological tradition and discards the other.

---

## 15. Timeline Filtering Architecture

To enable analytical inquiry without modifying underlying data, the Timeline Lens provides non-destructive client-side filters:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        TIMELINE FILTER CONTROLS                        │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Filter Dimension │ Conceptual Functionality                            │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Participant /  │ Filters events to those involving a specific        │
│   Entity**       │ character or group of entities.                     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Narrative      │ Limits display to events belonging to a specific    │
│   Period / Phase**│ narrative phase, epic book, or campaign interval.   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Location**     │ Filters events that occurred at a specific landmark │
│                  │ or kingdom.                                         │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Epistemic      │ Toggles visibility or emphasis of approximate or    │
│   Certainty**    │ conflicting chronological records.                  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Martial / War**│ Isolates combat events belonging to specific wars   │
│                  │ or campaign days.                                   │
└──────────────────┴─────────────────────────────────────────────────────┘
```

### 15.1 Non-Destructive Filtering Invariant
Filtering adjusts visibility, visual emphasis, and layout clustering only. It never mutates cached API responses or deletes events from client-side data stores.

---

## 16. Responsive Recomposition Across Viewport Classes

The timeline experience adapts across the four Block F4 viewport classes without relying on arbitrary pixel thresholds:

```
┌────────────────────────────────────────────────────────────────────────┐
│             RESPONSIVE TIMELINE RECOMPOSITION ARCHITECTURE             │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Viewport Class   │ Layout Paradigm & Recomposition Strategy            │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Compact**      │ - Single-column vertical chronological feed.        │
│ (Handhelds /     │ - Sticky narrative phase headers as user scrolls.   │
│  Small Screens)  │ - Compact event cards with expandable details.      │
│                  │ - Bottom-sheet drawer for event inspection.         │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Medium**       │ - Single-track vertical timeline with adjacent      │
│ (Tablets /       │   contextual milestones.                            │
│  Foldables)      │ - Collapsible macro scrubber docked at top.         │
│                  │ - Modal or sliding panel for event details.         │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Expanded**     │ - Multi-track chronological canvas with parallel    │
│ (Desktops /      │   branches for conflicting traditions.              │
│  Laptops)        │ - Sticky macro overview scrubber on the left.       │
│                  │ - Docked event inspection panel on the right.       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Wide**         │ - Expansive multi-track chronological matrix.       │
│ (Ultra-wide /    │ - Simultaneous side-by-side comparison of narrative │
│  Multi-Monitor)  │   events, geographic movements, and character arcs. │
│                  │ - Persistent Evidence Drawer sidebar.               │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 17. Dual-Mode Accessibility & Semantic Navigator

Visual chronological canvases and dynamic scrubbers present barriers to screen-reader and non-pointer users.

### 17.1 The Dual-Mode Architecture
Block F12 mandates a **Dual-Mode Presentation Model**:
1. **Interactive Chronological Canvas**: Optimized for spatial scanning, sequence navigation, and visual overview.
2. **Accessible Semantic Companion**: A fully structured, screen-reader accessible hierarchical HTML representation (e.g., nested semantic lists with landmark regions, chronological milestone headings, and explicit temporal relationship statements).

```
┌────────────────────────────────────────────────────────────────────────┐
│                   DUAL-MODE ACCESSIBILITY STRUCTURE                    │
├───────────────────────────────────┬────────────────────────────────────┤
│ Visual Chronological Canvas       │ Accessible Semantic Companion      │
│ (Spatial Visual View)             │ (Screen-Reader / Keyboard Feed)    │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Visual timeline axis            │ - `<nav aria-label="Chronology">`  │
│ - Geometric connecting milestones │ - `<ol>` ordered chronological list│
│ - Interactive pan, zoom & scrub   │ - Explicit semantic sequence, date,│
│ - Visual hover and focus rings    │   location, and participant items  │
│                                   │ - Fully keyboard-traversable events│
│                                   │ - Screen-reader milestone notices  │
└───────────────────────────────────┴────────────────────────────────────┘
```

### 17.2 Boundary with Block F16 and Block F17
- **F12 Ownership**: Defines the semantic companion model, chronological hierarchy representation, and equivalence requirements.
- **F16 Ownership**: Authoritatively defines concrete ARIA roles (`role="feed"`, `role="article"`, `aria-current`), roving tabindex implementations, focus management, and screen-reader announcements.
- **F17 Ownership**: Defines automated accessibility verification tests, screen-reader test scripts, and WCAG 2.2 Level AA quality gates.

---

## 18. Evidence & Provenance Integration (Block F15 Boundary)

Every event and chronological placement in the Mahābhārata is subject to scholarly textual grounding.

### 18.1 Evidence Affordance Boundary
- Event cards and sequence milestones feature an unobtrusive evidence affordance (e.g., a scholarly citation badge).
- Activating the evidence affordance invokes the canonical **Block F15 Evidence Drawer** for the relevant canonical claim or event record.
- F12 does not prescribe the internal event dispatch mechanism or payload format, nor does it render inline textual verses or footnotes inside the timeline canvas. All citation inspection is strictly delegated to Block F15.

---

## 19. Routing, Navigation & Deep-Linking Integration (Block F7)

The Timeline Lens conforms to the canonical route taxonomy established in Block F7 §4:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        CANONICAL TIMELINE ROUTES                       │
├──────────────────────────┬─────────────────────────────────────────────┤
│ Route Path Pattern       │ Exploration Perspective                     │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/timeline`              │ Universal Narrative Chronology & Sequence   │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/timeline/:slug`        │ Entity-Centric Chronological Trajectory     │
└──────────────────────────┴─────────────────────────────────────────────┘
```

### 19.1 Conceptual URL State & Parameter Alignment
- F12 identifies conceptual URL-worthy timeline exploration state (e.g., active focal entity, selected narrative phase, participant filter, and active chronological track).
- **F7 Ownership Boundary**: Block F7 alone authoritatively owns whether and how client state is serialized into URL query parameters. F12 does not create, define, or enforce any query parameter contracts. All parameter naming, serialization formats, validation, and defaults remain strictly under Block F7 authority and Stage 4 coordination.

### 19.2 Cross-Lens Deep-Linking
Every event node in the timeline view includes seamless navigation bridges to canonical entity views established by F7:
- **Entity Profile**: Link to participating characters at `/characters/:slug`.
- **Focus Graph**: Link to `/graph/:slug` centered on a participating character or entity when canonical entity context exists.
- **Lineage Tree**: Selecting a character with canonical lineage context provides a navigation bridge to `/lineage/:slug`.
- **Geographic Map**: Selecting an event location with canonical geography context opens `/geography/:slug`.
- **War Campaign**: Selecting an event associated with a canonical war entity opens `/wars/:war_slug`.

---

## 20. Lifecycle, Loading, Empty & Error States

The Timeline Lens manages asynchronous states with complete epistemic honesty:

```
┌────────────────────────────────────────────────────────────────────────┐
│                       TIMELINE LIFECYCLE STATES                        │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Lifecycle State  │ Visual & Semantic Behavior                          │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Initial        │ Chronological skeleton tiers display placeholder    │
│   Loading**      │ cards with shimmer animation along the sequence     │
│                  │ axis. Layout preserves stable track heights.        │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Incremental    │ When expanding or scrolling along a narrative phase,│
│   Loading**      │ localized loading indicators display without        │
│                  │ displacing currently visible event milestones.      │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Empty Event    │ When a focal entity, period, or filter query yields │
│   Result**       │ no matching event records, the UI displays a clean  │
│                  │ informational state stating that no canonical events│
│                  │ match the current exploration scope. Absence of     │
│                  │ matching records is strictly distinguished from     │
│                  │ epistemic uncertainty: the UI never implies that    │
│                  │ "no events found" means chronology is unknown, not  │
│                  │ researched, not applicable, or conflicting.         │
│                  │ Epistemic states are rendered only where applicable │
│                  │ to corresponding canonical fields and claims.       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Timeline Not   │ Display standard 404 entity view with search        │
│   Found**        │ recommendations, conforming to Block F7 error specs.│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Network / API  │ Inline non-destructive retry banner allowing user   │
│   Error**        │ to re-attempt data fetch without losing local state.│
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 21. Cross-Lens Transitions

Navigation between the Timeline Lens and other exploration lenses preserves user exploration continuity:
- **Timeline $\leftrightarrow$ Character**: Selecting a participant opens `/characters/:slug`. Navigating from a character profile opens `/timeline/:slug` centered on that character's milestones.
- **Timeline $\leftrightarrow$ Focus Graph**: Selecting a participating character or entity provides an affordance to navigate to `/graph/:slug` when canonical entity context exists.
- **Timeline $\leftrightarrow$ Lineage**: Selecting a character with canonical lineage context provides a navigation bridge to `/lineage/:slug`.
- **Timeline $\leftrightarrow$ Geography**: Selecting an event location with canonical geography context opens `/geography/:slug`.
- **Timeline $\leftrightarrow$ War**: Selecting an event associated with a canonical war entity opens `/wars/:war_slug`.

---

## 22. State Ownership Distribution

State management strictly follows the hierarchy established in Block F5:

```
┌────────────────────────────────────────────────────────────────────────┐
│                       STATE OWNERSHIP HIERARCHY                        │
├──────────────────┬──────────────────┬──────────────────────────────────┤
│ State Scope      │ Governing Layer  │ Managed State Attributes         │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **URL State**    │ Block F7 Router  │ Canonical route path (`/timeline`│
│                  │                  │ `/timeline/:slug`) & parameters  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Server Cache** │ Block F6 Query   │ Cached Event, Participant, and   │
│                  │ Client           │ Claim API response payloads      │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Lens State**   │ Timeline Lens    │ Expanded cluster IDs, active     │
│                  │ State            │ chronological track selection    │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Transient UI** │ Local Component  │ Scrubber drag position, tooltip  │
│                  │ State            │ coordinates, hover highlights    │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Preferences**  │ Block F5 Store   │ Global display preferences       │
│                  │                  │ (e.g., text size scaling)        │
└──────────────────┴──────────────────┴──────────────────────────────────┘
```

---

## 23. Performance & Rendering Boundaries

Timeline views can range from brief episodic sequences to extensive multi-century dynastic chronologies.

### 23.1 Rendering Strategy Spectrum (Library-Neutral)
The implementation may select semantic DOM, structured SVG, Canvas/WebGL, or hybrid rendering strategies based on chronological density, accessibility requirements, device capability, and performance verification:
1. **Semantic DOM / Virtualized Feed Tier**: Well-suited for Compact viewports, linear narrative feeds, and low-to-moderate event densities. Provides direct DOM accessibility and zero overhead.
2. **Structured SVG Tier**: Well-suited for Medium and Expanded viewports with multi-track branches and sequence bands. Provides scalable vector precision, native event handling, and accessible metadata.
3. **Canvas / WebGL / Hybrid Tier**: Well-suited for expansive wide-canvas macro chronologies spanning dense sequences across competing traditions, offloading drawing operations to maintain smooth scrolling and zooming.

### 23.2 Quantitative Verification Boundary
F12 defines conceptual rendering tiers and architectural invariants only. Fixed element-count thresholds, frame rate targets, and bundle size budgets are strictly owned and verified by **Block F17**.

---

## 24. Security & Untrusted Data Protections

1. **Safe Text Ingestion**: All event titles, narrative summaries, participant roles, and chronological notes from backend payloads are treated as untrusted data and rendered strictly via safe DOM text nodes or sanitized framework properties. Raw HTML injection is strictly prohibited.
2. **Untrusted Route Input Handling**: Handling and canonical validation of URL parameters (`/timeline/:slug`) are deferred strictly to Block F7 routing and validation architecture before any backend queries are initiated.

---

## 25. Architectural Ownership Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                     ARCHITECTURAL OWNERSHIP MATRIX                     │
├────────────────────────────┬─────────────┬─────────────────────────────┤
│ Architectural Concern      │ Owner Block │ Boundary Description        │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Canonical Event Schema     │ Block B2/B3 │ DB tables, FKs, sequence DB │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Event API Endpoints        │ Block B5    │ REST contracts & envelopes  │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Chronology Visual Model    │ Block F12   │ Narrative sequence layout   │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ General Relationship Graph │ Block F10   │ 2-hop topological network   │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Family Lineage Tree DAG    │ Block F11   │ Generational kinship DAG    │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Timeline Route & URL State │ Block F7    │ Route grammar & parameters  │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Evidence Drawer Citation   │ Block F15   │ Textual claims & citations  │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Concrete A11y & ARIA       │ Block F16   │ Concrete screen-reader code │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Performance Budgets & Tests│ Block F17   │ Quantitative verification   │
└────────────────────────────┴─────────────┴─────────────────────────────┘
```

---

## 26. Architectural Decision Records (ADRs)

### ADR-F12-01: Chronology as an Ordinal Narrative Sequence Rather than a Calendar Grid
- **Context**: Forcing ancient epic narrative into a modern Gregorian calendar grid requires fabricating arbitrary dates, years, and durations.
- **Decision**: Architect timeline visualization primarily around ordinal narrative sequence (`sequence_index`) and relative intervals, reserving absolute dates strictly for claims where canonically cataloged.
- **Consequences**: Eliminates chronological fabrication while reflecting authentic epic narrative structure.

### ADR-F12-02: Strict Preservation of Chronological Uncertainty
- **Context**: Gaps between events or events lacking definitive relative ordering risk being visually forced into arbitrary sequential arrangements.
- **Decision**: Preserve temporal uncertainty by rendering bracketed uncertainty spans, parallel contemporaneity cohorts, and explicit non-color epistemic state tokens.
- **Consequences**: Upholds Epistemic Honesty (Constitutional Principle 3 & 4) and prevents misleading user interpretations.

### ADR-F12-03: Coexistence of Competing Chronological Traditions
- **Context**: Ancient recensions and scholarly traditions propose differing sequences for epic episodes. Selecting a single "authoritative" timeline violates project principles.
- **Decision**: Support parallel chronological tracks and tagged branches that display competing accounts side-by-side, each linking to its backing claims in Block F15.
- **Consequences**: Supports multi-tradition scholarly inquiry without harmonizing divergent accounts.

### ADR-F12-04: Dual-Mode Accessibility with Semantic Companion Navigator
- **Context**: Visual chronological tracks and scrubbers are difficult or impossible to navigate using screen readers or keyboard controls alone.
- **Decision**: Require an accessible semantic companion representation presenting full hierarchical chronological lists alongside the visual canvas. Concrete ARIA and announcements are owned by F16; verification is owned by F17.
- **Consequences**: Fully complies with Constitutional Principle 10 (Accessibility as a First-Class Requirement).

### ADR-F12-05: Strict Boundary Between Timeline, Lineage, and Graph Lenses
- **Context**: Temporal, genealogical, and topological connections intersect across epic figures, risking duplicate or overlapping lens implementations.
- **Decision**: Strictly isolate chronological event sequences to Block F12, generational kinship DAGs to Block F11, and $D \le 2$ relationship networks to Block F10.
- **Consequences**: Clean separation of concerns, reusable components, and distinct user mental models.

---

## 27. Requirement Traceability Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                   REQUIREMENT TRACEABILITY MATRIX                      │
├───────────────────┬──────────────────┬─────────────────────────────────┤
│ Upstream Req ID   │ Source Document  │ Block F12 Architectural Mapping │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REQ-CORE-01**   │ Constitution;    │ Section 2.2, Section 5: Unified │
│                   │ Blueprint §1     │ event data consumed from B2/B3. │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REQ-CORE-02**   │ Constitution;    │ Section 4.1, Section 25: Strict │
│                   │ Rule 02          │ lens demarcation & system reuse.│
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REQ-GRP-05**    │ Constitution;    │ Section 2.2, Section 7.1: Zero  │
│                   │ Rule 03; B3 §7   │ chronological fabrication.      │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REL-002**       │ PRD §9.5; B3 §5  │ Section 5.1, Section 12: Event  │
│                   │                  │ participant relationships.      │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **SRC-005**       │ PRD §9.10; B3 §5 │ Section 13, Section 14: Parallel│
│                   │                  │ competing chronological tracks. │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **NAV-001**       │ PRD §9.12; F7 §4 │ Section 19: Canonical routes    │
│                   │                  │ `/timeline`, `/timeline/:slug`. │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **A11Y-001**      │ PRD §9.13; F3 §4 │ Section 17: Dual-mode semantic  │
│                   │                  │ companion representation.       │
└──────────────────┴──────────────────┴─────────────────────────────────┘
```

---

## 28. F12 Exit Criteria & Acceptance Checklist

- [x] Timeline purpose, UX role, and distinct boundary against F10, F11, F13, F14 established.
- [x] Read-only projection model defined transforming `Event` and `EventParticipant` rows into chronological views.
- [x] Chronological representation models (ordinal narrative sequence, approximate intervals, absolute dates) architected without date fabrication.
- [x] Strict sequence axis invariants (monotonic progression along a consistent axis) defined.
- [x] Density management, progressive disclosure, and event clustering specified conceptually.
- [x] Focal-character chronology (`/timeline/:slug`) architected to contextualize individual milestones within macro-narrative.
- [x] Event detail inspection model defined exposing participants, locations, and evidence affordances.
- [x] Epistemic status states (`epistemic_status`) strictly match the six canonical B2 states with non-color indicators per Block F3.
- [x] Competing chronological traditions visualized via parallel tracks without arbitrary harmonization.
- [x] Timeline filtering architecture defined conceptually without data mutation.
- [x] Responsive recomposition mapped across the four Block F4 viewport classes.
- [x] Dual-mode accessibility model (Visual Canvas + Accessible Companion Navigator) architected with F16/F17 demarcation.
- [x] Canonical routes `/timeline` and `/timeline/:slug` preserved; query-parameter serialization remains exclusively under F7 authority.
- [x] High-frequency interaction state isolated from URL/storage per Block F5.
- [x] Performance architecture defines conceptual rendering tiers (DOM, SVG, Canvas/WebGL) with budgets deferred to F17.
- [x] Security and untrusted data protections specified for text rendering and deep links.
- [x] Zero application source code, UI packages, or component files were introduced.
- [x] Zero Stage 1 backend documents or Blocks F1–F11 documents were modified.
