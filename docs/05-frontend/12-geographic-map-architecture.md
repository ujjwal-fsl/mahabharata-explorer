# Geographic Map Architecture (Block F13)

## 1. Document Status & Purpose

- **Document Identifier**: Block F13
- **Status**: Draft Architectural Specification (Review Pending)
- **Stage**: Stage 2 — Frontend Architecture
- **Location**: `docs/05-frontend/12-geographic-map-architecture.md`
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
- **Downstream Consumers**:
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
│ - Block B2: Location & Spatial DB │ ► BLOCK F13: GEOGRAPHIC MAP LENS   │
│ - Block B3: Knowledge Graph &     │   - F14: War & Tactical Vyuhas     │
│   Entity Relationships            │   - F15: Evidence Drawer UX        │
│ - Block B5: API Location Endpoints│   - F16: A11y & Typography Engine  │
│ - Block F3: Visual Grammar        │   - F17: Testing & Budgets         │
│ - Block F4: Viewport Classes      │                                    │
│ - Block F5: State Hierarchy       │                                    │
│ - Block F6: API Fetching/Caching  │                                    │
│ - Block F7: URL Grammar & Routing │                                    │
│ - Block F9: Entity Views Launch   │                                    │
│ - Block F10: Graph Visualization  │                                    │
│ - Block F11: Family Lineage DAG   │                                    │
│ - Block F12: Timeline Lens        │                                    │
└───────────────────────────────────┴────────────────────────────────────┘
```

This document establishes the frontend **architecture** for the Mahābhārata Explorer's Geographic Map visualization lens (`/geography`, `/geography/:slug`). Geography in epic literature encompasses historical kingdoms, ancient riverine systems, sacred hermitages, capital cities, pilgrimage circuits, remote forest tracts, and iconic battlegrounds.

Block F13 defines the visual, structural, responsive, and accessible projection of canonical spatial entities. In strict accordance with the project constitution, F13 projects canonical geographic information into a spatial exploration interface governed by **Zero Fabrication**, **Epistemic Honesty**, and **Accessible Equivalents**: it must never fabricate coordinates, modern identities, historical precision, routes, boundaries, regions, or spatial relationships.

---

## 2. Architectural Context & Foundational Principles

### 2.1 Pure Architectural Specification & Library Neutrality
Block F13 is an architectural specification only. It defines spatial projection models, coordinate handling, certainty visual semantics, unmapped-entity discovery, multi-scale clustering concepts, responsive transformations, and accessible semantic companions. It does **not**:
- Implement React components, custom hooks, or web mapping components.
- Select, mandate, or bundle concrete client-side mapping libraries (e.g., MapLibre GL, Leaflet, OpenLayers, Google Maps, or D3-geo).
- Introduce application source code, package dependencies, or map tile server configurations.
- Introduce direct database queries, spatial SQL, or PostGIS schema alterations.

### 2.2 Foundational Principles

#### 1. Geographic Knowledge Explorer, Not a Modern Navigation App (Constitutional Principle 1)
Ancient epic geography consists of literary, historical, and sacred spatial descriptions rather than modern road networks or satellite-derived administrative boundaries. The Geographic Map Lens is designed for scholarly exploration of ancient kingdoms, sacred topography, event settings, and textual attestations, rather than modern GPS navigation or route finding.

#### 2. The Absolute Zero-Fabrication Geographic Rule (Constitutional Principle 4 & Rule 03)
Under no circumstances may the frontend:
- Perform client-side geocoding against modern address databases to invent ancient coordinates.
- Infer or synthesize latitude/longitude values where backend records are `NULL`.
- Silently substitute modern urban center coordinates for ancient epic locations whose precise sites are unverified or debated.
- Fabricate administrative polygons, kingdom boundaries, territorial frontiers, or travel distances where canonical data records only point references or regional names.
- Synthesize point-to-point journey paths or marching routes purely by connecting chronological event sequences. Geographic paths or movement sequences may be rendered only when the canonical backend data model explicitly provides authoritative spatial sequence information sufficient to support that representation. F13 does not define such a backend model.

#### 3. Epistemic Honesty in Spatial Representation (Constitutional Principle 3 & Epistemic Honesty)
Spatial knowledge in ancient literature carries widely disparate levels of certainty:
- Locations with known coordinates are visually distinct from those with approximate regional correspondence or conflicting scholarly claims.
- Missing coordinates must never be hidden or discarded; they must produce an explicit, honest unmapped presentation that preserves the entity's full narrative presence.

#### 4. Multiple Competing Geographic Traditions are First-Class (Constitutional Principle 3 & Rule 03)
Different textual recensions and historical-geographical traditions propose competing identifications for ancient sites:
- Competing locations or dual candidate sites are preserved as distinct, side-by-side or selectable spatial representations anchored to their backing scholarly claims.
- F13 never arbitrarily selects one modern identification or recension hypothesis as authoritative over others.

#### 5. Accessible Equivalent Representation (Constitutional Principle 10)
Interactive visual maps pose insurmountable barriers to screen-reader and non-pointer users if implemented purely as visual canvases. The geographic exploration experience mandates a complete, fully navigable semantic companion representation (e.g., structured regional directories, location rosters, and spatial-event tables). Block F16 authoritatively governs concrete ARIA attributes, keyboard navigation, and screen-reader announcements; Block F17 validates accessibility conformance.

---

## 3. Scope of Block F13

### 3.1 In-Scope
- UX purpose and information architecture of the Geographic Map Lens (`/geography`, `/geography/:slug`).
- Conceptual data transformation projecting canonical `Location`, associated `Event`, and related entity records into a spatial visualization.
- Coordinate and certainty handling: canonical point coordinates, approximate regional markers, conflicting identification markers, and unmapped location collections.
- Epistemic-state rendering using the project-wide canonical B2 vocabulary where applicable, while respecting the field-level state subset defined by B2.
- Spatial visualization architecture: primary geographic canvas, marker representations, conceptual multi-scale clustering, and progressive disclosure.
- Unmapped-location discovery model: structured companion view guaranteeing complete accessibility and visibility for entities without spatial coordinates.
- Location inspection architecture: location identity, type, descriptive summary, coordinate certainty, associated events, participant rosters, and evidence affordances.
- Non-destructive client-side filtering: location type, regional context, event involvement, and coordinate status.
- Responsive recomposition across the four Block F4 viewport classes (Compact, Medium, Expanded, Wide).
- Dual-mode accessibility model: interactive geographic canvas paired with an accessible semantic companion directory.
- Evidence drawer trigger boundaries connecting geographic claims and identifications to Block F15.
- Conceptual URL state synchronization and deep-linking via Block F7 (`/geography`, `/geography/:slug`).
- Client state ownership across URL (F7), Server Cache (F6), Geography Lens State (F13), and UI State (F5).
- Performance and rendering boundaries: conceptual tiers across Semantic DOM, SVG, Canvas/WebGL, or hybrid vector rendering.
- Security against malicious injection in location names, summaries, alternate names, and deep links.

### 3.2 Out-of-Scope (Non-Goals)
- Implementing concrete map rendering engines, vector tile pipelines, or selecting specific npm map packages (deferred to Stage 4).
- Free-form multi-entity topological relationship networks ($D \le 2$ general graph owned by Block F10).
- Multi-generational genealogical trees and kinship DAGs (owned by Block F11: Family Lineage Tree).
- Temporal sequence ordering, narrative phases, and chronological progression (owned by Block F12: Chronology & Timeline).
- Battlefield formation geometry, tactical troop arrays, and daily combat movements (owned by Block F14: War & Tactical Vyuhas).
- Primary textual citation rendering, manuscript variants, and verse-level evidence drawer content (owned by Block F15: Evidence Drawer).
- System-wide contrast tokens, keyboard event listeners, and global ARIA implementations (owned by Block F16).
- Quantitative performance budgets, benchmark runners, and bundle size limits (owned by Block F17).
- Offline GIS spatial analysis tools, custom polygon editing, or shapefile export utilities are outside the scope of F13.

---

## 4. Geographic Lens Role in the Explorer Ecosystem

The Geographic Map Lens provides the spatial perspective of the Mahābhārata. While the Character View (Block F9) details biographical arcs, the Graph Lens (Block F10) captures immediate social topologies, the Lineage Lens (Block F11) visualizes dynastic descent, and the Timeline Lens (Block F12) orders temporal events, the Geographic Lens articulates where the narrative occurs and grounds ancient toponyms within physical and cultural landscapes:

```
┌────────────────────────────────────────────────────────────────────────┐
│                 GEOGRAPHIC LENS ROLE & CONTEXTUAL BRIDGES              │
├───────────────────────────────────┬────────────────────────────────────┤
│ User Mental Model                 │ Exploration Objective              │
├───────────────────────────────────┼────────────────────────────────────┤
│ "Where is this ancient kingdom or │ Physical and regional localization │
│  sacred site situated?"           │ of epic places on the subcontinent.│
├───────────────────────────────────┼────────────────────────────────────┤
│ "What major narrative events took │ Event clustering and historical    │
│  place in this specific location?"│ incident aggregation per landmark. │
├───────────────────────────────────┼────────────────────────────────────┤
│ "Which epic locations cannot be   │ Transparent discovery of ancient   │
│  confidently mapped to modern     │ places attested in texts but       │
│  geographic coordinates?"         │ lacking verified modern sites.     │
├───────────────────────────────────┼────────────────────────────────────┤
│ "Where do scholarly traditions    │ Visual inspection of dual or       │
│  disagree on site identification?"│ multiple competing location claims.│
└───────────────────────────────────┴────────────────────────────────────┘
```

### 4.1 Lens Demarcation Matrix
To prevent architectural overlap and maintain system clarity across Stage 2 lenses:

| Architectural Dimension | Graph Lens (F10) | Lineage Lens (F11) | Timeline Lens (F12) | Geographic Lens (F13) | War Lens (F14) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Primary Organizing Axis** | Free spatial topology | Generational hierarchy | Temporal / Narrative sequence | Spatial coordinates (Lat/Long) | Tactical formation / War day |
| **Core Entity Focus** | Heterogeneous network | Characters only | Events & EventParticipants | Locations & Regions | Wars, WarDays, Formations |
| **Primary Traversal** | Radial distance ($D \le 2$) | Generational offsets | Chronological sequence indices | Spatial proximity & regions | Daily tactical progression |
| **Canonical Route** | `/graph`, `/graph/:slug` | `/lineage`, `/lineage/:slug` | `/timeline`, `/timeline/:slug` | `/geography`, `/geography/:slug` | `/wars/:slug`, `/vyuhas/:slug` |

---

## 5. Canonical Geographic Data Boundary & Backend Consumption

### 5.1 Authoritative Backend Source (Block B2 & B3)
The Geographic Map Lens consumes geographic data strictly from canonical Stage 1 models:
- `locations` table (Block B2 §4.3): Location identity, `slug`, `name`, `alternate_names`, `summary`, `location_type`, `latitude`, `longitude`, `coordinate_status`, and `metadata`.
- `events` table (Block B2 §4.2): Narrative occurrences anchored to spatial sites via `location_id`.
- `event_participants` table (Block B2 §4.2, Block B3 §8): Characters linked to spatial events.
- `claims` table (Block B2 §4.11, Block B4): Epistemic claims, scholarly justifications, and source citations backing location identifications and coordinates.
- `sources` table (Block B2 §4.10, Block B4): Primary texts, critical editions, and scholarly references documenting toponyms.

### 5.2 Canonical Geographic Attributes
Geographic attributes are consumed exactly as defined by the authoritative B2 canonical model:
- `name`: Primary canonical name of the location.
- `alternate_names`: Array of aliases, historical variants, and multilingual transliterations (Block B2 §6.1).
- `location_type`: Categorical taxonomy consumed directly from the authoritative B2 canonical model (`kingdom`, `city`, `forest`, `river`, `battlefield`, `hermitage`, `region`, `sacred_site`).
- `latitude` / `longitude`: Floating-point geographic coordinates (nullable where unmapped).
- `coordinate_status`: Operational geographic status consumed from the authoritative B2 model. Its applicable values are defined by B2 and must not be expanded or reinterpreted by F13. While Block B2 §5.1 defines the project-wide six-state vocabulary (`known`, `unknown`, `not_researched`, `not_applicable`, `conflicting`, `approximate`), individual epistemic fields support only the semantically applicable subset established by B2. For `locations.coordinate_status`, B2 explicitly defines the applicable subset:
  - `known`: Uses the canonical B2 meaning; state meaning is not redefined by F13.
  - `approximate`: Regional or broad approximate location known without pinpoint precision.
  - `unknown`: Location mentioned in ancient texts but physically unlocatable or lost.
  - `conflicting`: Multiple credible scholarly traditions or textual claims identify divergent modern sites.
  - `not_researched`: Curation is in progress for this location's geographic coordinates.

> **Operational Status Distinction**: Locations where `latitude` and `longitude` are `NULL` represent an operational geographic state of **Unmapped Location**. "Unmapped" is a functional presentation category resulting from null coordinates (backed by `unknown`, `not_researched`, or `conflicting` status), **not** a seventh project-wide epistemic state. F13 does not alter the six canonical B2 epistemic states.

> **Architectural Invariant**: F13 does not create, rename, or invent geographic attributes. All coordinates, types, and certainty indicators are derived deterministically from canonical backend data.

### 5.3 API Consumption Contract (Block B5 & F6)
The Geographic Map Lens consumes location and spatial data via the canonical API and domain contracts established by Stage 1 (Blocks B5 and F6). F13 does not define or invent specific REST endpoint paths, spatial query syntax, bounding-box parameter specifications, or backend query traversal logic. Exact endpoint contracts, spatial indexing, response envelopes, and cache keys remain the authority of the API architecture, with F13 consuming canonical location collections and associated event records as provided by the backend interface.

---

## 6. Coordinate & Geographic Certainty Model

Ancient geography cannot be flattened into uniform high-precision pin markers without introducing gross scholarly fabrication. F13 architects a tiered certainty model:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   GEOGRAPHIC CERTAINTY MODEL TIERS                     │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Spatial State    │ Projection Strategy & Visual Semantics              │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Known**        │ Rendered as discrete canonical point markers at the │
│                  │ verified coordinates (`coordinate_status = 'known'`)│
│                  │ Solid visual marker stroke with settled visual token│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Approximate /  │ Rendered with generalized spatial indicator (e.g.,  │
│   Regional**     │ dashed marker perimeter or regional halo)           │
│                  │ indicating regional territory rather than a pinpoint│
│                  │ (`coordinate_status = 'approximate'`).              │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Conflicting /  │ Rendered as distinct dual or multiple candidate     │
│   Divergent**    │ markers tagged with corresponding tradition badges; │
│                  │ each connects to its backing claim in Block F15     │
│                  │ (`coordinate_status = 'conflicting'`).              │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Unmapped /     │ Coordinates are `NULL`. Completely excluded from    │
│   Unlocated**    │ map coordinates to avoid false pinpoint placement.  │
│                  │ Prominently accessible via the Unmapped Directory   │
│                  │ and Companion Navigator (§10).                      │
└──────────────────┴─────────────────────────────────────────────────────┘
```

