Mahābhārata Explorer

MASTER CONSOLIDATED PROJECT BLUEPRINT

Consolidated through Phase 2.1.5

Product vision • architecture • knowledge model • UX • visual system • Base44 build specification

# 1. Executive Summary

Mahābhārata Explorer is a beautiful, responsive, source-aware, interconnected knowledge explorer for the Mahābhārata. It is not merely a website, blog, timeline, family tree, or mind map. Its defining idea is one connected world that can be explored through many lenses.

ONE WORLD → MANY LENSES → CONNECTED EXPLORATION

The application is built around one shared Knowledge Graph. Characters, events, locations, groups, relationships, war structure, claims, evidence, and sources are connected data. Timeline, Map, Explore, War, Search, Focus, and entity detail experiences are views over that same graph.

The project has now moved beyond conceptual architecture. The epistemic model, entity model, relationship contract, minimum schema, seed-data strategy, and master build specification have been consolidated. The next work should therefore be implementation and testing rather than continued architectural expansion.

# 2. The Product in One Page

# 3. Product Philosophy & Identity

## 3.1 Modern first. Traditional second.

Build an excellent modern information product first, then intelligently embed Mahābhārata/Indic visual language into appropriate components, interactions, and micro-moments.

- Do not recreate an ancient manuscript as a website.
- Do not become a mythology blog or generic cultural website.
- Do not use a temple/fantasy-game aesthetic.
- Avoid parchment, burnt paper, brown/sepia, excessive warmth, gold, texture, and ornamental clutter.
- Indic details should be discovered through meaningful details rather than constant decoration.
## 3.2 Cultural enrichment, not cultural decoration

Cultural references should reinforce the meaning of the UI interaction. The meaning of an asset and the meaning of the interaction should be related.

## 3.3 Information is the hero

Visual beauty supports comprehension and never overwhelms the information architecture.

# 4. Foundational Architecture

These layers remain conceptually distinct while being explicitly connected. Settings can change presentation and experience, but cannot change factual relationships, provenance, event ordering, geographic evidence, uncertainty, or canonical data.

# 5. Master Architecture

ONE KNOWLEDGE GRAPH
↓
SHARED DATA MODELS
↓
SHARED COMPONENT SYSTEM
↓
MULTIPLE EXPLORATION LENSES
↓
GLOBAL FOCUS + SEARCH
↓
CROSS-CONTEXT NAVIGATION
↓
EVIDENCE / SOURCES
↓
PERSONALIZED PRESENTATION

The product should feel like a world in which any meaningful object can lead to another meaningful object without dead ends.

# 6. User Experience Architecture

## 6.1 Primary lenses

Search and Focus operate across all six.

## 6.2 Main navigation

Characters, Events, Relationships, and Sources are discoverable within explorers rather than exposed as a large database-style primary navigation.

## 6.3 UX loop

DISCOVER → EXPLORE → ZOOM → CONNECT → VERIFY → RETURN → DISCOVER SOMETHING ELSE

## 6.4 No-dead-ends rule

Whenever a user encounters a meaningful entity, meaningful paths to related information should be available.

# 7. Responsive & Interaction Model

Responsive design is a hard requirement. Desktop and mobile are first-class layouts, including landscape and portrait orientations.

# 8. Visual & Design System

## 8.1 Visual principles

- Modern first; traditional second.
- Relatively flat / dimensional hybrid.
- Clean, sophisticated, editorial, readable, spacious and restrained.
- No excessive parchment, brown/sepia, warmth, gold, texture or ornamental borders.
- No generic 'ancient India' aesthetic.
- Indic details act as accents and meaningful interaction references.
## 8.2 Design tokens

Central tokens: Colors, Typography, Spacing, Radius, Shadows, Borders, Motion, Breakpoints, Z-index.

Semantic colors: background, surface, surface-elevated, text-primary, text-secondary, border, accent, success, warning, error, info.

## 8.3 Shared component vocabulary

Avoid card explosion: use a Base Entity Card with variants. Use one reusable Context Drawer and a Context Bottom Sheet on mobile.

# 9. Knowledge Graph & Data Architecture

## 9.1 Core entities

Potential future entities, deliberately excluded from V1: Journey, DatasetVersion, EditorialNote, VariantAccount.

## 9.2 Relationship architecture

Character → Character
Character → Event
Character → Location
Character → Group
Event → Event
Event → Location
Event → WarDay
WarDay → War
Formation → Event
Claim → Entity
Evidence → Claim
Evidence → Source

