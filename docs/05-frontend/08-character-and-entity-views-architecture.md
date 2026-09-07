# Character & Entity Views Architecture (Block F9)

## 1. Architectural Purpose & Scope

This document establishes the **Character & Entity Views Architecture** for the **Mahābhārata Explorer** frontend. It defines the canonical entity detail view UX, layout composition, progressive disclosure hierarchy, cross-lens launch points, epistemic status presentation, and evidence integration boundaries for Stage 2.

```
┌────────────────────────────────────────────────────────────────────────┐
│                   STAGE 2 ARCHITECTURAL PLACEMENT                      │
├───────────────────────────────────┬────────────────────────────────────┤
│ UPSTREAM CONTRACTS & CONSTRAINTS  │ DOWNSTREAM FRONTEND SPECIFICATIONS │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Stage 1 Backend Architecture    │ - F10: Knowledge Graph Canvas      │
│   (B2 Data, B4 Provenance, B5 API,│ - F11: Family Lineage Tree         │
│    B6 Search, B9 Perf/Rate Limits)│ - F12: Timeline Scrubber           │
│ - Block F1: Frontend Constitution │ - F13: Geographic Map Engine       │
│ - Block F2: Tech Stack (React SPA)│ - F14: War & Tactical Vyuhas       │
│ - Block F3: Design System Tokens  │ - F15: Evidence & Provenance UX    │
│ - Block F4: Responsive Shell      │ - F16: Accessibility & IAST        │
│ - Block F5: State Management Model│ - F17: Testing & Budgets           │
│ - Block F6: API Client & Caching  │                                    │
│ - Block F7: Routing & URL Grammar │                                    │
│ - Block F8: Search & Autocomplete │                                    │
└───────────────────────────────────┴────────────────────────────────────┘
```

The primary detailed exemplar throughout this specification is the **Character View** (`/characters/:slug`), representing individual figures within the epic (such as *Arjuna*, *Karṇa*, *Bhīṣma*, or *Draupadī*). Concurrently, this document establishes a **Reusable Entity View Pattern** applicable to all canonical entity domains established by Stage 1 (`Location`, `Group`, `Event`, `War`, `Formation`, `Source`).

### 1.1 Pure Architectural Specification & Library Neutrality
Block F9 is strictly an architectural specification. It defines visual hierarchies, interaction flows, information disclosure rules, and state boundaries. It does not implement React components, import UI component libraries, bundle icons, configure network hooks, or implement visualization engines.

### 1.2 Explicit Ownership Boundaries
- **Block F9 Owns**:
  - Canonical entity detail view layouts and visual information hierarchy.
  - Character profile structure, facet information architecture, and metadata panels.
  - Progressive disclosure model across entity overview, narrative, relationships, events, and evidence.
  - Cross-lens navigation launch points (linking character profiles to Graph, Lineage, Timeline, Map, and War views).
  - Presentation contracts for the six canonical B2 epistemic states on entity profiles.
  - Invocation trigger boundaries for the Block F15 Evidence Drawer.
  - Reusable entity detail view contracts across other canonical domains.
  - Responsive recomposition of entity views across the four F4 viewport classes.
- **Block F9 Does NOT Own**:
  - Backend database schemas, joins, or SQL query performance (owned by B2/B5/Stage 4).
  - API client network fetching, caching, and conditional HTTP revalidation (owned by F6).
  - Canonical URL route grammar, slug parsing, and parameter validation (owned by F7).
  - Search entry, autocomplete suggestion orchestration, and `/search` results UX (owned by F8).
  - Deep visualization rendering engines (Graph canvas F10, Lineage DAG F11, Timeline scrubber F12, Map engine F13, Vyuha canvas F14).
  - Detailed scholarly evidence drawer contents, citation locators, and shloka viewers (owned by F15).
  - Global typography engine, IAST transliteration toggle, and system-wide accessibility implementation (owned by F16).
  - Formal frontend numeric performance budgets and bundle size audits (owned by F17).
  - Concrete React component implementations and styling packages (owned by Stage 4).

---

## 2. Core Architectural Principles

Entity detail views are governed by 10 foundational principles:

```
┌────────────────────────────────────────────────────────────────────────┐
│                      ENTITY VIEW PRINCIPLES                            │
├────────────────────────────────────────────────────────────────────────┤
│ 1. KNOWLEDGE EXPLORER FIRST: An entity view is an interconnected       │
│    knowledge node, offering immediate launch points into analytical    │
│    lenses rather than serving as a static dead-end biographical page.  │
│ 2. ONE GRAPH, MANY LENSES: The entity profile is the primary textual   │
│    anchor for an entity; graph, lineage, map, and timeline views       │
│    represent analytical perspectives centered on that same entity.     │
│ 3. ZERO DATA FABRICATION: Missing attributes (birth parentage, dates,  │
│    portraits, coordinates) are rendered as explicit epistemic states   │
│    or omitted cleanly; placeholder values are strictly prohibited.     │
│ 4. PROGRESSIVE DISCLOSURE: Information unfolds hierarchically from     │
│    glanceable identity to rich narrative, structured connections, and  │
│    deep primary textual evidence.                                      │
│ 5. EVIDENCE-AWARE PRESENTATION: Assertions, kinship links, and events  │
│    expose contextual scholarly citation triggers that open the F15     │
│    Evidence Drawer on demand.                                          │
│ 6. MULTILINGUAL & IAST INTEGRITY: Canonical names, epithets, and       │
│    aliases preserve accurate IAST transliteration and Devanāgarī.      │
│ 7. CLEAN DEEP-LINK FIDELITY: Entity views resolve cleanly from their   │
│    canonical F7 path (`/characters/:slug`), preserving URL-           │
│    representable exploration state only where defined by Block F7.     │
│ 8. RESPONSIVE RECOMPOSITION: Multi-pane desktop layouts recompose into │
│    ergonomic single-column flows on mobile without truncating data.   │
│ 9. VISUAL RESTRAINT: Modern scholarly aesthetics prioritize legibility │
│    and structural clarity over decorative mythologizing.               │
│ 10. UNTRUSTED DATA SAFETY: All entity summaries and metadata are       │
│     rendered safely without dynamic HTML injection.                    │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Canonical Entity View Model

Canonical entity views share a common five-tier information hierarchy, with domain-specific tiers or sections appearing only where applicable:

```
┌────────────────────────────────────────────────────────────────────────┐
│                     CANONICAL ENTITY VIEW MODEL                        │
├────────────────────────────────────────────────────────────────────────┤
│ TIER 1: IDENTITY & HEADER LANDMARK                                     │
│ - Canonical Name (IAST) + Script Representation (Devanāgarī)           │
│ - Curated Portrait (if authentic image exists; NO default silhouette)  │
│ - Domain Classification Badge (e.g., Character, Location, Group)       │
│ - Epistemic Certainty Indicator (Six B2 States)                        │
│ - Known Aliases / Epithets (`alternate_names`)                         │
├────────────────────────────────────────────────────────────────────────┤
│ TIER 2: PRIMARY BIOGRAPHICAL / DESCRIPTIVE OVERVIEW                    │
│ - Scholarly Summary (Curated high-level narrative context)             │
│ - Core Descriptive Metadata Grid (Kinship summary, allegiance, role)   │
│ - Global Evidence Trigger (Direct access to backing claims via F15)    │
├────────────────────────────────────────────────────────────────────────┤
│ TIER 3: CROSS-LENS EXPLORATION LAUNCHPAD                               │
│ - Direct navigation pills to contextual analytical lenses:             │
│   [Focus Graph ($D \le 2$)] [Lineage DAG] [Chronology] [Geography]     │
├────────────────────────────────────────────────────────────────────────┤
│ TIER 4: STRUCTURED FACET PANELS / INFORMATION SECTIONS                 │
│ - Narrative & Key Episodes (Parva-ordered event participation)         │
│ - Kinship & Generational Context (FamilyRelationship graph)            │
│ - Inter-Entity Relationships (1-hop Relationship edges)                │
│ - Military Context (Wars, War Days, Formations if applicable)          │
├────────────────────────────────────────────────────────────────────────┤
│ TIER 5: PROVENANCE & ATTRIBUTION FOOTER                                │
│ - Primary Edition References (e.g., Critical Edition citations)        │
│ - Optional provenance or curation metadata where supplied by backend   │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 4. Character Detail View Architecture (`/characters/:slug`)

The character detail view is the flagship realization of the canonical model, structured into coherent functional zones:

> **Illustrative layout example** — actual fields, quick facts, and metadata values are data-dependent based on available backend records; their presence and content depend entirely on available domain data.

