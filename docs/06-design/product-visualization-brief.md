# Product Visualization Brief — Mahābhārata Explorer

## Document Metadata
- **Document Identifier**: `docs/06-design/product-visualization-brief.md`
- **Stage**: Bridge between Stage 2 (Frontend Architecture F1–F17) and Stage 3 (Integration Architecture)
- **Role**: Design Exploration & Prototyping Specification for Stitch MCP
- **Authoritative Upstream Specifications**:
  - Stage 1: Backend Architecture Baseline (Blocks B1–B13, `docs/04-backend/`)
  - Stage 2: Frontend Architecture Baseline (Blocks F1–F17, `docs/05-frontend/`)
- **Status**: Authoritative Design Brief (Pure Design Bridge; Non-Implementation Artifact)

---

## 1. Purpose and Role of This Brief

### 1.1 Visualization Bridge Between Architecture and Prototyping
This document establishes the **Product Visualization Brief** for the **Mahābhārata Explorer**. It serves as an architectural-to-visual bridge, translating the authoritative architectural baselines from **Stage 1 (Backend Architecture B1–B13)** and **Stage 2 (Frontend Architecture F1–F17)** into a cohesive product-design specification for visual prototyping with **Stitch MCP**.

### 1.2 Preservation of Architecture — No Replacement or Modification
This brief is strictly derived from the established repository documentation. It:
- **Does NOT replace, supersede, or modify** any backend architectural decisions (B1–B13) or frontend architectural specifications (F1–F17).
- **Does NOT introduce new architecture**, new data schemas, alternative API shapes, or divergent routing contracts.
- **Does NOT commence Stage 3 (Integration Architecture)** or Stage 4 (Implementation).
- **Does NOT implement application code**, install NPM packages, select production runtime libraries, or configure build systems.

### 1.3 Design Exploration Artifact
This brief answers a singular, vital design question:
> *"What should the finished Mahābhārata Explorer actually look like, feel like, and behave like to a curious reader, student, or scholar?"*

It provides Stitch MCP with the exact visual language, layout paradigms, information hierarchies, interactive patterns, and epistemic guardrails required to generate high-fidelity prototypes that faithfully honor the project constitution (`AGENTS.md`).

---

## 2. Product Experience in One Paragraph

The **Mahābhārata Explorer** is a blazing-fast, culturally dignified, and intellectually rigorous digital exploration engine that unlocks the vast interconnected world of the ancient Indian epic through one unified knowledge graph experienced across ten analytical lenses. Rather than presenting the epic as a static, text-heavy encyclopedia or a mythologized fantasy spectacle, the interface feels like an editorial-grade scholarly atlas: clean slate and indigo surfaces framed by crisp micro-borders, warm saffron interactive accents, and flawless multilingual typography (IAST transliteration and Devanāgarī). Within milliseconds, a user exploring a hero like Arjuna or a complex figure like Karṇa can glide effortlessly between biographical summaries, multi-generational kinship trees, topological relationship networks, chronological Parva timelines, ancient cartographic landscapes, day-by-day battlefield combat logs, and geometric battle formations (vyūhas)—with claims, kinship links, and combat occurrences anchored to verifiable primary and secondary sources through a slide-out evidence drawer that presents textual uncertainties and competing recensions with radical epistemic honesty.

---

## 3. Core Product Mental Model: "One Graph, Many Lenses"

### 3.1 The Single Knowledge Graph
The foundational architecture of the Mahābhārata Explorer is defined by **Constitutional Principle 2 (REQ-CORE-01)**:
$$\text{ONE UNIFIED KNOWLEDGE GRAPH} \longleftrightarrow \text{MANY EXPLORATION LENSES}$$

In the user's mental model, the application is not a federation of separate databases or siloed sub-apps. There is only **one underlying universe**:
- The Explorer presents multiple canonical entity and relationship domains through one unified knowledge graph.
- A rich web of typed, directed, and claim-backed relationships (Kinship, Mentorship, Rivalry, Alliance, Combat, Participation, Geography).

### 3.2 Lenses as Analytical Viewing Windows
The ten canonical lenses are dynamic, perspective-shifting prisms focused on this singular graph:
```
                                 CANONICAL ENTITY
                     (e.g., Arjuna / Kurukṣetra War / Hastināpura)
                                         │
       ┌──────────────┬──────────────────┼──────────────────┬──────────────┐
       ▼              ▼                  ▼                  ▼              ▼
[Character View] [Lineage Tree]  [Timeline Event]   [Spatial Map]   [Focus Graph]
 (Biographical)   (Kinship DAG)   (Chronological)    (Geographic)    (Network D≤2)
       │              │                  │                  │              │
       └──────────────┴──────────────────┼──────────────────┴──────────────┘
                                         ▼
                         EVIDENCE & CITATION DRAWER (F15)
                  (Claims, Sanskrit Excerpts & Source References)
```

### 3.3 Seamless Cross-Lens Mobility
At any point in the user journey, the user is never trapped in a dead end:
- A user reading about **Arjuna** in the **Character Lens** sees interactive launchpads to view his position in the **Lineage Tree**, his 1-hop connections in the **Focus Graph**, his key milestones across the 18 Parvas in the **Timeline**, or the kingdoms he visited in the **Geography Lens**.
- Clicking into **Kurukṣetra War Day 14** in the **War Lens** highlights the active commanders, duels, and the deployment of a **Battlefield Vyūha**.
- Selecting the **Cakra-vyūha** in the **Vyūha Lens** displays its geometric tactical breakdown, who penetrated it, who defended it, and direct links to the relevant shlokas.
- Touching any relationship, lineage edge, or factual claim immediately summons the **Evidence Drawer**, surfacing the underlying proposition, Sanskrit excerpt (when supplied), English translation, and native citation locator without navigating away from the active screen.

---

## 4. Application Shell