### 6.1 Preserving Ancient vs. Modern Distinctions
- Ancient toponyms must never be displayed as identical to modern administrative jurisdictions without explicit qualification.
- Where modern settlements or archaeological excavation mounds bear approximate or contested correspondence to ancient sites, that relationship is surfaced strictly via descriptive metadata and canonical claims, never by silently replacing the ancient name with a modern municipality.

---

## 7. Geographic Representation Model

A core challenge of epic geography is that spatial information appears in diverse conceptual geometries:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   SPATIAL REPRESENTATION MODALITIES                    │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Geometry Type    │ Conceptual Visual Treatment                         │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Point Site**   │ Discrete landmark marker (city, hermitage, sacred   │
│                  │ grove, battlefield mound) with verified coordinates.│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Regional Area**│ Extents or kingdom territories displayed only where │
│                  │ supported by canonical data. Where no canonical     │
│                  │ boundary geometry exists, F13 may display an        │
│                  │ authoritative canonical representative coordinate   │
│                  │ if one is supplied, but must not imply that the     │
│                  │ coordinate represents a geographic centroid or      │
│                  │ boundary. If no authoritative coordinate exists,    │
│                  │ the location remains unmapped/insufficiently mapped.│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Linear Feature │ Linear geographic paths (rivers, mountain ranges)   │
│   (e.g. Rivers)**│ rendered only when backed by canonical spatial      │
│                  │ coordinates; otherwise marked as a landmark.        │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Spatial-Event  │ Dynamic spatial aggregation indicating events that  │
│   Concentration**│ occurred at a location, with badge count and link   │
│                  │ to event roster in the detail panel.                │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Journey & Path │ Geographic paths or movement sequences may be       │
│   Sequences**    │ rendered only when the canonical backend data model │
│                  │ explicitly provides authoritative spatial sequence  │
│                  │ information sufficient to support that              │
│                  │ representation. F13 does not define such a model.   │
└──────────────────┴─────────────────────────────────────────────────────┘
```

### 7.1 The Zero-Fabrication Rule for Travel Routes
In strict adherence to PRD §9.6 (MAP-004, MAP-005):
- The frontend must **never** draw interpolated travel trajectories, flight paths, or road networks between two sequential events simply because an entity participated in both.
- If an entity moves between two kingdoms, the UI displays their participation at both discrete locations. Geographic paths or movement sequences may be rendered only when the canonical backend data model explicitly provides authoritative spatial sequence information sufficient to support that representation. F13 does not define such a backend model.

---

## 8. Map Visual Architecture

The Geographic Map visual architecture balances cartographic clarity with scholarly precision:

```
┌────────────────────────────────────────────────────────────────────────┐
│                      MAP LENS VISUAL ARCHITECTURE                      │
├────────────────────────────────────────────────────────────────────────┤
│ [Global Map Controls & Layer Bar]                                      │
│   ├── Location type filter pills (from B2 canonical location_type)     │
│   ├── Certainty filter toggles (filtering by canonical                 │
│   │     coordinate_status: Known, Approximate, Conflicting)            │
│   ├── Narrative context filter (Associated Narrative Phase / Campaign) │
│   └── Switch to Semantic Companion Directory (A11y Equivalent)         │
├────────────────────────────────────────────────────────────────────────┤
│ [Primary Geographic Canvas]                                            │
│   ├── Contextual Basemap Layer (Subdued, culturally neutral tone)      │
│   ├── Spatial Clustering & Progressive Density Disclosure Nodes        │
│   ├── Canonical Location Markers:                                      │
│   │     ├── Location Icon (Styled by canonical `location_type`)        │
│   │     ├── Epistemic Stroke & Certainty Badge (Non-color indicator)   │
│   │     ├── Primary Canonical Name Label                               │
│   │     └── Event Concentration Indicator (Active incident counter)    │
│   ├── Competing Tradition Callouts (Dual candidate site markers)       │
│   └── Map Navigation Affordances (Pan, Zoom, Extent Reset)             │
├────────────────────────────────────────────────────────────────────────┤
│ [Docked / Contextual Location Detail Panel]                            │
│   └── Slides or docks alongside map when a location is selected        │
├────────────────────────────────────────────────────────────────────────┤
│ [Persistent Unmapped Location Drawer / Tray]                           │
│   └── Direct access to entities lacking coordinates (Zero Erasure)     │
└────────────────────────────────────────────────────────────────────────┘
```

### 8.1 Visual Marker Semantics (Non-Color Relying)
In strict accordance with Block F3 and Constitutional Principle 10:
- **Location Type**: Represented by distinct geometric shapes or symbolic glyphs (e.g., fortress for city/kingdom, tree for forest, crossed weapons for battlefield, shrine for hermitage/sacred site).
- **Certainty Status**: Represented by border stroke patterns and explicit symbol badges:
  - *Known*: Solid stroke with verified check glyph.
  - *Approximate*: Dashed stroke with tilde glyph (`~`).
  - *Conflicting*: Dotted dual-stroke with divergent arrow glyph (`⇄`).
- Color is used strictly as an auxiliary visual accent and never as the sole differentiator.

---

## 9. Spatial Density Management & Clustering Concepts

Ancient sites frequently cluster densely around major river valleys and historical capital regions:

### 9.1 Multi-Scale Progressive Disclosure
1. **Macro Level (Subcontinental Overview)**: High-level overview clustering proximal sites into regional cohorts. Displays aggregate count badges and dominant location types without label overlapping.
2. **Meso Level (Regional Basin / Kingdom Scale)**: Clusters de-aggregate into localized sub-clusters and prominent individual urban/capital nodes.
3. **Micro Level (Local Site / Landmark Scale)**: Individual point markers fully rendered with complete titles, type glyphs, and event counts.

### 9.2 Anti-Collision & Decluttering Invariant
- Marker titles and badges must employ non-overlapping decluttering algorithms.
- Zooming or panning must smoothly transition cluster groupings without disorienting layout jumps.
- When multiple historical sites share identical or near-identical coordinates, they expand into a circular or lateral spiderfy cohort on activation rather than occluding each other.

---

## 10. Unmapped Locations Architecture

A significant proportion of ancient epic toponyms are recorded in narrative episodes but cannot be mapped to physical coordinates with scholarly honesty.

### 10.1 The Zero-Erasure Principle
Ancient locations lacking modern coordinates must never be treated as "second-class" or hidden from users:
- The UI provides a dedicated, prominently accessible **Unmapped Locations Tray / Directory**.
- Users can inspect the same categories of canonical location information and associated entities/evidence that are available for the location, regardless of whether coordinates exist.
- An explicit epistemic badge clearly indicates:
  `"Location attested in canonical texts; geographic coordinates unverified / unmapped."`

---

## 11. Location Detail & Inspection Architecture

Selecting an individual location card or map marker exposes its complete structured context without requiring full-page navigation:

```
┌────────────────────────────────────────────────────────────────────────┐
│                       LOCATION DETAIL INSPECTION                       │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Detail Zone      │ Contextual Information & Functional Affordances     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Header**       │ Canonical name, transliteration, alternate names /  │
│                  │ aliases, location type glyph, and certainty badge.  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Spatial Data** │ Coordinate display (if known), modern region /      │
│                  │ district association (if cataloged), or explicit    │
│                  │ unmapped status indicator.                          │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Summary**      │ Curated encyclopedic summary of the location.       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Narrative      │ Chronological roster of events that occurred at this│
│   Events**       │ site, with direct navigation to `/timeline`.        │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Associated     │ Canonical associated characters, groups, events, and│
│   Entities**     │ other entities where such associations are supplied │
│                  │ by the authoritative data model (links to canonical │
│                  │ entity routes, e.g., `/characters/:slug`).          │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Martial**      │ Associated war campaigns or battles if battlefield  │
│                  │ (with bridge to War Lens `/wars/:slug`).            │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Evidence**     │ Evidence affordance invoking the Block F15 Evidence │
│                  │ Drawer for backing scholarly claims and sources.    │
└──────────────────┴─────────────────────────────────────────────────────┘
```

> **Routing Boundary**: Location inspection is performed in-situ (e.g., docked panel, overlay, or contextual view) or via the canonical route `/geography/:slug`. F13 does not introduce any non-canonical or nested sub-routes.

---

## 12. Geographic Uncertainty & Epistemic-State Presentation

Geographic statements in epic literature carry varying degrees of historical certainty. F13 embodies the project's core principle of Epistemic Honesty:

```
┌────────────────────────────────────────────────────────────────────────┐
│              EPISTEMIC STATE VISUAL GRAMMAR IN GEOGRAPHY               │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Epistemic State  │ Presentation Requirement                            │
│ (Block B2 §5.1)  │                                                     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `known`          │ Standard visual treatment for the `known` state as  │
│                  │ defined by B2/F3; state meaning is not redefined by │
│                  │ F13. Solid non-color visual indicator.              │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `conflicting`    │ Non-color visual indicator distinguishing multiple  │
│                  │ competing claims; candidate modern sites rendered   │
│                  │ simultaneously with distinct tradition callouts.    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `approximate`    │ Non-color visual indicator using the standard       │
│                  │ treatment defined by B2/F3; state meaning is not    │
│                  │ redefined by F13. Dashed boundary or regional halo. │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `unknown`        │ Non-color visual indicator using the standard       │
│                  │ treatment defined by B2/F3; state meaning is not    │
│                  │ redefined by F13. Maintained in unmapped directory. │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `not_researched` │ Non-color visual indicator using the standard       │
│                  │ treatment defined by B2/F3; state meaning is not    │
│                  │ redefined by F13. Maintained in unmapped directory. │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `not_applicable` │ Non-color visual indicator using the standard       │
│                  │ treatment defined by B2/F3; applied only where      │
│                  │ geographic attributes are structurally irrelevant.  │
└──────────────────┴─────────────────────────────────────────────────────┘
```

> **Visual Design Boundary**: F13 defines the semantic requirement that epistemic state must never rely on color alone. Block B2 authoritatively defines epistemic state meaning; Block F3 defines global visual design system tokens; Block F16/F17 govern accessibility and verification.

---

## 13. Conflicting Geographic Traditions & Divergent Identifications

Where the canonical data model supplies multiple geographic identifications for a location:
- Competing accounts are rendered as **Dual or Multiple Candidate Markers** on the map.
- Each marker clearly displays its scholarly tradition or textual recension source as provided by the authoritative data model.
- Activating a candidate marker or evidence affordance invokes the canonical Block F15 evidence experience, exposing the primary text citations and archaeological arguments supporting each claim.
- The UI never arbitrarily selects one geographical hypothesis and discards the other.

---

## 14. Geographic Filtering Architecture

To enable analytical inquiry without modifying underlying data, the Geographic Map Lens provides non-destructive client-side filters:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        MAP FILTER CONTROLS                             │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Filter Dimension │ Conceptual Functionality                            │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Location Type**│ Toggles display by canonical type (`kingdom`,       │
│                  │ `city`, `forest`, `river`, `battlefield`, etc.).    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Geographic     │ Filters by canonical `coordinate_status` values     │
│   Certainty**    │ (`known`, `approximate`, `unknown`, `conflicting`,  │
│                  │ `not_researched`).                                  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Coordinate     │ Toggles display between mapped locations and the    │
│   Presence**     │ unmapped locations directory (based on coordinate   │
│                  │ presence / nullness).                               │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Narrative      │ Highlights locations associated with events in a    │
│   Context**      │ specific narrative phase or campaign.               │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Entity         │ Filters locations associated with a selected        │
│   Involvement**  │ canonical entity through relationships or event     │
│                  │ participation supplied by the authoritative data    │
│                  │ model.                                              │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Text Search**  │ Fast client-side filtering across location names    │
│                  │ and alternate aliases.                              │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 15. Responsive Recomposition Across Viewport Classes

The Geographic Map Lens adapts across the four Block F4 viewport classes without relying on arbitrary pixel breakpoints:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   RESPONSIVE RECOMPOSITION MATRIX                      │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Viewport Class   │ Layout Structure & Interaction Mode                 │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Compact**      │ - Stacked layout: Map viewport on top, expandable   │
│ (Handhelds /     │   location drawer / companion list below.           │
│  Small Screens)  │ - Touch-optimized pan/pinch gestures with disabled  │
│                  │   scroll-capture until explicitly activated.        │
│                  │ - Bottom-sheet for location detail inspection.      │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Medium**       │ - Map-primary split view with a docked location     │
│ (Tablets /       │   roster or inspection panel.                       │
│  Foldables)      │ - Quick toggle to switch to Unmapped Directory.     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Expanded**     │ - Side-by-side layout: Primary spatial canvas with  │
│ (Desktops /      │   docked left-hand filter & search panel, and       │
│  Laptops)        │   docked right-hand location detail inspection view.│
│                  │ - Persistent overview minimap and extent controls.  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Wide**         │ - Panoramic high-resolution spatial canvas.         │
│ (Ultra-wide /    │ - Simultaneous side-by-side presentation of spatial │
│  Multi-Monitor)  │   map, location detail inspection, and persistent   │
│                  │   evidence drawer sidebar.                          │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 16. Dual-Mode Accessibility & Semantic Companion Navigator

Interactive web maps present significant accessibility barriers. Block F13 mandates an **Accessible Semantic Companion Representation**:

### 16.1 The Dual-Mode Architecture
1. **Interactive Visual Map Canvas**: Optimized for spatial orientation, geographic clustering, and visual discovery.
2. **Accessible Semantic Companion**: A structured, fully navigable screen-reader representation providing an equivalent experience without requiring visual map interaction:
   - Nested semantic regions: `<section aria-label="Geographic Explorer">`
   - Grouped regional and kingdom directories (`<nav aria-label="Locations by Region">`)
   - Detailed location lists with structured attributes, coordinates, and associated events
   - Complete access to unmapped locations, citations, and cross-lens navigation bridges

```
┌────────────────────────────────────────────────────────────────────────┐
│                   DUAL-MODE ACCESSIBILITY STRUCTURE                    │
├───────────────────────────────────┬────────────────────────────────────┤
│ Visual Map Canvas                 │ Accessible Semantic Companion      │
│ (Spatial Visual View)             │ (Screen-Reader / Keyboard Mode)    │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Interactive 2D map projection   │ - `<nav aria-label="Locations">`   │
│ - Visual markers and clusters     │ - Hierarchical structured lists    │
│ - Pinch-to-zoom and drag-to-pan   │ - Tabular location metadata view   │
│ - Visual hover tooltips           │ - Fully keyboard-traversable nodes │
│ - Minimap spatial extent view     │ - Screen-reader status updates     │
└───────────────────────────────────┴────────────────────────────────────┘
```

### 16.2 Boundary with Block F16 and Block F17
- **F13 Ownership**: Defines the dual-mode structure, semantic equivalence requirements, and geographic data hierarchy.
- **F16 Ownership**: Authoritatively defines concrete ARIA attributes, landmark regions, keyboard shortcut bindings, focus rings, and screen-reader announcements.
- **F17 Ownership**: Defines automated accessibility verification tests, screen-reader test scripts, and WCAG 2.1 AA quality gates.

---

## 17. Evidence & Provenance Integration (Block F15 Boundary)

Where canonical evidence/provenance is available, F13 exposes an evidence affordance invoking the canonical **Block F15 Evidence Drawer**:
- Activating the `[Evidence ↗]` affordance invokes the canonical **Block F15 Evidence Drawer**.
- The evidence drawer displays the primary textual passages, manuscript editions, archaeological reports, and scholarly commentary justifying the location's identification.
- **Boundary Invariant**: F13 defines the invocation affordance and contextual metadata payload; Block F15 authoritatively governs citation display, manuscript drawer layouts, and excerpt rendering.

---

## 18. Routing, Navigation & Deep-Linking Integration (Block F7)

The Geographic Map Lens conforms to the canonical route taxonomy established in Block F7 §4:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        CANONICAL GEOGRAPHY ROUTES                      │
├──────────────────────────┬─────────────────────────────────────────────┤
│ Route Path Pattern       │ Exploration Perspective                     │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/geography`             │ Subcontinental Epic Map & Regional Index    │
├──────────────────────────┼─────────────────────────────────────────────┤
│ `/geography/:slug`       │ Focused Location Detail & Regional Context  │
└──────────────────────────┴─────────────────────────────────────────────┘
```

### 18.1 Conceptual URL State & Parameter Alignment
- F13 identifies conceptual URL-worthy geographic exploration state (e.g., selected location slug, active location type filters, coordinate certainty filters, and active spatial view mode).
- **F7 Ownership Boundary**: Block F7 alone authoritatively owns whether and how client state is serialized into URL query parameters. F13 does not create, define, or enforce any query parameter contracts. All parameter naming, serialization formats, validation, and defaults remain strictly under Block F7 authority and Stage 4 coordination.

### 18.2 Cross-Lens Deep-Linking
Every location node in the geographic view includes seamless navigation bridges to canonical entity views established by F7:
- **Character Profile**: Link to associated figures at `/characters/:slug`.
- **Focus Graph**: Link to `/graph/:slug` centered on a location entity when canonical entity context exists.
- **Timeline**: Link to `/timeline` filtered by events that occurred at this location.
- **War Campaign**: Link to `/wars/:slug` when the location is a battlefield or war theater.

---

## 19. Lifecycle, Loading, Empty & Error States

The Geographic Map Lens manages asynchronous states with complete epistemic honesty:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        MAP LIFECYCLE STATES                            │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Lifecycle State  │ Visual & Semantic Behavior                          │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Initial        │ Map canvas renders base basemap layer while marker  │
│   Loading**      │ overlays display non-blocking skeleton markers and  │
│                  │ directory placeholders.                             │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Incremental    │ When panning or zooming into a new spatial bounding │
│   Loading**      │ box, subtle localized spinners appear in the status │
│                  │ bar without freezing map interactivity.             │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Empty Query    │ When a search or filter combination yields no       │
│   Result**       │ matching location records, the UI displays a clean  │
│                  │ informational state stating that no canonical       │
│                  │ locations match the current filter criteria.        │
│                  │ Absence of matching records is strictly             │
│                  │ distinguished from epistemic uncertainty.           │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Location Not   │ Display standard 404 entity view with search        │
│   Found**        │ recommendations, conforming to Block F7 error specs.│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Network / API  │ Inline non-destructive retry banner allowing user   │
│   Error**        │ to re-attempt data fetch without losing map extent. │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 20. Cross-Lens Transitions

Navigation between the Geographic Lens and other exploration lenses preserves user exploration continuity:
- **Geography $\leftrightarrow$ Character**: Selecting an associated figure opens `/characters/:slug`. Navigating from a character profile opens `/geography` presenting the canonical geographic context associated with that character.
- **Geography $\leftrightarrow$ Focus Graph**: Selecting a location entity provides an affordance to navigate to `/graph/:slug` when canonical entity context exists.
- **Geography $\leftrightarrow$ Timeline**: Selecting a location's event roster bridges to `/timeline` filtered by events situated at that site.
- **Geography $\leftrightarrow$ War**: Selecting a battlefield location associated with a canonical war entity opens `/wars/:slug`.

---

## 21. State Ownership Distribution

State management strictly follows the hierarchy established in Block F5:

```
┌────────────────────────────────────────────────────────────────────────┐
│                       STATE OWNERSHIP HIERARCHY                        │
├──────────────────┬──────────────────┬──────────────────────────────────┤
│ State Scope      │ Governing Layer  │ Managed State Attributes         │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **URL State**    │ Block F7 Router  │ Canonical route path             │
│                  │                  │ (`/geography`, `/geography/:slug`)│
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Server Cache** │ Block F6 Query   │ Cached Location, Event, and      │
│                  │ Client           │ Claim API response payloads      │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Lens State**   │ Geography Lens   │ Active location type filters,    │
│                  │ State            │ geographic certainty filters     │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Transient UI** │ Local Component  │ Map pan center coordinates, zoom │
│                  │ State            │ level, active hover tooltip,     │
│                  │                  │ unmapped drawer expanded state   │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Preferences**  │ Block F5 Store   │ Global display preferences       │
│                  │                  │ (e.g., basemap theme contrast)   │
└──────────────────┴──────────────────┴──────────────────────────────────┘
```

---

## 22. Performance & Rendering Boundaries

Geographic exploration can encompass hundreds of subcontinental toponyms, regional labels, and associated event milestones.

### 22.1 Rendering Strategy Spectrum (Library-Neutral)
The implementation may select semantic DOM, structured SVG, Canvas/WebGL, or hybrid vector map rendering strategies based on marker density, device capability, and performance verification:
1. **Semantic DOM Tier**: Well-suited for Companion Directory views, low-density location lists, and Compact viewports. Provides direct DOM accessibility with zero WebGL overhead.
2. **Structured SVG Tier**: Well-suited for medium-density vector overlays, regional kingdom maps, and static boundary indicators. Provides scalable vector resolution and native event handling.
3. **Canvas / WebGL / Vector Tile Tier**: Well-suited for expansive subcontinental maps with smooth continuous panning, multi-scale clustering, and dense marker arrays.

### 22.2 Quantitative Verification Boundary
F13 defines conceptual rendering tiers and architectural invariants only. Fixed marker-count thresholds, frame rate targets, and bundle size budgets are strictly owned and verified by **Block F17**.

---

## 23. Security & Untrusted Data Protections

1. **Safe Text Ingestion**: All location names, transliterations, summaries, alternate names, and regional descriptions from backend payloads are treated as untrusted data and rendered strictly via safe DOM text nodes or sanitized framework properties. Raw HTML injection is strictly prohibited.
2. **Untrusted Route Input Handling**: Handling and canonical validation of URL parameters (`/geography/:slug`) are deferred strictly to Block F7 routing and validation architecture before any backend queries are initiated. F13 does not define a separate slug grammar.

---

## 24. Architectural Ownership Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                     ARCHITECTURAL OWNERSHIP MATRIX                     │
├────────────────────────────┬─────────────┬─────────────────────────────┤
│ Architectural Concern      │ Owner Block │ Boundary Description        │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Canonical Location Model   │ Block B2    │ DB tables, coordinates, FKs │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Knowledge Graph Relations  │ Block B3    │ Entity relationships & edges│
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Location API Endpoints     │ Block B5    │ REST contracts & envelopes  │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Geographic Visual Model    │ Block F13   │ Spatial layout & map UX     │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ General Relationship Graph │ Block F10   │ 2-hop topological network   │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Family Lineage Tree DAG    │ Block F11   │ Generational kinship DAG    │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Timeline Chronology        │ Block F12   │ Narrative event sequencing  │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Geography Route & URL State│ Block F7    │ Route grammar & parameters  │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Evidence Drawer Citation   │ Block F15   │ Textual claims & citations  │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Concrete A11y & ARIA       │ Block F16   │ Concrete screen-reader code │
├────────────────────────────┼─────────────┼─────────────────────────────┤
│ Performance Budgets & Tests│ Block F17   │ Quantitative verification   │
└────────────────────────────┴─────────────┴─────────────────────────────┘
```

