Mahābhārata Explorer

PRODUCT REQUIREMENTS DOCUMENT

PRD • V1 Product Definition • Functional Requirements • UX • Data • Evidence • Scope

Based on the project’s established consolidation/audit and visual-language specifications.

# 1. Purpose & Status

This PRD converts the project’s established architectural, UX, evidence, scope and visual-language decisions into explicit product requirements. It is intended to be the product-definition layer between the Detailed Master Reference and the eventual Base44 Build Specification.

This document does not silently turn future ideas into V1 requirements. Where the project material identifies something as future, potential or conditional, that status is preserved.

# 2. Product Definition

Mahābhārata Explorer / Mahābhārata Digital Atlas is a beautiful, responsive, source-aware, interconnected knowledge explorer in which the Mahābhārata is represented as one connected knowledge graph.

ONE WORLD
↓
MANY LENSES
↓
CONNECTED EXPLORATION

The primary lenses are Time, Space, People, Relationships, Events and Evidence. Search and Focus operate across them.

## 2.1 It is not

- A static encyclopedia
- A mythology blog
- Merely a timeline, family tree or mind map
- A generic Indian cultural website
- An open-world game
- A speculative battlefield simulator
- A V1 AI Q&A product
# 3. Product Vision & Experience

Users should be able to enter through a meaningful object—person, event, place, relationship, war day or source—and continue into connected information without dead ends.

DISCOVER → EXPLORE → ZOOM → CONNECT → VERIFY → RETURN → DISCOVER SOMETHING ELSE

# 4. Product Principles

- Information is the hero.
- One knowledge graph, many exploration lenses.
- Connected exploration over isolated pages.
- Data before decoration.
- Evidence before certainty.
- Never fabricate missing information.
- Preserve uncertainty and source differences.
- No false connections.
- No false precision.
- No false authority.
- Progressive disclosure keeps complexity manageable.
- Responsive behavior is fundamental.
- Accessibility is foundational.
- The user controls presentation, not facts.
- Reuse systems instead of duplicating them.
- Build architecture first, thematic assets second.
- Modern first, traditional second.
# 5. Foundational Architecture

# 6. Knowledge Graph & Data Model

## 6.1 Core entities

- Character
- Event
- Location
- Group
- Relationship
- FamilyRelationship
- War
- WarDay
- Formation
- Source
- Claim
- Evidence
## 6.2 Future entities

- Journey
- DatasetVersion
- EditorialNote
- VariantAccount
## 6.3 Relationship model

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

- Relationships are first-class data.
- Avoid hard-coded relationships in page logic where data can represent them.
- Different exploration lenses must reuse the same relationship data.
- Visual proximity must never create an implied factual relationship.
## 6.4 War model

There is one Event entity. A War Event is an Event with War/WarDay context. War therefore reuses the common Event system instead of creating a second event engine.

# 7. Information Architecture

Characters, Events, Relationships and Sources should be discoverable within the exploration systems rather than becoming disconnected top-level silos.

## 7.1 Shared entity-detail structure

Entity Header → Summary → Key Information → Relationships → Timeline → Map → Evidence → Related Items

Sections are conditional on available structured data.

# 8. Shared Product Systems

- Navigation
- Timeline
- Map
- Entity
- Relationship
- Event
- Search
- Focus
- Evidence
- Source
- Filtering
- Settings/Preferences
- Responsive Layout
- Design System
The product must reuse these systems instead of creating separate engines for character journey vs war journey, main map vs war map, main timeline vs war timeline, or character/event/war/source searches.

# 9. Functional Requirements

## 9.1 Application Shell

APP-001 — Support desktop landscape, tablet landscape/portrait, mobile portrait and mobile landscape.

APP-002 — Provide primary navigation for Explore, Timeline, Map, War and Search.

APP-003 — Provide contextual navigation and breadcrumbs/back context for deep exploration.

APP-004 — Provide shared loading, empty, error and partial-data states.

## 9.2 Timeline

TIM-001 — Provide chronological navigation.

TIM-002 — Support multiple detail/zoom levels.

TIM-003 — Represent events with reusable markers/cards.

TIM-004 — Allow event selection and event detail.

TIM-005 — Provide contextual links to related entities.

TIM-006 — Adapt interaction for mobile rather than merely shrinking desktop.

TIM-007 — Use relative/approximate placement when exact dates are not established.

TIM-008 — Use level-of-detail/progressive rendering for large datasets.

## 9.3 Characters

CHR-001 — Provide character profile/detail.

CHR-002 — Show explicit relationships.

CHR-003 — Show event connections.

CHR-004 — Show timeline presence where supported.