Translating Block **F4 (Responsive Layout & Application Shell)** into visual design:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│ GLOBAL APPLICATION SHELL (DESKTOP / EXPANDED VIEWPORT)                                 │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ [Header Bar: Brand Logo ── Canonical Lens Nav ── Search (Ctrl+K) ── Theme Switcher]     │
├──────────────┬──────────────────────────────────────────┬──────────────────────────────┤
│ NAVIGATION   │ PRIMARY EXPLORATION WORKSPACE            │ CONTEXTUAL INSPECTOR /       │
│ SIDEBAR/RAIL │ (Active Lens: Character, Graph, Map,     │ EVIDENCE DRAWER (F15)        │
│              │  Timeline, Lineage, War, or Search)      │                              │
│ • Characters │                                          │ [Entity Header / Claim]      │
│ • Lineage    │ [Breadcrumb / Lens Switcher Context]     │                              │
│ • Graph      │                                          │ [Verbatim Native Locator]    │
│ • Timeline   │ [Interactive Canvas / Profile Grid /     │ [Sanskrit Shloka Box]        │
│ • Geography  │  Data-Dense Facets]                      │ [Scholarly Assessment]       │
│ • Factions   │                                          │                              │
│ • Wars       │                                          │ [Source Bibliographic Card]  │
│ • Vyūhas     │                                          │                              │
│ • Search     │                                          │ [Close / Dismiss ✕]          │
└──────────────┴──────────────────────────────────────────┴──────────────────────────────┘
```

### 4.1 Global Shell Hierarchy
1. **Header Bar (Persistent)**:
   - Left: Dignified brand emblem (`Mahābhārata Explorer`) with refined typography.
   - Center/Right: Prominent global search entry field displaying keyboard shortcut cues (`Ctrl+K` / `⌘K` or `/`).
   - Far Right: High-contrast Theme Toggle (Light/Dark mode) and subtle external documentation links.
2. **Primary Navigation (Structural Lens Navigation)**:
   - On **Expanded/Wide** screens: A persistent left navigation sidebar (or compact icon-plus-label rail) granting instant one-click switching between the canonical exploration lenses.
   - On **Medium** screens: Collapses into an ergonomic icon-only navigation rail to preserve canvas width.
   - On **Compact** screens: Transforms into a clean, thumb-accessible bottom navigation bar displaying primary discovery hubs, with secondary lenses accessible via a lightweight slide-up sheet.
3. **Contextual Navigation (In-Lens Sub-Navigation)**:
   - Dedicated local controls positioned directly above the workspace: e.g., the 18-day Kurukṣetra War horizontal stepper, Parva timeline segment scrubbers, character facet section tabs (Overview, Episodes, Kinship, Relations, Military), or graph layout/depth filters ($D=1, D=2$).
4. **Entity Deep Navigation**:
   - Entity references throughout the application (characters, places, factions, formations) render as interactive semantic cards, chips, or graph nodes. Clicking any entity triggers an immediate transition to its canonical route (`/characters/:slug`, `/geography/:slug`, etc.) as defined by Block F7.
5. **Transient Surfaces**:
   - Modals, popovers, tooltip hover cards, and search dropdowns float above the workspace with high-contrast borders and low-blur directional elevation (`elevation-2` and `elevation-3`).
   - Ephemeral UI states (hover, zoom level, pan coordinates) never pollute the canonical browser URL.
6. **Responsive Recomposition (Desktop vs. Compact)**:
   - **Expanded / Wide Layouts**: Side-by-side multi-pane studio. The primary visualization or biographical facet shares the screen with a persistent or slide-over contextual inspection side-panel.
   - **Compact Layouts**: Pure single-column vertical stacked flow. Complex tables adapt into stacked key-value cards; multi-pane inspectors slide up from the bottom as ergonomic swipeable bottom sheets; the search bar expands into a focused, distraction-free full-screen overlay.
   - *Strict Invariant*: In accordance with Block F4, responsive behavior is governed by behavioral layout transitions across the four space-based viewport classes without inventing fixed pixel breakpoints.

---

## 5. Visual Language

Translating Block **F3 (Design System & Visual Grammar)** into aesthetic direction:

### 5.1 "Modern First. Traditional Second" (Principle 11 & Rule 11)
The visual tone is **uncompromisingly contemporary, clean, and restrained**. It evokes the aesthetic of high-end scholarly platforms, modern data atlases, and premier digital journalism (e.g., modern digital humanities archives, Financial Times visual essays, Nature data visualizations):
- **Culturally Resonant, Not Decorative**: Subtle warmth and Sanātana resonance are conveyed through typography, authentic Sanskrit text pairing, and restrained saffron accents.
- **Strict Prohibition of Kitsch**:
  - ❌ **NO faux-parchment** or simulated aged paper textures.
  - ❌ **NO burnt-edge**, torn-paper, or papyrus skeuomorphism.
  - ❌ **NO heavy gold gradients**, muddy yellow-brown color washes, or simulated metallic plating.
  - ❌ **NO temple-style arched framing**, pillar borders, or religious iconographic buttons.
  - ❌ **NO fantasy-game aesthetics** (no glowing magical runes, video-game HUD elements, or comic-book typography).
  - ❌ **NO decorative Sanskrit wallpaper** used as meaningless background fill.

### 5.2 Typography System
Scholarly precision demands impeccable multilingual type hierarchy:
- **UI & Body Text**: A crisp, humanist sans-serif with complete Unicode Latin-Extended support, ensuring flawless rendering of all IAST diacritical marks (`ā, ī, ū, ṛ, ṝ, ḷ, ṅ, ñ, ṭ, ḍ, ṇ, ś, ṣ, ḥ, ṃ`).
- **Display Headings (H1/H2)**: A refined, high-editorial display serif (or crisp contemporary display sans) providing authoritative dignity to major canonical entity names and lens headers.
- **Devanāgarī Script**: Rendered in a high-clarity Unicode Devanāgarī typeface, set with optical balance alongside its IAST transliteration (e.g., **Arjuna** • <span lang="sa-Deva">अर्जुन</span>).
- **Tabular Monospace**: A clean, proportional monospace font dedicated to native citation locators (e.g., `1.57.1–12`), temporal sequence indices (`sequence_index: 1042`), and graph metric counters.

### 5.3 Spacing, Borders, and Surfaces
- **Systematic Base-8 Grid**: Layout padding and component gaps conceptually follow a standard scale ($4\text{px}, 8\text{px}, 12\text{px}, 16\text{px}, 24\text{px}, 32\text{px}, 48\text{px}$), maintaining visual rhythm across data-dense views.
- **Flat Surface Architecture**:
  - `surface-canvas`: Deep, calming backdrop behind visualization graphs and maps.
  - `surface-base`: Clean application background for editorial and reading views.
  - `surface-raised`: Individual content cards, facet containers, and data panels.
  - `surface-overlay`: High-priority dialogs, search overlays, and the evidence drawer.
  - `surface-sunken`: Inset quotation wells for Sanskrit shlokas, translations, and raw metadata.
- **Subtle 1px Borders**: Depth is established primarily through crisp $1\text{px}$ borders (`border-subtle`, `border-muted`) rather than heavy dropshadows or 3D bevels.
- **Border Radii**: Restrained radii ($4\text{px}$ for badges and inputs; $8\text{px}$ for cards; $12\text{px}$ for floating drawers; full pill for active tags).

### 5.4 Semantic Color Roles & Restraint
*(Note: Color values and roles presented in this brief represent visual exploration guidance for Stitch prototyping; formal semantic design tokens are authoritatively governed by Block F3.)*
- **Brand Primary**: Deep Slate Navy / Rich Indigo structural tones representing scholarly gravitas and structure.
- **Brand Accent**: Warm Saffron / Ochre tones used sparingly for active lens indicators, focus halos, interactive tabs, and selection states.
- **Non-Color Redundancy**: Color is **never** the sole carrier of meaning. Every color-coded element must be reinforced by an icon glyph, distinctive border pattern (solid, dashed, dotted), or explicit text label.

### 5.5 Dual Light and Dark Theming
- **Light Mode (Default)**: High-contrast, airy editorial environment with crisp neutral surfaces, optimized for sustained scholarly reading and daytime study.
- **Dark Mode**: Low-luminance, rich slate/charcoal environment optimized for deep night-time graph exploration, preserving exact epistemic contrast and relationship hue boundaries without eye strain.

---

## 6. Canonical Exploration Lenses

The Mahābhārata Explorer organizes all user discovery into **exactly ten canonical lenses** (established in Blocks F1, F7, and Stage 1). Stitch must represent these ten lenses without inventing new ones:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                          THE TEN CANONICAL EXPLORATION LENSES                          │
├────┬────────────────────────┬──────────────────────────┬───────────────────────────────┤
│ #  │ Canonical Lens Name    │ Primary Canonical Route  │ Primary Analytical Focus      │
├────┼────────────────────────┼──────────────────────────┼───────────────────────────────┤
│ 1  │ Character              │ `/characters/:slug`      │ Biographical core & attributes│
│ 2  │ Family Lineage         │ `/lineage/:slug`         │ Multi-generational kinship DAG│
│ 3  │ General Relationships  │ `/relationships/:slug`   │ Typed non-familial network    │
│ 4  │ Timeline & Chronology  │ `/timeline/:slug`        │ Parva sequence & event LOD    │
│ 5  │ Geography              │ `/geography/:slug`       │ Ancient Indian epic landscape │
│ 6  │ Dynasties & Factions   │ `/factions/:slug`        │ Clans, royal houses & sides   │
│ 7  │ War                    │ `/wars/:war_slug/day/:day`│ Day-by-day battlefield combat │
│ 8  │ Battlefield Vyūha      │ `/vyuhas/:slug`          │ Tactical military formations  │
│ 9  │ Global Search          │ `/search` (+ overlay)    │ Instant multi-domain discovery│
│ 10 │ Global Focus Graph     │ `/graph/:slug`           │ Bounded ego network ($D \le 2$)│
└────┴────────────────────────┴──────────────────────────┴───────────────────────────────┘
```

