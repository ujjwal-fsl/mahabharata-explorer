MahIbhIrata Explorer • Phase 2 Audit • Page 1
MahIbhIrata Explorer
Phase 2 — Consolidation & Architecture Audit
Complete 133-Point Audit
PHASE 2 — CONSOLIDATION & ARCHITECTURE AUDIT
The purpose of this phase
We're going to answer six questions:
1. What have we actually decided?
2. Which things are shared systems rather than individual features?
3. Where are we accidentally designing the same thing twice?
4. Are any decisions conflicting?
5. What belongs in V1, V2, and later?
6. What exact architecture should Base44 be instructed to build?
The goal is to emerge with:
One coherent Master Blueprint.
1. THE BIGGEST ARCHITECTURAL DISCOVERY
Looking back at everything we've designed, I think the project is actually composed of five layers, not simply a
collection of pages.
Data
Evidence
Presentation
Exploration
User State
2. LAYER 1 — KNOWLEDGE / DATA
This is the actual world we're representing.
Core entities:
Characters
Events
Locations
Groups
Relationships
Family Relationships
Wars
War Days
Formations
Sources
Claims
Evidence
The most important conclusion:

MahIbhIrata Explorer • Phase 2 Audit • Page 2
These are entities in one connected knowledge graph.
3. LAYER 2 — EVIDENCE
This is what we established in Phase 2.11.
Claim → Evidence → Source
Evidence isn't a separate "Sources page."
It's a cross-cutting layer underneath the entire product.
4. LAYER 3 — EXPLORATION
This is how users navigate the knowledge graph.
Timeline
Character Explorer
Event Explorer
Relationship Explorer
Family Tree
Map
War Explorer
Search
These should not each invent independent interaction systems.
5. LAYER 4 — PRESENTATION
This is the visual layer:
Typography
Colors
Cards
Panels
Diagrams
Animation
Responsive layouts
Mahabharata-inspired assets
And this is where our "Modern first, traditional second" principle lives.
6. LAYER 5 — USER STATE
Examples:
Focus
Selected Day
Selected Event
Active Filters
Search Query
Theme
Density
Text Size
Motion
Visual Character
This distinction is extremely important.

MahIbhIrata Explorer • Phase 2 Audit • Page 3
7. THE MOST IMPORTANT CONSOLIDATION
I would now formally define:
Data = What exists.
Evidence = Why it is represented.
Exploration = How the user navigates it.
Presentation = How it looks.
State = What the user is currently doing/prefering.
8. THE MASTER KNOWLEDGE GRAPH
The application should fundamentally behave as one connected knowledge graph:
Source
↓
Evidence
↓
Claim
↓
Entity
Entities connect as:
Character
Event
Location
Group
Relationship
War
War Day
Formation
Those are exposed through:
Timeline
Map
Explorers
Focus
Search
9. DUPLICATION AUDIT
This is where I see some potentially dangerous duplication.
Character Journey and War Journey should not be two systems.
Character Journey = general journey.
War Journey = filtered/contextual Character Journey.
10. WAR EVENTS
We have Events and War Events.
There should be only one Event entity.
A War Event is simply:
Event + war context + war day context, where applicable.
11. MAPS

MahIbhIrata Explorer • Phase 2 Audit • Page 4
Main Map
War Map
Character Journey Map
Location Map
These shouldn't be separate map engines.
They should be one Map system with different datasets/layers/context.
12. TIMELINES
Main Timeline
War Timeline
Character Journey Timeline
Day Timeline
These should share one timeline visualization architecture.
13. SOURCES
Event sources
Character sources
War sources
Formation sources
Map sources
These are not separate source systems.
They all connect to Source → Evidence → Claim.
14. FOCUS
Character Focus
Event Focus
War Focus
Map Focus
These aren't different features.
They are one global Focus system operating on different entity types.
15. SEARCH
We don't want independent Character Search, Event Search, War Search, Source Search, and Location Search.
Instead:
Global Search with entity type, context, ranking, and filters.
16. MASTER REUSABLE SYSTEMS
1. Navigation System
2. Timeline System
3. Map System
4. Entity System
5. Relationship System
6. Event System
7. Search System
8. Focus System