```
┌────────────────────────────────────────────────────────────────────────┐
│                 CHARACTER DETAIL VIEW LAYOUT (EXPANDED)                │
├───────────────────────────────────┬────────────────────────────────────┤
│ LEFT PANEL: IDENTITY & METADATA   │ RIGHT PANEL: NARRATIVE & FACETS    │
├───────────────────────────────────┼────────────────────────────────────┤
│ [Portrait / Icon Badge]           │ [Canonical Name: Arjuna]           │
│ [IAST Name: Arjuna]               │ [Devanāgarī: अर्जुन]                │
│ [Epistemic State: Known]          │ [Aliases: Pārtha, Dhanañjaya, ...] │
│                                   │                                    │
│ Quick Facts:                      │ Biographical Summary:              │
│ - Father: Pāṇḍu (Biological: Indra│ Full narrative summary curated     │
│ - Mother: Kuntī                   │ from critical edition accounts...  │
│ - Faction: Pāṇḍava                │                                    │
│ - Parva Range: Ādi to Svargārohaṇa│ Lens Launchpad:                    │
│                                   │ [Explore Graph] [View Lineage]     │
│ Primary Backing Claims:           │ [View on Map]   [War Actions]      │
│ [{count} Verified Claims ↗ (F15)] │                                    │
│                                   │ ────────────────────────────────── │
│                                   │ Facet Navigation / Sections:       │
│                                   │ [Episodes] [Kinship] [Relations]   │
│                                   │                                    │
│                                   │ Active Facet Content Panel         │
│                                   │ (Rendered dynamically based on     │
│                                   │  active selection)                 │
└───────────────────────────────────┴────────────────────────────────────┘
```

---

## 5. Reusable Entity View Architecture for Other Canonical Domains

While Character profiles contain specialized kinship and martial facets, the underlying information architecture generalizes across all canonical Stage 1 entity domains:

```
┌────────────────────────────────────────────────────────────────────────┐
│                 DOMAIN-SPECIFIC FACET SPECIALIZATION                   │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Domain (B2/B6)   │ Specialized Facet Panels & Contextual Lenses        │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Location**     │ - Geographic Coordinates & Spatial Precision        │
│ (`/geography/    │ - Significant Events Occurred Here (Timeline link)  │
│  :slug`)         │ - Associated Rulers, Dynasties & Sages              │
│                  │ - Interactive Map Launchpad (`/geography/:slug`)    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Group**        │ - Dynastic / Clan Hierarchy & Leadership Succession │
│ (`/factions/     │ - Collective Alliances & Historic Feuds             │
│  :slug`)         │ - Member Roster with Direct Character Links         │
│                  │ - Graph Launchpad centered on Group (`/graph/:slug`)│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Event**        │ - Chronological Placement (Sequence index, Parva)   │
│ (`/timeline/     │ - Geographic Location of Occurrence                 │
│  :slug`)         │ - Participating Characters & Their Specific Roles   │
│                  │ - Timeline Scrubber Launchpad (`/timeline/:slug`)   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **War**          │ - Campaign Summary & Temporal Arc                   │
│ (`/wars/         │ - War-day / campaign navigation launchpoint, where  │
│  :war_slug`)     │   applicable (linking to F14)                       │
│                  │ - Overall Faction Rosters & Command Structure       │
│                  │ - Tactical Military Formations Executed             │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Formation**    │ - Tactical Geometry Description (Described vs Visual│
│ (`/vyuhas/       │ - Associated War Day & Commander                    │
│  :slug`)         │ - Historic Counter-Formations & Breaching Incidents │
│                  │ - Vyuha Canvas Launchpad (`/vyuhas/:slug`)          │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Source**       │ Contextual provenance/source representation;        │
│ (Canonical B2)   │ detailed source and evidence presentation is        │
│                  │ delegated to Block F15, subject to the canonical    │
│                  │ frontend navigation contract established by Block F7│
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 6. Entity Identity & Header Architecture

The entity header serves as the visual anchor and authority landmark:

```
┌────────────────────────────────────────────────────────────────────────┐
│                       ENTITY HEADER ANATOMY                            │
├────────────────────────────────────────────────────────────────────────┤
│ [Optional Authentic Portrait]                                          │
│   ├── If present: Appropriate authentic media asset supplied through   │
│   │   the media architecture (Block B8).                               │
│   └── If absent: Omit cleanly. Render scholarly monogram or domain     │
│       icon. DO NOT fabricate generic silhouettes or fictional AI faces.│
│                                                                        │
│ [Typography Hierarchy]:                                                │
│   ├── H1: Canonical Roman Name with IAST Diacritics (e.g., *Bhīṣma*)   │
│   ├── Secondary: Original Script Representation (e.g., भीष्म)          │
│   └── Tertiary: Comma-separated Epithets / Aliases (e.g., *Gāṅgeya*)   │
│                                                                        │
│ [Status & Verification Badges]:                                        │
│   ├── Domain Badge: Semantic tag (*Character*, *Location*, *Group*)    │
│   └── Epistemic Badge: Non-color differentiated indicator (B2 state)   │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 7. Progressive Disclosure Hierarchy