### 6.1 Character Lens (`/characters`, `/characters/:slug`)
- **User Intent**: Understand an individual's identity, epithets, birth circumstances, narrative deeds, moral choices, allegiances, and primary sources.
- **Visual Structure**: Multi-column editorial profile on desktop; single-column stacked stream on mobile.
- **Information Hierarchy**: Five-tier model: (1) Identity/Header → (2) Biographical Overview & Metadata → (3) Cross-Lens Launchpad → (4) Facet Panels (Narrative, Kinship, Relations, Military) → (5) Provenance & Attribution Footer.
- **Navigational Departures**: Instant one-click launch into Lineage, Focus Graph, Timeline, Map, or War Day actions.
- **Interconnection**: Serves as the central textual and identity anchor of the entire knowledge graph.

### 6.2 Family Lineage Lens (`/lineage`, `/lineage/:slug`)
- **User Intent**: Trace complex ancestry, descent, multiple wives, divine parentage, adoption, and generational relationships across epic dynasties (Kuru, Yādava, Pañcāla).
- **Visual Structure**: Hierarchical generational Directed Acyclic Graph (DAG) with discrete horizontal generation bands, vertical descent lines, and distinct spousal connectors.
- **Information Hierarchy**: Generation tiers ordered chronologically top-to-bottom; focal character highlighted with accent halo; collapsible sub-branches to prevent cognitive overload.
- **Navigational Departures**: Clicking any ancestor, descendant, or spouse navigates directly to their Character Profile or reframes the Lineage DAG around them.
- **Interconnection**: Feeds directly into Dynasties/Factions and Character Kinship facets.

### 6.3 General Relationships Lens (`/relationships`, `/relationships/:slug`)
- **User Intent**: Explore non-biological connections: alliances, bitter rivalries, teacher-disciple mentorships, curses, boons, oaths, and diplomatic pacts.
- **Visual Structure**: Categorized relationship cards and matrix groupings arranged by semantic bond type (`mentorship`, `rivalry`, `alliance`, `devotion`, `hostility`).
- **Information Hierarchy**: Focal entity header → Category grouping headers → Individual relationship cards showing counterpart entity, bond description, epistemic status, and backing citation trigger.
- **Navigational Departures**: Selecting a counterpart entity navigates to their profile or relationship network; clicking the evidence trigger opens the backing claim.
- **Interconnection**: Directly informs the Focus Graph and Character military/narrative contexts.

### 6.4 Timeline & Chronology Lens (`/timeline`, `/timeline/:slug`)
- **User Intent**: Discover *when* events happened in epic sequence, from early ancestral origins through the dice game, exile, war, and aftermath across the 18 Parvas.
- **Visual Structure**: Horizontal sequence scrubber paired with a vertical narrative event stream; segmented by Parvas (Books 1–18).
- **Information Hierarchy**: Parva marker ribbon → Sequential event cards ordered by backend `sequence_index` → Milestone badges indicating pivotal epic turning points → Participant entity chips.
- **Navigational Departures**: Clicking an event participant opens their Character profile; clicking an event location opens Geography; clicking temporal evidence reveals textual basis.
- **Interconnection**: Connects narrative progression with War Days and Character life episodes.

### 6.5 Geography Lens (`/geography`, `/geography/:slug`)
- **User Intent**: Discover ancient Indian kingdoms, sacred rivers, pilgrimage routes (Tīrthas), battlefields, capital cities (Hastināpura, Indraprastha), and exile forests (Kāmyaka, Dvaitavana).
- **Visual Structure**: Interactive cartographic viewport of the Indian subcontinent paired with a collapsible location directory sidebar.
- **Information Hierarchy**: Mapped sites with canonical location markers → Regional kingdom boundaries → Explicit "Unmapped Historical Sites" drawer for locations with textual descriptions but no verifiable modern coordinates (`coordinate_status = unmapped`).
- **Navigational Departures**: Selecting a location surfaces associated characters, dynasties, and events; clicking a location node opens the full Location profile.
- **Interconnection**: Contextualizes exile journeys, military campaigns, and kingdom allegiances.

### 6.6 Dynasties & Factions Lens (`/factions`, `/factions/:slug`)
- **User Intent**: Understand the geopolitical structure of the epic: the Lunar Dynasty (Candravamśa), Solar Dynasty (Sūryavamśa), the Pāṇḍava coalition, the Kaurava alliance, and neutral kingdoms (e.g., Vidarbha, Bhoja).
- **Visual Structure**: Faction identity cards, kingdom allegiance rosters, and comparative alliance matrices.
- **Information Hierarchy**: Faction banner/emblem → Primary leadership & royal lineage → Member character roster with loyalty roles → War alignment indicator (Pāṇḍava / Kaurava / Neutral).
- **Navigational Departures**: Jumps into character profiles, kingdom geographic locations, or Kurukṣetra War faction battle orders.
- **Interconnection**: Bridges high-level dynastic politics with granular war day allegiances.

### 6.7 War Lens (`/wars`, `/wars/:war_slug`, `/wars/:war_slug/day/:day`)
- **User Intent**: Inspect the canonical subdivision of the Kurukṣetra War: commanders and key participants where supplied, tactical summaries where documented, combat duels, fallen heroes, and decisive conflict sequences.
- **Visual Structure**: Multi-day chronological stepper bar across the top; central daily combat and narrative sequence; dual-flank faction panel (e.g., Pāṇḍava alliance vs. Kaurava alliance).
- **Information Hierarchy**: Day selector (Day 1 through Day 18) → Day title and marshals/commanders (where supplied) → Narrative and Tactical Summary → Major Duels Table → Fallen Participants Roll with epistemic status badges.
- **Navigational Departures**: Clicking fallen participants or duelists links to Character profiles; clicking deployed formations links to the Battlefield Vyūha Lens.
- **Interconnection**: Links epic chronology, character combat records, and tactical formation contexts without simulating unrecorded troop numbers or movements.

### 6.8 Battlefield Vyūha Lens (`/vyuhas`, `/vyuhas/:slug`)
- **User Intent**: Inspect ancient military tactical formations deployed during the Kurukṣetra War (e.g., Cakra-vyūha, Krauñca-vyūha, Garuḍa-vyūha, Padma-vyūha, Maṇḍala-vyūha) where canonical structural or textual data is supplied.
- **Visual Structure**: Authoritative vector (SVG) schematic where geometric data is explicitly supplied in the canonical model, paired with a structured tactical breakdown card and an accessible text table alternative. Where geometric data is absent, falls back to structured descriptive presentation without fabricating troop positions.
- **Information Hierarchy**: Formation Name (IAST + Devanāgarī) → Textual Description & Strategic Role → Vector Schematic or Structural Breakdown (where supplied) displaying key participant assignments → Deployed War Days → Recorded Tactical Encounters.
- **Navigational Departures**: Clicking participant nodes on the diagram navigates to Character profiles; clicking deployment days navigates to the War Day view.
- **Interconnection**: Provides structural martial context to the War Lens and military facets of Character profiles.

