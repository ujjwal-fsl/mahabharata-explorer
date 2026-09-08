# Evidence & Provenance UI Architecture (Block F15)

## 1. Document Status & Purpose

```
┌────────────────────────────────────────────────────────────────────────┐
│                          DOCUMENT ATTRIBUTION                          │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Document ID      │ docs/05-frontend/14-evidence-and-provenance-ui-      │
│                  │ architecture.md                                     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ Stage / Block    │ Stage 2 (Frontend Architecture) — Block F15         │
├──────────────────┼─────────────────────────────────────────────────────┤
│ Upstream Sources │ - Stage 1 Block B2: Data Architecture               │
│                  │ - Stage 1 Block B3: Knowledge Graph Architecture    │
│                  │ - Stage 1 Block B4: Evidence & Provenance           │
│                  │ - Stage 1 Block B5: API Architecture                │
│                  │ - Stage 2 Block F1: Frontend Architecture Context   │
│                  │ - Stage 2 Block F2: Technology & Build Architecture │
│                  │ - Stage 2 Block F3: Design System & Visual Grammar  │
│                  │ - Stage 2 Block F4: Layout & Application Shell      │
│                  │ - Stage 2 Block F5: State Management Architecture   │
│                  │ - Stage 2 Block F6: Fetching & Caching Architecture │
│                  │ - Stage 2 Block F7: Routing & Navigation            │
│                  │ - Stage 2 Blocks F8–F14: Search and Explorer Lenses │
├──────────────────┼─────────────────────────────────────────────────────┤
│ Downstream Deps  │ - Stage 2 Block F16: Accessibility Architecture     │
│                  │ - Stage 2 Block F17: Performance & Testing          │
│                  │ - Stage 4: Implementation Phase                     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ Status           │ Approved Architectural Specification                │
└──────────────────┴─────────────────────────────────────────────────────┘
```

This document establishes the frontend **architecture** for displaying, navigating, and inspecting evidence, citations, and provenance throughout the **Mahābhārata Explorer**.

The product's central foundation is **One Knowledge Graph, Many Exploration Lenses**. Grounding this graph in verifiable evidence is fundamental to its integrity: every node, edge, attribute, and temporal event rests on a rigorous chain of provenance connecting propositions to physical and scholarly sources. Block F15 architects the user experience and interface mechanisms that make factual claims traceable without turning the application into an impenetrable citation database UI.

The user must be able to transition fluidly from macro-level exploration into granular provenance inspection:

$$\text{Entity / Graph Edge} \longrightarrow \text{Claim} \longrightarrow \text{Evidence} \longrightarrow \text{Source}$$

This architectural specification defines how the canonical four-tier provenance model ([04-evidence-and-provenance.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/04-evidence-and-provenance.md)) is experienced across all client interfaces, preserving epistemic uncertainty, competing recensions, incomplete curation, and source fidelity under strict zero-fabrication principles.

---

## 2. Scope and Constitutional Principles

### 2.1 Core Architectural Principles
In accordance with the project constitution (`AGENTS.md`) and Stage 1 Block B4:

1. **Zero Fabrication**:
   - The UI must never invent claims, supporting evidence, author names, editions, quotations, page numbers, shloka locators, or certainty ratings.
   - Absence of evidence must be presented honestly and distinctly from lack of research or textual silence. Missing citations must never be populated with placeholder or synthetic academic data.
2. **Block B4 is the Authoritative Source of Truth**:
   - Block F15 provides the **presentation architecture** for the canonical provenance model established in Block B4.
   - F15 does not create a second provenance model, alternative claim schema, or divergent certainty classification.
3. **Six Project-Wide Epistemic States Only**:
   - The UI strictly consumes the established project-wide vocabulary:
     `known`, `unknown`, `not_researched`, `not_applicable`, `conflicting`, `approximate`.
   - Colloquial terms such as "disputed", "legendary", "mythical", or "speculative" must never be introduced as application state identifiers or substitutes for the canonical vocabulary.
4. **Preservation of Conflict & Multi-Claim Parity**:
   - Competing textual traditions, recensions, and variant readings must never be flattened, averaged, or suppressed.
   - The interface must present competing claims with structural parity, preventing accidental visual endorsement of a single variant merely due to rendering order or visual prominence.
5. **Categorical Distinction: Source ≠ Evidence ≠ Claim**:
   - The UI must not collapse the provenance hierarchy into a generic flat "citation" tag.
   - It maintains clear cognitive differentiation between:
     - **What is asserted** (Claim)
     - **Why it is substantiated** (Evidence & Excerpt)
     - **Where it originates** (Source & Critical Edition)
6. **Native Citation Locator Fidelity**:
   - Verbatim native citation strings (e.g., `Ādi Parva 1.216.5–12`, `Vol. 4, p. 142`, `[native locator]`) must be presented faithfully without mutation.
   - Normalized locator fields (`parva_number`, `adhyaya_number`, `shloka_range`) may support grouping, filtering, and ordering without replacing the native locator.
7. **Evidence is Contextual, Not Decorative**:
   - Evidence affordances must serve genuine verification and contextual understanding rather than functioning as ornamental badges.
8. **Progressive Disclosure**:
   - Provenance supports three depth tiers:
     - *Lightweight Affordances*: Ambient indicators and verification badges during general exploration.
     - *Contextual Inspection*: Quick-inspection previews surfacing backing claims and source information.
     - *Exhaustive Provenance Surface*: Full inspection surface detailing verbatim excerpts where supplied, editorial assessments, and complete bibliographic metadata.