## 9.3 Consolidation decisions

- One Event entity: a War Event is an Event with War/WarDay context.
- One Map system with contextual datasets/layers.
- One Timeline architecture with contextual views.
- One global Search system.
- One global Focus system.
- Character Journey and War Journey are not separate systems.
- Sources are unified through Source → Evidence → Claim.
- Derivation belongs to provenance/lineage rather than a separate reasoning graph.
# 10. Claims, Evidence, Assessment & Derivation

SOURCE → EVIDENCE → CLAIM → ENTITY

- Direct and derived knowledge remain distinguishable.
- Significant derivations identify inputs, method/rule, output and provenance.
- Inference requires an applicable authorized rule.
- Derived status describes origin, not credibility.
- Material uncertainty must not be silently discarded.
- AI generation does not automatically confer canonical status.
- Material intermediate dependencies are preserved in multi-step derivations.
## 10.1 Research quality gate

Question → Source Discovery → Source Quality Check → Extraction → Cross-check → Structuring → Claim → Evidence → Database

Source identified? → Credible? → Specific reference? → Claim extracted? → Conflicts checked? → Structured? → Published

Initially, research and source verification remain a deliberate workflow outside Base44. Base44 manages and renders the structured knowledge system; it should not autonomously invent the knowledge base.

# 11. V1 Minimum Data Schema

## Character

- name (required)
- slug (required)
- alternate_names[]
- summary
- portrait_url
- status
- metadata
## Event

- title (required)
- slug (required)
- summary
- date_value
- date_precision
- sequence_index
- location_id
- war_id
- war_day_id
- status
- metadata
## Location

- name (required)
- slug (required)
- alternate_names[]
- summary
- location_type
- latitude
- longitude
- status
- metadata
## Group

- name (required)
- slug (required)
- alternate_names[]
- summary
- group_type
- status
- metadata
## Relationship

- source_entity_type (required)
- source_entity_id (required)
- target_entity_type (required)
- target_entity_id (required)
- relationship_type (required)
- directionality (required)
- summary
- status
- claim_id
- metadata
## FamilyRelationship

- source_character_id (required)
- target_character_id (required)
- relationship_type (required)
- directionality (required)
- summary
- status
- claim_id
## War

- name (required)
- slug (required)
- summary
- start_date_value
- start_date_precision
- end_date_value
- end_date_precision
- status
- metadata
## WarDay

- war_id (required)
- day_number (required)
- title
- summary
- date_value
- date_precision
- status
- metadata
## Formation

- name (required)
- slug (required)
- summary
- event_id
- description
- visualization_status
- claim_id
- metadata
## Source

- title (required)
- author
- source_type
- publication_info
- identifier
- url
- summary
- status
- metadata
## Claim

- subject_entity_type (required)
- subject_entity_id (required)
- claim_text (required)
- claim_type
- certainty
- status
- metadata
## Evidence

- claim_id (required)
- source_id (required)
- locator
- excerpt
- evidence_type
- assessment
- metadata
# 12. Data Integrity Rules

1. Never fabricate missing Mahābhārata information.
1. Never infer a relationship merely because two entities appear together or nearby.
1. Never invent exact dates, coordinates, sequence, troop positions, statistics, or causal relationships when unsupported.
1. Preserve conflicting accounts rather than silently overwriting them.
1. Distinguish known, unknown, not researched, not applicable, conflicting and approximate states.
1. If a location lacks coordinates, show it and indicate map placement is unavailable.
1. If an event lacks an exact date, use supported relative/approximate positioning.
1. If a source has no URL, show the source reference rather than a broken button.
1. If a character lacks a portrait, use a neutral placeholder rather than an AI-generated face.
1. If formation visualization is unreliable, show the textual information and state that visualization is unavailable.
1. Settings may alter presentation but cannot alter facts, provenance, relationships, uncertainty or canonical data.
# 13. Edge States, Performance & Accessibility

## 13.1 Required states

- Loading
- Empty
- Error
- Partial data
- No search results
- Unavailable source
- Unsupported visualization
- Mobile fallback
## 13.2 Performance

Application shell → Primary content → Secondary relationships → Heavy visualizations

- Lazy-load images, maps, complex diagrams and large relationship graphs when appropriate.
- Timeline uses level-of-detail rendering.
- War loads relevant day detail progressively.
- Relationship graph shows a limited overview and expands on focus/zoom.
- Map uses layers, clustering and contextual filtering.
## 13.3 Accessibility

