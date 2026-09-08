# War & Tactical Vyuha Architecture (Block F14)

## 1. Document Status & Purpose

- **Document Identifier**: Block F14
- **Status**: Approved Architectural Specification
- **Stage**: Stage 2 — Frontend Architecture
- **Location**: `docs/05-frontend/13-war-and-tactical-vyuha-architecture.md`
- **Upstream Dependencies**:
  - `docs/04-backend/02-data-architecture.md` (Stage 1 Block B2)
  - `docs/04-backend/03-knowledge-graph.md` (Stage 1 Block B3)
  - `docs/04-backend/04-evidence-and-provenance.md` (Stage 1 Block B4)
  - `docs/04-backend/05-api-architecture.md` (Stage 1 Block B5)
  - `docs/04-backend/06-search-architecture.md` (Stage 1 Block B6)
  - `docs/04-backend/08-media-and-asset-architecture.md` (Stage 1 Block B8)
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
  - `docs/05-frontend/11-timeline-chronology-architecture.md` (Block F12)
  - `docs/05-frontend/12-geographic-map-architecture.md` (Block F13)
- **Downstream Consumers**:
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
│ - Block B2: War, Day & Formation  │ ► BLOCK F14: WAR & VYUHA LENS      │
│ - Block B3: Event & Participant   │   - F15: Evidence Drawer UX        │
│ - Block B5: War & Vyuha Endpoints │   - F16: A11y & Typography Engine  │
│ - Block F3: Visual Grammar        │   - F17: Testing & Budgets         │
│ - Block F4: Viewport Classes      │                                    │
│ - Block F5: State Hierarchy       │                                    │
│ - Block F6: API Fetching/Caching  │                                    │
│ - Block F7: URL Grammar & Routing │                                    │
│ - Block F9: Entity Views Launch   │                                    │
│ - Block F10: Graph Visualization  │                                    │
│ - Block F11: Family Lineage DAG   │                                    │
│ - Block F12: Timeline Lens        │                                    │
│ - Block F13: Geographic Map Lens  │                                    │
└───────────────────────────────────┴────────────────────────────────────┘
```

This document establishes the frontend **architecture** for the Mahābhārata Explorer's War & Tactical Vyuha visualization lens (`/wars`, `/wars/:war_slug`, `/wars/:war_slug/day/:day`, `/vyuhas`, `/vyuhas/:slug`).

The Kurukshetra War represents the epic's dramatic, philosophical, and martial climax, spanning eighteen intensely chronicled days of tactical deployment, massive chariot and infantry combat, martial formations (*vyūhas*), and pivotal individual confrontations. Block F14 defines the structural, interactive, responsive, accessible, and epistemic presentation of canonical war campaigns, daily subdivisions, and military formations.

In strict compliance with the project constitution, F14 projects canonical martial and tactical records into an analytical exploration interface governed by **Zero Fabrication**, **Epistemic Honesty**, and **Accessible Equivalents**: it must never fabricate troop numbers, army sizes, unit positions, formation geometry, battle lines, movement trajectories, tactical maneuvers, battlefield boundaries, distances, sequence of tactical actions, or reconstructed vyuha geometry from textual names alone.

---

## 2. Architectural Context & Foundational Principles

### 2.1 Pure Architectural Specification & Library Neutrality
Block F14 is an architectural specification only. It defines structural projection models, daily battle sequence navigation, tactical fidelity tiers, epistemic status visual semantics (`epistemic_status`), responsive recompositions, and accessible semantic companions. It does **not**:
- Implement React components, canvas renderers, or UI view models.
- Select, mandate, or bundle concrete client-side graphics libraries or rendering engines.
- Introduce application source code, package dependencies, or CSS stylesheets.
- Introduce direct database queries, schema migrations, or backend API modifications.

### 2.2 Foundational Principles

#### 1. Analytical Military History Explorer, Not an RTS Game or Modern Wargame Simulation
Ancient epic warfare consists of poetic, heroic, and doctrinal descriptions of battle orders, royal commanders, divine armaments, and ethical confrontations. The War Lens is designed for scholarly and cultural exploration of canonical conflicts, daily battle sequences, commanders, and textual formations, rather than real-time tactical simulation, interactive gaming mechanics, or synthetic battlefield physics.

#### 2. The Absolute Zero-Fabrication Tactical Rule (Constitutional Principle 4 & Rule 03)
Under no circumstances may the frontend:
- Fabricate troop numbers, casualty figures, akshauhini compositions, or division sizes where canonical data is silent or unrecorded.
- Synthesize coordinate positions, geometric perimeters, or tactical vectors for formations whose textual record provides only a traditional name or metaphorical description.
- Invent spatial battle lines, frontlines, flanking maneuvers, or dynamic movement animations purely to make diagrams visually exciting.
- Derive authoritative geometric formations merely from:
  - A traditional formation name (e.g., assuming a "Krauncha / Heron" formation must look like a modern biological bird layout without canonical coordinate/geometric data).
  - Popular modern illustrations or modern wargame conventions.
  - Textual descriptions that lack concrete structural coordinates.
- Treat missing tactical information as permission to interpolate or synthesize artificial battlefield structure.

#### 3. Epistemic Honesty in Tactical Representation (Constitutional Principle 3 & Epistemic Honesty)
Martial knowledge in ancient texts exhibits widely divergent levels of structural detail:
- Certain days and events feature explicit rosters of participants and combat encounters, while others mention only broad melees.
- Where canonical backend data supplies sufficient authoritative structural information, F14 may present a schematic representation. Where authoritative geometric data is supplied, F14 may present a geometric representation. Where such information is absent or insufficient, F14 falls back to descriptive and semantic presentation.
- Competing claims regarding fallen commanders, daily battle outcomes, or formation structures across recensions must be presented side-by-side rather than artificially harmonized.

#### 4. Conditional Rendering Rule
Where the canonical backend supplies authoritative data sufficient to support a specific representation, F14 projects that representation. Where such authoritative data is not supplied, F14 falls back gracefully to structured semantic and textual representations, never synthesizing missing data.

#### 5. Accessible Equivalent Representation (Constitutional Principle 10)
Tactical battle diagrams and formation layouts pose insurmountable barriers to screen-reader and non-pointer users if implemented purely as graphical visual elements. The War & Tactical Lens mandates a complete, fully navigable semantic companion representation (e.g., structured day-by-day rosters, tactical position breakdowns, participant role lists, and event milestones). Block F16 authoritatively governs concrete ARIA attributes, keyboard navigation, and screen-reader announcements; Block F17 validates accessibility conformance.

---

## 3. Scope & Non-Goals

### 3.1 In-Scope (Block F14 Architectural Authority)
1. **War Overview Architecture (`/wars`, `/wars/:war_slug`)**:
   - Structured presentation of canonical war campaigns (principally the Kurukshetra War).
   - War campaign identity, summary, duration, participating factions/groups, and associated geographic settings.
   - High-level sequence navigation across the WarDay sequence as established by canonical WarDay records; for the V1 Kurukshetra War, these currently comprise 18 days.
2. **War-Day Exploration Architecture (`/wars/:war_slug/day/:day`)**:
   - Daily combat breakdown, commanders and key participants where canonically supplied, and fallen heroes.
   - Chronological sequencing of canonical daily events and tactical milestones.
   - Associated battlefield locations and tactical formations deployed on that specific day.
   - Adjacent-day navigation and deep-linking into canonical event views.
3. **Vyuha / Formation Architecture (`/vyuhas`, `/vyuhas/:slug`)**:
   - Tactical military formations catalog and individual formation detail view.
   - Canonical name, alternate names/transliterations, description, and historical context.
   - Fidelity-tier projection model distinguishing descriptive/semantic, structural/schematic, and authoritative geometric modes.
4. **Tactical Visualization Architecture**:
   - Library-neutral visualization pipeline for martial arrays and formations.
   - Conceptual rendering spectrum (Semantic DOM, Structured SVG, Canvas/WebGL).
5. **Epistemic & Data-Availability Handling**:
   - Project-wide six-state epistemic representation (`known`, `unknown`, `not_researched`, `not_applicable`, `conflicting`, `approximate`) applied to war, day, and formation attributes.
   - Epistemic distinction between absence of data, actively researched data, and conflicting textual traditions.
6. **Cross-Lens Integration & Navigation Bridges**:
   - Deep-linking from wars and formations to Timeline (`/timeline`), Geography (`/geography`), Characters (`/characters`), Knowledge Graph (`/graph`), and Evidence / Provenance via Block F15.
7. **Responsive & Dual-Mode Accessible Transformations**:
   - Viewport recomposition across Compact, Medium, Expanded, and Wide layouts.
   - Structured semantic companion architecture ensuring parity for non-visual interaction.

### 3.2 Out-of-Scope (Strict Non-Goals)
1. **No Application Source Code**: No React components, JSX/TSX files, CSS modules, or client runtime scripts.
2. **No Concrete Mapping / Graphics Library Selection**: No selection, installation, or hard dependency on specific graphics or canvas rendering libraries or SVG frameworks.
3. **No General Graph Engine Duplication**: F14 does not build a general topological network graph; general relationship visualization belongs strictly to Block F10.
4. **No Kinship DAG Duplication**: F14 does not build ancestral or genealogical trees; family trees belong strictly to Block F11.
5. **No Chronology Redefinition**: F14 consumes chronological sequence indexes (`sequence_index`) defined by Block B2 and organized by Block F12; it does not define timeline sequencing logic.
6. **No Geographic Map Redefinition**: F14 consumes location records and links to Block F13; it does not render full subcontinental maps, geographic tiles, or map layers.
7. **No Evidence / Citation UI Ownership**: F14 provides evidence affordances invoking Block F15; it does not render citation drawers, manuscript cards, or text comparison panels.
8. **No Concrete ARIA or Shortcut Implementation**: Concrete keyboard event handlers and ARIA tokens are authoritatively owned by Block F16.
9. **No Numerical Performance Budgets**: Quantitative frame rate targets, milliseconds, and bundle budgets are authoritatively owned by Block F17.

---

## 4. F14 Role in the Explorer Ecosystem

The Mahābhārata Explorer is organized around **One Knowledge Graph, Many Lenses**. The War & Tactical Lens provides the dedicated martial and operational perspective over this unified graph:

```
┌────────────────────────────────────────────────────────────────────────┐
│             UNIFIED KNOWLEDGE GRAPH (BLOCKS B2, B3, B4)                │
│       Characters • Events • Locations • Groups • Wars • Formations      │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │
      ┌─────────────────────────────┼─────────────────────────────┐
      ▼                             ▼                             ▼