---

## 25. Architectural Decision Records (ADRs)

### ADR-F13-01: Explicit Separation of Coordinate Presence from Epistemic State
- **Context**: Locations lacking physical coordinates risk being assigned synthetic dummy coordinates or being conflated with a generic "unmapped" epistemic state.
- **Decision**: Preserve coordinate absence as `NULL` coordinates representing an operational Unmapped state, while consuming the exact five B2 field-level states (`known`, `approximate`, `unknown`, `conflicting`, `not_researched`) for `locations.coordinate_status`.
- **Consequences**: Strictly adheres to the project's Zero Fabrication rule and avoids inventing a seventh epistemic state.

### ADR-F13-02: Zero Fabrication of Travel Routes and Frontiers
- **Context**: Connecting sequential narrative events with interpolated lines creates misleading impressions of ancient roads, travel routes, or army marches.
- **Decision**: Restrict route rendering strictly to instances where the canonical backend data model explicitly provides authoritative spatial sequence information sufficient to support that representation. Chronological event sequences alone must never be rendered as travel paths.
- **Consequences**: Upholds Epistemic Honesty and prevents the visualization of fabricated ancient geography.

### ADR-F13-03: Coexistence of Competing Geographic Traditions
- **Context**: Where the canonical data model supplies multiple geographic identifications for an ancient location, the frontend requires a deterministic presentation strategy that avoids arbitrary selection.
- **Decision**: Where supplied by the canonical data model, support simultaneous display of dual or multiple candidate markers tagged with their scholarly tradition, each linking to its backing claim in Block F15.
- **Consequences**: Supports unbiased scholarly inquiry without arbitrarily selecting a single hypothesis or inventing backend storage models.