To prevent cognitive overload while maintaining scholarly rigor, information is presented in four distinct disclosure layers:

```
┌────────────────────────────────────────────────────────────────────────┐
│                    PROGRESSIVE DISCLOSURE LAYERS                       │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Layer            │ Scope & Content Presentation                        │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Layer 1:       │ Identity, canonical name, IAST/script rendering,    │
│ Glance**         │ primary domain classification, epistemic state, and │
│                  │ essential one-line context.                         │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Layer 2:       │ Curated biographical summary, core relationship     │
│ Overview**       │ badges, primary Parva occurrences, and cross-lens   │
│                  │ exploration launch pills.                           │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Layer 3:       │ Structured facets: event chronologies, direct-family│
│ Deep Structure** │ relationships and lineage context, 1-hop relation   │
│                  │ rosters, and martial context.                       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Layer 4:       │ Primary textual evidence, specific Critical Edition │
│ Provenance**     │ shloka references, recension variances, and claims  │
│                  │ displayed in the F15 Evidence Drawer.               │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 8. Core Character Information Areas & Facets

For Character profiles, Tier 4 encompasses four core information areas/facets: (1) Narrative & Episodes, (2) Kinship & Family, (3) Inter-Entity Relationships, and (4) Military & War Context. These facets may be realized as tabs, stacked sections, accordions, or responsive equivalents depending on the viewport and layout context; Block F9 defines the information hierarchy, while Block F4 and Stage 4 govern their responsive component realization. Block F9 consumes the public entity sub-resource API representations established by Stage 1 Block B5 without defining or altering the API contract:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   CORE CHARACTER INFORMATION FACETS                    │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Facet Area       │ Scope & Content Presentation                        │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **1. Narrative   │ Chronological listing of discrete narrative events  │
│ & Episodes**     │ where the character participated (from B5           │
│                  │ `/characters/:slug/events`). Ordered by sequence    │
│                  │ index and grouped by Parva.                         │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **2. Kinship &   │ Direct ancestral lineage, siblings, consorts, and   │
│ Family**         │ descendants (from B5 `/characters/:slug/family`).   │
│                  │ Displays compact family cards and provides a direct │
│                  │ link to the full interactive Lineage DAG (F11).     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **3. Inter-Entity│ Directed 1-hop graph connections (from B5           │
│ Relationships**  │ `/characters/:slug/relationships`), categorized by  │
│                  │ canonical type (e.g., ally, rival, teacher_of).     │
│                  │ Offers launchpad to the Focus Graph Canvas (F10).   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **4. Military &  │ Conflict participation in Kurukshetra: war days     │
│ War Context**    │ active, tactical military formations joined, duels  │
│                  │ fought, and day of fall (linking to F14).           │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 9. Relationship & Family Presentation Boundaries

1. **1-Hop Direct Presentation**: Entity detail views present direct 1-hop inter-entity relationships (e.g., `ally`, `rival`, `teacher_of`, `disciple_of`) and kinship connections (e.g., `father`, `mother`, `son`, `daughter`, sibling, spouse) using the canonical relationship types supplied by B2/B5.
2. **Kinship Graph Boundaries**: The character view renders direct relatives in structured cards (parents, spouses, siblings, children). Deep multi-generational branching is delegated to the **Family Lineage Tree (Block F11)** via a prominent launch action.
3. **Graph Traversal Boundaries**: Multi-hop graph exploration ($D \le 2$) is delegated to the **Knowledge Graph Canvas (Block F10)**. Entity views never attempt to embed a full force-directed physics engine within the profile page.

---

## 10. Timeline & Event Participation Boundaries

1. **Chronological Participation List**: The "Narrative & Episodes" facet displays a structured chronological list of events where the entity participated, consuming the B5 `/characters/:slug/events` endpoint.
2. **Event Item Anatomy**: Each event entry presents the event title, relative chronological label, Parva attribution, brief narrative excerpt, and a direct link to the canonical event route (`/timeline/:slug`).
3. **Interactive Timeline Delegation**: Comprehensive multi-entity timeline scrubbing, temporal zoom, and cross-parva filtering are delegated to the **Timeline Scrubber (Block F12)**.

---

## 11. Geography & Location Presentation Boundaries

1. **Geographic Association**: For characters and groups, associated locations (birthplace, kingdom of rule, sacred sites visited during pilgrimage, battlegrounds) are displayed as contextual metadata cards with coordinate status badges.
2. **Interactive Mapping Delegation**: Geospatial map rendering, GIS layers, terrain topography, and coordinate uncertainty radii are delegated to the **Geographic Map Engine (Block F13)** via the `/geography/:slug` route.

---

## 12. War, Formations & Faction Context Boundaries

1. **War Participation Summary**: For epic warriors, profile views summarize faction allegiance (e.g., *Pāṇḍava* or *Kaurava* alliance), key battles, and war days active.
2. **Formation (Vyuha) Interaction**: If a warrior commanded or breached a military formation, the profile references the formation with an epistemic badge and links directly to `/vyuhas/:slug`.
3. **Tactical Simulation Delegation**: Interactive formation geometry, tactical unit distributions, and animated daily battle steppers are delegated to **War & Tactical Vyuhas (Block F14)**.

---

## 13. Epistemic-State Presentation Architecture

Block B2 is authoritative for epistemic semantics and Block F3 is authoritative for visual grammar; Block F9 defines only entity-view presentation and UI application of the exact six canonical B2 epistemic states:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   EPISTEMIC PRESENTATION CONTRACT                      │
├──────────────────┬──────────────────┬──────────────────────────────────┤
│ Epistemic State  │ Non-Color Glyph  │ Entity-View Presentation Behavior│
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **`known`**      │ Solid dot / check│ Rendered with standard indicator │
│                  │                  │ and direct link to backing claim.│
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **`conflicting`**│ Split circle /   │ Competing accounts rendered      │
│                  │ divergent arrows │ side-by-side with claim links.   │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **`approximate`**│ Tilde / dashed   │ Presented with approximate badge │
│                  │ perimeter        │ without false precision.         │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **`unknown`**    │ Open question mark Rendered with explicit unknown    │
│                  │                  │ state; zero fabricated values.   │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **`not_researched`│ Dotted ring     │ Displayed with unresearched badge│
│                  │                  │ without asserting absence.       │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **`not_applicable`│ Diagonal strike │ Cleanly omitted or marked as     │
│                  │                  │ structurally non-applicable.     │
└──────────────────┴──────────────────┴──────────────────────────────────┘
```
*Zero Fabrication Rule*: When parentage, dates, or attributes are contested, competing accounts are rendered side-by-side with `conflicting` badges linking to backing claims; certainty is never fabricated.