┌──────────────┐              ┌──────────────┐              ┌──────────────┐
│ TIMELINE     │              │ GEOGRAPHY    │              │ WAR & VYUHA  │
│ (Block F12)  │              │ (Block F13)  │              │ (Block F14)  │
│ Chronology & │              │ Subcontinent │              │ Campaign,    │
│ Parva Stream │              │ Map & Realms │              │ Days & Vyuhas│
└──────┬───────┘              └──────┬───────┘              └──────┬───────┘
       │                             │                             │
       └─────────────────────────────┼─────────────────────────────┘
                                     ▼
                    ┌─────────────────────────────────┐
                    │ CROSS-LENS NAVIGATION BRIDGES   │
                    │ - War Event ↔ Timeline Marker   │
                    │ - Battlefield ↔ Map Location    │
                    │ - Hero / Warrior ↔ Profile      │
                    │ - Formation ↔ Evidence Drawer   │
                    └─────────────────────────────────┘
```

### 4.1 Boundary Demarcations Across Exploration Lenses
- **Against Block F10 (Focus Graph)**: F10 projects bounded $D \le 2$ topological relationship graphs around entities. F14 projects ordered martial campaigns, daily battle rosters, and tactical formation structures.
- **Against Block F11 (Family Lineage Tree)**: F11 projects multi-generational kinship DAGs. F14 visualizes battlefield alliances, commander hierarchies, and martial units participating in war events.
- **Against Block F12 (Chronology & Timeline)**: F12 owns the global narrative timeline across all 18 Parvas. F14 owns the focused, high-density daily operational breakdown across the canonical WarDay sequence (comprising 18 days for the Kurukshetra War), consuming `Event` records scoped to `war_days`.
- **Against Block F13 (Geographic Map)**: F13 owns spatial cartography, coordinates, and regional territories. F14 links to battlefield locations (e.g., Kurukshetra, Samantapanchaka) and contextualizes where battles occurred without replicating GIS mapping engines.

---

## 5. Canonical War & Tactical Data Boundary

In accordance with Stage 1 Data Architecture (Block B2) and Knowledge Graph Architecture (Block B3), F14 operates strictly over canonical entities and relationships. It introduces zero new tables, fields, or relationship types.

### 5.1 Canonical Entities Consumed by F14

```
┌────────────────────────────────────────────────────────────────────────┐
│                   CANONICAL BACKEND ENTITIES FOR F14                   │
├───────────────────┬────────────────────────────────────────────────────┤
│ Entity (Block B2) │ Primary Role in War Lens                           │
├───────────────────┼────────────────────────────────────────────────────┤
│ `War`             │ Campaign identity, overview, date bounds, metadata │
├───────────────────┼────────────────────────────────────────────────────┤
│ `WarDay`          │ Sequence day number, daily title, summary, metadata│
├───────────────────┼────────────────────────────────────────────────────┤
│ `Formation`       │ Vyuha identity, description, canonical event links │
├───────────────────┼────────────────────────────────────────────────────┤
│ `Event`           │ Discrete chronological combat actions, encounters  │
├───────────────────┼────────────────────────────────────────────────────┤
│ `Character`       │ Commanders, warriors, duelists, fallen heroes      │
├───────────────────┼────────────────────────────────────────────────────┤
│ `Group`           │ Factions, divisions, alliances                     │
├───────────────────┼────────────────────────────────────────────────────┤
│ `Location`        │ Battleground setting, tactical terrain landmarks   │
├───────────────────┼────────────────────────────────────────────────────┤
│ `Claim` /         │ Scholarly assertions and textual citations for     │
│ `Evidence`        │ formations, outcomes, casualties, and recensions   │
└───────────────────┴────────────────────────────────────────────────────┘
```

### 5.2 Canonical Entity Attributes & Structural Foreign Keys
F14 consumes the authoritative fields defined in Block B2:
1. **`War` (Block B2 §4.7)**:
   - `id`, `slug`, `name`, `summary`
   - `start_date_value`, `start_date_precision`, `end_date_value`, `end_date_precision`
   - `status`, `metadata` (e.g., casualty summaries, overall commander listings)
2. **`WarDay` (Block B2 §4.8)**:
   - `id`, `war_id` (foreign key to `wars.id`)
   - `day_number` (`INTEGER NOT NULL CHECK (day_number BETWEEN 1 AND 18)` for the V1 Kurukshetra War)
   - `title`, `summary`, `date_value`, `date_precision`
   - `status`, `metadata` (e.g., daily commanders, key fallen warriors, faction state)
3. **`Formation` (Block B2 §4.9)**:
   - `id`, `slug`, `name`, `summary`, `description`
   - `event_id` (foreign key to `events.id`, nullable)
   - `claim_id` (foreign key to `claims.id`, nullable)
   - `status`, `metadata`
   > *Note on Tactical Fidelity*: Selection among F14's frontend representation tiers (descriptive, schematic, geometric) is determined conditionally by the presence of authoritative structural or geometric data supplied by the backend, rather than an invented schema field.
4. **`Event` (Block B2 §4.2)**:
   - `id`, `slug`, `title`, `summary`, `sequence_index`, `date_value`, `date_precision`, `chronology_status`
   - `location_id` (foreign key to `locations.id`)
   - `war_id` (foreign key to `wars.id`)
   - `war_day_id` (foreign key to `war_days.id`)
5. **`EventParticipant` (Block B3 §3.2)**:
   - `character_id`, `event_id`
   - `role` (`'commander'`, `'warrior'`, `'speaker'`, `'listener'`, `'mediator'`, `'victim'`, `'witness'`)
   - `claim_id`

> **Architectural Constraint**: F14 does not create synthetic fields (such as `troop_count`, `unit_coordinates`, or `formation_angle`) in client state unless explicitly supplied in canonical backend `metadata` or schema fields.

---

## 6. War Exploration Architecture

The War Overview experience (`/wars`, `/wars/:war_slug`) provides high-level narrative, strategic, and factional context for major conflicts.

```
┌────────────────────────────────────────────────────────────────────────┐
│                         WAR OVERVIEW ARCHITECTURE                      │
├────────────────────────────────────────────────────────────────────────┤
│ 1. CAMPAIGN HEADER & IDENTITY                                          │
│    - Canonical Name, Sanskrit title, alternate transliterations        │
│    - Chronological duration & precision badges (Block B2 §4.7)         │
│    - Primary geographic theater (Location link to Block F13)           │
│    - Evidence affordance invoking Block F15                            │
├────────────────────────────────────────────────────────────────────────┤
│ 2. STRATEGIC SUMMARY & NARRATIVE CONTEXT                               │
│    - Curated campaign summary and ethical / philosophical context      │
│    - Causes and prelude link to Timeline (`/timeline`)                 │
├────────────────────────────────────────────────────────────────────────┤
│ 3. FACTION & COMMAND STRUCTURE (Canonical Data Only)                   │
│    - Canonical participating groups/factions and associated commanders │
│      or participants where supplied by the authoritative data model   │
│    - Associated Group records linked to `/factions/:slug`              │
├────────────────────────────────────────────────────────────────────────┤
│ 4. DAILY CHRONOLOGY STEPPER (Days 1–18 for Kurukshetra)                │
│    - Sequential day selector across canonical WarDay records           │
│    - Visual indicators for days featuring canonical formations (Vyuhas)│
│    - Direct activation transitions to `/wars/:war_slug/day/:day`      │
├────────────────────────────────────────────────────────────────────────┤
│ 5. CAMPAIGN CASUALTIES & AFTERMATH                                     │
│    - Major fallen warriors across the campaign (from canonical events) │
│    - Post-war events transition to narrative timeline                  │
└────────────────────────────────────────────────────────────────────────┘
```

### 6.1 War Index (`/wars`)
Where multiple conflicts exist or are cataloged, `/wars` provides an indexed directory of canonical conflicts, displaying campaign name, narrative epoch, participating groups/factions and canonical war-day coverage. For V1, the primary conflict is the **Kurukshetra War** (`kurukshetra-war`).

### 6.2 War Detail View (`/wars/:war_slug`)
- Displays comprehensive campaign metadata loaded via `/api/v1/wars/:slug`.
- Embeds the **Chronology Stepper** (e.g., 18-day carousel for the Kurukshetra War), enabling users to inspect the sequence and pacing of the war before drilling down into individual days.
- Connects to participating groups and factions via canonical `Group` associations (`/factions/:slug`).

---

## 7. War-Day Exploration Architecture

The War Day experience (`/wars/:war_slug/day/:day`) provides a detailed, day-specific operational breakdown for a single day of the conflict (Days 1–18 for the Kurukshetra War).

```
┌────────────────────────────────────────────────────────────────────────┐
│                        WAR-DAY EXPLORATION VIEW                        │
├────────────────────────────────────────────────────────────────────────┤
│ 1. DAY CONTEXT & BREADCRUMB NAVIGATION                                 │
│    - War Campaign link ❯ Day Number (e.g., "Kurukshetra War ❯ Day 13") │
│    - Previous Day (← Day 12) / Next Day (Day 14 →) stepper controls    │
├────────────────────────────────────────────────────────────────────────┤
│ 2. DAILY OPERATIONAL HEADER                                            │
│    - Daily Title (e.g., "Day 13: The Chakravyuha & Abhimanyu's Stand")  │
│    - Canonical date / phase label (e.g., "Drona Parva, Day 13")       │
│    - Participating commanders and heroes where supplied by data        │
├────────────────────────────────────────────────────────────────────────┤
│ 3. TACTICAL FORMATIONS DEPLOYED ON THIS DAY                            │
│    - Canonical formation card(s) where deployed (links to `/vyuhas`)  │
│    - Tactical fidelity tier indicator (descriptive / schematic / geom) │
├────────────────────────────────────────────────────────────────────────┤
│ 4. CHRONOLOGICAL COMBAT EVENTS & MILESTONES                            │
│    - Sequenced list of `Event` records where `war_day_id = current_day`│
│    - Event title, narrative summary, and sequence index order          │
│    - Participating warriors with canonical roles (`commander`, etc.)   │
│    - Key encounters, heroic duels, and fallen warriors                 │
├────────────────────────────────────────────────────────────────────────┤
│ 5. PARTICIPATING ROSTER & FACTION STATUS                               │
│    - Warriors active on this day (derived from `EventParticipant`)     │
│    - Casualties suffered on this day (derived from canonical events)   │
└────────────────────────────────────────────────────────────────────────┘
```

### 7.1 Graceful Handling of Uneven Daily Data
Ancient texts do not chronicle every single day with identical granularity:
- **High-Detail Days (e.g., Days 10, 13, 14)**: Feature complex multi-phase events, explicit formations, extensive participant rosters, and nocturnal combat.
- **Low-Detail / Broad Days (e.g., Days 1–3, 6–8)**: Feature general descriptions of heavy fighting without individual duel milestones or named formations.
- **Architectural Requirement**: F14 must render sparse days cleanly without empty visual holes or synthesized filler content. If no formation was deployed or cataloged for a given day, the UI displays an explicit, honest indicator: `"No formal tactical formation cataloged for this day in canonical sources."`

### 7.2 Event Sequencing Within a War Day
- Events belonging to a war day are sorted strictly by canonical `sequence_index` (Block B2 §4.2).
- F14 renders these events in a clear chronological combat stream.
- Each event card displays:
  - Event title and narrative summary.
  - Participating characters grouped by role (`commander`, `warrior`, `victim`, etc.) as supplied by `EventParticipant`.
  - Evidence affordance linking to backing textual passages in Block F15.

---

## 8. Vyuha / Formation Architecture

A *Vyūha* is an ancient military array or strategic battlefield formation chronicled in epic literature and deployed during major combat operations.

```
┌────────────────────────────────────────────────────────────────────────┐
│                        FORMATION (VYUHA) DETAIL                        │
├────────────────────────────────────────────────────────────────────────┤
│ 1. FORMATION IDENTITY & EPITHETS                                       │
│    - Canonical Name (e.g., "Chakravyuha" / "Padmavyuha")              │
│    - Sanskrit designation and alternate transliterations               │
│    - Associated War & War Day where recorded (`/wars/:war_slug/day/:day`)│
├────────────────────────────────────────────────────────────────────────┤
│ 2. TACTICAL DESCRIPTION & CONTEXT                                      │
│    - Curated summary and detailed description (Block B2 §4.9)          │
│    - Canonical strategic or tactical purpose where explicitly          │
│      represented in the supplied authoritative description/evidence    │
│    - Associated commanders or key figures where canonically recorded   │
├────────────────────────────────────────────────────────────────────────┤
│ 3. TACTICAL VISUALIZATION CANVAS (Conditional Fidelity Tiers §9)       │
│    - Tier 1: Descriptive & Semantic Mode (Default)                     │
│    - Tier 2: Structural Schematic Mode (if structural order supplied)  │
│    - Tier 3: Authoritative Geometric Mode (if geometry data supplied)  │
│    - Prominent non-fabrication disclosure for conceptual layouts       │
├────────────────────────────────────────────────────────────────────────┤
│ 4. CANONICAL PARTICIPANTS & ASSOCIATED ROLES                           │
│    - Warriors associated with the formation as supplied by canonical   │
│      event participation and relationships                             │
├────────────────────────────────────────────────────────────────────────┤
│ 5. ASSOCIATED EVENT & EVIDENCE AFFORDANCES                             │
│    - Direct link to the battle event where formation was deployed      │
│    - Evidence affordance invoking Block F15 Evidence Drawer            │
└────────────────────────────────────────────────────────────────────────┘
```

### 8.1 Formation Catalog (`/vyuhas`)
The formations directory lists all canonical vyuhas cataloged across the epic:
- Supports browsing by canonical name, associated conflict, and deployment context.
- Displays formation name, summary, and tactical description.

### 8.2 Formation Detail (`/vyuhas/:slug`)
Provides the focused, in-depth view of a specific formation, combining textual descriptions with schematic or geometric visualizations where supported by authoritative backend data.

---

## 9. Tactical Visualization & Representation Model

Visualizing ancient military formations presents severe historical and epistemic risks. Most textual descriptions specify the poetic structure (e.g., "a needle at the mouth, a wheel in the center") or name specific warriors defending specific positions, but do not provide mathematical Cartesian coordinates.

### 9.1 The Three Tactical Fidelity Tiers
To prevent visual fabrication while providing rich exploration, F14 establishes a **Three-Tier Frontend Representation Model** governed strictly by backend data availability:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   TACTICAL VISUALIZATION FIDELITY TIERS                │
├──────────────┬───────────────────────────────┬─────────────────────────┤
│ Tier Level   │ Data Condition                │ Visualization Strategy  │
├──────────────┼───────────────────────────────┼─────────────────────────┤
│ **Tier 1:    │ Authoritative geometric or    │ Rich textual exposition,│
│ Descriptive  │ structural data is absent     │ structured descriptions,│
│ & Semantic** │ or insufficient               │ and canonical role lists│
│              │                               │ (No synthetic geometry) │
├──────────────┼───────────────────────────────┼─────────────────────────┤
│ **Tier 2:    │ Canonical structural          │ Abstract schematic      │
│ Structural / │ positions or roles supplied   │ topological diagram     │
│ Schematic**  │ by authoritative data, but no │ showing relative order  │
│              │ coordinate points             │ (Labeled illustrative)  │
├──────────────┼───────────────────────────────┼─────────────────────────┤
│ **Tier 3:    │ Authoritative geometric data  │ Coordinate-projected    │
│ Authoritative│ is explicitly supplied by the │ interactive vector      │
│ Geometric**  │ canonical backend             │ canvas displaying exact │
│              │                               │ points and perimeters   │
└──────────────┴───────────────────────────────┴─────────────────────────┘
```