CHR-005 — Show sources/evidence where available.

CHR-006 — Support search, canonical route and Focus.

## 9.4 Events

EVT-001 — Provide event detail.

EVT-002 — Represent chronological position without false precision.

EVT-003 — Show participants where available.

EVT-004 — Show location where available.

EVT-005 — Show related events.

EVT-006 — Connect to sources/evidence.

EVT-007 — Connect to Timeline.

EVT-008 — Support War/WarDay context using the shared Event entity.

## 9.5 Relationships & Family

REL-001 — Provide reusable relationship visualization.

REL-002 — Represent relationship types as data.

REL-003 — Support selection, focus, zoom and reset where appropriate.

REL-004 — Provide family-tree structures over the underlying relationship data.

REL-005 — Do not infer relationships from visual proximity.

## 9.6 Map

MAP-001 — Provide location markers and location detail.

MAP-002 — Connect locations to relevant events.

MAP-003 — Support responsive interaction.

MAP-004 — Support journey paths only when cleanly supported by data.

MAP-005 — Never invent coordinates.

MAP-006 — Use layers, clustering and contextual filtering where needed.

## 9.7 War Explorer

WAR-001 — Provide War overview.

WAR-002 — Provide War Day navigation.

WAR-003 — Show day-specific events and participants.

WAR-004 — Connect War content to events, Timeline, Map and Sources.

WAR-005 — Treat formation visualization as conditional on data readiness.

WAR-006 — Progressively load detailed day information.

## 9.8 Global Search

SRCH-001 — Search across supported entity types.

SRCH-002 — Identify entity type in results.

SRCH-003 — Provide relevant ranked results.

SRCH-004 — Open canonical entity routes.

SRCH-005 — Provide no-results behavior.

SRCH-006 — Support mobile search.

## 9.9 Focus

FOC-001 — Allow Focus on meaningful entities.

FOC-002 — Highlight focused and relevant related information.

FOC-003 — De-emphasize unrelated information without losing context.

FOC-004 — Provide a clear exit-focus action.

FOC-005 — Preserve Focus when compatible page selections change unless explicitly intended otherwise.

## 9.10 Sources & Evidence

SRC-001 — Provide source metadata.

SRC-002 — Provide contextual source access.

SRC-003 — Provide source detail.

SRC-004 — Represent Evidence → Claim → Source relationships.

SRC-005 — Represent uncertainty/conflicting accounts where applicable.

SRC-006 — Do not show broken online-source actions when no URL exists.

## 9.11 Settings

SET-001 — Provide appearance/theme control.

SET-002 — Provide text-size control.

SET-003 — Provide density control.

SET-004 — Provide motion/reduced-motion behavior.

SET-005 — Provide visual-character control where implemented.

SET-006 — Persist supported preferences.

SET-007 — Never let settings change factual/provenance data.

# 10. Evidence & Source Requirements

Research question → Source Discovery → Quality Check → Extraction → Cross-check → Structuring → Claim → Evidence → Database

Source identified? → Credible? → Specific reference? → Claim extracted? → Conflicts checked? → Structured? → Published

The research/verification process remains deliberate and outside Base44. Base44 should consume and render the curated structured knowledge system, not fabricate Mahābhārata information.

- The original PDF is the seed dataset, not the complete source universe.
- Future expansion uses credible, identifiable sources and retains provenance.
- AI-generated or derived content must be distinguishable from source-derived content.
# 11. Data Integrity & Edge Cases

A blank field must never automatically mean “unknown.”

# 12. State, URLs & Shareability

- Global state: Focus.
- Page state: selected tab/day/view.
- Local state: selected event/node/marker.
- Temporary state: search/filter/drawer.
- Major navigational states should be representable in URLs where practical.
- Canonical routes should support sharing, bookmarks, browser history and reload persistence.
- Search should lead to canonical entity routes.
- The same canonical URL should work across desktop and mobile.
- Transient hover state should generally not be encoded in URLs.
Example: character/arjuna
Example: war/kurukshetra/day/13

# 13. Responsive Requirements

Responsive means interaction-model adaptation, not simply narrower columns.

# 14. Design System & Visual Requirements

## 14.1 Design system

- Central color tokens
- Typography tokens
- Spacing scale
- Radius system
- Shadows
- Borders
- Motion tokens
- Breakpoints
- Z-index
- Reusable component variants
## 14.2 Core visual philosophy

MODERN FIRST. TRADITIONAL SECOND.

The cultural identity should emerge through deliberate details. The product should feel like a modern digital atlas of an ancient epic—not an ancient manuscript recreated as a website.