### 6.9 Global Search Lens (`/search`, header overlay)
- **User Intent**: Instantly find any character, event, place, dynasty, formation, or scholarly source across the entire epic corpus using English, IAST diacritics, or Devanāgarī.
- **Visual Structure**: Dual-tier interface: rapid keyboard-driven modal overlay from the header (`Ctrl+K`), expanding into a dedicated full-page filterable search hub (`/search?q=...`).
- **Information Hierarchy**: Autocomplete quick-matches grouped by domain → Instant entity summary preview → Filter facet bar (Entity Type, Epistemic Status, Parva) → Paginated results collection.
- **Navigational Departures**: Selecting an autocomplete suggestion navigates directly to that canonical entity; pressing Enter navigates to the `/search` collection view.
- **Interconnection**: The universal rapid-entry gateway into every node in the knowledge graph.

### 6.10 Global Focus Graph Lens (`/graph`, `/graph/:slug`)
- **User Intent**: Interactively explore the topology of the epic's network, discovering unexpected connections, intermediaries, and community clusters centered around a focal figure.
- **Visual Structure**: Interactive network canvas constrained to bounded depth ($D \le 2$), flanked by node inspection cards and edge-type filter controls.
- **Information Hierarchy**: Focal Node ($D_0$, 100% opacity, accent halo) → Immediate Neighbors ($D_1$, primary connections) → Second-degree horizon ($D_2$, $70\%$ opacity) → Directed relationship edges with semantic type badges.
- **Navigational Departures**: Clicking any node opens a preview card with a button to "Make Focal Node" (reframes graph to `/graph/:new_slug`) or "View Full Profile" (`/characters/:new_slug`).
- **Interconnection**: Visualizes the pure topological reality of the unified knowledge graph.

---

## 7. Flagship Experience — Character Detail (`/characters/:slug`)

Translating Block **F9 (Character & Entity Views Architecture)** into the user-facing centerpiece:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│ CHARACTER DETAIL VIEW — FIVE-TIER INFORMATION HIERARCHY (F9)                           │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ TIER 1: IDENTITY & HEADER LANDMARK                                                     │
│ [Portrait / Monogram]  [Canonical Name] • [Native Script]                              │
│                        Domain: Character  │  Epistemic State: [● Known]                │
│                        Aliases: [Illustrative Epithets / Aliases]                      │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ TIER 2: PRIMARY BIOGRAPHICAL & DESCRIPTIVE OVERVIEW                                    │
│ [Scholarly narrative synthesis curated with neutral tone...]                           │
│ Quick Facts: [Paternal Kinship] │ [Maternal Kinship] │ [Faction / Group]               │
│ [ ↗ [Count] Source References in Evidence Drawer ]                                     │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ TIER 3: CROSS-LENS EXPLORATION LAUNCHPAD                                               │
│ [ 🕸 Explore Focus Graph (D≤2) ] [ 🌳 View Family Lineage ] [ ⏳ Chronology in 18 Parvas ]│
│ [ 🗺 View Geography on Map ]    [ ⚔ War Combat Records ]                               │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ TIER 4: STRUCTURED FACET PANELS                                                        │
│ [ Responsive Facets: Narrative Episodes │ Kinship DAG │ Relationships │ War Actions ]  │
│                                                                                        │
│ • Active Facet: Narrative Episodes (Parva-ordered event milestones)                    │
│   - [Parva Name]: [Event Name] [Citation Locator]                                      │
│   - [Parva Name]: [Event Name] [Citation Locator]                                      │
│                                                                                        │
│ • Active Facet: Military & War Context                                                 │
│   - Faction: [Supplied Faction / Combat Role]                                          │
│   - Major Duels: [Supplied Combat Encounters / Opponents]                              │
│   - Vyūha Participation: [Supplied Formation Actions] [Evidence ↗]                     │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ TIER 5: PROVENANCE & ATTRIBUTION FOOTER                                                │
│ Bibliographic Sources: [Bibliographic Source Attribution]                              │
│ Curation Metadata: [Audited Claims Count] • Epistemic Classification: Known            │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

### 7.1 Tier 1: Identity & Header Landmark
- **Visual Placement**: Top of profile, commanding immediate recognition.
- **Elements**: Canonical name in bold display type with accurate IAST diacritics; authentic Devanāgarī script beside it; domain classification badge (`Character`); canonical epistemic indicator badge (`known`, `conflicting`, etc.); list of canonical aliases and epithets (*Pārtha*, *Dhanañjaya*, *Kirīṭin*).
- **Curated Portrait vs. Monogram**: If an authentic, curated classical miniature or manuscript painting exists in the database, it renders cleanly in an aspect-ratio container with attribution. If no authentic portrait exists, the UI renders an **elegant semantic monogram badge** (styled initial in subtle saffron/slate). **Zero generic silhouettes, placeholder question marks, or AI-hallucinated portraits are permitted.**

### 7.2 Tier 2: Primary Overview & Core Metadata Grid
- **Narrative Lead**: A concise, rigorous scholarly summary synthesizing the character's role in the epic, drafted with complete neutrality.
- **Core Metadata Grid**: High-density property-value grid displaying parental lineage, maternal line, spouse(s), primary dynasty, and Parva appearance range where supplied.
- **Global Evidence Trigger**: A prominent interactive affordance (e.g., `[ ↗ [Count] Source References ]`) allowing the user to immediately inspect the backing evidence for all Tier 1 & 2 assertions in the F15 Evidence Drawer.

### 7.3 Tier 3: Cross-Lens Exploration Launchpad
- **Visual Design**: A horizontal row of distinctive, tactile action pills positioned between the overview and deep facet panels.
- **Interactive Role**: Invites the user to transition from passive reading into active multidimensional exploration:
  - `[ Explore Focus Graph ]`: Opens `/graph/:slug`, centering the topological network on this character.
  - `[ View Family Lineage ]`: Opens `/lineage/:slug`, tracing ancestral and descendant branches.
  - `[ Chronological Timeline ]`: Opens `/timeline/:slug`, showing every event milestone featuring this character.
  - `[ Geographic Travels ]`: Opens `/geography/:slug`, mapping the sites and kingdoms visited.
  - `[ Kurukṣetra War Record ]`: Opens `/wars/kurukshetra` filtered to this character's combats.

### 7.4 Tier 4: Structured Facet Panels
An organized facet presentation (tabs, stacked sections, or collapsible panels per F9 responsive layout) allowing in-depth inspection without cognitive clutter:
1. **Narrative Episodes**: Chronological listing of major life events, organized by Parva, showing the character's active deeds and narrative milestones.
2. **Family & Kinship DAG**: High-clarity mini-tree showing immediate parents, siblings, co-wives, and offspring where recorded.
3. **Inter-Entity Relationships**: Filterable list of 1-hop relationships (alliances, bitter rivalries, mentorships), each displaying a direct citation badge.
4. **Military & War Context**: Martial descriptions, divine weapons (*astras* where recorded), combat duels, and participation across war days as supplied by the data model.

### 7.5 Tier 5: Provenance & Attribution Footer
- Low-contrast, high-rigor baseline grounding the entire view in scholarly sources. References the primary bibliographic source records (such as critical or traditional editions) and indicates curation audit status.

---

## 8. Visualization Experiences