#### Tier 1: Descriptive & Semantic Representation
- Applied when authoritative geometric or structural data is absent or insufficient.
- The interface renders a high-density, accessible layout of the formation:
  - Canonical textual description and tactical summary.
  - Participating characters and their associated roles where supplied by canonical event participation records.
  - An explicit epistemic disclosure badge:
    `"Formation structure attested through textual description; no authoritative coordinate geometry exists."`

#### Tier 2: Structural / Schematic Representation
- Applied when canonical data supplies explicit structural positions, roles, or other formation relationships sufficient to support a non-metric schematic, but without metric coordinates.
- Rendered as an abstract schematic topological diagram representing relative structural relationships.
- **Mandatory Non-Fabrication Disclosure**: The diagram must be prominently labeled:
  `"Conceptual schematic layout based on textual order; does not represent historical metric coordinates or troop positions."`

#### Tier 3: Authoritative Geometric Projection
- Applied **only** when authoritative geometric data is explicitly supplied by the canonical backend.
- The interface projects the coordinates onto an interactive vector canvas, displaying:
  - Canonical formation perimeters and structural outlines.
  - Participating figures at their specific canonical coordinate points where supplied.
  - Interactive selection of positions to reveal character profiles and combat events.

### 9.2 Rendering Technology Spectrum (Library-Neutral)
The implementation may select appropriate rendering mechanisms based on the fidelity tier and device capabilities:
1. **Semantic DOM Tier**:
   - Best suited for Tier 1 Descriptive mode, participant rosters, and mobile compact viewports.
   - 100% native HTML markup, full screen-reader transparency, zero canvas overhead.
