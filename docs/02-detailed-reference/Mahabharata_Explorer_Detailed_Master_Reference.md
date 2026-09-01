Mahābhārata Explorer

DETAILED MASTER REFERENCE

Product • Architecture • Knowledge Graph • Evidence • UX • Visual Language • Base44 • Scope

Working reference consolidated from the project material available through Phase 2 and the visual-language working document.

# 1. Purpose of This Document

This is the detailed companion to the shorter Master Consolidated Project Blueprint. The short blueprint answers “what is the project?” at a glance. This document preserves the much larger body of decisions, rules, inventories, architectural consolidations, interaction ideas, visual-language methodology, V1/V2 boundaries, and implementation guidance discussed during the project.

It is intended to be read by a future collaborator, designer, developer, researcher, or by us after a break in the project. The reader should be able to reconstruct the project’s intended behavior and design philosophy without relying on scattered conversation history.

Important scope note: this document consolidates what the available project material actually establishes. It does not silently invent missing decisions or turn future ideas into committed V1 requirements.

# 2. Project Definition

## 2.1 What we are building

Mahābhārata Explorer is a major interactive web application / digital atlas for exploring the Mahābhārata as an interconnected knowledge system.

It is explicitly not just:

- a mythology website
- a blog
- a static encyclopedia
- a family-tree website
- a timeline website
- a mind map
- a visual artwork project
- a game or open-world simulation
The intended product is a beautiful, responsive, source-aware, deeply interconnected knowledge explorer in which users can navigate through time, people, events, places, relationships and the Kurukshetra War, while being able to inspect the evidence and sources behind information.

## 2.2 Product differentiator

ONE WORLD
↓
MANY LENSES
↓
CONNECTED EXPLORATION

The differentiator is not any single visualization. Timeline, map, family tree, relationship graph and war explorer are lenses over the same world.

# 3. Foundational Architecture

Formal distinction: Data = what exists; Evidence = why it is represented; Exploration = how the user navigates it; Presentation = how it looks; State = what the user is doing or prefers.

# 4. Master Knowledge Graph

Source
↓
Evidence
↓
Claim
↓
Entity

Entities then connect to each other through first-class relationships. The graph is exposed through Timeline, Map, Explorers, Focus and Search.

## 4.1 Core entities

Potential future entities explicitly identified: Journey, DatasetVersion, EditorialNote, VariantAccount.

## 4.2 Relationship examples

Character → Character
Character → Event
Character → Location
Character → Group
Event → Event
Event → Location
Event → WarDay
Formation → Event
Claim → Entity
Evidence → Claim
Evidence → Source

Relationships should be represented as data wherever possible, not hard-coded into page logic.

# 5. Architectural Consolidation Decisions

## 5.1 One system instead of duplicated systems

## 5.2 Shared systems

- Navigation System
- Timeline System
- Map System
- Entity System
- Relationship System
- Event System
- Search System
- Focus System
- Evidence System
- Source System
- Filtering System
- Settings/Preferences System
- Responsive Layout System
- Design System
# 6. Information Architecture & UX

## 6.1 User-facing navigation

Internal database concepts such as Claims, Evidence, Relationships and War Days should not become the primary navigation. The navigation should be organized around user exploration tasks.

## 6.2 Entity detail layout

Entity Header → Summary → Key Information → Relationships → Timeline → Map → Evidence → Related Items

Not every entity needs every section. The interface should be data-driven and only render sections supported by available data.

## 6.3 No-dead-ends rule

- Character → Events / Timeline / Relationships / Family / War / Sources
- Event → People / Location / Timeline / War / Sources
- Location → Events / People / Map / Sources
- Source → Claims / Entities / References
## 6.4 Product UX loop

DISCOVER → EXPLORE → ZOOM → CONNECT → VERIFY → RETURN → DISCOVER SOMETHING ELSE

## 6.5 First-time user understanding

# 7. Focus, State & Navigation Persistence

## 7.1 State hierarchy

GLOBAL: Focus
PAGE: Selected tab / day / view
LOCAL: Selected event / node / marker
TEMPORARY: Search / filter / drawer

The UI needs a clear active-state hierarchy so combinations such as Focus = Arjuna, Filter = Day 13, Search = Karna and Selected Event = X do not become confusing.

## 7.2 Persistence rules