---

## 14. Provenance & Evidence Integration Boundaries (Block F15)

1. **Evidence Triggers, Not Full Drawers**: Entity detail views provide compact evidence trigger pills and claim indicators (e.g., `[Evidence: {count} Citations ↗]`).
2. **Drawer Invocation**: Clicking an evidence trigger dispatches a request to open the **Block F15 Evidence Drawer**, passing the specific `claim_id` or `entity_id`.
3. **Deep-Link State**: When an evidence trigger is activated, the URL is updated with the claim identifier (`/characters/:slug?claim={claim_id}`) in compliance with Block F7 (§9).

---

## 15. Search to Entity Navigation Continuity (Block F8)

1. **Direct Landing**: Users arriving from Block F8 autocomplete or search result cards land directly on the canonical route (`/characters/:slug`).
2. **Search Term Highlighting**: If navigation occurred from a committed search query, matching text occurrences in the entity summary may be subtly highlighted using non-distracting semantic markers.
3. **History Preservation**: Navigating back via browser history returns the user to their exact previous search state (`/search?q={query}&offset={n}`), restoring the URL-representable search state, including applicable filters and pagination parameters.

---

## 16. URL & Deep-Link State Integration (Block F7)

Entity views strictly comply with the routing architecture established by Block F7:

1. **Canonical Route Paths**: Entity identity is defined authoritatively by the canonical route paths established in Block F7 (`/characters/:slug`, `/geography/:slug`, `/factions/:slug`, `/timeline/:slug`, `/wars/:war_slug`, `/vyuhas/:slug`).
2. **Sub-View URL Integration**: Entity sub-views (such as facet sections or contextual panels) may be URL-representable where defined by Block F7. Block F9 defines the sub-view concepts, layout, and UX hierarchy, while Block F7 defines any concrete URL representation, parameter naming, and query serialization.
3. **Evidence Parameter Serialization**: Activation of contextual claims or evidence triggers coordinates with Block F7 for any concrete URL representation (such as query parameters representing active evidence claims).
4. **F7 Path Validation Authority**: Block F7 authoritatively parses and validates the `:slug` path parameter. If `:slug` does not exist, it resolves to a standard 404 Not Found outcome without silent redirection or clamping.