- Keyboard navigation
- Screen-reader compatibility
- Visible focus
- Semantic controls
- Adequate contrast
- Reduced motion
- Responsive text
- Meaning not communicated by color alone
# 14. V1 Seed Dataset & Build-Test Strategy

The first dataset is deliberately representative, not comprehensive. Its purpose is to exercise the architecture before the knowledge base becomes large.

- Character ↔ Character relationships
- Character ↔ Event participation
- Event ↔ Location
- Event ↔ WarDay ↔ War
- Formation where reliable data exists
- Claim ↔ Evidence ↔ Source
- Search
- Cross-context navigation
- Partial and uncertain data states where genuinely supported
SMALL REPRESENTATIVE DATASET → BUILD → TEST → ARCHITECTURE CORRECTION → EXPAND DATASET

# 15. Base44 Implementation Blueprint

1. DATA FOUNDATION
2. KNOWLEDGE GRAPH
3. SHARED COMPONENT SYSTEM
4. EXPLORATION EXPERIENCES
5. EVIDENCE / SOURCE LAYER
6. SEARCH + FOCUS
7. RESPONSIVE + ACCESSIBILITY
8. REPRESENTATIVE DATASET
9. TESTING
10. DATA EXPANSION

Base44's role is to implement and render the structured system defined above. It should not become the research authority.

## 15.1 Shared entity detail

Entity Header → Summary → Key Information → Relationships → Timeline → Map → Evidence → Related Items

Not every entity needs every section; sections are data-dependent.

# 16. Scope Discipline

## 16.1 V1

- Beautiful, responsive, deeply interconnected Mahābhārata knowledge explorer.
- Navigate through time, people, events, places, relationships and the Kurukshetra War.
- Inspect sources behind information.
- One Knowledge Graph and shared data models.
- Timeline, Map, Explore, War and Search.
- Evidence architecture.
- Representative dataset first.
## 16.2 Later / V2 candidates

- Journey as a first-class entity if future requirements justify it.
- DatasetVersion
- EditorialNote
- VariantAccount
- More advanced scholarly/editorial workflows.
Feature discipline: an amazing idea belongs in V2 when it does not materially strengthen the first coherent product.

# 17. Master Design & Engineering Principles

- Modern first, traditional second.
- Information is the hero.
- Feel distinctly Mahābhārata/Indic without becoming stereotypically 'ancient'.
- No excessive warmth, parchment, brown, gold or ornamental clutter.
- Responsive is fundamental, not desktop adaptation.
- One knowledge graph, many exploration lenses.
- Reuse systems instead of duplicating them.
- Data before decoration.
- Evidence before certainty.
- Never fabricate missing information.
- Preserve uncertainty and source differences.
- Progressive disclosure keeps complexity manageable.
- Accessibility is foundational.
- The user controls presentation, not facts.
- Build architecture first, thematic assets second.
# 18. Current Project Status

# 19. Project Boundary Going Forward

The biggest identified risk is feature proliferation. New concepts should be introduced only when they solve a concrete implementation problem.

ARCHITECT → AUDIT → LOCK → MOVE ON → BUILD → TEST → IMPROVE

The goal is the Knowledge Universe—not an infinite document describing how to build it.

# 20. Immediate Next Step

Begin the actual Base44 implementation sequence using this document as the master specification. The first implementation milestone should establish the core data entities and relationships, load a small representative dataset, and verify that the shared graph can power the first exploration experience.

MASTER BLUEPRINT
↓
BASE44 DATA FOUNDATION
↓
REPRESENTATIVE DATA
↓
FIRST WORKING EXPERIENCE
↓
TEST + REFINE
↓
EXPAND

# Appendix A — Source Basis & Scope Note

The principal architectural source is the project's Mahābhārata Explorer Phase 2 Consolidation & Architecture Audit (Complete 133-Point Audit), which establishes the five foundational layers, one-graph architecture, reusable systems, evidence model, responsive philosophy, visual principles, data-quality rules, and representative-dataset strategy.

The document also incorporates the project's established visual-language direction and the subsequent 2.1.3–2.1.5 decisions developed in this project thread. Where a schema field or implementation detail was decided during the later working phases rather than stated verbatim in the original audit, it is treated as the current working implementation decision.