---

## 3. Relationship to Stage 1 Backend Architecture

Block F15 consumes the data contracts and architectural rules established in Stage 1 without modification:

```
┌────────────────────────────────────────────────────────────────────────┐
│             BACKEND TO FRONTEND PROVENANCE MAPPING                     │
├────────────────────────────┬───────────────────────────────────────────┤
│ Backend Layer (Stage 1 B4) │ Frontend Presentation Layer (Block F15)   │
├────────────────────────────┼───────────────────────────────────────────┤
│ **Layer 1: Canonical Data**│ **Lens Presentation & Anchors**           │
│ - Node: `Character`, etc.  │ - Entity Header & Fact Badges (F9)        │
│ - Edge: `Relationship`,    │ - Graph Edge Selection & Hit Targets (F10)│
│   `FamilyRelationship`,    │ - Lineage Node/Edge Affordances (F11)     │
│   `EventParticipant`       │ - Timeline Milestone Badges (F12)         │
│                            │ - Tactical Formation & War Day Cards (F14)│
├────────────────────────────┼───────────────────────────────────────────┤
│ **Layer 2: Claim**         │ **Claim Inspection Model**                │
│ - `subject_entity_type`    │ - Subject Anchor Contextual Header        │
│ - `claim_text`             │ - Proposition Typography & Statement      │
│ - `claim_type`             │ - Semantic Domain Tag                     │
│ - `certainty`              │ - Canonical Certainty Indicator           │
│ - `epistemic_status`       │ - Canonical Epistemic Visual Grammar (F3) │
│ - `provenance_origin`      │ - Canonical Provenance Origin Display     │
├────────────────────────────┼───────────────────────────────────────────┤
│ **Layer 3: Evidence**      │ **Evidence & Excerpt Surface**            │
│ - `locator` (Verbatim)     │ - Prominent Native Citation Display       │
│ - `sanskrit_excerpt`       │ - Canonical Sanskrit Excerpt Container    │
│ - `excerpt_translation`    │ - Editorial Translation Block             │
│ - `evidence_type`          │ - Evidence Category Tag                   │
│ - `assessment`             │ - Critical Scholarly Annotation Card      │
│ - Normalized fields        │ - Structured Parva / Adhyaya / Shloka Bar │
├────────────────────────────┼───────────────────────────────────────────┤
│ **Layer 4: Source**        │ **Bibliographic & Edition Card**          │
│ - `title`, `short_title`   │ - Source Title & Header                   │
│ - `author`, `source_type`  │ - Critical Editor / Recension Context     │
│ - `publication_info`       │ - Publication, Volume, City & Year        │
│ - `identifier` (ISBN/DOI)  │ - Standard Academic Identifiers           │
│ - `url` (External Link)    │ - Untrusted Digital Repository Link       │
└────────────────────────────┴───────────────────────────────────────────┘
```

### 3.1 Dual-Link Provenance Consumption
In accordance with B4 §4.2, F15 supports both provenance traversal directions:
1. **Subject Entity Traversal**: Querying all claims where an entity is the subject (`Claim.subject_entity_id = Entity.id`), consumed via B5 `/api/v1/entities/:entity_type/:slug/provenance`.
2. **Edge Traversal**: Querying the specific backing claim referenced directly by a graph edge (`Relationship.claim_id`, `FamilyRelationship.claim_id`, `EventParticipant.claim_id`), consumed via B5 `/api/v1/relationships/:id/evidence` or `/api/v1/claims/:id`.

---

## 4. F15 Ownership and System Boundaries

```
┌────────────────────────────────────────────────────────────────────────┐
│                        F15 OWNERSHIP MATRIX                            │
├───────────────────┬──────────────────┬─────────────────────────────────┤
│ System Area       │ Block Authority  │ Block F15 Responsibility        │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **Provenance Data**│ Block B4         │ Consumes B4 entities without    │
│                   │ (Backend)        │ inventing fields or schemas.    │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **API Contracts** │ Block B5         │ Consumes B5 endpoints strictly; │
│                   │ (Backend)        │ introduces zero new endpoints.  │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **Visual Grammar**│ Block F3         │ Reuses F3 epistemic tokens,     │
│                   │ (Design System)  │ typography, and surface rules.  │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **Shell Layout**  │ Block F4         │ Maps provenance surface into F4 │
│                   │ (Shell/Responsive│ drawers, sheets, and panels.    │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **Client State**  │ Block F5         │ Manages ephemeral UI state; no  │
│                   │ (State Mgmt)     │ competing global stores.        │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **Query & Cache** │ Block F6         │ Delegates all network queries   │
│                   │ (Data Client)    │ and cache keys to F6.           │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **URL & Routing** │ Block F7         │ Identifies deep-linking needs;  │
│                   │ (Router)         │ defers all grammar to F7.       │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **Exploration**   │ Blocks F8–F14    │ Provides reusable provenance    │
│ **Lenses**        │ (Lenses)         │ affordances for lens triggers.  │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **Accessibility** │ Block F16        │ Specifies semantic hierarchy;   │
│                   │ (A11y Authority) │ defers concrete ARIA/focus rules│
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **Performance**   │ Block F17        │ Designs progressive loading;    │
│                   │ (Testing/Perf)   │ defers quantitative budgets.    │
└───────────────────┴──────────────────┴─────────────────────────────────┘
```

---

## 5. Provenance Interaction Model

The provenance interaction model bridges high-level epic exploration and granular textual verification across three progressive stages:

```
┌────────────────────────────────────────────────────────────────────────┐
│                    PROVENANCE INTERACTION STAGES                       │
├────────────────────────────────────────────────────────────────────────┤
│ STAGE 1: AMBIENT DISCOVERY (During Macro-Exploration)                  │
│  - User views entity cards, timeline markers, or graph edges.          │
│  - Subtle, standardized provenance affordances indicate verification   │
│    status and epistemic condition without visual clutter.              │
│  - User immediately perceives whether a statement is known,           │
│    conflicting, approximate, or unresearched.                          │
├────────────────────────────────────────────────────────────────────────┤
│ STAGE 2: CONTEXTUAL INSPECTION (In-Situ Preview)                       │
│  - User hovers, taps, or keyboard-focuses a provenance affordance.     │
│  - An ephemeral contextual popover reveals:                            │
│    - The exact claim proposition.                                      │
│    - Primary native citation string (e.g., "[native locator]").        │
│    - Canonical Certainty level.                                        │
│    - One-click trigger: "Inspect Full Evidence & Sources".             │
├────────────────────────────────────────────────────────────────────────┤
│ STAGE 3: EXHAUSTIVE PROVENANCE SURFACE (Deep Verification)             │
│  - User activates deep inspection.                                     │
│  - The dedicated Provenance Inspection Surface opens:                  │
│    - Preserves underlying lens context without full-page navigation.   │
│    - Surfaces available corroborating or conflicting claims returned   │
│      by canonical provenance data.                                     │
│    - Displays verbatim Sanskrit excerpts where supplied, translations, │
│      and native locators.                                              │
│    - Presents source bibliographic records and methodology context.    │
│    - Provides fluid return to the active exploration canvas.           │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 6. Provenance Affordances Across Lenses

To prevent each lens from inventing ad-hoc citation patterns, Block F15 standardizes three core provenance affordances reusable across all exploration contexts:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   STANDARDIZED PROVENANCE AFFORDANCES                  │
├──────────────────┬──────────────────┬──────────────────────────────────┤
│ Affordance Type  │ Visual Character │ Exploration Context              │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **1. Epistemic   │ Compact tag with │ - Entity Profile Headers (F9)    │
│    Badge**       │ canonical glyph  │ - Timeline Event Cards (F12)     │
│                  │ and text label   │ - Geographic Realm Cards (F13)   │
│                  │ (Block F3 §5)    │ - War Day Overview Headers (F14) │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **2. Citation    │ Interactive pill │ - Entity Fact Attributes (F9)    │
│    Pill**        │ displaying short │ - Graph Edge Inspector (F10)     │
│                  │ native locator   │ - Lineage Kinship Ties (F11)     │
│                  │ (e.g. `[CE 1.2]`)│ - Combat Outcome Notes (F14)     │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **3. Conflict    │ Dual-tone pill   │ - Conflicting Parentage (F9, F11)│
│    Indicator**   │ with forked      │ - Divergent Day Events (F12, F14)│
│                  │ branch glyph     │ - Competing Realm Sites (F13)    │
└──────────────────┴──────────────────┴──────────────────────────────────┘
```

### 6.1 Lens-Specific Affordance Integration

```
┌────────────────────────────────────────────────────────────────────────┐
│                     LENS INTEGRATION SPECIFICATION                     │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Lens / Feature   │ Provenance Affordance Trigger & Context Preservation│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Entity Views** │ - Header displays entity-level epistemic badge.     │
│ (Block F9)       │ - Biographical facts and attributes pair with       │
│                  │   inline Citation Pills.                            │
│                  │ - Clicking an attribute pill opens the Provenance   │
│                  │   Surface focused on that specific claim.           │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Focus Graph**  │ - Relationship edge selection highlights edge line  │
│ (Block F10)      │   and surfaces Citation Pill in edge inspector.     │
│                  │ - Edge touch targets augmented per F10 §11.2.       │
│                  │ - Trigger dispatches Provenance Surface with        │
│                  │   `Relationship.claim_id` context.                  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Family Tree**  │ - Kinship connections display inline epistemic      │
│ (Block F11)      │   markers where parentage/consort claims diverge.   │
│                  │ - Activating marker opens Provenance Surface with   │
│                  │   the relevant `FamilyRelationship.claim_id`.       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Timeline**     │ - Narrative event milestones embed Citation Pills   │
│ (Block F12)      │   indicating Parva / Adhyaya textual foundation.    │
│                  │ - Conflicting chronological placements trigger      │
│                  │   side-by-side claim comparison.                    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Map Lens**     │ - Location detail panel pairs coordinate status     │
│ (Block F13)      │   with evidence pills citing textual geography.     │
│                  │ - Conflicting modern site identifications present   │
│                  │   variant scholarly claims without bias.            │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **War & Vyuha**  │ - Daily operational logs cite specific battle       │
│ (Block F14)      │   episodes via native locators.                     │
│                  │ - Formation cards link to textual evidence          │
│                  │   grounding the three-tier tactical representation. │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Search**       │ - Scholarly search results display matching source  │
│ (Block F8)       │   editions and verbatim locator coordinates.        │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 7. Claim Inspection Architecture

A `Claim` represents a single, discrete propositional assertion regarding an entity, event, or relationship (B4 §2.1).

### 7.1 Information Architecture of a Claim
When inspected, a Claim is structured into five distinct semantic zones:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        CLAIM INSPECTION CARD                           │
├────────────────────────────────────────────────────────────────────────┤
│ 1. SUBJECT CONTEXT & PROPOSITION HEADER                                │
│    - Subject Entity Badge (e.g., "Character: [Entity Name]")           │
│    - Claim Proposition Statement (Clear, unambiguous factual assertion)│
│    - Semantic Domain Tag (e.g., `genealogy`, `combat_outcome`)         │
├────────────────────────────────────────────────────────────────────────┤
│ 2. EPISTEMIC & CERTAINTY STATUS BAR                                    │
│    - Canonical Epistemic State Badge (e.g., `known`, `conflicting`)   │
│    - Canonical Certainty Indicator (e.g., `traditional_consensus`)     │
│    - Canonical Provenance Origin (e.g., `human_source`)                │
├────────────────────────────────────────────────────────────────────────┤
│ 3. CORROBORATING EVIDENCE PASSAGES (1:N)                               │
│    - List of attached Evidence records supporting this proposition     │
│    - Verbatim native locators and source attributions                  │
├────────────────────────────────────────────────────────────────────────┤
│ 4. CRITICAL EDITORIAL ASSESSMENT                                       │
│    - Scholarly commentary regarding manuscript variations or context   │
│      where supplied by canonical data (omitted cleanly if absent)      │
├────────────────────────────────────────────────────────────────────────┤
│ 5. CONTEXTUAL GRAPH ACTIONS                                            │
│    - Link to subject entity profile (`/characters/:slug`)              │
│    - Link to associated narrative event (`/timeline/:slug`)            │
└────────────────────────────────────────────────────────────────────────┘
```