---

## 17. State Ownership Integration (Blocks F5, F6, F7, F9)

```
┌────────────────────────────────────────────────────────────────────────┐
│                        ENTITY STATE GOVERNANCE                         │
├──────────────┬──────────────┬──────────────────────────────────────────┤
│ Layer        │ Owner        │ State Scope & Responsibility             │
├──────────────┼──────────────┼──────────────────────────────────────────┤
│ **URL State**│ **Block F7** │ Canonical entity slug (`:slug`) and any  │
│              │              │ URL-representable sub-view or claim state│
├──────────────┼──────────────┼──────────────────────────────────────────┤
│ **Server     │ **Block F6** │ Cached character profile DTO, family DAG,│
│ State**      │              │ relationships list, events list, ETag    │
├──────────────┼──────────────┼──────────────────────────────────────────┤
│ **UI State** │ **Block F9** │ Expanded/collapsed accordion panels,     │
│              │              │ transient panel interaction state        │
├──────────────┼──────────────┼──────────────────────────────────────────┤
│ **Client     │ **Block F5** │ Transliteration script preference (IAST  │
│ Prefs**      │              │ vs. Devanāgarī via F16 engine)           │
└──────────────┴──────────────┴──────────────────────────────────────────┘
```

---

## 18. Responsive Recomposition across F4 Viewport Classes

Entity views adapt structurally across Block F4's four viewport classes:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   RESPONSIVE RECOMPOSITION MATRIX                      │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Viewport Class   │ Entity Profile Layout & Behavioral Adaptation       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Wide**         │ Two-column asymmetric layout: fixed left identity   │
│ (F4 Wide Class)  │ card (portrait, quick facts, lens launchpad); right │
│                  │ wide narrative pane with horizontal facet navigation│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Expanded**     │ Balanced two-column grid; identity card scales      │
│ (F4 Expanded     │ proportionally; facet panels utilize full right     │
│  Class)          │ column width.                                       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Medium**       │ Single-column stacked layout: identity header on    │
│ (F4 Medium Class)│ top; quick facts collapsed into expandable card;    │
│                  │ horizontal scrollable navigation bar for facets.    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Compact**      │ Vertical single-column stream: sticky minimal header│
│ (F4 Compact      │ with back navigation; facet sections transform into │
│  Class)          │ vertical collapsible accordions for touch scrolling.│
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 19. Accessibility Architecture Boundaries (Block F16)

1. **Landmark Semantics**: Entity views provide clear structural landmarks (header, main, entity navigation landmarks).
2. **Facet Navigation Semantics**: Facet navigation provides standard accessible selection and grouping semantics (such as tabs or accordions), ensuring unambiguous programmatic association between controls and content panels.
3. **Heading Hierarchy**: H1 strictly reserved for the canonical entity name; H2 for major section headings; H3 for event and relation titles.
4. **Accessible Script Switching**: Script toggles (IAST to Devanāgarī) update DOM text nodes and accessible labels simultaneously without disrupting assistive technology pronunciation.
5. **System-wide A11y Delegation**: Detailed focus rings, keyboard navigation traps, contrast token validation, screen reader live announcements, and exact ARIA attribute implementations are authoritatively governed by Block **F16** and implemented in Stage 4.

---

## 20. Lifecycle, Loading, Empty & Error States