- Selecting a day should not silently clear a character focus unless explicitly intended.
- Opening a source drawer should not clear the selected event.
- Major navigational states should be representable in URLs where practical.
## 7.3 Deep linking

character/arjuna
war/kurukshetra/day/13

Canonical deep links support sharing, bookmarks, browser history and reload persistence. Search results should open canonical entity routes rather than only temporary modals. Transient states such as hover generally should not be encoded in URLs.

# 8. Responsive Design

Responsive is a hard product requirement. Required contexts: desktop landscape, tablet landscape, tablet portrait, mobile portrait and mobile landscape.

Responsive means adapting the interaction model, not simply shrinking columns.

Every visual concept should be designed as both a desktop implementation and a mobile implementation. Cultural assets must remain usable in both contexts.

# 9. Visual Language System

## 9.1 Core philosophy

MODERN FIRST. TRADITIONAL SECOND.

The goal is not to decorate a modern website with Indian motifs. The goal is to build an excellent modern information product and embed Mahābhārata/Indic visual language intelligently into components, interactions and micro-moments.

Target feeling: “A modern digital atlas of an ancient epic.”

## 9.2 Avoided aesthetics

- ancient manuscript recreation
- mythology-blog aesthetic
- generic Indian cultural website
- temple-themed website
- overly ornamental Sanatan aesthetic
- brown/parchment/sepia historical interface
- gold-heavy mythology website
- generic SaaS dashboard with Indian decorations
- heavy textures
- ornamental borders everywhere
- excessive mandalas/lotuses
- unnecessary Sanskrit decoration
- overly complicated visual effects
## 9.3 Cultural enrichment vs decoration

The strongest design references are semantic. Example: a loading indicator can use the circular motion of the Sudarshan Chakra because the reference naturally communicates rotation and processing. A bow/arrow can inspire directional transitions or causal paths because its geometry and movement have a functional relationship to the interaction.

## 9.4 Three cultural layers

Level 3 should be used more sparingly.

## 9.5 Asset abstraction scale

Preferred levels in most cases: 1, 2 and 4. Level 3 is selective.

# 10. Modular Cultural Asset Strategy

Generic Component → Theme / Asset Slot → Mahābhārata Asset

All cultural assets must be modular and replaceable. The interface should be able to launch with generic components and progressively replace selected visual slots with culturally enhanced versions.

# 11. Component-by-Component Cultural Design Method

1. A. Identify the UI component.
1. B. Identify what the component communicates.
1. C. Find natural visual metaphors.
1. D. Find Mahābhārata/Indic references that embody the metaphor.
1. E. Critically evaluate appropriateness.
1. F. Choose the abstraction: icon, illustration, animation, geometry, transition, pattern, typography, sound, or nothing.
1. G. Define where the reference should NOT be used.
1. H. Define the actual UI treatment and asset specification.
## 11.1 Side-by-side evaluation format

# 12. Complete UI / Visual Component Inventory

## Brand / Identity

Logo • Wordmark • App icon • Favicon • Splash screen • Loading screen • Hero visual • Brand emblem • Background motif • Watermark

## Navigation

Main navigation • Mobile navigation • Header • Breadcrumbs • Back • Forward • Menu • View switcher • Section navigation • Active-state indicator

## Timeline

Timeline axis • Timeline line • Beginning • Ending • Progress • Marker • Major marker • Minor marker • Selected marker • Turning-point marker • Era marker • Era divider • Event connector • Branch connector • Causal connector • Character-event connector • Highlighted path • Minimap • Zoom controls • Transitions

## Events

Event card • Major event card • Compact event card • Expanded event card • Event icon • Event-type icon • Importance indicator • Date indicator • Location indicator • Character indicator • Source indicator • Event header • Detail panel • Related events • Previous/Next event

## Cause & Effect

What led here? • What happened next? • Trace Back • Trace Forward • Causal chain • Causal connector • Path highlighting • Traversal animation • How did we get here? visualization • Consequence visualization

## Characters

Character card • Portrait • Silhouette • Icon • Emblem • Name • Epithet • Faction indicator • Family indicator • Status • Selection • Hover • Focus • Journey • Timeline • Turning point • Signature object • Signature weapon • Character-specific motif

## Relationships

Relationship graph • Character node • Selected node • Related node • Parent-child • Sibling • Marriage • Friendship • Rivalry • Teacher-student • Ally • Enemy • Political • Event connection • Graph animation • Focus • Zoom • Reset