2. **Structured SVG Tier**:
   - Best suited for Tier 2 Schematic layouts and Tier 3 Geometric formations.
   - Scalable vector graphics with native DOM element binding, sharp typography, and direct CSS styling.
3. **Canvas / WebGL Tier**:
   - Optional enhancement for high-density particle or vector field effects, if warranted by future requirements.
   - Library choice is strictly deferred to implementation (Stage 4).

---

## 10. Epistemic & Data-Availability Presentation

In strict alignment with Block B2 §5.1, F14 implements the project-wide six-state epistemic vocabulary. It does not introduce non-canonical states such as `disputed`, `speculative`, or `traditional`.

```
┌────────────────────────────────────────────────────────────────────────┐
│               EPISTEMIC STATE PRESENTATION IN WAR LENS                 │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Epistemic State  │ Tactical Presentation Requirement                   │
│ (Block B2 §5.1)  │                                                     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `known`          │ Standard visual treatment as defined by B2/F3;      │
│                  │ state meaning is not redefined by F14. Confirmed    │
│                  │ commanders, day sequences, and canonical events.    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `conflicting`    │ Rendered with non-color indicator distinguishing    │
│                  │ competing textual recensions (e.g., divergent daily │
│                  │ casualty lists or conflicting formation details).   │
│                  │ Both claims presented side-by-side with links.      │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `approximate`    │ Applied to relative narrative sequences or estimated│
│                  │ timeframes without false precision.                 │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `unknown`        │ Explicitly indicates that primary texts are silent  │
│                  │ regarding a commander, formation, or outcome.       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `not_researched` │ Indicates curation is in progress; displays honest  │
│                  │ empty state without implying a negative fact.       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `not_applicable` │ Applied where structural tactical attributes are   │
│                  │ irrelevant to the entity category.                  │
└──────────────────┴─────────────────────────────────────────────────────┘
```