### 7.2 Semantic Classification & Canonical Provenance Origin
In accordance with B4 §8, F15 displays the canonical `provenance_origin` value supplied by B4 and does not define, extend, reinterpret, or rename its taxonomy:
- F15 preserves the foundational distinction between:
  - **`epistemic_status`**: The six project-wide epistemic states (`known`, `unknown`, `not_researched`, `not_applicable`, `conflicting`, `approximate`) governing the objective state of knowledge across the corpus.
  - **`certainty`**: The separate B4 certainty classification (`established`, `traditional_consensus`, `disputed`, `approximate`, `unresolved`) representing scholarly and traditional consensus levels.
  - **`provenance_origin`**: The origin classification supplied directly by the canonical backend.
- Where B4 leaves exact taxonomy details or enum expansions deferred (B4 §8 / Block B10), F15 remains strictly taxonomy-neutral, rendering the canonical origin value faithfully without creating client-side taxonomy tiers.

---

## 8. Evidence Inspection Architecture

`Evidence` directly anchors a `Claim` to a specific passage within a `Source` (B4 §3.3).

### 8.1 Evidence Presentation Structure

```
┌────────────────────────────────────────────────────────────────────────┐
│                       EVIDENCE PASSAGE CONTAINER                       │
├────────────────────────────────────────────────────────────────────────┤
│ 1. NATIVE CITATION LOCATOR (Verbatim)                                  │
│    - Exact citation string as published (e.g., "[native locator]")     │
│    - Non-mutation guarantee: Rendered exactly as recorded in B4.       │
├────────────────────────────────────────────────────────────────────────┤
│ 2. NORMALIZED COORDINATE BAR                                           │
│    - Structured reference tags: Parva Number • Adhyaya • Shloka / Page │
│    - Related context affordance: Explores related evidence sharing     │
│      canonical locator context where supported.                        │
├────────────────────────────────────────────────────────────────────────┤
│ 3. PRIMARY PASSAGE TEXT (Where supplied)                               │
│    - Sanskrit excerpt presentation uses the canonical text as supplied;│
│      script and transliteration presentation follows available         │
│      canonical data and applicable project-wide language/script        │
│      architecture.                                                     │
│    - Clear, readable typography per Block F3 typography rules.         │
├────────────────────────────────────────────────────────────────────────┤
│ 4. TRANSLATION BLOCK (Where supplied)                                  │
│    - Translated passage text in editorial English                      │
│    - Translator / edition attribution noted clearly                    │
├────────────────────────────────────────────────────────────────────────┤
│ 5. EVIDENCE METADATA & PASSAGE TYPE                                    │
│    - Evidence Type Tag (e.g., `direct_mention`, `primary_narrative`,   │
│      `commentary_gloss`, `variant_manuscript`)                         │
├────────────────────────────────────────────────────────────────────────┤
│ 6. AUTHORITATIVE SOURCE ANCHOR                                         │
│    - Embedded Source Summary Card (linking to full bibliographic view) │
└────────────────────────────────────────────────────────────────────────┘
```

### 8.2 Handling Missing Excerpts Honestly
- A canonical `Evidence` record always possesses a `locator` and `source_id`. However, `sanskrit_excerpt` and `excerpt_translation` are nullable (B4 §3.3).
- **Zero Fabrication Invariant**: If a textual excerpt is absent in the backend dataset, the UI renders the native citation locator with an honest semantic notice:
  `"Excerpt not supplied in the current record."`
- The UI must never synthesize an excerpt or substitute AI-generated translations.

---

## 9. Source Inspection Architecture

A `Source` represents the physical or scholarly work containing the evidence (B4 §3.1).

### 9.1 Bibliographic Source Card