MahIbhIrata Explorer • Phase 2 Audit • Page 5
9. Evidence System
10. Source System
11. Filtering System
12. Settings/Preferences System
13. Responsive Layout System
14. Design System
17. DESIGN SYSTEM AUDIT
We need to prevent every phase from inventing its own card, button, panel, modal, drawer, badge, filter, timeline
marker, etc.
We need a shared component vocabulary.
18. CORE UI COMPONENTS
Navigation:
Global Header
Desktop Navigation
Mobile Navigation
Breadcrumbs
Context Navigation
Content:
Entity Card
Event Card
Character Card
Location Card
Source Card
Summary Card
Interaction:
Button
Icon Button
Segmented Control
Tabs
Filter
Search
Dropdown
Toggle
Overlays:
Drawer
Bottom Sheet
Modal
Tooltip
Popover
Toast
Data visualization:
Timeline
Timeline Marker
Relationship Node
Relationship Edge

MahIbhIrata Explorer • Phase 2 Audit • Page 6
Map Marker
Journey Path
Formation Diagram
States:
Loading
Empty
Error
No Results
Partial Data
19. THE CARD PROBLEM
Avoid card explosion.
Use a Base Entity Card with variants:
compact
standard
featured
horizontal
and flexible content slots.
20. DRAWERS
Don't build separate Character Drawer, Event Drawer, Source Drawer, Location Drawer, War Drawer.
Build a reusable Context Drawer, with a Context Bottom Sheet on mobile.
21. DETAIL PAGES
Use a shared Entity Detail Layout.
Potential sections:
Entity Header
Summary
Key Information
Relationships
Timeline
Map
Evidence
Related Items
Not every entity needs every section.
22. INFORMATION ARCHITECTURE AUDIT
The overall information architecture:
Home / Entry
Timeline
Explore
Characters
Events
Locations
Relationships

MahIbhIrata Explorer • Phase 2 Audit • Page 7
Family Tree
Map
War Explorer
Search
23. ENTITY DETAIL
Every entity detail can connect to:
Related Events
Related People
Related Places
Timeline
Map
Evidence
24. USER-FACING NAVIGATION
Do not expose our internal database architecture as the primary navigation.
The user should not have to choose between:
Sources
Claims
Evidence
Relationships
War Days
etc.
Navigation should be organized around exploration tasks.
25. MAIN NAVIGATION
A cleaner conceptual navigation is:
Explore
Timeline
Map
War
Search
Characters, Events, Relationships, and Sources are discoverable within the explorers.
26. DESKTOP NAVIGATION
Use a persistent navigation rail where information density justifies it, but don't force it onto every screen.
27. MOBILE NAVIGATION
Do not reproduce the desktop rail on mobile.
Use compact top navigation plus contextual bottom/action navigation where useful.
28. RESPONSIVE AUDIT
Responsive is a hard project requirement.
The application must work beautifully in:
Desktop landscape

MahIbhIrata Explorer • Phase 2 Audit • Page 8
Tablet landscape
Tablet portrait
Mobile portrait
Mobile landscape
Responsive means the interaction model adapts, not merely that columns become narrower.
29. DESKTOP VS MOBILE
Examples:
Desktop side panel → Mobile bottom sheet.
Desktop horizontal timeline → Mobile-adapted timeline.
Desktop persistent filters → Mobile filter button + sheet.
Desktop rail → Mobile navigation.
30. VISUAL SYSTEM AUDIT
Locked visual principles:
Modern first.
Traditional second.
Flat / dimensional hybrid.
No excessive parchment.
No brown-heavy palette.
No generic "ancient India" aesthetic.
No excessive gold.
No temple/fantasy-game UI.
Indic details as accents.
31. MAHABHARATA ASSET AUDIT
The asset strategy should be:
Generic Component
↓
Theme/Asset Slot
↓
Mahabharata Asset
This allows development to start before the asset hunt is complete.
32. ASSET SLOTS
An Asset Slot can conceptually contain:
component
purpose
default asset
thematic asset
fallback
Example:
Loading Indicator → Generic spinner → Sudarshan Chakra → Static reduced-motion fallback.
33. EVIDENCE AUDIT