### 10.1 Epistemic State vs. Data Availability vs. Rendering Capability
To ensure absolute epistemic honesty and prevent conflation of concepts, F14 maintains a strict tripartite separation:

1. **Canonical Epistemic State (Block B2 §5.1)**:
   - The authoritative truth state of backend knowledge (`known`, `unknown`, `not_researched`, `not_applicable`, `conflicting`, `approximate`).
   - `unknown` strictly uses its canonical B2 definition (the primary texts explicitly note the information is missing or unknowable). F14 does not redefine its meaning.
   - `not_researched` represents incomplete curation.
   - `conflicting` represents competing textual recensions or scholarly claims.
2. **Data Availability / Presence**:
   - The physical presence or absence of data fields in backend payloads (e.g., whether coordinates, structural outlines, or participant rosters are populated or `NULL`).
   - Absence of data is not an epistemic state; it is a data-presence condition.
3. **Frontend Rendering Capability**:
   - F14's UI projection modes (Descriptive Tier 1, Schematic Tier 2, Geometric Tier 3) determined conditionally by available data.
   - A descriptive-only presentation is a frontend representation mode chosen due to the absence of geometric data; it is **not** an epistemic state.

---

## 11. Responsive Architecture Across Viewport Classes

The War & Tactical Lens adapts across the four Block F4 viewport classes without relying on arbitrary pixel breakpoints or fixed dimensional ratios:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   RESPONSIVE RECOMPOSITION MATRIX                      │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Viewport Class   │ Layout Structure & Interaction Mode                 │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Compact**      │ - Stacked vertical layout.                          │
│ (Handhelds /     │ - Horizontal swipeable day stepper (e.g., Days 1–18 │
│  Small Screens)  │   for the Kurukshetra War).                         │
│                  │ - Formations rendered in Tier 1 Semantic mode or    │
│                  │   zoomable vector card with touch-panning.          │
│                  │ - Bottom-sheet for warrior/role detail inspection.  │
│                  │ - Full-bleed chronological combat event stream.     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Medium**       │ - Split-view or multi-card layout.                  │
│ (Tablets /       │ - Sticky day stepper navigation bar.                │
│  Foldables)      │ - Side-by-side display of tactical formation card   │
│                  │   and daily combat event roster.                    │
│                  │ - Docked contextual inspection drawer.              │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Expanded**     │ - Two-column workspace layout.                      │
│ (Desktops /      │ - Left column: Persistent war-day stepper (e.g.,    │
│  Laptops)        │   18-day stepper for Kurukshetra), day overview,    │
│                  │   and chronological combat event stream.            │
│                  │ - Right column: Dedicated tactical visualization    │
│                  │   canvas and selected warrior/participant inspector.│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Wide**         │ - Three-column panoramic analytical layout.         │
│ (Ultra-wide /    │ - Left: Campaign day navigator (e.g., 18-day        │
│  Multi-Monitor)  │   navigator for Kurukshetra) and day summary.       │
│                  │ - Center: Primary tactical formation canvas and     │
│                  │   combat timeline stream.                           │
│                  │ - Right: Persistent warrior inspection panel and    │
│                  │   docked Block F15 Evidence Drawer.                 │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 12. Accessibility & Semantic Companion Architecture