```
┌────────────────────────────────────────────────────────────────────────┐
│                   ENTITY VIEW LIFECYCLE MATRIX                         │
├───────────────────┬────────────────────────────────────────────────────┤
│ State             │ User Experience & Presentation Contract            │
├───────────────────┼────────────────────────────────────────────────────┤
│ **Loading**       │ Structural skeleton screen matching the 2-column   │
│                   │ layout; prevents cumulative layout shift (CLS).    │
├───────────────────┼────────────────────────────────────────────────────┤
│ **Ready**         │ Complete entity profile rendered with Tier 1–5     │
│                   │ information hierarchy.                             │
├───────────────────┼────────────────────────────────────────────────────┤
│ **Not Found       │ Clean 404 Not Found outcome with helpful links to  │
│ (Nonexistent      │ global search and the relevant entity/lens index   │
│  Resource)**      │ where applicable.                                  │
├───────────────────┼────────────────────────────────────────────────────┤
│ **Network Error** │ Inline user-facing error notice with retry button; │
│                   │ preserves cached data if available under F6.       │
├───────────────────┼────────────────────────────────────────────────────┤
│ **Missing Data**  │ Clean omission or explicit epistemic badge; zero   │
│                   │ fabricated placeholder strings (e.g., no "N/A").   │
└───────────────────┴────────────────────────────────────────────────────┘
```

---

## 21. Performance Architecture Boundaries

1. **Payload Budgets**: Entity profile views consume the Stage 1 Tier 2 canonical payload limit ($\le 100\text{ KB}$ as established in B9 §5).
2. **Sub-Resource Lazy Loading**: Heavy sub-resource collections (e.g., extensive event participation lists or secondary claims) are fetched on-demand when their respective facets or sections are activated.
3. **Caching & Revalidation**: HTTP caching and ETag revalidation are handled entirely by Block **F6**.
4. **Formal Frontend Performance Budgets**: Concrete render latency budgets, component bundle sizes, and Core Web Vitals targets are governed by Block **F17**.

---

## 22. Security & Untrusted Data Considerations

1. **Safe Text Rendering**: All biographical summaries, event descriptions, and alias arrays from backend APIs are treated as untrusted data and rendered without unescaped HTML injection.
2. **Secure Media Assets**: Portrait images are loaded exclusively from authorized CDN origins established in Stage 1 Block B8.
3. **Anonymous Exploration**: Entity detail views operate entirely within the public anonymous read-only boundary established by Stage 1; administrative curation actions are completely excluded.

---