## Family Tree

Family node • Generation marker • Parent-child line • Marriage line • Sibling structure • Family branch • Family group • Expand/collapse • Generation navigation

## Search

Search field • Search icon • Loading • Autocomplete • Search result • Character result • Event result • Location result • Relationship result • Highlighting • Recent search • No results

## Filters

Filter button • Filter icon • Filter chip • Selected chip • Filter drawer • Dropdown • Checkbox • Radio • Multi-select • Range selector • Reset • Active-filter indicator

## Event Type Iconography

Birth • Death • Marriage • Battle • Conflict • Journey • Exile • Political event • Coronation • Negotiation • Betrayal • Vow • Boon • Curse • Training • Assembly • Ritual • Revelation • Spiritual event • Turning point

## Kurukshetra

War emblem • Battlefield visual • War progress • 18-day selector • Day marker • Active day • Completed day • Battlefield marker • Commander marker • Fallen warrior marker • Army/faction marker • Battle event • Death event • Turning point • Chariot • Chariot wheel • Horse • Bow • Arrow • Conch • Banner • Mace • Sword • Shield • Spear • Battlefield formation

## Map

Map • Kingdom marker • City marker • River • Mountain • Forest • Palace • Battlefield • Journey route • Starting point • Destination • Character journey marker • Compass • Legend • Geographic transition

## Focus Mode

Activation • Focus icon • Focused node • Focus ring • Related-node highlight • Unrelated-node fade • Focus transition • Exit focus • Focus breadcrumb

## Compare Mode (future)

Compare button • Split view • Divider • Character comparison • Shared-event marker • Difference marker • Similarity marker • Comparison timeline • Comparison transition

## Loading / Transitions

Initial loading • Page loading • Timeline loading • Graph loading • Search loading • Character loading • Map loading • Filter loading • Inline loading • Button loading • Page transitions • Event opening/closing • Character transition • Timeline traversal • Era transition • View switching

## Empty States

No search results • No event • No character • No relationship • No location • Empty timeline • Empty graph • Empty bookmarks

## Error States

Generic error • Data error • Network error • Search error • 404 • Retry • Error icon • Recovery animation

## Source / Scholarly UI

Source icon • Citation marker • Chapter marker • Verse marker • Edition marker • Source badge • Source panel • Citation tooltip • View source

## Micro-interactions

Hover • Click • Selection • Expansion • Collapse • Scroll • Zoom • Pan • Focus • Search • Filtering • Navigation • Loading • Completion • Error • View switching • Timeline traversal • Graph traversal

# 13. Event-Type Iconography Principles

The event icon system should not automatically turn every concept into a literal object. Each category should be evaluated as:

- abstract geometry
- symbolic icon
- Mahābhārata object
- Indic-inspired symbol
- modern icon
The semantic meaning of the event should determine the visual language. Decorative literalism is not the goal.

# 14. Evidence, Sources & Scholarly Trust

## 14.1 Evidence as a cross-cutting layer

Evidence should appear in context rather than being relegated to a bibliography page. The same Source/Evidence/Claim architecture should support event sources, character sources, war sources, formation sources and map sources.

## 14.2 Source expansion

Original seed dataset + Additional credible sources + Future source expansion → Unified Knowledge Base

## 14.3 Data quality workflow

Question → Source Discovery → Source Quality Check → Extraction → Cross-check → Structuring → Claim → Evidence → Database

Source identified? → Credible? → Specific reference? → Claim extracted? → Conflicting accounts checked? → Structured? → Published

The initial research and source-verification workflow remains deliberate and outside Base44. Base44 should render and manage the structured knowledge system rather than autonomously researching the Mahābhārata.

## 14.4 Truth-preservation rules

- No false connections: only explicitly represented relationships are shown.
- No false precision: never invent exact dates, coordinates, sequence, troop positions, numerical statistics or causal relationships.
- No false authority: do not make an illustration look like a primary-source diagram, an editorial interpretation look like a quotation, an AI summary look like a source, or a disputed account look universally settled.
# 15. Data Completeness & Edge Cases

A blank field must never automatically mean “unknown.”

# 16. Performance Architecture

Potentially heavy systems include large event sets, relationships, maps, images, timelines, diagrams, sources and animations.

Application shell → Primary content → Secondary relationships → Heavy visualizations