MahIbhIrata Explorer • Phase 2 Audit • Page 9
Phase 2.11 is a core V1 architecture even if some evidence UI is initially minimal.
34. SOURCE EXPANSION
The original PDF is not the complete source universe.
Initial Dataset + Additional Credible Sources + Future Source Expansion
→ Unified Knowledge Base
35. CREDIBILITY RULE
New information should enter the knowledge base only through identifiable, credible sources, with provenance
retained.
The workflow is:
Research → verify → source → structure → evidence → database.
36. BASE44 SHOULD NOT INVENT THE KNOWLEDGE BASE
The eventual Base44 prompt should explicitly say:
Build the application architecture and data model to consume the supplied structured dataset. Do not fabricate
missing Mahabharata information.
37. AI-GENERATED CONTENT
Base44 should not fill empty fields with plausible-looking content.
If data is missing:
show an appropriate empty/unknown state.
38. DATA COMPLETENESS
Distinguish:
Known
Unknown
Not yet researched
Not applicable
Conflicting
Approximate
A blank field should never automatically mean "unknown."
39. EDGE STATE AUDIT
Every major component needs:
Loading
Empty
Error
Partial data
No search results
Unavailable source
Unsupported visualization
Mobile fallback
40. MAP EDGE CASE

MahIbhIrata Explorer • Phase 2 Audit • Page 10
If a location has no coordinates:
do not crash and do not invent coordinates.
Show that location information exists but map placement is unavailable.
41. TIMELINE EDGE CASE
If an event has no exact date:
do not fabricate a timestamp.
Use relative sequence or approximate position.
42. SOURCE EDGE CASE
If a source has no online URL:
show that a source reference is available rather than a broken Open Source button.
43. CHARACTER EDGE CASE
If no portrait exists:
use an elegant neutral placeholder, not an AI-generated face.
44. FORMATION EDGE CASE
If textual information exists but reliable visualization is not possible:
show the textual description and indicate that visualization is unavailable.
45. PERFORMANCE AUDIT
The project can become heavy because of:
large event sets
relationships
maps
images
timelines
diagrams
sources
animations
We should not load everything at once.
46. PROGRESSIVE LOADING
Load:
Application shell
↓
Primary content
↓
Secondary relationships
↓
Heavy visualizations
47. LAZY LOADING
Especially lazy-load when appropriate:
images
maps

MahIbhIrata Explorer • Phase 2 Audit • Page 11
complex diagrams
large relationship graphs
48. TIMELINE PERFORMANCE
Use level-of-detail rendering.
At broad zoom:
aggregated markers.
At close zoom:
individual events.
49. WAR PERFORMANCE
Do not load every day's full event structures if the user is viewing one day.
Load relevant detail progressively.
50. RELATIONSHIP GRAPH PERFORMANCE
At overview:
show major relationships / limited nodes.
On zoom or focus:
expand.
51. MAP PERFORMANCE
Do not render every possible marker at once.
Use layers, clustering, and contextual filtering.
52. ACCESSIBILITY AUDIT
Accessibility is foundational, not a later QA pass.
The application must support:
keyboard navigation
screen readers
visible focus
semantic controls
contrast
reduced motion
responsive text
non-color-only meaning
53. DESIGN TOKENS
Establish a central Design Token System containing:
Colors
Typography
Spacing
Radius
Shadows
Borders
Motion