### ADR-F13-04: Dual-Mode Accessibility with Semantic Companion Navigator
- **Context**: Visual interactive maps are fundamentally inaccessible to screen readers and keyboard-only users.
- **Decision**: Require an accessible semantic companion representation presenting full hierarchical regional directories and location rosters alongside the visual map.
- **Consequences**: Fully complies with Constitutional Principle 10 (Accessibility as a First-Class Requirement).

### ADR-F13-05: Strict Boundary Between Geography, Timeline, and War Lenses
- **Context**: Locations intersect with temporal narrative events and tactical battlefield formations, risking duplicated architectural responsibilities.
- **Decision**: Strictly isolate spatial layout and toponym inspection to Block F13, narrative sequence ordering to Block F12, and tactical battlefield formations to Block F14.
- **Consequences**: Clean separation of concerns, reusable components, and distinct user mental models.

---

## 26. Requirement Traceability Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                   REQUIREMENT TRACEABILITY MATRIX                      │
├───────────────────┬──────────────────┬─────────────────────────────────┤
│ Upstream Req ID   │ Source Document  │ Block F13 Architectural Mapping │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REQ-LOC-01**    │ B0 §3.G;         │ Section 5.1, Section 5.2:       │
│                   │ PRD §9.6 (MAP-001│ Canonical location attributes.  │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REQ-LOC-02**    │ B0 §3.G; Rule 03;│ Section 2.2, Section 6, §10:    │
│                   │ PRD §9.6 (MAP-005│ Zero coordinate fabrication.    │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REQ-LOC-03**    │ B0 §3.G;         │ Section 7.1, Section 11: Event  │
│                   │ PRD §9.6 (MAP-002│ associations & path rules.      │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REQ-LOC-04**    │ B0 §3.G;         │ Section 8, Section 9, §14:      │
│                   │ PRD §9.6 (MAP-006│ Spatial layers & clustering.    │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **MAP-003**       │ PRD §9.6         │ Section 15: Responsive layout   │
│                   │                  │ across 4 viewport classes.      │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **NAV-001**       │ PRD §9.12; F7 §4 │ Section 18: Canonical routes    │
│                   │                  │ `/geography`, `/geography/:slug`│
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **A11Y-001**      │ PRD §9.13; F3 §4 │ Section 16: Dual-mode semantic  │
│                   │                  │ companion representation.       │
└──────────────────┴──────────────────┴─────────────────────────────────┘
```

---

## 27. F13 Exit Criteria & Acceptance Checklist

- [x] Geographic map purpose, UX role, and distinct boundary against F10, F11, F12, F14 established.
- [x] Read-only projection model defined transforming canonical `Location` and event records into spatial views.
- [x] Absolute zero-fabrication rule defined: no client geocoding, no inferred coordinates, no fabricated boundaries or routes.
- [x] Coordinate certainty model establishes clear distinction between canonical coordinates, approximate correspondence, conflicting traditions, and unmapped sites.
- [x] Explicit preservation of the rule that "Unmapped" is an operational presentation state resulting from null coordinates, not a seventh epistemic state.
- [x] Field-level epistemic state subset for `locations.coordinate_status` matches canonical B2 definition (`known`, `approximate`, `unknown`, `conflicting`, `not_researched`).
- [x] Dedicated unmapped location directory architected ensuring zero erasure of unlocated ancient toponyms.
- [x] Location detail inspection model defined exposing identity, summary, coordinates, associated events, entities, and evidence affordances.
- [x] Visual marker semantics specified with non-color indicators (shapes, stroke patterns, glyphs) per Block F3.
- [x] Geographic filtering architecture defined conceptually without data mutation or invented parameters.
- [x] Responsive recomposition mapped across the four Block F4 viewport classes.
- [x] Dual-mode accessibility model (Visual Map + Accessible Companion Navigator) architected with F16/F17 demarcation.
- [x] Canonical routes `/geography` and `/geography/:slug` preserved; query-parameter serialization remains exclusively under F7 authority.
- [x] High-frequency interaction state isolated from URL/storage per Block F5.
- [x] Performance architecture defines conceptual rendering tiers (DOM, SVG, Canvas/WebGL) with budgets deferred to F17.
- [x] Security and untrusted data protections specified for text rendering and deep links; route validation deferred to F7.
- [x] Zero application source code, UI packages, or concrete mapping libraries were selected or introduced.
- [x] Zero Stage 1 backend documents or Blocks F1–F12 documents were modified.