- Lazy-load images, maps, complex diagrams and large relationship graphs.
- Timeline: aggregated markers at broad zoom, individual events at close zoom.
- War: load relevant day detail progressively rather than every day at once.
- Relationship graph: show major/limited nodes at overview; expand on focus/zoom.
- Map: use layers, clustering and contextual filtering rather than every marker simultaneously.
# 17. Accessibility & Usability

- Keyboard navigation
- Screen readers
- Visible focus
- Semantic controls
- Contrast
- Reduced motion
- Responsive text
- Non-color-only meaning
Cultural assets must remain secondary to usability. Essential information cannot depend solely on color, symbolism, animation or cultural knowledge. Important information must remain understandable through text, hierarchy, labels and conventional accessible controls.

# 18. Settings & Presentation Control

Settings customize the experience, not the truth.

# 19. Data Integrity, Editing & Editorial Architecture

- Frontend must not be the source of truth.
- Validation belongs at the data layer.
- User-editable settings must be validated.
- Source metadata must not be silently modified.
- Generated/AI content must be distinguished from source-derived content.
## 19.1 V1 editing

No public editing. The knowledge base is curated.

Public editing would require moderation, revision history, contributor accounts, conflict resolution, vandalism prevention and source validation—effectively a different product.

## 19.2 Future editorial layer

Editorial Database → Validation → Published Knowledge Base → Public Explorer

- add source
- add character
- edit event
- attach evidence
- create relationship
- review changes
- publish dataset
A full CMS is not required for V1, but the structures should be importable/editable by the project owner.

# 20. Scope: V1, V2, V3

## 20.1 V1 — Must have

- Responsive application shell
- Timeline
- Characters
- Events
- Locations
- Basic relationships
- Map
- War Explorer
- Search
- Focus
- Source/Evidence foundation
- Settings foundation
- Central design tokens
- Reusable cards/drawers/sheets
- Loading/empty/error states
- Accessibility foundation
## 20.2 V1 detail expectations

## 20.3 V2 candidates

- Progressive chronology / spoiler mode
- Advanced Formation Explorer
- Advanced relationship visualization
- Character journey playback
- Source comparison
- Variant account comparison
- Advanced research search
- More sophisticated map layers
- Richer analytics
- Saved explorations
- Bookmarks
- Notes
- Multilingual content
## 20.4 V3 / long-term candidates

- Research Mode
- AI-grounded Q&A
- Source passage exploration
- Advanced textual comparison
- Contributor/editorial workflows
- Dataset version history
- Collaborative research
- Advanced visualization
- Public API
## 20.5 Explicit V1 exclusions

- AI Q&A — structured knowledge first, evidence second, grounded AI later.
- Giant 3D Mahābhārata world — the product is a knowledge explorer, not an open-world game.
- Battle simulation — do not imply speculative battlefield movement.
- Static encyclopedia architecture — the core value is connected exploration.
# 21. Representative Dataset Strategy

The original PDF is the seed dataset, not the complete source universe. V1 should deliberately use a representative subset with enough complexity to exercise the architecture.

- Timeline
- Character
- Event
- Relationship
- Map
- War
- Evidence
- Search
Small representative dataset → Build → Test → Architecture correction → Expand dataset

This is intentionally safer than loading thousands of records before the graph and UI have been validated.

# 22. Base44 Build Philosophy

Base44 should be instructed to consume the supplied structured dataset and implement the reusable architecture. It should not fabricate missing Mahābhārata information.

## 22.1 Build order

1. Data Foundation
2. Knowledge Graph
3. Shared Component System
4. Exploration Experiences
5. Evidence / Source Layer
6. Search + Focus
7. Responsive + Accessibility
8. Representative Dataset
9. Testing
10. Data Expansion

## 22.2 Data-driven UI rule

The frontend should render based on available structured data. If formation is null, do not render an empty Formation section. If sources are empty, do not render a broken source drawer. If information is missing, show an appropriate state.

## 22.3 Future-proofing

New records should be able to enter the knowledge base without requiring redesign of the visualization system.

# 23. Visual Development Workflow

The visual work is intentionally sequential rather than a giant asset-generation exercise.