Translating Blocks **F10, F11, F12, F13, F14** into distinct interactive visual experiences:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        INTERACTIVE VISUALIZATION MODALITIES                            │
├──────────────────┬──────────────────┬──────────────────────────┬───────────────────────┤
│ Visualization    │ Primary Visual   │ Key Interactive Feel     │ Retained Context &    │
│ Experience       │ Projection       │                          │ Canonical Return      │
├──────────────────┼──────────────────┼──────────────────────────┼───────────────────────┤
│ **Relationship   │ Bounded force-   │ Tactile node dragging,   │ Inspector sidebar with│
│ Graph (F10)**    │ directed network │ smooth focus transition, │ node quick-facts;     │
│                  │ ($D \le 2$)      │ non-jittery equilibrium │ "View Profile" button │
├──────────────────┼──────────────────┼──────────────────────────┼───────────────────────┤
│ **Family Lineage │ Hierarchical     │ Expand/collapse branches,│ Generational band     │
│ Tree (F11)**     │ multi-gen DAG    │ horizontal pan, spousal  │ labels; instant jump  │
│                  │ (top-to-bottom)  │ connector highlighting   │ to any member profile │
├──────────────────┼──────────────────┼──────────────────────────┼───────────────────────┤
│ **Timeline &     │ Segmented Parva  │ Smooth horizontal scrub, │ Active event card     │
│ Chronology (F12)│ track + cards    │ Level-of-Detail (LOD)    │ panel; links to all   │
│                  │                  │ milestone zoom           │ participating heroes  │
├──────────────────┼──────────────────┼──────────────────────────┼───────────────────────┤
│ **Geographic     │ Subcontinent GIS │ Smooth pan/zoom, cluster │ Sidebar of regional   │
│ Map (F13)**      │ cartography      │ expansion, unmapped      │ sites; links to events│
│                  │                  │ site drawer toggle       │ and kingdoms          │
├──────────────────┼──────────────────┼──────────────────────────┼───────────────────────┤
│ **War & War Day  │ Multi-day stepper│ Day-by-day progression,  │ Faction duel matrix;  │
│ Analysis (F14)** │ + combat dual-   │ duel outcome filtering,  │ fallen heroes list;   │
│                  │ pane             │ commander comparison     │ links to Vyūhas       │
├──────────────────┼──────────────────┼──────────────────────────┼───────────────────────┤
│ **Battlefield    │ Vector geometric │ Interactive structural   │ Position/role key;    │
│ Vyūha (F14)**    │ schematic (SVG)  │ position pins, participant│ battle day links;     │
│                  │ or text fallback │ role hover disclosures   │ accessible text table │
└──────────────────┴──────────────────┴──────────────────────────┴───────────────────────┘
```

### 8.1 Relationship Graph Lens (F10)
- **What It Communicates**: Topological proximity, clustering of alliances, cross-faction ties, and key intermediaries across the epic network.
- **Interaction Feel**: Calm, stable, and tactile. Nodes settle deterministically without erratic physics oscillations. Clicking a node brings up an inspector panel; double-clicking or selecting "Refocus" smoothly recenters the graph around that new node ($D_0$).
- **Retained Context**: An inspector sidebar remains visible, displaying the active node's portrait/monogram, domain, and 1-hop degree.
- **Return to Entity**: A prominent primary button (*"Open Full Profile"*) navigates directly to the canonical entity URL.

### 8.2 Family Lineage Tree Lens (F11)
- **What It Communicates**: Multi-generational dynastic inheritance, divine origins, complex co-parentage, and inter-clan marriages without confusing line crossings.
- **Interaction Feel**: Clean, structured architectural drafting aesthetic. Users pan horizontally across generations and vertically across lineages. Branches can be collapsed or expanded to isolate specific houses (e.g., isolating the Pāṇḍavas from the wider Kuru dynasty).
- **Retained Context**: Generational headers (Generation 0: Ancestors; Generation +1: Pāṇḍu/Dhṛtarāṣṭra; Generation +2: Pāṇḍavas/Kauravas; Generation +3: Abhimanyu/Upapāṇḍavas; Generation +4: Parikṣit) anchor the vertical axis.
- **Return to Entity**: Clicking any node opens a quick card with a direct link to the full Character Profile.

### 8.3 Timeline & Chronology Lens (F12)
- **What It Communicates**: The relentless forward momentum of epic time, organized strictly by textual sequence (`sequence_index`) across the 18 Parvas.
- **Interaction Feel**: An expansive horizontal scrubber ribbon at the top displays the 18 Parva blocks (with proportional visual weights). Scrubbing the timeline smoothly updates the vertical card stack below. Level-of-Detail (LOD) controls allow zooming between macro-milestones (Dice Game, Exile, War) and micro-events.
- **Retained Context**: The current Parva and chronological sequence index are persistently displayed.
- **Return to Entity**: Event cards feature interactive chips for all participating characters, places, and participating groups.

### 8.4 Geographic Map Lens (F13)
- **What It Communicates**: The epic's physical theater—from Gandhāra (modern Afghanistan) in the northwest to Prāgjyotiṣa (Assam) in the east, and down to the southern kingdoms.
- **Interaction Feel**: Fluid cartographic exploration. Modern geographical reference contours are rendered in subtle, low-contrast slate tones, allowing ancient kingdoms, rivers (Gaṅgā, Yamunā, Sarasvatī), and capitals to stand out crisply.
- **Unmapped Sites Treatment**: A prominent visual drawer highlights *Unmapped Epic Sites* (locations mentioned in the text whose geographical coordinates cannot be verified without academic fabrication), complete with their textual descriptions and epistemic tags.
- **Return to Entity**: Selecting a mapped pin opens an entity card with direct links to the Place profile and related events.

### 8.5 War & War-Day Analysis Lens (F14)
- **What It Communicates**: The chronological, day-by-day escalation of the 18-day Kurukṣetra War based on canonical combat records.
- **Interaction Feel**: A structured historical combat explorer. An 18-day horizontal stepper allows navigation between combat days. The view presents faction columns (e.g., Pāṇḍava forces, Kaurava forces) displaying active commanders, supplied tactical summaries, and combat encounters.
- **Retained Context**: Canonical day index and documented participants frame the header.
- **Return to Entity**: Fallen heroes and dueling participants link directly to their respective Character profiles.

### 8.6 Battlefield Vyūha Lens (F14)
- **What It Communicates**: The structural, tactical, and textual deployment of ancient Indian battle arrays where authoritative data is recorded.
- **Interaction Feel**: Clean, technical vector drafting experience. Where authoritative geometric data is supplied, the formation (e.g., Cakra-vyūha) renders as a geometric SVG schematic showing concentric structural tiers and marked warrior positions. Where geometric data is absent, the lens falls back to structured textual/semantic breakdowns.
- **Retained Context**: Textual description, commanding marshal (where recorded), and day of deployment remain anchored beside the diagram.
- **Return to Entity**: Warrior pins on the schematic link directly to Character views; the deployed day badge links back to the War Day view.

---

## 9. Search Experience

Translating Block **F8 (Global Search & Autocomplete UX Architecture)** into product design:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│ GLOBAL SEARCH OVERLAY & AUTOCOMPLETE MODAL (Ctrl+K / ⌘K)                               │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ 🔍 [ Search characters, events, places, formations... (e.g., "Arjuna" / "अर्जुन") ] ✕ │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ SUGGESTIONS                                                                            │
│ ┌────────────────────────────────────────────────────────────────────────────────────┐ │
│ │ Arjuna • अर्जुन  [Character]                                                       │ │
│ │ Son of Pāṇḍu & Kuntī • Hero of Kurukṣetra • Pāṇḍava Coalition                      │ │
│ │ Status: [● Known]  │   Source References: [Count] citations ↗                       │ │
│ └────────────────────────────────────────────────────────────────────────────────────┘ │
│ ┌────────────────────────────────────────────────────────────────────────────────────┐ │
│ │ Cakra-vyūha • चक्रव्यूह  [Formation]                                                │ │
│ │ Multi-tier circular tactical battle array deployed on Day 13 by Droṇa              │ │
│ └────────────────────────────────────────────────────────────────────────────────────┘ │
│ ┌────────────────────────────────────────────────────────────────────────────────────┐ │
│ │ Kurukṣetra • कुरुक्षेत्र  [Location]                                                │ │
│ │ Sacred plain in Kuru realm • Epic battlefield of 18-day war                        │ │
│ └────────────────────────────────────────────────────────────────────────────────────┘ │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ [ ↑↓ Navigate ]   [ ↵ Select & Jump ]   [ ESC Dismiss ]   [ ↵ View All 14 Results ]    │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

### 9.1 Global Search Entry & Ergonomics
- **Header Activation**: Persistent search input in the top desktop header with clear visual shortcut cues (`Ctrl+K` / `⌘K` or `/`). On mobile, an omnipresent search icon launches a full-screen search view.
- **Dual-Tier Model**:
  1. *Instant Autocomplete*: A focused, transient overlay appearing immediately on keystroke, delivering high-probability matches within milliseconds.
  2. *Dedicated Search Page (`/search?q=...`)*: For comprehensive scholarly exploration, filterable by entity domain, epistemic status, and Parva.

### 9.2 Search Intelligence & Script Resilience
- **Multilingual Query Acceptance**: Seamlessly handles standard Latin spelling ("Arjuna", "Karna"), IAST diacritics ("Arjuna", "Karṇa"), and native Devanāgarī ("अर्जुन", "कर्ण").
- **Phonetic & Transliteration Matching**: Robust fuzzy and transliteration tolerance ensures users find entities regardless of diacritic precision.

### 9.3 Autocomplete Information Architecture
Each autocomplete suggestion item is an informative mini-card, containing:
- Canonical Name (IAST) + Native Script (Devanāgarī).
- Domain Type Badge (`Character`, `Location`, `Event`, `War`, `Formation`, `Source`).
- One-line scholarly contextual summary.
- Epistemic status indicator glyph.

### 9.4 Fast Entity Jump
Selecting an autocomplete result navigates directly to that entity's canonical URL (`/characters/arjuna`), bypassing intermediate search result lists.

### 9.5 Empty and Error States
- If no results are found, the search interface displays a dignified, helpful message (*"No records found for '{query}'"*) accompanied by search suggestions, alternative transliteration hints, and zero mock or fabricated entities.

---

## 10. Evidence and Provenance Experience

Translating Block **F15 (Evidence & Provenance UI Architecture)** into user experience:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│ EVIDENCE & CITATION DRAWER (SLIDE-OUT FROM RIGHT / BOTTOM SHEET ON MOBILE)             │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ [ HEADER: Evidence for Proposition ]                                              [ ✕ ]│
│ Assertion: [Proposition Statement, e.g., "Arjuna received the Gāṇḍīva bow..."]         │
│ Subject: [Subject Entity]  │  Domain: [Entity Domain]  │  Certainty: [Claim Certainty] │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ CANONICAL FOUR-TIER PROVENANCE TRACE (B4 / F15)                                        │
│                                                                                        │
│ 1. CLAIM                                                                               │
│    Proposition: [Factual claim statement]                                              │
│    Epistemic Status: [● Known / Conflicting / Approximate / Unknown]                   │
│    Certainty: [Claim Certainty: established / traditional_consensus / disputed]        │
│                                                                                        │
│ 2. EVIDENCE & VERBATIM CITATION LOCATOR                                                │
│    Native Locator: [Parva and verse reference, e.g., Ādi Parva 1.216.5–15]             │
│                                                                                        │
│    Sanskrit Excerpt (when supplied):                                                   │
│    ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│    │ [Original Sanskrit shloka text, if supplied]                                    │ │
│    └─────────────────────────────────────────────────────────────────────────────────┘ │
│    IAST Transliteration:                                                               │
│    [Transliterated verse excerpt, if supplied]                                         │
│                                                                                        │
│    Editorial Translation:                                                              │
│    "[Scholarly translation of the excerpt, if supplied]"                               │
│                                                                                        │
│    Assessment & Contextual Commentary:                                                 │
│    [Attestation notes, manuscript context, or recension variants]                      │
│                                                                                        │
│ 3. SOURCE BIBLIOGRAPHIC RECORD                                                         │
│    Title: [Bibliographic Source Title]                                                 │
│    Editor/Publisher: [Scholarly editor or publishing body]                             │
│    Identifier: [Source ID / Edition / Volume / Parva]                                  │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

### 10.1 The Four-Tier Provenance Chain
In strict accordance with Stage 1 Block B4 and Stage 2 Block F15, provenance is never flattened into a simple web link or generic footnote. The user experiences the true scholarly chain:
$$\text{Entity / Graph Edge} \longrightarrow \text{Claim} \longrightarrow \text{Evidence} \longrightarrow \text{Source}$$

### 10.2 The Slide-Out Evidence Drawer
- **Activation**: Clicking any citation badge, shloka locator tag, or epistemic badge across any lens smoothly opens the Evidence Drawer from the right side of the screen (or as an ergonomic bottom sheet on mobile).
- **Preserved Context**: The main workspace remains fully visible behind the drawer, ensuring the user maintains their spatial orientation in the graph or narrative.

### 10.3 Native Citation Locators
Verbatim manuscript and edition locators (e.g., `Ādi Parva 1.216.5–12`, `[Source ID] Vol. 1, p. 892`) are displayed prominently in tabular monospace type without alteration.

### 10.4 Non-Hierarchical Presentation of Competing Claims
A critical constitutional mandate: **When source traditions disagree, the Explorer never silently picks a winner.**
- Competing claims (e.g., divergent source traditions or variant accounts, or varying parentage traditions) are displayed side-by-side in **Split-Tone Multi-Claim Containers**.
- Neither claim is given visual seniority, larger typography, or a "verified" badge over the other. Both are presented with equal visual weight, clearly attributing Tradition A to Source A and Tradition B to Source B.

### 10.5 Strict Architectural Distinction: Epistemic Status vs. Certainty vs. Provenance
The UI maintains cognitive separation between three related but distinct concepts:
1. **`epistemic_status`**: What kind of knowledge state is this? (One of the six project-wide states: `known`, `conflicting`, `approximate`, `unknown`, `not_researched`, `not_applicable`).
2. **`certainty`**: Canonical certainty level of this specific claim (one of the five canonical values: `established`, `traditional_consensus`, `disputed`, `approximate`, `unresolved`).
3. **`provenance`**: What is the historical source trail supporting this claim? (The four-tier citation chain).

---

## 11. Epistemic Visual Language

Translating the **Six Canonical Epistemic States** (from Stage 1 Block B2 and Stage 2 Block F3) into interface elements:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                      THE SIX CANONICAL EPISTEMIC STATES                                │
├──────────────────┬──────────────────┬─────────────────┬────────────────────────────────┤
│ Epistemic State  │ Visual Treatment │ Border & Icon   │ Semantic Scholarly Meaning     │
├──────────────────┼──────────────────┼─────────────────┼────────────────────────────────┤
│ `known`          │ Neutral Slate /  │ Solid Border    │ Affirmatively represented as   │
│                  │ High Contrast    │ Check Glyph [✓] │ known in curated model         │
├──────────────────┼──────────────────┼─────────────────┼────────────────────────────────┤
│ `conflicting`    │ Dual-Tone Split  │ Split Border    │ Genuine competing claims       │
│                  │ Multi-Claim Card │ Fork Glyph [⑂]  │ across manuscript traditions   │
├──────────────────┼──────────────────┼─────────────────┼────────────────────────────────┤
│ `approximate`    │ Muted Amber /    │ Dotted Border   │ Inherently approximate dating, │
│                  │ Warm Neutral     │ Tilde Glyph [~] │ quantity, or duration          │
├──────────────────┼──────────────────┼─────────────────┼────────────────────────────────┤
│ `unknown`        │ Muted Slate /    │ Dashed Border   │ Affirmatively unknown or silent│
│                  │ De-emphasized    │ Question [?]    │ in all available source texts  │
├──────────────────┼──────────────────┼─────────────────┼────────────────────────────────┤
│ `not_researched` │ Low-Contrast     │ Dotted Border   │ Field or entity not yet audited│
│                  │ Subtle Badge     │ Clock/Pause [◷] │ or curated in the database     │
├──────────────────┼──────────────────┼─────────────────┼────────────────────────────────┤
│ `not_applicable` │ Ghosted Badge /  │ Diagonal Hash   │ Attribute does not apply to    │
│                  │ Dimmed Text      │ Null Glyph [⊘]  │ this entity category           │
└──────────────────┴──────────────────┴─────────────────┴────────────────────────────────┘
```