MahIbhIrata Explorer • Phase 2 Audit • Page 12
Breakpoints
Z-index
54. TOKENIZED COMPONENTS
Every component should use shared tokens instead of hardcoded one-off values.
55. COLOR SYSTEM
Use semantic tokens:
background
surface
surface-elevated
text-primary
text-secondary
border
accent
success
warning
error
info
56. MAHABHARATA PALETTE
The thematic palette should also be tokenized.
Theme Layer
↓
Semantic Color Tokens
↓
Components
57. TYPOGRAPHY SYSTEM
Centralize:
display
heading
subheading
body
caption
metadata
labels
58. SPACING SYSTEM
Use a consistent spacing scale.
59. RADIUS SYSTEM
Use a consistent radius system so the interface feels like one product.
60. MOTION TOKENS
Define a small set of motion tokens such as:
fast

MahIbhIrata Explorer • Phase 2 Audit • Page 13
standard
slow
reduced-motion
61. RESPONSIVE BREAKPOINT SYSTEM
Use centralized breakpoints.
Prefer content-driven breakpoints over rigid device labels where practical.
62. NAVIGATION AUDIT
Every screen should answer:
Where did I come from?
Where am I?
Where can I go?
How do I return?
63. BREADCRUMBS
Deep contexts should support breadcrumbs, for example:
Mahabharata → War → Day 13 → Event
On mobile this can collapse to a contextual back label.
64. FOCUS AUDIT
Focus is powerful but can become confusing if multiple states are active.
Example:
Focus: Arjuna
Filter: Day 13
Search: Karna
Selected Event: X
The UI needs a clear active-state hierarchy.
65. STATE HIERARCHY
Global:
Focus
Page:
Selected tab / day / view
Local:
Selected event / node / marker
Temporary:
Search / filter / drawer
66. STATE PERSISTENCE RULE
Selecting a day should not silently clear a character focus unless explicitly intended.
Opening a source drawer should not clear the selected event.
67. URL / STATE SYNCHRONIZATION

MahIbhIrata Explorer • Phase 2 Audit • Page 14
Major navigational states should be representable in URLs where practical.
Examples:
character/arjuna
war/kurukshetra/day/13
This supports sharing, bookmarks, browser history, and reload persistence.
68. NOT EVERY STATE BELONGS IN URL
Do not encode transient states such as hover or purely visual drawer state unless there is a strong product reason.
69. SHAREABILITY
Users should eventually be able to share:
a character
an event
a specific day
a location
a war view
70. SEARCH → DEEP LINK
Search result for Arjuna should open the character's canonical detail route, not merely a temporary modal.
71. SOURCE → DEEP LINK
Source detail should eventually be directly addressable.
72. RESPONSIVE DEEP LINKS
The same canonical URL should work across desktop and mobile.
73. SECURITY / DATA INTEGRITY
Core rules:
Frontend must not be the source of truth.
Validation belongs at the data layer.
User-editable settings must be validated.
Source metadata must not be silently modified.
Generated/AI content must be distinguished from source-derived content.
74. USER EDITING
V1 recommendation:
No public editing.
The knowledge base is curated.
75. WHY NO PUBLIC EDITING
Public editing would require:
moderation
revision history
contributor accounts
conflict resolution
vandalism prevention

MahIbhIrata Explorer • Phase 2 Audit • Page 15
source validation
That is effectively a different product.
76. ADMIN / EDITOR SYSTEM
Eventually build a private editorial layer for:
add source
add character
edit event
attach evidence
create relationship
review changes
publish dataset
77. PUBLIC VS EDITORIAL DATA
Conceptually:
Editorial Database
↓
Validation
↓
Published Knowledge Base
↓
Public Explorer
78. V1 EDITORIAL WORKFLOW
A full CMS is not required for V1, but the data structures should be importable/editable by the project owner.
79. V1 / V2 / V3 AUDIT
Strict prioritization is necessary because the project can easily become enormous.
80. V1 — MUST HAVE
Core experience:
responsive application shell
Timeline
Characters
Events
Locations
Basic Relationships
Map
War Explorer
Search
Focus
Source/Evidence foundation
Settings foundation
81. V1 TIMELINE
Must have:
zoom/detail levels