1. 1. Loading
1. 2. Timeline
1. 3. Timeline markers
1. 4. Era dividers
1. 5. Event cards
1. 6. Event selection
1. 7. Trace Back / Trace Forward
1. 8. Character cards
1. 9. Character journey
1. 10. Relationship graph
1. 11. Family tree
1. 12. Focus mode
1. 13. Kurukshetra
1. 14. Map
1. 15. Navigation
1. 16. Search
1. 17. Empty/error states
1. 18. Micro-interactions
1. 19. Typography
1. 20. Decorative system
For each important component, compare a neutral modern version against a Mahābhārata-enhanced version. This allows the cultural version to be rejected when it does not genuinely improve the product.

# 24. Cultural Asset Research Categories

Research the concept first; do not force an asset into the interface because it is visually beautiful.

# 25. When the Correct Answer Is “Keep Modern”

- Basic search field
- Ordinary checkboxes
- Basic dropdowns
- Settings controls
- Accessibility controls
- Ordinary form inputs
If a cultural reference does not improve the experience, the correct design decision is to keep the component modern.

# 26. Explicit Design Traps

- Random lotus decoration
- Random Om symbols
- Generic temple silhouettes
- Excessive mandalas
- Gold borders everywhere
- Brown parchment
- Fake ancient-paper texture
- Excessive Devanagari
- Random Sanskrit words
- Generic Indian-pattern backgrounds
- Decorative weapons without semantic purpose
- Overly ornate cards
- Mythological paintings everywhere
- Excessive glow
- Excessive particles
- Overly cinematic interfaces
- Video-game-like UI unless specifically appropriate
The Mahābhārata should be felt through design intelligence, not visual noise.

# 27. Asset Specifications

Prefer lightweight, scalable formats whenever possible.

# 28. Master Principles — The Rules We Should Not Accidentally Break

- Modern first, traditional second.
- Information is the hero.
- Make the interface feel Indian without becoming stereotypically ancient.
- No excessive warmth, parchment, brown, gold or ornamental clutter.
- Responsive is fundamental.
- One knowledge graph, many exploration lenses.
- Reuse systems instead of duplicating them.
- Data before decoration.
- Evidence before certainty.
- Never fabricate missing information.
- Preserve uncertainty and source differences.
- Use progressive disclosure.
- Accessibility is foundational.
- The user controls presentation, not facts.
- Build architecture first, thematic assets second.
- Do not create false connections.
- Do not create false precision.
- Do not create false authority.
- Do not create feature-specific duplicate engines.
- Prefer connected exploration over isolated pages.
# 29. Primary Project Risk & Scope Discipline

The biggest identified risk is feature proliferation. Almost every idea can be exciting, which makes the project unusually vulnerable to growing beyond its core objective.

Amazing idea → V2
when it does not materially strengthen the first coherent product.

The project should therefore follow:

ARCHITECT → AUDIT → LOCK → MOVE ON → BUILD → TEST → IMPROVE

New architectural systems should not be introduced merely because they are interesting.

# 30. Current State & Next Stage

The project has moved beyond the original idea of “make an interactive mind map.” The correct mental model is now a beautiful, responsive, source-aware, interconnected Mahābhārata knowledge explorer.

# 31. The Complete Mental Model

KNOWLEDGE GRAPH
  ├─ Characters
  ├─ Events
  ├─ Locations
  ├─ Groups
  ├─ Relationships
  ├─ War / War Days / Formations
  └─ Claims / Evidence / Sources

        ↓
EXPLORATION LENSES
  ├─ Time
  ├─ Space
  ├─ People
  ├─ Relationships
  ├─ Events
  └─ Evidence

        ↓
SHARED INTERACTION SYSTEMS
  ├─ Search
  ├─ Focus
  ├─ Filters
  ├─ Navigation
  ├─ Timeline
  └─ Map

        ↓
PRESENTATION SYSTEM
  ├─ Design tokens
  ├─ Shared components
  ├─ Responsive layouts
  ├─ Motion
  └─ Mahābhārata cultural layer

        ↓
USER EXPERIENCE
DISCOVER → EXPLORE → ZOOM → CONNECT → VERIFY → RETURN

# Appendix A — Reference Material Used

This detailed reference was built from the project’s Phase 2 Consolidation & Architecture Audit (Complete 133-Point Audit) and the project’s visual-language working document. The audit explicitly defines the five layers, master graph, reusable systems, responsive requirements, evidence model, V1/V2/V3 scope, data-quality rules and representative-data strategy. The visual document establishes the Modern First / Traditional Second philosophy, cultural abstraction levels, component inventory, side-by-side workflow, asset strategy, and cultural design rules.