### 11.1 Principle of Epistemic Neutrality
- **No State Is "Defective"**: An `unknown` or `conflicting` state is an **honest scholarly finding**, not an error or a visual failure.
- **Dignified Styling**: `unknown` is styled with the same graphic dignity and precision as `known`. It is rendered as a clean, intentional dashed container (*"Parentage: Unknown in source tradition"*) rather than a blank space, red error alert, or ugly broken placeholder.
- **Zero Hallucination / Zero Fabrication**: Missing or absent information must render using the appropriate canonical epistemic state supplied by the data model, never hidden, fabricated, or inferred merely from absence. The UI never invents speculative details to fill empty layout slots.

---

## 12. Responsive Experience

Translating Block **F4 (Responsive Shell)** and Block **F16 (Accessibility & Reflow)** across the four space-based viewport classes:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                         SPACE-BASED RESPONSIVE COMPOSITION                             │
├──────────────┬──────────────────┬──────────────────────────────────────────────────────┤
│ Viewport     │ Layout Model     │ Visual & Shell Adaptation                            │
├──────────────┼──────────────────┼──────────────────────────────────────────────────────┤
│ **Compact**  │ Single-Column    │ • Header simplifies to Brand + Search + Menu         │
│ (Phone)      │ Stacked Stream   │ • Primary navigation moves to Bottom Nav Bar         │
│              │                  │ • Visualizations expand full-width with touch pinch  │
│              │                  │ • Contextual inspector / Evidence opens as Bottom    │
│              │                  │   Sheet with gesture drag handle                     │
│              │                  │ • Data tables reflow into stacked key-value cards    │
├──────────────┼──────────────────┼──────────────────────────────────────────────────────┤
│ **Medium**   │ Adaptive Two-    │ • Primary navigation compresses into icon-only rail  │
│ (Tablet)     │ Pane / Overlay   │ • Workspaces support side-by-side or slide-over      │
│              │                  │ • Evidence drawer slides out as a side inspection    │
│   panel                                              │
│              │                  │ • Touch targets maintain minimum 44×44px hit areas   │
├──────────────┼──────────────────┼──────────────────────────────────────────────────────┤
│ **Expanded** │ Persistent Two-  │ • Persistent left navigation sidebar with labels     │
│ (Laptop/PC)  │ Pane Workspace   │ • Workspace and contextual inspector sit side-by-    │
│              │                  │   side without obscuring the visualization           │
│              │                  │ • Full keyboard shortcut navigation active           │
├──────────────┼──────────────────┼──────────────────────────────────────────────────────┤
│ **Wide**     │ Multi-Pane       │ • Expansive multi-column studio layout               │
│ (Ultra-wide) │ Scholarly Studio │ • Left Nav Rail + Primary Visualization Canvas +     │
│              │                  │   Persistent Metadata Panel + Evidence Surface       │
│              │                  │ • Maximizes comparative scholarly analysis           │
└──────────────┴──────────────────┴──────────────────────────────────────────────────────┘
```

---

## 13. Accessibility Experience

Translating Block **F16 (Accessibility Architecture)** into core design requirements:

### 13.1 Normative Target: WCAG 2.2 Level AA
The target standard for the finished application is **WCAG 2.2 Level AA**. (Note: In accordance with project rules, this brief states the target without falsely claiming current certified compliance).

### 13.2 Visual Contrast and Focus Rings
- **Contrast Ratios**: All body text maintains $\ge 4.5:1$ contrast against surface backgrounds; major headings and interactive icons maintain $\ge 3.0:1$.
- **Focus Rings**: Keyboard navigation produces an unmissable $2\text{px}$ high-contrast focus outline with a $2\text{px}$ offset (`border-focus`). Focus rings are never hidden.

### 13.3 Accessible Alternatives to Visualizations
For every graphical visualization (Graph canvas, Lineage DAG, Timeline scrubber, Geographic map, Vyūha diagram), the interface provides an **immediate accessible alternative**:
- A semantic HTML data table or structured hierarchical disclosure list presenting the identical underlying entities, relationships, sequence numbers, and citations in screen-reader-accessible form.
- A prominent toggle button (e.g., `[ ☷ View as Accessible Table ]`) allows users to switch modes instantly.

### 13.4 Motion Sensitivity (`prefers-reduced-motion`)
When the user's operating system requests reduced motion:
- Dynamic graph layout motion is reduced or halted according to the user's reduced-motion preference.
- Canvas panning and zooming animations are replaced with instant visual state changes.
- Slide-out drawers and modals appear instantly without sliding or fading transitions.

### 13.5 Semantic Hierarchy and Script Tags
- Strict single `<h1>` per page, followed by logical `<h2>` and `<h3>` sectioning.
- All non-English script passages are wrapped with accurate semantic language attributes:
  - `<span lang="sa-Latn">` for IAST transliteration.
  - `<span lang="sa-Deva">` for native Devanāgarī.

---

## 14. Representative User Journeys

### 14.1 Journey 1: Lineage, Kinship & Backing Evidence (Illustrative Walkthrough)
```
Home Discovery Hub
  │
  ▼ [Search: "Arjuna"]