## 23. Upstream Dependency & Downstream Ownership Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                     DOWNSTREAM OWNERSHIP MATRIX                        │
├────────────────────────────────────────┬───────────────────────────────┤
│ Architectural Domain                   │ Authoritative Target Block    │
├────────────────────────────────────────┼───────────────────────────────┤
│ Canonical Entity Domain & Data Types   │ **Stage 1 (B2 / B5 / B6)**    │
│ Media Asset Origins & Storage          │ **Stage 1 (B8)**              │
│ Design Tokens & Visual Badges          │ **Block F3**                  │
│ Application Shell & Viewport Classes   │ **Block F4**                  │
│ State Management & Client Store Model  │ **Block F5**                  │
│ API Client, Network & Cache Layer      │ **Block F6**                  │
│ Route Grammar & URL Parameter Schema   │ **Block F7**                  │
│ Search UX, Autocomplete & Direct Links │ **Block F8**                  │
│ Entity Views Architecture (Character)  │ **Block F9 (This Document)**  │
│ Knowledge Graph Visualization Canvas   │ **Block F10**                 │
│ Family Lineage Tree DAG Engine         │ **Block F11**                 │
│ Timeline Scrubber & Chronology Lens    │ **Block F12**                 │
│ Geographic Map Engine & GIS Layers     │ **Block F13**                 │
│ War & Tactical Vyuha Visualization     │ **Block F14**                 │
│ Evidence Drawer Citation & Text UX     │ **Block F15**                 │
│ Accessibility Standards & Script Engine│ **Block F16**                 │
│ Performance Budgets & Testing Audits   │ **Block F17**                 │
│ Concrete Component Setup & Packages    │ **Stage 4 (Implementation)**  │
└────────────────────────────────────────┴───────────────────────────────┘
```

---

## 24. Architectural Decision Records (ADRs)

| Decision ID | Architectural Decision | Chosen Approach | Alternatives Evaluated | Rationale & Trade-offs | Owner | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **ADR-FE-49** | Entity View Role | **Analytical Node Anchor** | Static Wikipedia-style page | Entity profiles serve as the primary textual entry point into deep multi-lens graph exploration.| F9 | **DECIDED** |
| **ADR-FE-50** | Portrait Policy | **Authentic Media or Clean Omission** | Generic AI face generation | Strictly enforces Zero Data Fabrication; avoids misleading pictorial claims. | F9 | **DECIDED** |
| **ADR-FE-51** | Multi-Hop Graph Traversal | **Delegated to Specialized Lenses** | Embedded graph physics in profile | Keeps profile views lightweight ($\le 100\text{ KB}$); delegates complex rendering to F10/F11.| F9 | **DECIDED** |
| **ADR-FE-52** | Sub-View Deep-Linking | **URL-Representable Sub-Views** | Internal unlinked component state | Enables direct linking to specific entity sub-views where defined by Block F7.| F9 | **DECIDED** |
| **ADR-FE-53** | Missing Data Presentation | **Epistemic State Indicators** | Placeholder values (e.g., "N/A") | Communicates scholarly truthfulness by distinguishing unknown vs. unresearched data.| F9 | **DECIDED** |
| **ADR-FE-54** | Reusable Domain Pattern | **Common 5-Tier Hierarchy** | Bespoke architectures per domain | Guarantees visual and cognitive consistency across canonical entity domains.| F9 | **DECIDED** |

---

## 25. Requirement Traceability Matrix

| Requirement / Principle | Source Document | Implementing Block F9 Section | Coverage Status |
| :--- | :--- | :--- | :--- |
| **Full Character Profiles** | PRD §8, B5 §5.2 | §3 (Entity Model), §4 (Character Layout) | **SATISFIED** |
| **Zero Data Fabrication** | Rule 04, F1 §5.4 | §6 (Portrait Policy), §13 (Epistemic States) | **SATISFIED** |
| **Epistemic Honesty** | B2 §5, F3 §12 | §13 (Epistemic States Presentation) | **SATISFIED** |
| **One Graph, Many Lenses** | Rule 02, F1 §4.1 | §3 (Tier 3 Launchpad), §9–§12 (Lens Boundaries) | **SATISFIED** |
| **Stateless Deep-Linking** | F1 §5.8, F7 §6.2 | §16 (URL & Deep-Link State Integration) | **SATISFIED** |
| **Responsive Shell Layout** | F4 §3, §9 | §18 (Responsive Recomposition Matrix) | **SATISFIED** |
| **Tier 2 Payload Limit** | B9 §5 | §21 (Performance Architecture Boundaries) | **SATISFIED** |
| **Evidence Drawer Triggers**| B4 §3, F15 | §14 (Provenance & Evidence Boundaries) | **SATISFIED** |
| **A11Y Landmark Hierarchy** | F1 §5.10, F16 | §19 (Accessibility Architecture Boundaries) | **SATISFIED** |

---

## 26. F9 Exit Criteria Checklist

- [x] Canonical entity view model formalized across a common 5-tier information hierarchy.
- [x] Character detail view (`/characters/:slug`) architecture specified with progressive disclosure.
- [x] Reusable entity view architecture defined for other canonical domains (Location, Group, Event, War, Formation, Source).
- [x] Entity identity/header anatomy established with strict authentic portrait policy (zero fabrication).
- [x] Progressive disclosure hierarchy defined from glanceable identity to primary evidence.
- [x] Core information sections (Narrative, Kinship, Relationships, Military) clearly defined.
- [x] Clear boundaries established for handoffs to F10 (Graph), F11 (Lineage), F12 (Timeline), F13 (Map), and F14 (War/Vyuha).
- [x] Exact six B2 epistemic states mapped to non-color differentiated visual indicators.
- [x] Evidence trigger boundaries established with Block F15 Evidence Drawer.
- [x] Search-to-entity navigation continuity established from Block F8.
- [x] URL and deep-link integration strictly aligned with Block F7 route grammar.
- [x] Responsive recomposition defined across the four F4 viewport classes.
- [x] Accessibility contracts defined for structural landmarks, accessible facet navigation, and heading hierarchy.
- [x] Performance boundaries respect B9 Tier 2 payload limits ($\le 100\text{ KB}$) and defer budgets to F17.
- [x] Zero application source code, UI packages, or component files were introduced.
- [x] Zero Stage 1 backend documents or Blocks F1–F8 documents were modified.