This is a working master reference, not a replacement for the underlying source documents. When a future decision changes a locked principle, the master reference should be updated deliberately rather than allowing the change to remain only in conversation history.

| Layer | Definition | Examples |
| --- | --- | --- |
| Data | What exists. | Characters, Events, Locations, Groups, Relationships, Wars, War Days, Formations. |
| Evidence | Why it is represented. | Claims, Evidence, Sources, provenance, uncertainty. |
| Exploration | How users navigate the knowledge graph. | Timeline, Character/Event/Relationship explorers, Family Tree, Map, War, Search. |
| Presentation | How the product looks and behaves. | Typography, colors, cards, panels, diagrams, animation, responsive layouts, cultural assets. |
| User State | What the user is currently doing or prefers. | Focus, selected day/event, filters, search query, theme, density, text size, motion, visual character. |

| Entity | Role |
| --- | --- |
| Character | A person represented in the knowledge base. |
| Event | A meaningful occurrence in the narrative/world. |
| Location | A geographic or narrative place. |
| Group | A collective such as a clan, kingdom, army, etc. |
| Relationship | A general graph connection between entities. |
| FamilyRelationship | A specialized familial relationship. |
| War | A major conflict. |
| WarDay | A structured subdivision of a War. |
| Formation | A battlefield formation where supported by reliable data. |
| Source | An identifiable information source. |
| Claim | A specific assertion about an entity or relationship. |
| Evidence | Evidence that supports, qualifies or contradicts a claim. |

| Old-looking duplication | Consolidated model |
| --- | --- |
| Character Journey + War Journey | One Journey concept; War Journey is contextual/filtered character journey. |
| Events + War Events | One Event entity; War/WarDay are context. |
| Main Map + War Map + Character Journey Map + Location Map | One Map system with different datasets/layers/context. |
| Main/War/Journey/Day timelines | One Timeline visualization architecture. |
| Event/Character/War/Formation/Map source systems | One Source → Evidence → Claim architecture. |
| Character/Event/War/Map Focus | One global Focus system. |
| Character/Event/War/Source/Location search | One Global Search with entity type, context, ranking and filters. |

| Primary navigation | Purpose |
| --- | --- |
| Explore | Broad exploration of people, events, locations and relationships. |
| Timeline | Explore chronology. |
| Map | Explore geography and journeys. |
| War | Explore the Kurukshetra War. |
| Search | Find anything across the knowledge base. |

| Question | Desired answer |
| --- | --- |
| What is this? | A Mahābhārata explorer. |
| What can I do? | Explore people, events, places and chronology. |
| How do I go deeper? | Click anything. |
| Can I verify information? | Yes; sources are attached throughout. |

| Desktop pattern | Mobile pattern |
| --- | --- |
| Side panel | Bottom sheet |
| Horizontal timeline | Mobile-adapted timeline |
| Persistent filters | Filter button + sheet |
| Navigation rail | Compact top navigation + contextual actions |
| Large relationship graph | Focused node-and-relationship view |

| Level | Description | Examples |
| --- | --- | --- |
| 1 — Indic visual language | Abstract geometry, symmetry, radial structures, line work, rhythm, subtle patterns, typography and icon geometry. | May not be recognizable as Mahābhārata. |
| 2 — Symbolic Mahābhārata references | Recognizable but restrained symbols. | Chakra, conch, bow, arrow, chariot wheel, dice, crown, banner. |
| 3 — Literal Mahābhārata assets | Actual illustrated/animated representations. | Chakra animation, chariot, character portrait, weapon, battlefield visual. |

| Level | Treatment |
| --- | --- |
| 0 | Completely modern. |
| 1 | Indic geometry / abstract influence. |
| 2 | Recognizable symbolic cultural reference. |
| 3 | Literal object or illustration. |
| 4 | Narrative interaction: the behavior itself is inspired by the epic. |

| Component | Neutral V1 | Possible enhanced version |
| --- | --- | --- |
| Loading | Generic spinner | Sudarshan Chakra |
| Timeline marker | Simple circle | Custom Indic/Mahābhārata marker |
| Event icon | Standard icon | Custom symbolic icon |
| Relationship node | Modern graph node | Custom geometry |
| Character | Neutral placeholder | Custom illustration/symbol |
| Era divider | Modern line + typography | Custom visual treatment |