MahIbhIrata Explorer • Phase 2 Audit • Page 16
chronological navigation
event cards
event detail
contextual navigation
responsive behavior
loading/empty states
82. V1 CHARACTER SYSTEM
Must have:
character profile
relationships
timeline presence
event connections
source information
search
focus
83. V1 EVENT SYSTEM
Must have:
event detail
chronological position
participants
locations
related events
sources
timeline connection
84. V1 MAP
Must have:
locations
basic markers
location detail
event connection
responsive interaction
Basic journey path may be included if the dataset supports it cleanly.
85. V1 WAR
Must have:
war overview
day navigation
day events
participants
event links
timeline connection
map connection
source connection
Formation visualization is conditional on data readiness.

MahIbhIrata Explorer • Phase 2 Audit • Page 17
86. V1 SEARCH
Must have:
global search
entity type recognition
relevant results
deep links
empty states
mobile search
87. V1 EVIDENCE
Must have:
Source entity
source metadata
evidence relationship
contextual source drawer
source detail
claim-level provenance where needed
uncertainty/status architecture
88. V1 SETTINGS
Must have:
theme
text size
density
motion
visual character
responsive behavior
preference persistence
Some advanced settings can remain architectural placeholders.
89. V1 DESIGN
Must have:
central design tokens
responsive component system
typography system
semantic colors
reusable cards
reusable drawers/sheets
loading states
empty states
error states
accessibility foundation
90. V2
Potential V2:
Progressive chronology / spoiler mode
Advanced Formation Explorer
Advanced relationship visualization

MahIbhIrata Explorer • Phase 2 Audit • Page 18
Character journey playback
Source comparison
Variant account comparison
Advanced research search
More sophisticated map layers
Richer analytics
Saved explorations
Bookmarks
Notes
Multilingual content
91. V3 / LONG-TERM
Potentially:
Research Mode
AI grounded Q&A
Source passage exploration
Advanced textual comparison
Contributor/editorial workflows
Dataset version history
Collaborative research
Advanced visualization
Public API
92. CRITICAL V1 EXCLUSION: AI Q&A;
Do not build AI Q&A into V1.
First:
structured knowledge
then:
evidence
then:
grounded AI
93. CRITICAL V1 EXCLUSION: 3D WORLD
Do not build a giant 3D Mahabharata world.
The product is a knowledge explorer, not an open-world game.
94. CRITICAL V1 EXCLUSION: BATTLE SIMULATION
Do not build animated battle simulations that imply speculative battlefield movement.
95. CRITICAL V1 EXCLUSION: STATIC ENCYCLOPEDIA
Do not turn the product into a giant encyclopedia page system.
The core value is connected exploration.
96. THE REAL PRODUCT DIFFERENTIATOR
The Mahabharata is represented as a connected, explorable knowledge graph across time, people, places, events,
relationships, and sources.

MahIbhIrata Explorer • Phase 2 Audit • Page 19
The UI is the mechanism through which users explore that graph.
97. THE SIX PRIMARY LENSES
TIME
People / Events / War
SPACE
Locations / Map / Journeys
PEOPLE
Characters / Groups
RELATIONSHIPS
Family / Social / Conflict
EVENTS
Chronology / War / Consequences
EVIDENCE
Sources / Claims / Variants
Search and Focus operate across all six.
98. HOMEPAGE PHILOSOPHY
The experience should communicate:
Explore the Mahabharata through time, people, places, events and relationships.
99. NO DEAD ENDS RULE
Whenever the user encounters an entity, meaningful paths to related information should be available.
Character → Events / Timeline / Relationships / Family / War / Sources
Event → People / Location / Timeline / War / Sources
Location → Events / People / Map / Sources
Source → Claims / Entities / References
100. NO FALSE CONNECTIONS RULE
Only show relationships explicitly represented by the data.
Proximity on a timeline or map does not automatically imply a relationship.
101. NO FALSE PRECISION RULE
Do not invent:
exact dates
exact coordinates
exact sequence
troop positions
numerical statistics
causal relationships
when the data doesn't establish them.
102. NO FALSE AUTHORITY RULE