Tactical diagrams, formation schematics, and daily battle sequences must be fully navigable by screen-reader users and keyboard-only navigators without requiring graphical canvas interaction.

### 12.1 The Dual-Mode Semantic Architecture
1. **Visual Tactical Canvas**: Optimized for spatial orientation, structural relationships, and interactive element inspection.
2. **Accessible Semantic Companion**: A structured, semantic DOM representation providing complete functional parity:
   - **Semantic War-Day Navigation**: Keyboard-traversable navigation through canonical war days with active-day indicators.
   - **Structured Formation Descriptions**: Hierarchical semantic representation detailing the canonical formation name, textual description, and strategic context.
   - **Canonical Formation-Role & Position Rosters**: Structured presentation of formation positions, roles, and assigned participants where supplied by the authoritative data model.
   - **Chronological Combat Event Sequences**: Ordered list of battle events with explicit sequence markers and participant rosters.

### 12.2 Architectural Demarcation with Blocks F16 and F17
- **Block F14 Ownership**: Defines the structural hierarchy, semantic elements, text alternatives, and data parity requirements for war and formation exploration.
- **Block F16 Ownership**: Authoritatively defines concrete ARIA roles, landmark regions, keyboard shortcut sequences, live-region screen-reader announcements, and focus management.
- **Block F17 Ownership**: Defines automated accessibility verification test suites, screen-reader user testing protocols, and WCAG 2.2 Level AA conformance validation.

---

## 13. Evidence & Provenance Integration (Block F15 Boundary)

Every war record, war day summary, formation description, and combat outcome connects to authoritative textual sources and scholarly claims:
- Where canonical evidence/provenance is available, F14 exposes a standard evidence affordance (`[Evidence ↗]`).
- Activating the affordance invokes the canonical **Block F15 Evidence Drawer**, passing the relevant `claim_id` or entity slug.
- The drawer presents primary Sanskrit text locators (Parva, Adhyaya, Shloka), canonical source references and evidence locators, translation excerpts where supplied, and contextual assessment commentary (`evidence.assessment`).
- **Boundary Invariant**: F14 defines the invocation trigger and contextual payload; Block F15 authoritatively governs citation display, manuscript drawer layouts, and excerpt rendering.

---

## 14. Routing, Navigation & Deep-Linking Integration (Block F7)