```
┌────────────────────────────────────────────────────────────────────────┐
│                       BIBLIOGRAPHIC SOURCE CARD                        │
├────────────────────────────────────────────────────────────────────────┤
│ [Source Title & Abbreviation]                                          │
│   ├── Full Bibliographic Title (from `sources.title`)                  │
│   └── Scholarly Abbreviation Badge (from `sources.short_title`)        │
│                                                                        │
│ [Critical Edition & Recension Context]                                 │
│   ├── Source Type: `critical_edition`, `traditional_recension`, etc.   │
│   └── Tradition Context (where supplied by canonical data)             │
│                                                                        │
│ [Scholarly Attribution & Publication]                                  │
│   ├── Author / Editor / Translator (where supplied by canonical data)  │
│   └── Publication Details (where supplied by canonical data)           │
│                                                                        │
│ [Standard Identifiers & External Repository]                           │
│   ├── Identifier (ISBN, DOI, URN, etc., where supplied by data)        │
│   └── External Link (where supplied by canonical `sources.url`)        │
│                                                                        │
│ [Methodology / Overview]                                               │
│   └── Summary / critical overview (where supplied by canonical data)   │
└────────────────────────────────────────────────────────────────────────┘
```

### 9.2 Source Types & Metadata Presentation
Source presentation is tied strictly to canonical `source_type` and associated metadata fields supplied by Block B4:
- The UI presents the canonical `source_type` value (e.g., `critical_edition`, `traditional_recension`, `scholarly_analysis`, `commentary`, `regional_retelling`) as recorded in the authoritative data.
- **Conditional Presentation Rule**: Optional bibliographic fields—including author/editor/translator, publication details, identifiers (ISBN, DOI, URN, etc.), tradition/recension context, external repository URLs, and methodology summaries—are presented strictly **where supplied by the canonical Source record**. F15 does not mandate these fields when absent in backend data, nor does it encode subjective scholarly interpretations or textual conclusions about source authority.

---

## 10. Conflicting Claims Architecture

In strict accordance with Rule 03 and B4 §6, **competing traditions and variant manuscript readings must never be flattened, averaged, or overwritten**:

```
┌────────────────────────────────────────────────────────────────────────┐
│               CONFLICTING CLAIMS SIDE-BY-SIDE INTERFACE                │
├────────────────────────────────────────────────────────────────────────┤
│ CONFLICT NOTICE BAR: Epistemic State 'conflicting'                     │
│ "Multiple variant accounts exist across recensions and traditions."    │
├───────────────────────────────────┬────────────────────────────────────┤
│ CLAIM A: Source A / Tradition A   │ CLAIM B: Source B / Tradition B    │
├───────────────────────────────────┼────────────────────────────────────┤
│ Proposition:                      │ Proposition:                       │
│ "[Factual proposition A as        │ "[Variant factual proposition B    │
│ recorded in canonical data]"      │ as recorded in canonical data]"    │
│                                   │                                    │
│ Certainty: `traditional_consensus`│ Certainty: `disputed`              │
│                                   │                                    │
│ Evidence Citations:               │ Evidence Citations:                │
│ - Locator: [native locator A]     │ - Locator: [native locator B]      │
│ - Source: [Source A title]        │ - Source: [Source B title]         │
│                                   │                                    │
│ Excerpt:                          │ Excerpt:                           │
│ [Verbatim passage A where supplied│ [Verbatim passage B where supplied]│
│ Translation:                      │ Translation:                       │
│ [Translation A where supplied]    │ [Translation B where supplied]     │
├───────────────────────────────────┴────────────────────────────────────┤
│ ASSESSMENT & CONTEXTUAL COMMENTARY (`evidence.assessment`)             │
│ Curated editorial note detailing textual variations and context        │
│ where supplied by canonical data (from `evidence.assessment`).         │
└────────────────────────────────────────────────────────────────────────┘
```

### 10.1 Multi-Claim Non-Biased Presentation Rules
1. **Non-Hierarchical Presentation**: Competing claims are presented non-hierarchically and non-dismissively without silent privileging of one variant over another. Concrete responsive layout (such as comparative side-by-side containers or linked comparative cards across viewports) is left to Stage 4 implementation. Neither variant is hidden behind an "alternative" sub-menu.
2. **Neutral Ordering**: Display ordering adheres to canonical backend delivery or source publication date; the UI never labels one variant as the "true" version and another as "false".
3. **No Averaged Values**: Where claims disagree on dates, durations, or genealogical links, the UI must never average them into synthetic values (e.g., claiming an exile lasted "12.5 years" if sources say 12 and 13).
4. **Epistemic Labeling**: Both claims carry the canonical `epistemic_status = 'conflicting'`.

---

## 11. Missing / Partial Provenance States

Missing information is an unavoidable reality of ancient epic literature and progressive digital curation. The UI presents the canonical epistemic state and displays explanatory context only when that context is actually supplied by canonical data, distinguishing clearly between absence states rather than collapsing them into a generic placeholder:

```
┌────────────────────────────────────────────────────────────────────────┐
│                 ABSENCE & PARTIAL PROVENANCE MATRIX                    │
├──────────────────────┬─────────────────────────────────────────────────┤
│ Condition            │ Architectural UI Presentation                   │
├──────────────────────┼─────────────────────────────────────────────────┤
│ **1. Canonical       │ - Epistemic State: `unknown`                    │
│    Unknown**         │ - Canonical Label: "Unknown"                    │
│                      │ - Presentation: Renders canonical B3/F3 unknown │
│                      │   treatment; explanatory context displayed only │
│                      │   if supplied by canonical data.                │
├──────────────────────┼─────────────────────────────────────────────────┤
│ **2. Not Researched**│ - Epistemic State: `not_researched`             │
│                      │ - Canonical Label: "Not Researched"             │
│                      │ - Presentation: Renders canonical dotted badge; │
│                      │   does not infer reasons or timelines for       │
│                      │   curation.                                     │
├──────────────────────┼─────────────────────────────────────────────────┤
│ **3. Not Applicable**│ - Epistemic State: `not_applicable`             │
│                      │ - Canonical Label: "Not Applicable"             │
│                      │ - Presentation: Renders ghosted badge; applies  │
│                      │   where attribute does not apply to entity type.│
├──────────────────────┼─────────────────────────────────────────────────┤
│ **4. Claim Without   │ - Proposition displayed with caveat indicator:  │
│    Evidence Record** │   "Assertion without attached evidence record"  │
│                      │ - Preserves the proposition without inventing   │
│                      │   a backing citation.                           │
├──────────────────────┼─────────────────────────────────────────────────┤
│ **5. Evidence Without│ - Native locator string displayed prominently.  │
│    Excerpt**         │ - Note: "Excerpt text not supplied in record"   │
│                      │ - Preserves citation coordinate honestly.       │
├──────────────────────┼─────────────────────────────────────────────────┤
│ **6. Incomplete      │ - Available metadata displayed faithfully.      │
│    Source Metadata** │ - Note: "Source metadata partially recorded"    │
│                      │ - Omits missing optional fields cleanly without │
│                      │   synthetic placeholders.                       │
└──────────────────────┴─────────────────────────────────────────────────┘
```

---

## 12. Responsive Provenance Surface Architecture

In accordance with Block F4 §4 and §9, the primary provenance inspection surface recomposes dynamically across the four space-based viewport classes:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   RESPONSIVE PROVENANCE RECOMPOSITION                  │
├──────────────┬──────────────────┬──────────────────────────────────────┤
│ Viewport     │ Surface Manifest │ Interaction & Layout Behavior        │
├──────────────┼──────────────────┼──────────────────────────────────────┤
│ **Compact**  │ Bottom Sheet     │ - Draggable bottom sheet with        │
│ (Handhelds / │ Overlay          │   half-height and full-height snaps. │
│  Small)      │                  │ - Stacked vertical reading flow.     │
│              │                  │ - Swipe-down to dismiss.             │
│              │                  │ - Excerpts collapsed with "Read more"│
├──────────────┼──────────────────┼──────────────────────────────────────┤
│ **Medium**   │ Slide-Over       │ - Right-anchored sliding drawer      │
│ (Tablets /   │ Modal Drawer     │   covering ~50% of screen width.     │
│  Foldables)  │                  │ - Background exploration dimming.    │
│              │                  │ - Sticky top navigation and tabs.    │
├──────────────┼──────────────────┼──────────────────────────────────────┤
│ **Expanded** │ Persistent Side  │ - Side-by-side workspace layout.     │
│ (Desktops /  │ Workspace Panel  │ - Right column persistent inspector. │
│  Laptops)    │                  │ - Fluid resizing; no backdrop dim.   │
│              │                  │ - Parallel multi-source comparison.  │
├──────────────┼──────────────────┼──────────────────────────────────────┤
│ **Wide**     │ Dedicated Studio │ - Panoramic multi-pane view.         │
│ (Ultra-wide /│ Column           │ - Left: Primary lens workspace.      │
│  Multi-Mon)  │                  │ - Center: Contextual entity details. │
│              │                  │ - Right: Pinned Provenance Studio    │
│              │                  │   displaying full textual apparatus. │
└──────────────┴──────────────────┴──────────────────────────────────────┘
```

### 12.1 Scroll Containment & Avoidance of Traps
In strict compliance with Block F4 §10:
- The provenance surface owns its scrolling context and prevents interaction from unintentionally propagating to the underlying exploration surface.
- Interaction gestures within the evidence drawer never propagate to or displace the underlying graph, map, or timeline canvas.
- Horizontal text overflows in verbatim Sanskrit verses utilize contained scrollable verse blocks with clear visual affordances.

---

## 13. State and Data Lifecycle

In strict adherence to the project state hierarchy established in Block F5:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        STATE LIFECYCLE MATRIX                          │
├──────────────────┬──────────────────┬──────────────────────────────────┤
│ State Scope      │ Governing Layer  │ Managed Attributes               │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Server State** │ Block F6 Query   │ Cached Source, Claim, and        │
│                  │ Client           │ Evidence API responses           │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **URL State**    │ Block F7 Router  │ Active claim deep-link query     │
│                  │                  │ (e.g., `?claim=:claim_id`)       │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Lens State**   │ Active Lens      │ Currently selected edge/node     │
│                  │ (F9–F14)         │ triggering provenance inspection │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Transient UI** │ Local Component  │ Drawer open/close, active tab,   │
│                  │ State            │ script / view preference toggle  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Preferences**  │ Block F5 Store   │ Display script preference (F5),  │
│                  │                  │ scholarly notes expanded/closed  │
└──────────────────┴──────────────────┴──────────────────────────────────┘
```

### 13.1 Query Identity & Data Fetching
All provenance data fetching and cache identity are governed by Block F6. F15 does not define client cache-key structures.

---

## 14. Routing and Deep-Linking Boundary

Block F7 is the sole authority governing route paths, query parameter syntax, serialization, and canonicalization. Block F15 establishes the functional requirements for provenance linking without dictating route implementations:

1. **State Independence**: Opening or closing the provenance drawer during active exploration is primarily a transient UI interaction that does not replace the primary route.
2. **Deep-Linkable Citations**: F15 may require claim-level or source-level deep-linkability where useful; the exact URL representation, parameter naming, serialization, canonicalization, and validation are determined exclusively by F7.
3. **Stateless Hydration**: When navigating directly to a URL containing a supported provenance parameter, the application shell renders the parent entity or lens and hydrates the provenance inspection surface in its open state.
4. **Clean Exit**: Dismissing the provenance surface updates URL state per Block F7 rules without triggering a disruptive full-page reload.