MahIbhIrata Explorer • Phase 2 Audit • Page 20
Do not make:
an illustration look like a primary-source diagram
an editorial interpretation look like a quotation
an AI summary look like a source
a disputed account look universally settled
103. NO VISUAL OVERLOAD RULE
Complexity should exist in the data architecture, not necessarily in the first screen.
Progressive disclosure is essential.
104. FIRST-TIME USER EXPERIENCE
A first-time visitor should quickly understand:
What is this? A Mahabharata explorer.
What can I do? Explore people, events, places and chronology.
How do I go deeper? Click anything.
Can I verify information? Yes, sources are attached throughout.
105. PRODUCT UX LOOP
DISCOVER
↓
EXPLORE
↓
ZOOM
↓
CONNECT
↓
VERIFY
↓
RETURN
↓
DISCOVER SOMETHING ELSE
106. EXAMPLE EXPLORATION LOOP
Search "Karna"
↓
Character Profile
↓
War Journey
↓
Day 17
↓
Event
↓
Location
↓
Map
↓
Source
↓

MahIbhIrata Explorer • Phase 2 Audit • Page 21
Related Character
↓
Character Profile
107. AUDIT CONCLUSION
The product has a clear identity, coherent information architecture, identifiable shared systems, strong evidence
architecture, responsive philosophy, and a consistent visual identity.
The main areas requiring formalization are:
data architecture
V1 scope
source ingestion workflow
108. BIGGEST RISK
The biggest risk is feature proliferation.
Almost every idea is genuinely cool, which makes the project particularly vulnerable to expanding beyond its core
objective.
109. FEATURE DISCIPLINE
Use:
Amazing idea → V2
when an idea does not materially strengthen the first coherent product.
110. THE PRODUCT WE SHOULD BUILD FIRST
A beautiful, responsive, deeply interconnected Mahabharata knowledge explorer where users can navigate
through time, people, events, places, relationships and the Kurukshetra War—and inspect the sources behind the
information.
111. MASTER ARCHITECTURE
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
112. FIVE FOUNDATIONAL LAYERS
Data → Evidence → Exploration → Presentation → User State

MahIbhIrata Explorer • Phase 2 Audit • Page 22
113. ARCHITECTURAL LOCK
One Knowledge Graph
Shared Data Models
Shared Component System
Multiple Exploration Lenses
Global Focus + Search
Cross-Context Navigation
Evidence / Sources
Personalized Presentation
114. DATA MODEL — MASTER VERSION
Core entities:
Character
Event
Location
Group
Relationship
FamilyRelationship
War
WarDay
Formation
Source
Claim
Evidence
Potential future:
Journey
DatasetVersion
EditorialNote
VariantAccount
115. RELATIONSHIP MODEL
Relationships should be first-class.
Examples:
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
116. NO HARD-CODED RELATIONSHIPS
Relationships should be represented as data wherever possible, not hardcoded into page logic.