## 14.3 Cultural abstraction levels

Levels 1, 2 and 4 are generally preferred; Level 3 is selective.

## 14.4 Cultural constraints

- No excessive parchment, burnt paper, brown/sepia or warmth.
- No excessive gold.
- No generic “ancient India” aesthetic.
- No temple/fantasy-game UI.
- No ornamental borders everywhere.
- No random lotus/Om/mandala decoration.
- No random Sanskrit words or excessive Devanagari.
- No decorative weapons without semantic purpose.
- No excessive glow or particles.
- No unnecessary 3D.
- If a cultural reference does not improve the experience, keep the component modern.
## 14.5 Modular asset architecture

Generic Component → Theme / Asset Slot → Mahābhārata Asset

Cultural assets must be replaceable without rebuilding the interface.

# 15. Accessibility Requirements

- Keyboard navigation
- Screen-reader support
- Visible focus
- Semantic controls
- Contrast
- Reduced motion
- Responsive text
- Non-color-only meaning
Essential information must remain understandable through text, hierarchy, labels and accessible controls, not only through cultural symbolism, color or animation.

# 16. Performance Requirements

- Progressive loading: shell → primary content → secondary relationships → heavy visualizations.
- Lazy-load images, maps, complex diagrams and large graphs where appropriate.
- Timeline uses level-of-detail rendering.
- War details load progressively.
- Relationship graphs expand on focus/zoom.
- Maps use layers, clustering and contextual filtering.
No numeric performance budget has been established in the source material; concrete budgets should be defined during technical specification if required.

# 17. Editorial & Security Requirements

- Frontend must not be the source of truth.
- Validation belongs at the data layer.
- Source metadata must not be silently modified.
- Generated/AI content must be distinguished from source-derived content.
- V1 knowledge base is curated.
- Public editing is out of scope for V1.
Editorial Database → Validation → Published Knowledge Base → Public Explorer

A future editorial layer may support adding sources/characters, editing events, attaching evidence, creating relationships, reviewing changes and publishing the dataset.

# 18. Release Scope

## 18.1 V1 — Must Have

- Responsive application shell
- Timeline
- Characters
- Events
- Locations
- Basic relationships
- Map
- War Explorer
- Global Search
- Focus
- Source/Evidence foundation
- Settings foundation
- Central design tokens and reusable components
- Loading/empty/error/partial states
- Accessibility foundation
## 18.2 V2 — Potential

- Progressive chronology/spoiler mode
- Advanced Formation Explorer
- Advanced relationship visualization
- Character journey playback
- Source comparison
- Variant account comparison
- Advanced research search
- Advanced map layers
- Analytics
- Saved explorations
- Bookmarks
- Notes
- Multilingual content
## 18.3 V3 / Long-term

- Research Mode
- AI-grounded Q&A
- Source passage exploration
- Advanced textual comparison
- Editorial/contributor workflows
- Dataset version history
- Collaborative research
- Advanced visualization
- Public API
# 19. Explicit V1 Exclusions

- AI Q&A: structured knowledge first, evidence second, grounded AI later.
- Giant 3D world: this is a knowledge explorer, not an open-world game.
- Battle simulation: do not imply speculative battlefield movement.
- Static encyclopedia architecture: the core value is connected exploration.
- Public editing: requires moderation and governance outside V1 scope.
# 20. Representative Dataset Strategy

V1 should begin with a deliberately selected representative subset containing enough complexity to exercise Timeline, Character, Event, Relationship, Map, War, Evidence and Search.

Small representative dataset → Build → Test → Architecture correction → Expand dataset

The purpose is to discover relationship/model flaws before thousands of records make architectural correction expensive.

# 21. Product Build Sequence

1. Formalize data, entity, relationship and evidence models.
1. Create representative structured dataset.
1. Establish design tokens and shared component system.
1. Build responsive application shell.
1. Build Timeline.
1. Build Character and Event systems.
1. Build Relationships and Family Tree.
1. Build Map.
1. Build War Explorer.
1. Build Global Search.
1. Build Focus/state/deep links.
1. Integrate Evidence/Source layer.
1. Implement settings foundation.
1. Implement accessibility and edge states.
1. Test representative dataset across all lenses.
1. Correct architecture based on testing.
1. Expand dataset.
1. Progressively add selected Mahābhārata visual assets.
# 22. V1 Acceptance Criteria