---

## 15. Accessibility Boundary

Block F15 establishes the semantic information hierarchy and logical interaction flow for evidence presentation. Concrete accessibility mechanisms, assistive technology patterns, and conformance audits are formally governed by downstream blocks:
- **Block F16 (Accessibility Architecture)** authoritatively defines:
  - Accessible names, ARIA roles (`region`, `complementary`, `dialog`), and landmark regions for the provenance surface.
  - Keyboard focus trapping within modal bottom sheets and focus return to triggering pills upon dismissal.
  - Live-region announcements (`aria-live="polite"`) when evidence panels update dynamically.
  - High-contrast visual compliance for script diacritics.
- **Block F17 (Testing & Performance)** defines:
  - Automated screen-reader test suites and keyboard navigation verification protocols.

---

## 16. Security & Untrusted Content Boundary

Provenance data inherently incorporates external bibliographic links, quotations, and textual excerpts:
1. **Untrusted External URIs**: External URLs stored in `sources.url` are treated as strictly untrusted. Outbound links rendered in the UI must enforce secure link isolation preventing reverse tab hijacking, accompanied by an explicit external-link glyph.
2. **XSS Prevention in Excerpts**: Textual passages, Sanskrit excerpts, and editorial translations are rendered strictly as escaped plain text. Raw, unescaped HTML injection into the document DOM is prohibited.
3. **Identifier Sanitization**: ISBN, DOI, and URN identifiers are sanitized before being interpolated into external scholarly lookup links.

---

## 17. Performance & Progressive Loading Boundary

Provenance data can be voluminous, containing multi-verse Sanskrit passages and extensive apparatus notes. To maintain responsive exploration:
1. **Progressive Payload Disclosure**:
   - Lightweight provenance indicators are available during exploration and detailed evidence is obtained when the provenance surface is opened, subject to B5 API contracts and F6 fetching architecture. F15 does not modify B5 endpoint contracts or prescribe backend payload structures.
2. **Virtualization of Extensive Citation Lists**:
   - Entities with dozens of corroborating citations utilize virtualized list rendering to prevent DOM bloat.
3. **Budget Deferral**: Quantitative rendering times, payload byte budgets, and Lighthouse metrics are authoritatively governed by Block F17.

---

## 18. Cross-Lens Integration & Navigation

Provenance inspection acts as a connective tissue across the entire Mahābhārata Explorer ecosystem:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   PROVENANCE CROSS-LENS NAVIGATION                     │
├────────────────────────────────────────────────────────────────────────┤
│                          [PROVENANCE SURFACE]                          │
│                                   │                                    │
│        ┌──────────────────────────┼──────────────────────────┐         │
│        ▼                          ▼                          ▼         │
│ ┌──────────────┐           ┌──────────────┐           ┌──────────────┐ │
│ │ ENTITY LENS  │           │ TIMELINE     │           │ GEOGRAPHY    │ │
│ │ (Block F9)   │           │ (Block F12)  │           │ (Block F13)  │ │
│ │ Jump to      │           │ Jump to cited│           │ Jump to      │ │
│ │ character /  │           │ Parva / event│           │ verified     │ │
│ │ group profile│           │ in sequence  │           │ ancient realm│ │
│ └──────────────┘           └──────────────┘           └──────────────┘ │
└────────────────────────────────────────────────────────────────────────┘
```

- Every claim displayed within the provenance surface maintains active, semantic links to related entities, events, and geographic realms.
- Transitioning from a citation link to another lens updates the URL via Block F7, preserving scholarly context across navigation steps.

---

## 19. Ownership Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                     FRONTEND PROVENANCE OWNERSHIP                      │
├──────────────────────────┬──────────────────┬──────────────────────────┤
│ Architectural Domain     │ Governing Module │ Responsible Block        │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ Provenance Data Schema   │ Backend Layer 2  │ Block B4                 │
│ Public Provenance APIs   │ REST API Client  │ Block B5 / Block F6      │
│ Epistemic Visual Grammar │ Design System    │ Block F3                 │
│ Surface Composition      │ App Shell        │ Block F4                 │
│ Provenance UI State      │ Client State     │ Block F5 / Block F15     │
│ URL Deep-Linking         │ Router           │ Block F7                 │
│ Evidence Trigger Layout  │ Lens Views       │ Blocks F8–F14            │
│ Evidence Presentation    │ Provenance UI    │ Block F15                │
│ Assistive Tech / ARIA    │ Accessibility    │ Block F16                │
│ Performance Verification │ Quality Gates    │ Block F17                │
└──────────────────────────┴──────────────────┴──────────────────────────┘
```

---

## 20. Architectural Decision Records (ADRs)

### ADR-F15-01: Three-Tier Progressive Disclosure Model
- **Context**: Displaying scholarly apparatus and multi-recension variants directly on exploration canvases causes severe visual clutter and alienates non-academic users. Conversely, hiding citations undermines trustworthiness.
- **Decision**: Architect three distinct provenance disclosure tiers: (1) Ambient Epistemic Badges, (2) In-Situ Contextual Previews, and (3) Dedicated Provenance Inspection Surface.
- **Consequences**: Balances effortless epic exploration for casual users with exhaustive textual verification for researchers.