MahIbhIrata Explorer • Phase 2 Audit • Page 23
117. DATA-DRIVEN UI
The frontend should render based on available structured data.
If formation is null, do not render an empty Formation section.
If sources are empty, do not render a broken source drawer.
118. FUTURE-PROOFING
If new records are added, the application should not need redesign.
The data grows; the visualization system remains stable.
119. SOURCE EXPANSION FIT
The architecture supports:
original PDF dataset
+
Source A
+
Source B
+
Source C
+
future credible sources
→
one knowledge base
120. RESEARCH WORKFLOW
Question
↓
Source Discovery
↓
Source Quality Check
↓
Extraction
↓
Cross-check
↓
Structuring
↓
Claim
↓
Evidence
↓
Database
121. RESEARCH WORKFLOW OUTSIDE BASE44
Initially, research and source verification should remain a deliberate workflow between the project owner and
research process.
Base44's job is to render and manage the structured knowledge system, not autonomously research the
Mahabharata.

MahIbhIrata Explorer • Phase 2 Audit • Page 24
122. DATA QUALITY GATE
Before information enters the published dataset:
Source identified?
↓
Credible?
↓
Specific reference?
↓
Claim extracted?
↓
Conflicting accounts checked?
↓
Structured?
↓
Published
123. MASTER DESIGN PRINCIPLES
01 — Modern first, traditional second.
02 — Information is the hero.
03 — The interface should feel Indian without becoming stereotypically "ancient."
04 — No excessive warmth, parchment, brown, gold, or ornamental clutter.
05 — Responsive is fundamental, not a desktop adaptation.
06 — One knowledge graph, many exploration lenses.
07 — Reuse systems instead of duplicating them.
08 — Data before decoration.
09 — Evidence before certainty.
10 — Never fabricate missing information.
11 — Preserve uncertainty and source differences.
12 — Progressive disclosure keeps complexity manageable.
13 — Accessibility is foundational.
14 — The user controls presentation, not facts.
15 — Build architecture first, thematic assets second.
124. THE REAL PRODUCT DIFFERENTIATOR — REFINED
The product is not primarily the artwork, not primarily the timeline, and not primarily the family tree.
Its differentiator is:
ONE WORLD
↓
MANY LENSES
↓
CONNECTED EXPLORATION
125. USER EXPERIENCE AS A CONNECTED GRAPH
The application should feel like a world in which any meaningful object can lead to another meaningful object
without dead ends.
126. EVIDENCE AS A CROSS-CUTTING SYSTEM
Evidence should be available in context rather than isolated on a bibliography page.

MahIbhIrata Explorer • Phase 2 Audit • Page 25
127. SETTINGS AS PRESENTATION CONTROL
Settings customize the experience, not the truth.
They can change:
appearance
density
text size
motion
visual character
chronological reveal
They cannot change:
source provenance
factual relationships
event ordering
geographic evidence
uncertainty
canonical data
128. ASSET STRATEGY
Build generic components first.
Then progressively replace selected visual slots with Mahabharata-inspired assets.
129. RESPONSIVE-FIRST STRATEGY
Every major experience must be designed for desktop and mobile as first-class layouts, including landscape and
portrait orientations.
130. SOURCE STRATEGY
The original PDF is the seed dataset.
It is not assumed to be complete.
Future expansion must use credible, identifiable sources and preserve provenance.
131. V1 STRATEGY
Build a deliberately selected representative subset of the data first, containing enough complexity to exercise:
Timeline
Character
Event
Relationship
Map
War
Evidence
Search
Then expand the knowledge base.
132. WHY REPRESENTATIVE DATA FIRST
If a large dataset exposes a flawed relationship model, fixing it after thousands of records have been loaded is
painful.

MahIbhIrata Explorer • Phase 2 Audit • Page 26
Instead:
Small representative dataset
↓
Build
↓
Test
↓
Architecture correction
↓
Expand dataset
133. FINAL AUDIT VERDICT
The project has moved far beyond the original idea of "make an interactive mind map."
It is now best understood as:
A beautiful, responsive, source-aware, interconnected Mahabharata knowledge explorer.
The original PDF remains the seed.
The product architecture is now:
Data → Evidence → Exploration → Presentation → User State.
The next step should be a formal technical consolidation of the knowledge graph, entity model, relationship model,
claims/evidence model, and information architecture before translating the work into Base44 implementation
instructions.