| Question | Answer |
| --- | --- |
| What is it? | A modern digital atlas / knowledge explorer for the Mahābhārata. |
| Core differentiator | One connected knowledge world explored through multiple lenses. |
| Primary activities | Discover, explore, connect, zoom into context, verify through sources, return and continue. |
| Core navigation | Explore • Timeline • Map • War • Search |
| Core data model | One Knowledge Graph with shared entities and first-class relationships. |
| Evidence model | Claim → Evidence → Source, available throughout the product. |
| Visual philosophy | Modern first, traditional second; cultural enrichment rather than decoration. |
| V1 strategy | Representative connected dataset first; test the architecture; then expand. |
| Implementation | Base44. |

| Layer | Meaning | Examples |
| --- | --- | --- |
| Data | What exists. | Characters, Events, Locations, Relationships, War. |
| Evidence | Why it is represented. | Claims, Evidence, Sources, provenance. |
| Exploration | How users navigate it. | Timeline, Map, Explore, War, Search, Focus. |
| Presentation | How it looks and behaves. | Typography, colors, cards, panels, diagrams, motion, responsive layouts. |
| User State | What the user is doing or prefers. | Focus, selected day, filters, query, theme, density, text size, motion. |

| Lens | Purpose |
| --- | --- |
| Time | People, events, chronology, War. |
| Space | Locations, Map, journeys. |
| People | Characters and Groups. |
| Relationships | Family, social and conflict connections. |
| Events | Chronology, War, consequences. |
| Evidence | Sources, Claims, evidence and variants. |

| Item | Role |
| --- | --- |
| Explore | General entry into people, events, locations and relationships. |
| Timeline | Chronological exploration. |
| Map | Geographic exploration. |
| War | Kurukshetra War exploration. |
| Search | Global discovery across entity types. |

| Desktop | Mobile adaptation |
| --- | --- |
| Persistent navigation rail where justified | Compact top navigation plus contextual bottom/action navigation. |
| Side panel / drawer | Bottom sheet where appropriate. |
| Horizontal timeline | Mobile-adapted timeline. |
| Persistent filters | Filter button + sheet. |

| Category | Components |
| --- | --- |
| Navigation | Global Header, Desktop Navigation, Mobile Navigation, Breadcrumbs, Context Navigation |
| Content | Entity Card, Event Card, Character Card, Location Card, Source Card, Summary Card |
| Interaction | Button, Icon Button, Segmented Control, Tabs, Filter, Search, Dropdown, Toggle |
| Overlays | Drawer, Bottom Sheet, Modal, Tooltip, Popover, Toast |
| Visualization | Timeline, Timeline Marker, Relationship Node, Relationship Edge, Map Marker, Journey Path, Formation Diagram |
| States | Loading, Empty, Error, No Results, Partial Data |

| Entity | Meaning |
| --- | --- |
| Character | Person represented in the knowledge base. |
| Event | Meaningful occurrence in the narrative/world. |
| Location | Geographic or narrative place. |
| Group | Collective such as clan, kingdom, army, etc. |
| Relationship | General first-class graph connection. |
| FamilyRelationship | Specialized familial connection. |
| War | Major conflict. |
| WarDay | Structured subdivision of a War. |
| Formation | Battlefield formation where supported by data. |
| Source | Identifiable information source. |
| Claim | Specific assertion about an entity or relationship. |
| Evidence | Evidence supporting, qualifying, or contradicting a Claim. |

| Concept | Question |
| --- | --- |
| Proposition | What is being stated semantically? |
| Claim | What proposition is being epistemically managed/evaluated? |
| Assertion | What structured content represents the claim? |
| Evidence | What supports, qualifies, or contradicts the claim? |
| Assessment | How has the claim/evidence been evaluated? |
| Epistemic status | What is the current governed state? |

| Build gate | Question |
| --- | --- |
| Graph integrity | Can the application traverse the same underlying graph across entity types? |
| Evidence | Can important assertions be traced to evidence and source? |
| Data integrity | Does the UI show only supported structured data? |
| Responsive | Does the experience remain coherent on desktop and mobile? |
| Reusability | Can new records use existing components without page-specific systems? |

| Area | Status |
| --- | --- |
| Product identity | LOCKED |
| Five-layer architecture | LOCKED |
| Knowledge Graph architecture | LOCKED |
| Epistemic architecture (2.1.3) | CLOSED |
| Entity model (2.1.4) | CLOSED |
| Minimum schema | LOCKED |
| Relationship & Graph Contract | LOCKED |
| Build specification (2.1.5) | CLOSED |
| Representative-data strategy | LOCKED |
| Actual Base44 implementation | NEXT |