The War & Tactical Vyuha Lens conforms strictly to the canonical route taxonomy established in Block F7 §4:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        CANONICAL WAR & VYUHA ROUTES                    │
├────────────────────────────┬───────────────────────────────────────────┤
│ Route Path Pattern         │ Exploration Perspective & Scope           │
├────────────────────────────┼───────────────────────────────────────────┤
│ `/wars`                    │ Wars & Conflicts Directory                │
├────────────────────────────┼───────────────────────────────────────────┤
│ `/wars/:war_slug`          │ War Campaign Summary & Narrative Overview │
├────────────────────────────┼───────────────────────────────────────────┤
│ `/wars/:war_slug/day/:day` │ Daily Operational & Tactical Breakdown    │
├────────────────────────────┼───────────────────────────────────────────┤
│ `/vyuhas`                  │ Military Formations (Vyuhas) Directory    │
├────────────────────────────┼───────────────────────────────────────────┤
│ `/vyuhas/:slug`            │ Tactical Formation Detail View            │
└────────────────────────────┴───────────────────────────────────────────┘
```

### 14.1 Route Authority & URL State Ownership
- F14 uses **only** the canonical route paths defined above. It does not introduce alternative paths (e.g., `/war/...`, `/battles/...`, `/formations/...`).
- **F7 Ownership Boundary**: Block F7 alone authoritatively owns whether and how client view state is serialized into URL query parameters. F14 describes conceptual URL-worthy state (e.g., active day or selected formation identity where F7 determines that the state is URL-worthy), but defers parameter naming, defaults, serialization syntax, and canonicalization strictly to Block F7.

---

## 15. Cross-Lens Transitions & Bridges

The War & Tactical Lens integrates seamlessly with other exploration lenses across the application:

```
┌────────────────────────────────────────────────────────────────────────┐
│                       CROSS-LENS NAVIGATION MATRIX                     │
├───────────────────┬────────────────────────────────────────────────────┤
│ Target Lens       │ Canonical Transition Affordance                    │
├───────────────────┼────────────────────────────────────────────────────┤
│ **Timeline**      │ Opens the Timeline lens in the relevant canonical  │
│ (`/timeline`)     │ event/war context, with URL serialization governed │
│                   │ by Block F7.                                       │
├───────────────────┼────────────────────────────────────────────────────┤
│ **Geography**     │ Opens the Geographic Map lens centered on canonical│
│ (`/geography`)    │ location detail (e.g., `/geography/kurukshetra`).  │
├───────────────────┼────────────────────────────────────────────────────┤
│ **Characters**    │ Opens canonical character view for commanders or   │
│ (`/characters`)   │ participants (`/characters/:slug`).                │
├───────────────────┼────────────────────────────────────────────────────┤
│ **Factions**      │ Opens canonical faction view for participating     │
│ (`/factions`)     │ alliances (`/factions/pandava-alliance`).          │
├───────────────────┼────────────────────────────────────────────────────┤
│ **Focus Graph**   │ Opens `/graph/:slug` centered on an entity where   │
│ (`/graph`)        │ canonical relationship graph context exists.       │
├───────────────────┼────────────────────────────────────────────────────┤
│ **Formations**    │ Direct bidirectional links between a war day and   │
│ (`/vyuhas`)       │ formations deployed on that day (`/vyuhas/:slug`). │
└───────────────────┴────────────────────────────────────────────────────┘
```

> **Relationship Semantics Invariant**: Cross-lens transitions are constructed strictly from canonical relational links (`Event.war_id`, `Event.location_id`, `EventParticipant.character_id`, `Formation.event_id`). No synthetic or inferred connections are created.

---

## 16. State Ownership & Architecture

In strict adherence to the project state hierarchy established in Block F5:

```
┌────────────────────────────────────────────────────────────────────────┐
│                      STATE MANAGEMENT HIERARCHY                        │
├──────────────────┬──────────────────┬──────────────────────────────────┤
│ State Scope      │ Governing Layer  │ Managed State Attributes         │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **URL State**    │ Block F7 Router  │ Canonical path segments          │
│                  │                  │ (`/wars/:war_slug/day/:day`,     │
│                  │                  │ `/vyuhas/:slug`)                 │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Server Cache** │ Block F6 Query   │ Cached War, WarDay, Formation,   │
│                  │ Client           │ Event, and Participant payloads  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Lens State**   │ War Lens State   │ Active day context,              │
│                  │                  │ participant-role presentation    │
│                  │                  │ filters, and other War-lens      │
│                  │                  │ presentation state               │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Transient UI** │ Local Component  │ Canvas zoom level, pan offsets,  │
│                  │ State            │ hovered element tooltip, active  │
│                  │                  │ inspector drawer open/close      │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Preferences**  │ Block F5 Store   │ High-contrast mode, reduced      │
│                  │                  │ motion preference                │
└──────────────────┴──────────────────┴──────────────────────────────────┘
```

- **High-Frequency Interaction Isolation**: Pan and zoom manipulations on tactical canvases are isolated strictly to local component state and must never write to URL state or global storage stores.
- **Server Cache Invariant**: API query keys for war and formation data are authoritatively governed by Block F6.

---

## 17. Lifecycle, Loading, Empty & Error States

The War Lens defines explicit lifecycle states to ensure predictable behavior and honest user feedback:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        LIFECYCLE STATE MATRIX                          │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Lifecycle State  │ Presentation & Behavioral Requirement               │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Initial        │ Renders accessible skeleton structure preserving   │
│ Loading**        │ layout stability; displays campaign title placeholder│
│                  │ and day stepper outlines.                           │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Incremental   │ Day stepper remains interactive; loading indicator  │
│ Day Loading**    │ is isolated strictly to the day detail canvas.      │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Ready**        │ Full tactical data rendered according to available  │
│                  │ fidelity tier.                                      │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Partial Day    │ Displays available combat events and commanders;    │
│ Data**           │ displays honest indicator for unrecorded formations.│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Described-Only │ Renders Tier 1 Semantic layout with prominent       │
│ Formation**      │ disclosure that metric geometry is unrecorded.      │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Entity Not     │ Displays contextual 404 screen with link to wars    │
│ Found**          │ directory (`/wars`) or global search (`/search`).   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **API / Network  │ Non-destructive error banner offering retry         │
│ Error**          │ affordance without corrupting existing cache.       │
└──────────────────┴─────────────────────────────────────────────────────┘
```

> **Data-Availability Invariant**: A formation lacking coordinate geometry or structural data is **not** an error state. It is an honest, normal canonical representation and must be displayed with full dignity and richness in Tier 1 Descriptive mode.

---

## 18. Performance & Rendering Boundaries

The War & Tactical Lens encompasses dense participant rosters, multi-event combat streams, and vector graphics.

### 18.1 Progressive Loading Strategy
- War overview and day headers load eagerly.
- Detailed event participant rosters and formation geometry payloads load progressively or upon day selection.
- Switching between war days utilizes cached query data managed by Block F6.

### 18.2 Quantitative Verification Boundary
- F14 defines conceptual rendering tiers, component lifecycle isolation, and progressive disclosure strategies.
- **Block F17 Ownership**: Strict numerical performance thresholds, frame rate benchmarks (e.g., 60 FPS targets), bundle size budgets, and network latency limits are authoritatively defined and verified by **Block F17**. F14 does not invent quantitative performance limits.

---

## 19. Security & Untrusted Data Protections

1. **Safe Text Rendering**: All war summaries, daily titles, formation descriptions, Sanskrit transliterations, and warrior annotations originating from backend payloads are treated as untrusted data. Content is rendered strictly via safe framework text bindings or sanitized properties. Direct HTML injection is strictly prohibited.
2. **Untrusted Route Input Handling**: Route parameters (`:war_slug`, `:day`, `:slug`) are treated as untrusted user input. Canonical validation and sanitization are deferred strictly to Block F7 routing and validation architecture before any backend queries are initiated. F14 does not define a separate slug grammar.

---