Character Detail (`/characters/arjuna`)
  │ (Inspects Identity, Overview, and Quick Facts)
  ▼ [Clicks: "View Family Lineage" launchpad]
Family Lineage DAG (`/lineage/arjuna`)
  │ (Explores ancestors: Pāṇḍu, Vicitravīrya, Śaṁtanu; inspects descendant: Abhimanyu)
  ▼ [Selects: Mentor edge to Droṇa]
General Relationship View (`/relationships/arjuna`)
  │ (Reviews mentorship bond, narrative context, and ethical dilemmas)
  ▼ [Clicks: Citation badge on relationship claim]
Evidence Drawer (F15)
  │ (Inspects source reference with citation locator and translation excerpt)
```

### 14.2 Journey 2: Topological Graph Discovery & Second-Degree Horizon (Illustrative Walkthrough)
```
Header Bar (Global Shortcut `Ctrl+K`)
  │
  ▼ [Types: "Bhīṣma" → Hits Enter]
Character Detail (`/characters/bhishma`)
  │ (Reads biographical overview and patriarchal role)
  ▼ [Clicks: "Explore Focus Graph" launchpad]
Global Focus Graph (`/graph/bhishma?d=1`)
  │ (Inspects 1-hop ties: Satyavatī, Ambā, Dhṛtarāṣṭra, Pāṇḍu, Duryodhana)
  ▼ [Toggles Depth: D=2 query parameter]
Expanded Focus Horizon (`/graph/bhishma?d=2`)
  │ (Discovers 2-hop topological connections across dynasties and factions)
  ▼ [Clicks: Relational edge]
Evidence Drawer (F15)
  │ (Inspects textual provenance and source references)
```

### 14.3 Journey 3: War Day Exploration & Tactical Formations (Illustrative Walkthrough)
```
Navigation Rail
  │
  ▼ [Selects: "Wars" lens]
Kurukṣetra War Overview (`/wars/kurukshetra`)
  │ (Reviews 18-day subdivision, faction alignments, and narrative progression)
  ▼ [Clicks: Day 13 on the 18-Day Stepper]
War Day 13 Breakdown (`/wars/kurukshetra/day/13`)
  │ (Reviews documented commanders, tactical summary, and combat duels)
  ▼ [Clicks: "Cakra-vyūha" formation card]