- The product communicates its purpose quickly.
- Primary navigation works across Explore, Timeline, Map, War and Search.
- Core entities use shared underlying models.
- Users can move between meaningful connected entities without dead ends.
- Timeline is usable and responsive.
- Character views connect to events, relationships, chronology and sources where data exists.
- Event views connect to participants, locations, chronology and sources where data exists.
- Map connects locations to relevant events and works responsively.
- War supports overview, War Days and connected events.
- Global Search finds supported entities and opens canonical routes.
- Focus works as a cross-cutting state.
- Evidence/source information is available contextually.
- Missing/uncertain information is never replaced by fabricated precision.
- Shared design tokens/components are consistently used.
- Desktop and mobile interaction models are intentionally designed.
- Accessibility foundations are present.
- Major systems have loading, empty, error and partial-data states.
- Representative data exercises every major V1 system before large-scale expansion.
# 23. Definition of Done — Feature Level

- The feature has a clear role in the knowledge graph.
- It reuses shared systems rather than creating a duplicate engine.
- It renders from structured data rather than hard-coded facts.
- Missing/partial/uncertain data behavior is defined.
- Desktop and mobile behavior are defined.
- Accessibility behavior is defined.
- Loading/empty/error states are handled.
- Relevant URL/state behavior is defined.
- Performance implications are considered.
- Any cultural enhancement is semantically justified and modular.
- The feature has been tested with representative data.
# 24. Open Items for the Technical Build Specification

The source material does not establish these implementation-level decisions, so they should be specified separately rather than guessed in this PRD:

- Exact field-level database schema
- Exact search ranking
- Map technology and cartographic implementation
- Timeline technology and interaction physics
- Exact colors and font selections
- Exact breakpoints
- Numeric performance budgets
- Detailed source/edition metadata schema
- Formation data schema and visualization rules
- Final cultural asset library
- Authentication/admin implementation
# 25. Final Product Statement

Mahābhārata Explorer V1 is a responsive, source-aware, interconnected knowledge explorer built around one shared knowledge graph. Timeline, Map, Character/Event exploration, Relationships, War, Search and Focus are coordinated lenses over one world—not isolated mini-products.

Its core promise is connected exploration with trustworthy provenance. Its visual identity is deliberately modern first and Mahābhārata second. Its architecture is designed to grow from a representative dataset into a much larger knowledge base without requiring the visualization system to be reinvented.

| Experience goal | Requirement |
| --- | --- |
| Understand | A first-time visitor quickly understands that this is a Mahābhārata explorer. |
| Explore | Users can move from broad context to specific entities. |
| Connect | Meaningful related entities are accessible from each other. |
| Verify | Relevant source/evidence information is available in context. |
| Trust | Uncertainty and conflicting information are preserved rather than hidden. |
| Adapt | The experience works intentionally across desktop and mobile. |

| Layer | Meaning | Examples |
| --- | --- | --- |
| Data | What exists | Characters, Events, Locations, Groups, Relationships, Wars, War Days, Formations |
| Evidence | Why it is represented | Claims, Evidence, Sources, provenance, uncertainty |
| Exploration | How users navigate | Timeline, explorers, family tree, map, war, search |
| Presentation | How it looks/behaves | Typography, colors, cards, panels, diagrams, motion, cultural assets |
| User State | What the user is doing/preferring | Focus, selected day/event, filters, theme, density, text size, motion |

| Primary area | Purpose |
| --- | --- |
| Explore | Broad exploration of people, events, locations and relationships |
| Timeline | Chronological exploration |
| Map | Geographic exploration |
| War | Kurukshetra War exploration |
| Search | Global discovery |

| Data state / case | Required behavior |
| --- | --- |
| Known | Render established information. |
| Unknown | Explicitly communicate that the information is unknown. |
| Not yet researched | Do not imply that research was completed. |
| Not applicable | Do not treat as missing information. |
| Conflicting | Preserve the conflict/status. |
| Approximate | Communicate approximation. |
| No coordinates | Do not invent map placement. |
| No exact date | Use sequence/approximate placement. |
| No online source URL | Show source reference without a broken link. |
| No character portrait | Use neutral placeholder; do not fabricate a face. |
| No reliable formation visualization | Show text/data and state that visualization is unavailable. |

| Desktop | Mobile |
| --- | --- |
| Side panel | Bottom sheet |
| Horizontal timeline | Mobile-adapted timeline |
| Persistent filters | Filter button + sheet |
| Navigation rail | Compact/contextual navigation |
| Expansive relationship graph | Focused node-and-relationship view |

| Level | Treatment |
| --- | --- |
| 0 | Modern; no cultural influence |
| 1 | Indic geometry / abstract influence |
| 2 | Recognizable symbolic cultural/Mahābhārata reference |
| 3 | Literal illustration/object |
| 4 | Narrative interaction inspired by the epic |