## 20. Architectural Ownership Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                     ARCHITECTURAL OWNERSHIP MATRIX                     │
├────────────────────────────┬─────────────┬─────────────────────────────┤
│ Architectural Concern      │ Owner Block │ Boundary Description        │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Canonical War/Vyuha Schema │ Block B2    │ Database tables, FKs, types │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Knowledge Graph Relations  │ Block B3    │ EventParticipant & edges    │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ War & Vyuha API Endpoints  │ Block B5    │ REST contracts & envelopes  │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ War & Vyuha Visual Lens    │ Block F14   │ Campaign, day & vyuha UX    │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ General Relationship Graph │ Block F10   │ 2-hop topological network   │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Family Lineage Tree DAG    │ Block F11   │ Generational kinship DAG    │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Narrative Timeline         │ Block F12   │ Global event chronology     │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Geographic Spatial Map     │ Block F13   │ Subcontinental map lens     │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Evidence Drawer Citation   │ Block F15   │ Textual claims & citations  │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Concrete A11y & ARIA       │ Block F16   │ Concrete screen-reader code │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Performance Budgets & Tests│ Block F17   │ Quantitative verification   │
└────────────────────────────┴─────────────┴─────────────────────────────┘
```

---

## 21. Architectural Decision Records (ADRs)

### ADR-F14-01: Zero Fabrication of Tactical Geometry and Troop Numbers
- **Context**: Visualizing ancient military formations creates a strong temptation to synthesize realistic battlefield geometry, unit coordinates, and soldier numbers from poetic names.
- **Decision**: Strictly prohibit the synthesis of coordinates, unit positions, or troop numbers. Formations without authoritative backend geometry must be rendered in Tier 1 Descriptive mode.
- **Consequences**: Protects scholarly integrity and complies strictly with Constitutional Principle 4 (Zero Fabrication).

### ADR-F14-02: Three-Tier Tactical Fidelity Model
- **Context**: Formations in epic literature vary widely in recorded structural detail—from general names and summaries to detailed warrior assignments and positions.
- **Decision**: Architect three explicit frontend representation tiers (Descriptive, Structural Schematic, Authoritative Geometric) determined conditionally by available canonical data.
- **Consequences**: Enables rich, informative presentation for every formation without misrepresenting poetic descriptions as geometric facts or inventing backend schema fields.

### ADR-F14-03: Separation of War Day Operations from Narrative Timeline
- **Context**: Both Block F12 (Timeline) and Block F14 deal with events and chronology, risking duplicated components and conflicting state.
- **Decision**: Block F12 exclusively owns the global multi-Parva narrative timeline stream. Block F14 owns the focused operational view of the canonical WarDay sequence (comprising 18 days for the Kurukshetra War), consuming `Event` records scoped to `war_days`.
- **Consequences**: Clear architectural boundaries, reusable components, and dedicated mental models for global narrative vs daily battlefield tactics.

### ADR-F14-04: Dual-Mode Accessibility with Semantic Tactical Outlines
- **Context**: Graphical formation canvases and interactive battlefield diagrams are inaccessible to screen-reader and non-pointer users.
- **Decision**: Mandate a full semantic companion representation featuring structured formation-role/position outlines where supplied, ordered commander lists, and daily event logs.
- **Consequences**: Fulfills Constitutional Principle 10 (Accessibility as a First-Class Requirement) without degrading visual exploration.

### ADR-F14-05: Strict Boundary on URL Query Serialization
- **Context**: Storing active day numbers, selected formation context, and filter states in URL query parameters requires clear governance.
- **Decision**: Canonical route paths (`/wars/:war_slug/day/:day`, `/vyuhas/:slug`) are used for primary identity; all query parameter naming, serialization, and validation remain strictly under Block F7 authority.
- **Consequences**: Preserves URL hygiene and prevents fragmented parameter conventions across lenses.

---

## 22. Requirement Traceability Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                   REQUIREMENT TRACEABILITY MATRIX                      │
├───────────────────┬──────────────────┬─────────────────────────────────┤
│ Upstream Req ID   │ Source Document  │ Block F14 Architectural Mapping │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **WAR-001**       │ PRD §9.7         │ Section 6: War Overview         │
│                   │                  │ architecture and campaign view. │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **WAR-002**       │ PRD §9.7         │ Section 7: War Day navigation   │
│                   │                  │ and canonical WarDay sequence   │
│                   │                  │ stepper (18 days Kurukshetra).  │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **WAR-003**       │ PRD §9.7         │ Section 7.2: Day-specific       │
│                   │                  │ events and participant rosters. │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **WAR-004**       │ PRD §9.7         │ Section 13, Section 15: Cross-  │
│                   │                  │ lens bridges to Timeline, Map.  │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **WAR-005**       │ PRD §9.7         │ Section 9: Three-tier fidelity  │
│                   │                  │ model for formation visualizer. │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **WAR-006**       │ PRD §9.7         │ Section 18: Progressive loading │
│                   │                  │ of daily combat details.        │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **NAV-001**       │ PRD §9.12; F7 §4 │ Section 14: Canonical routes    │
│                   │                  │ `/wars`, `/vyuhas`, etc.        │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **A11Y-001**      │ PRD §9.13; F3 §4 │ Section 12: Dual-mode accessible│
│                   │                  │ semantic companion outlines.    │
└──────────────────┴──────────────────┴─────────────────────────────────┘
```

---

## 23. F14 Exit Criteria & Acceptance Checklist

- [x] War exploration purpose, UX role, and distinct boundary against F10, F11, F12, F13 established.
- [x] Canonical backend entities consumed strictly as defined by B2 (`War`, `WarDay`, `Formation`, `Event`, `Character`, `Group`, `Location`).
- [x] Absolute zero-fabrication tactical rule established: no fabricated troop numbers, army sizes, unit positions, or synthetic geometry.
- [x] War overview architecture defined covering campaign identity, commanders, factions, and canonical WarDay sequence navigation (comprising 18 days for Kurukshetra).
- [x] War-day exploration architecture defined with daily titles, commanders, combat events, and participant rosters.
- [x] Vyuha / Formation architecture specified with three-tier fidelity model (Descriptive, Structural Schematic, Authoritative Geometric).
- [x] Explicit requirement that formation names alone must never be converted into synthetic geometric shapes.
- [x] Epistemic status representation adheres strictly to canonical B2 six-state vocabulary (`epistemic_status`: `known`, `unknown`, `not_researched`, `not_applicable`, `conflicting`, `approximate`).
- [x] Responsive recomposition mapped across the four Block F4 viewport classes without arbitrary dimensions.
- [x] Dual-mode accessibility model architected with structural semantic companion and explicit demarcation against F16 and F17.
- [x] Canonical routes `/wars`, `/wars/:war_slug`, `/wars/:war_slug/day/:day`, `/vyuhas`, `/vyuhas/:slug` preserved; query serialization strictly under F7 authority.
- [x] High-frequency canvas interaction state isolated from URL/storage per Block F5.
- [x] Progressive disclosure and caching defined per Block F6; quantitative performance budgets deferred to Block F17.
- [x] Security and untrusted data protections specified for text rendering; route validation deferred to Block F7.
- [x] Zero application source code, UI packages, or concrete graphics/canvas libraries selected or introduced.
- [x] Zero Stage 1 backend documents or Blocks F1–F13 documents modified.