Battlefield Vyūha Detail (`/vyuhas/chakravyuha`)
  │ (Inspects structural/geometric representation or accessible textual breakdown)
  ▼ [Selects: Abhimanyu participant node]
Character Detail (`/characters/abhimanyu`)
  │ (Examines combat occurrences, kinship links, and narrative episodes)
  ▼ [Clicks: Provenance trigger]
Evidence Drawer (F15)
  │ (Examines source bibliographic citations recording the battle)
```

---

## 15. Design Principles & Non-Negotiables (Stitch MCP Guardrails)

When generating visual mockups and interactive prototypes with Stitch MCP, the following principles are **mandatory and non-negotiable**:

1. **Knowledge Explorer First**: The interface is an intellectual discovery engine, not a shallow blog, an e-commerce catalog, or a fantasy wiki.
2. **Evidence-Aware UI**: Citations, verse locators, and primary sources must be visually integrated as natural, first-class elements on every page.
3. **Zero Fabrication**:
   - **NEVER** generate fictional characters, invented dates, fake genealogies, or hallucinated Sanskrit verses.
   - **NEVER** invent modern GPS coordinates for ancient mythical/unverified sites.
   - **NEVER** generate AI placeholder portraits that invent character appearances. Use semantic monogram badges when no authentic art exists.
4. **Modern First, Traditional Second**: Clean, restrained contemporary digital design. No faux parchment, no burnt paper, no heavy gold glitter, no temple arch borders.
5. **One Graph, Many Lenses**: Every screen must communicate that it is an analytical perspective on the unified epic graph.
6. **Canonical Entities Remain Navigable**: Every entity mention (character, place, clan, war day, vyūha) must look and behave like a clickable link to its canonical route.
7. **Epistemic Neutrality**: The six epistemic states (`known`, `conflicting`, `approximate`, `unknown`, `not_researched`, `not_applicable`) must be rendered with equal graphic dignity. Missing or absent information must render using the appropriate canonical epistemic state supplied by the data model, never hidden, fabricated, or inferred merely from absence.
8. **Responsive Parity**: The experience must feel native, ergonomic, and fully realized across Compact (phone), Medium (tablet), Expanded (laptop), and Wide (desktop) layouts.
9. **Accessibility First**: Visible focus indicators, high contrast ($\ge 4.5:1$), readable typography, and structured alternative tables for all visualizations.
10. **Authentic Media Only**: Curated classical artwork only (miniatures, murals, sculptures) with proper museum/manuscript attribution. When absent, rely on typography and semantic monograms.
11. **Illustrative Exemplar Status**: All named entities, example relationships, claims, counts, citations, and narrative details used in mockups are illustrative design exemplars unless explicitly supplied by the canonical application data. Stitch must not treat illustrative content as seeded or authoritative application data.

---

## 16. Intentional Undecided Areas (Must NOT Be Guessed by Stitch)

To preserve architectural integrity and prevent premature decisions, Stitch MCP **must not** guess, decide, or lock in choices for the following areas:

1. **Visualization Runtime Libraries**: Whether the production app will use dedicated visualization runtime libraries, mapping/rendering libraries, or custom SVG engines is an architectural decision deferred to Stage 4. Stitch must visualize the *experience*, not generate library-specific boilerplate.
2. **Exact Pixel Breakpoints**: The responsive system is space-based (Compact, Medium, Expanded, Wide). Stitch must demonstrate responsive reflow without declaring hardcoded `@media (min-width: 768px)` rules.
3. **Specific Component Library Packages**: Stitch must not assume or mandate specific third-party component libraries. The design system is token-driven and styling-agnostic.
4. **Concrete Icon Library**: While lightweight vector icon libraries are conceptually recommended, exact icon glyphs and SVG assets are finalized in Stage 4.
5. **Backend Implementation Details**: Stitch prototypes must treat backend data as standard JSON contracts matching Stage 1 B5; it must not speculate on database schemas or backend server architecture.
6. **Production Build Configurations**: Bundler settings, Vite configs, post-CSS plugins, and Docker deployment belong strictly to Stage 4.

---

## 17. Stitch Design Brief (Handoff Specification)

### 17.1 Product Character & Visual Tone
- **Character**: Authoritative, serene, scholarly, intellectually captivating, culturally dignified.
- **Color Palette (Exploration Guidance for Prototyping; Non-Production Tokens)**:
  - Backgrounds: Crisp clean light paper neutral (`#F8F9FA` / `#FFFFFF`) for Light mode; deep slate-charcoal (`#0F172A` / `#1E293B`) for Dark mode.
  - Primary Structural: Deep Indigo / Slate Navy (`#1E1B4B` / `#312E81`).
  - Accent / Focus: Subtle Saffron / Ochre (`#D97706` / `#B45309`) used sparingly for interactive highlights, focus rings, and active lens badges.
  - Borders: Whisper-thin $1\text{px}$ borders (`#E2E8F0` in light, `#334155` in dark).
- **Typography**:
  - Headings: Refined, elegant serif with classical proportions.
  - UI & Body: Ultra-legible humanist sans-serif with complete IAST diacritic support.
  - Citations & Codes: Tabular monospace.

### 17.2 Core Screens to Prototype with Stitch
1. **Home / Discovery Hub (`/`)**:
   - Dignified hero introducing the "One Graph, Many Lenses" paradigm.
   - Quick launch tiles into the 10 canonical lenses.
   - Featured epic paths (e.g., "The Lineage of the Kurus", "The 18 Days of Kurukṣetra", "Tactical Formations of the Epic").
2. **Character Detail View (`/characters/:slug`) [Flagship Screen]**:
   - Illustrative entity exemplar (e.g., Arjuna or Karṇa).
   - Visualizing all five tiers: Identity Header, Narrative Overview, Cross-Lens Launchpad, Facet Panels (Episodes, Kinship, Relations, Military), and Provenance & Attribution Footer.
3. **Global Focus Graph (`/graph/:slug`)**:
   - Bounded topological network ($D \le 2$) centered on a focal entity.
   - High-contrast nodes, directional typed edges, halo focus, and inspection sidebar.
4. **Family Lineage Tree (`/lineage/:slug`)**:
   - Multi-generational dynastic DAG of illustrative royal houses.
   - Horizontal generation bands, spousal connectors, and collapsible branches.
5. **Timeline & Chronology (`/timeline`)**:
   - 18 Parva horizontal scrubber paired with a vertical chronological event feed.
6. **Geographic Map (`/geography`)**:
   - Map of ancient Bhāratavarṣa with mapped kingdom pins and an explicit drawer for unmapped textual sites.
7. **Kurukṣetra War Day 13 (`/wars/kurukshetra/day/13`)**:
   - Dual-faction breakdown, supplied commanders, combat duels, and fallen heroes.
8. **Battlefield Vyūha Detail (`/vyuhas/chakravyuha`)**:
   - Geometric SVG schematic or structural breakdown of the Cakra-vyūha with participant positions where supplied.
9. **Global Search Modal (`Ctrl+K`)**:
   - Autocomplete dropdown with domain-tagged suggestions and epistemic badges.
10. **Evidence & Provenance Drawer (`F15`)**:
    - Slide-out drawer showcasing the 4-tier chain (Entity/Edge → Claim → Evidence → Source).
    - Competing claims card showcasing side-by-side tradition parity.

### 17.3 What Stitch Must NOT Fabricate
- Do not invent fantasy narrative text.
- Do not invent fictitious family members or combat duels.
- Do not invent speculative dates or modern GPS coordinates.
- Do not create gaudy, decorative "mythological video-game" interfaces.
- Strictly adhere to the 10 canonical lenses and 6 epistemic states.