### ADR-F15-02: Strict Separation of Claim, Evidence, and Source
- **Context**: Many applications collapse citations into a single flat string (e.g., `"[Source Locator]"`).
- **Decision**: Preserve Block B4's distinct three-layer structure in the UI: Claims represent what is asserted, Evidence represents why it is believed (with excerpts, locators, and assessments), and Sources represent where it originates (the bibliographic source record).
- **Consequences**: Enables multi-source corroboration, variant tradition comparisons, and transparent scholarly evaluation.

### ADR-F15-03: Multi-Claim Non-Hierarchical Conflict Parity
- **Context**: Conflicting accounts across variant traditions or recensions risk being presented hierarchically, implying that one reading is canonical and the other is erroneous.
- **Decision**: Mandate non-hierarchical, non-dismissive presentation and neutral chronological/source-based ordering for all competing claims without silent privileging.
- **Consequences**: Directly upholds Constitutional Rule 03 (Preservation of Conflicting Traditions) and prevents synthetic editorial bias.

### ADR-F15-04: Non-Mutation of Native Citation Locators
- **Context**: Normalizing all locators into a uniform decimal format risks corrupting standard scholarly citation conventions across different editions.
- **Decision**: Render verbatim native citation strings exactly as published, using internal normalized fields solely for indexing, sorting, and UI filtering.
- **Consequences**: Guarantees academic reliability and seamless cross-referencing with physical library volumes.

### ADR-F15-05: Space-Based Adaptive Provenance Surface
- **Context**: A modal dialog obstructs canvas exploration, while an embedded inline section breaks graph and map visualization layouts.
- **Decision**: Map the provenance surface dynamically across Block F4 viewport classes: Bottom Sheet on Compact, Slide-Over Drawer on Medium, and Persistent Workspace Panel on Expanded/Wide.
- **Consequences**: Ensures ergonomically sound, responsive interaction across handheld, tablet, desktop, and multi-monitor workstations.

---

## 21. Requirement Traceability Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                   REQUIREMENT TRACEABILITY MATRIX                      │
├───────────────────┬──────────────────┬─────────────────────────────────┤
│ Upstream Req ID   │ Source Document  │ Block F15 Architectural Mapping │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REQ-EVD-01**    │ B4 §1; PRD §9.10 │ Section 2.1, Section 8: Zero    │
│                   │                  │ fabrication of citations.       │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REQ-EVD-02**    │ B4 §2; PRD §9.10 │ Section 3, Section 7–9: Four-   │
│                   │                  │ tier provenance hierarchy.      │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REQ-EVD-03**    │ B4 §5; PRD §9.10 │ Section 8.1, Section 20 (ADR 04)│
│                   │                  │ Native locator preservation.    │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REQ-EVD-04**    │ B4 §7; PRD §9.10 │ Section 2.1, Section 7: Six     │
│                   │                  │ canonical epistemic states.     │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REQ-EVD-05**    │ B4 §6; PRD §9.10 │ Section 10: Conflicting claims  │
│                   │                  │ side-by-side parity.            │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **REQ-EVD-06**    │ B4 §8; PRD §10   │ Section 7.2: Presentation of    │
│                   │                  │ canonical provenance_origin     │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **NAV-001**       │ F7 §4, §5        │ Section 14: Deep-linking query  │
│                   │                  │ coordination with Block F7.     │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **A11Y-001**      │ F3 §4; F4 §9     │ Section 15: Semantic hierarchy  │
│                   │                  │ demarcation with Block F16.     │
└──────────────────┴──────────────────┴─────────────────────────────────┘
```

---

## 22. F15 Exit Criteria & Acceptance Checklist

- [x] Provenance UI purpose, UX role, and distinct boundaries against F9, F10, F11, F12, F13, F14 established.
- [x] Canonical backend entities consumed strictly as defined by B4 (`Source`, `Claim`, `Evidence`).
- [x] Absolute zero-fabrication rule enforced: no invented claims, evidence, excerpts, author names, or certainty levels.
- [x] Canonical six-state epistemic vocabulary adhered to without colloquial state substitutions (`known`, `unknown`, `not_researched`, `not_applicable`, `conflicting`, `approximate`).
- [x] Four-tier provenance hierarchy (`Entity/Edge → Claim → Evidence → Source`) preserved without flattening into generic citations.
- [x] Native citation locator fidelity established: verbatim strings displayed without destructive reformatting.
- [x] Three-tier progressive disclosure model specified (Ambient Badges → Contextual Previews → Deep Provenance Surface).
- [x] Standardized provenance affordances architected for entity views, graph edges, family ties, timeline events, map realms, and war formations.
- [x] Conflicting traditions and multi-claim architecture specified with side-by-side parity and non-biased ordering.
- [x] Honest absence states defined for unknown attributes, unresearched fields, missing excerpts, and non-applicable properties.
- [x] Responsive recomposition mapped across the four Block F4 viewport classes (Bottom Sheet, Slide-Over, Persistent Panel, Dedicated Column).
- [x] Independent scroll containment specified to prevent nested-scroll trapping per Block F4.
- [x] State management aligned with Block F5; caching and query keys aligned with Block F6.
- [x] Deep-linking and URL parameter coordination specified under strict Block F7 authority.
- [x] Concrete accessibility implementation deferred cleanly to Block F16; quantitative budgets deferred to Block F17.
- [x] Security protections defined for external repository URLs and untrusted textual markup.
- [x] Zero application source code, UI packages, or concrete component libraries selected or introduced.
- [x] Zero Stage 1 backend documents or Blocks F1–F14 documents modified.