| Field | What to record |
| --- | --- |
| Component | Name and role. |
| Modern / Neutral Version | How a generic modern interface solves it. |
| Mahābhārata-Enhanced Version | How the cultural layer changes/enhances it. |
| Why the reference works | Semantic relationship. |
| Visual treatment | Shape, proportion, line weight, color, animation, size, placement. |
| Restraint level | Minimal / Moderate / Strong. |
| Recommended? | Yes / Maybe / No. |
| Asset required | SVG / PNG / illustration / Lottie / CSS / icon / 3D / etc. |

| State | Meaning |
| --- | --- |
| Known | Supported by the current structured knowledge. |
| Unknown | The relevant fact is not known/established. |
| Not yet researched | The project has not yet completed research. |
| Not applicable | The field does not apply. |
| Conflicting | Sources/accounts differ. |
| Approximate | Only approximate information is supported. |

| Scenario | Required behavior |
| --- | --- |
| No coordinates | Show the location; do not invent placement. |
| No exact event date | Use relative sequence or approximate position. |
| Source has no online URL | Show source reference rather than broken link. |
| Character has no portrait | Use elegant neutral placeholder, not AI-generated face. |
| Formation text exists but visualization is unreliable | Show text and state that visualization is unavailable. |
| Missing structured data | Render appropriate empty/unknown state; never fabricate content. |

| May change | Must not change |
| --- | --- |
| Appearance | Source provenance |
| Density | Factual relationships |
| Text size | Event ordering |
| Motion | Geographic evidence |
| Visual character | Uncertainty |
| Chronological reveal | Canonical data |

| System | Minimum requirements |
| --- | --- |
| Timeline | Zoom/detail levels, chronological navigation, event cards, event detail, contextual navigation, responsive behavior, loading/empty states. |
| Character | Profile, relationships, timeline presence, event connections, source information, search, focus. |
| Event | Detail, chronological position, participants, locations, related events, sources, timeline connection. |
| Map | Locations, basic markers, location detail, event connection, responsive interaction; basic journey path only if data supports it cleanly. |
| War | Overview, day navigation, day events, participants, event links, timeline, map, sources; formation visualization only when data-ready. |
| Search | Global search, entity recognition, relevant results, deep links, empty states, mobile search. |
| Evidence | Source entity, metadata, evidence relationship, contextual source drawer, source detail, claim-level provenance where needed, uncertainty/status. |
| Settings | Theme, text size, density, motion, visual character, preference persistence. |

| Category | Examples |
| --- | --- |
| Objects | Weapons, divine weapons, conch, chakra, bows, arrows, maces, chariot components, dice, crowns, thrones, banners, seals. |
| Characters | Silhouettes, headgear, weapons, clothing, distinctive objects, attributes. |
| Architecture | Sabhas, palaces, thrones, gates, pillars, court structures. |
| Nature | Rivers, mountains, forests, fire, sun, moon, stars, sky, lotus, trees. |
| Geometry | Radial structures, concentric circles, symmetry, repetition, interlocking forms, line patterns, mandala-like / yantra-like geometry. |
| Narrative concepts | Oath, journey, exile, succession, rivalry, alliance, consequence, dharma, choice, fate, war, peace. |
| Textual traditions | Devanagari, Sanskrit typography, chapter structure, verse numbering, scholarly notation, manuscript-inspired hierarchy. |

| Use | Preferred format / approach |
| --- | --- |
| UI icons | SVG generally preferred. |
| Animations | Lottie, SVG or CSS where appropriate. |
| Raster imagery | PNG/WebP where appropriate. |
| Complex illustrations | Illustration asset. |
| Reusable animation/visual components | Sprite or other reusable format where justified. |
| 3D | Avoid unnecessary 3D. |

| Area | Status |
| --- | --- |
| Product identity | Established |
| Five-layer architecture | Established / locked |
| Knowledge Graph architecture | Established / locked |
| Evidence architecture | Established / core V1 |
| Entity model | Established |
| Relationship consolidation | Established |
| Responsive philosophy | Established |
| Visual language philosophy | Established |
| Component inventory | Established |
| V1/V2/V3 boundaries | Established |
| Representative-data strategy | Established |
| Base44 build direction | Established |
| Actual implementation | Next major stage |
