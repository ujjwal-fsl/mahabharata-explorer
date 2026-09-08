# Accessibility Architecture (Block F16)

## 1. Document Status & Purpose

```
┌────────────────────────────────────────────────────────────────────────┐
│                          DOCUMENT ATTRIBUTION                          │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Document ID      │ docs/05-frontend/15-accessibility-architecture.md   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ Stage / Block    │ Stage 2 (Frontend Architecture) — Block F16         │
├──────────────────┼─────────────────────────────────────────────────────┤
│ Upstream Sources │ - Stage 1 Blocks B1–B13: Backend Architecture       │
│                  │ - Stage 2 Block F1: Frontend Architecture Context   │
│                  │ - Stage 2 Block F2: Technology & Build Architecture │
│                  │ - Stage 2 Block F3: Design System & Visual Grammar  │
│                  │ - Stage 2 Block F4: Layout & Application Shell      │
│                  │ - Stage 2 Block F5: State Management Architecture   │
│                  │ - Stage 2 Block F6: Fetching & Caching Architecture │
│                  │ - Stage 2 Block F7: Routing & Navigation            │
│                  │ - Stage 2 Block F8: Search Architecture             │
│                  │ - Stage 2 Block F9: Entity Views Architecture       │
│                  │ - Stage 2 Block F10: Graph Visualization Arch.      │
│                  │ - Stage 2 Block F11: Lineage / Family Tree Arch.    │
│                  │ - Stage 2 Block F12: Timeline & Chronology Arch.    │
│                  │ - Stage 2 Block F13: Geographic Map Architecture    │
│                  │ - Stage 2 Block F14: War & Tactical Vyuha Arch.     │
│                  │ - Stage 2 Block F15: Evidence & Provenance UI       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ Downstream Deps  │ - Stage 2 Block F17: Performance & Testing          │
│                  │ - Stage 4: Implementation Phase                     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ Status           │ Draft Architectural Specification (Review Pending) │
└──────────────────┴─────────────────────────────────────────────────────┘
```

This document establishes the **Accessibility Architecture** for the **Mahābhārata Explorer** frontend. It serves as the authoritative specification governing semantic document structure, non-pointer interaction models, focus management, screen-reader semantics, dynamic status announcements, non-visual visualization alternatives, epistemic parity, responsive reflow, and assistive technology access across all lenses and application surfaces.

In accordance with Constitutional Rule 10, **accessibility is a first-class architectural requirement, not an afterthought or retrofitted patch**. The Mahābhārata Explorer provides an interconnected exploration environment spanning high-density narrative texts, complex genealogical DAGs, force-directed topological networks, multi-layered spatial cartography, chronological sequence streams, tactical battle formations, and rigorous bibliographic citations. Every user—regardless of motor, visual, auditory, cognitive, or situational capabilities—must be able to navigate, discover, verify, and comprehend the epic knowledge graph with equal semantic depth and operational independence.

---

## 2. Normative Accessibility Target & Conformance Scope

### 2.1 Conformance Target
The Mahābhārata Explorer establishes a normative target of **WCAG 2.2 Level AA** conformance across all public and exploratory client surfaces.

```
┌────────────────────────────────────────────────────────────────────────┐
│                   NORMATIVE CONFORMANCE FRAMEWORK                      │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Dimension        │ Architectural Policy & Scope                        │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Baseline**     │ WCAG 2.2 Level AA conformance across all routes.    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Principles**   │ Perceivable, Operable, Understandable, Robust.      │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Scope**        │ All application shell regions, exploration lenses,  │
│                  │ interactive visualizations, search interfaces,      │
│                  │ contextual drawers, and evidence surfaces.          │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Distinction**  │ F16 establishes architectural requirements. Product │
│                  │ compliance is not claimed merely by defining specs; │
│                  │ compliance is verified empirically in Stage 4/F17.  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Metric Policy**│ Zero synthetic "accessibility scores" or proprietary│
│                  │ ranking badges. Adherence is measured against       │
│                  │ objective normative success criteria and parity.    │
└──────────────────┴─────────────────────────────────────────────────────┘
```

### 2.2 Structural Distinctions
To maintain architectural discipline throughout this specification:
- **Normative Success Criteria**: The authoritative accessibility standards required for Level AA compliance across perceivability, operability, understandability, and robustness.
- **Architectural Requirements**: Binding structural constraints that every component, lens, and surface must fulfill within the Mahābhārata Explorer system topology.
- **Implementation Guidance**: Descriptive technical patterns and recommendations provided for Stage 4 realization without prescribing closed code libraries, specific DOM APIs, or rigid styling classes.
- **Later Empirical Verification**: Observable, testable behaviors against which quality gates and testing harnesses (governed by Block F17) evaluate production compliance.

---

## 3. Core Accessibility Principles

```
┌────────────────────────────────────────────────────────────────────────┐
│                   CORE ACCESSIBILITY CONSTITUTION                      │
├────────────────────────────────────────────────────────────────────────┤
│ 1. FIRST-CLASS ARCHITECTURAL CONCERN: Accessibility requirements shape │
│    frontend semantic structures, interaction models, and rendering     │
│    architecture from day zero while consuming established canonical    │
│    data and routing contracts.                                         │
├────────────────────────────────────────────────────────────────────────┤
│ 2. NATIVE SEMANTICS FIRST: Standard semantic HTML elements are         │
│    preferred for all structural landmarks, text blocks, lists, controls│
├────────────────────────────────────────────────────────────────────────┤
│ 3. ARIA AS SUPPLEMENT, NOT REPLACEMENT: ARIA attributes are introduced │
│    strictly to supply missing semantic roles, states, and properties   │
│    where standard HTML lacks adequate expressive mechanisms.           │
├────────────────────────────────────────────────────────────────────────┤
│ 4. COMPLETE NON-POINTER PARITY: Every meaningful exploration pathway,  │
│    filter, entity inspect, and citation jump must be fully operable    │
│    without a pointing device.                                          │
├────────────────────────────────────────────────────────────────────────┤
│ 5. VISUALIZATIONS ARE NOT SOLE SOURCES OF TRUTH: Graphical visualizers │
│    must be paired with an equivalent accessible semantic companion     │
│    exposing the same canonical data.                                   │
├────────────────────────────────────────────────────────────────────────┤
│ 6. CANONICAL DATA FIDELITY: Accessible alternatives derive from the    │
│    exact SAME canonical backend data as graphical visualizers; zero    │
│    alternate or fabricated accessibility datasets are permitted.       │
├────────────────────────────────────────────────────────────────────────┤
│ 7. PRESERVATION OF ZERO FABRICATION: Accessible representations must   │
│    never invent facts, parentage, troop counts, or historical causes   │
│    for missing data.                                                   │
├────────────────────────────────────────────────────────────────────────┤
│ 8. EPISTEMIC TRUTH PRESERVATION: Accessible representations communicate│
│    canonical epistemic status, certainty, and provenance origin without│
│    conflation or inferred rationales.                                  │
├────────────────────────────────────────────────────────────────────────┤
│ 9. PREDICTABLE FOCUS HYGIENE: Focus placement, containment, and        │
│    restoration must remain deterministic and preserve user context.    │
├────────────────────────────────────────────────────────────────────────┤
│ 10. POLITE STATUS ANNOUNCEMENTS: Dynamic updates inform assistive      │
│     technology users without verbal flooding or disruptive frequency.  │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 4. F16 Ownership Matrix & System Boundaries

F16 is the authoritative frontend architecture block for accessibility. It coordinates with existing architectural contracts across the system without taking ownership away from them:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        F16 OWNERSHIP MATRIX                            │
├──────────────────────────┬──────────────────┬──────────────────────────┤
│ Architectural Concern    │ Governing Block  │ Block F16 Responsibility │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ **Accessibility Arch.**  │ **Block F16**    │ Authoritative owner for  │
│                          │                  │ a11y architecture & specs│
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ **Visual Tokens & Lang.**│ Block F3         │ Reuses F3 color tokens,  │
│                          │                  │ surfaces, and typography │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ **Shell & Viewports**    │ Block F4         │ Establishes a11y across  │
│                          │                  │ F4 shell and viewports   │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ **State Hierarchy**      │ Block F5         │ Integrates a11y with F5  │
│                          │                  │ UI/URL/Preference state  │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ **Data Fetching & Cache**│ Block F6         │ Respects F6 query states │
│                          │                  │ for status announcements │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ **Routing & Deep-Links** │ Block F7         │ Coordinates post-nav     │
│                          │                  │ focus & title management │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ **Search IA**            │ Block F8         │ Defines a11y combobox &  │
│                          │                  │ suggestion semantics     │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ **Entity Views IA**      │ Block F9         │ Establishes accessible   │
│                          │                  │ identity & facet access  │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ **Graph Architecture**   │ Block F10        │ Architects non-visual    │
│                          │                  │ relational graph companion│
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ **Lineage Architecture** │ Block F11        │ Architects non-visual    │
│                          │                  │ parent-child kinship DOM │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ **Timeline Arch.**       │ Block F12        │ Enforces narrative order │
│                          │                  │ sequence semantics       │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ **Geographic Map Arch.** │ Block F13        │ Maps spatial cartography │
│                          │                  │ to non-spatial entities  │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ **War & Vyuha Arch.**    │ Block F14        │ Architects accessible    │
│                          │                  │ three-tier battle orders │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ **Evidence & Provenance**│ Block F15        │ Preserves 4-tier chain   │
│                          │                  │ without flattening       │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ **Performance & Testing**│ Block F17        │ Defines verification reqs│
│                          │                  │ (Budgets deferred to F17)│
└──────────────────────────┴──────────────────┴──────────────────────────┘
```

---

## 5. Semantic Document Structure & Landmarks

### 5.1 Structural Landmark Capabilities
The application shell and page workspaces are organized using standard landmark roles to allow immediate assistive technology navigation:

```
┌────────────────────────────────────────────────────────────────────────┐
│                     LANDMARK CAPABILITIES TOPOLOGY                     │
├────────────────────────────────────────────────────────────────────────┤
│ Banner Landmark : Global Shell Header                                  │
│   ├── Brand Identity & Primary Home Link                               │
│   ├── Search Landmark : Global Exploration Search Interface            │
│   └── Navigation Landmark : Primary Lens Navigation                    │
├────────────────────────────────────────────────────────────────────────┤
│ Main Landmark : Active Exploration Workspace                           │
│   ├── Level 1 Heading (H1) : Primary Lens / Entity Identity Landmark   │
│   ├── Navigation Landmark : In-page Steppers, Facet Controls           │
│   ├── Section Landmarks : Structured Domain Facets & Modules           │
│   └── Visual Canvas + Synchronized Semantic Companion Container        │
├────────────────────────────────────────────────────────────────────────┤
│ Complementary Landmark : Contextual Inspector / Evidence Panel         │
│   ├── Entity / Node Contextual Overview                                │
│   └── Backing Provenance & Citation Inspector                          │
├────────────────────────────────────────────────────────────────────────┤
│ Contentinfo Landmark : Global Metadata & Repository Attribution        │
│   └── Epistemic notices, source editions, accessibility declarations   │
└────────────────────────────────────────────────────────────────────────┘
```

### 5.2 Native Elements vs. Synthetic ARIA
In accordance with web platform standards and system reuse:
1. **Interactive Controls**:
   - Navigation links use standard anchor elements with valid destinations.
   - Action triggers, toggles, panel openers, and dismissers use standard button elements.
   - Form controls, inputs, and search inputs use standard input elements.
2. **Tabular & Structured Data**:
   - Data grids, battle orders, and listings use semantic table structures (`table`, `thead`, `tbody`, `tr`, `th`, `td`) with column and row header scopes, or structured lists (`ol`, `ul`, `dl`) where appropriate.
3. **Restrained ARIA Rule**:
   - Synthetic button roles and tab indices on non-interactive elements are avoided where standard platform elements can be used.
   - ARIA is introduced strictly to express dynamic behaviors, expanded states, popups, and live status updates lacking standard markup representation.

---

## 6. Heading Hierarchy & Information Architecture

Heading levels convey logical document outline and structural hierarchy rather than visual typographical scale:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        HEADING HIERARCHY MODEL                         │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Heading Level    │ Semantic Scope & Structural Function                │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Level 1 (H1)** │ Primary document landmark: Single H1 per route view │
│                  │ - Entity Profile: Canonical Entity Name             │
│                  │ - War Day: Canonical War Day Sequence Title         │
│                  │ - Timeline: Epic Narrative Sequence Landmark        │
│                  │ - Graph Lens: Focus Graph Landmark                  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Level 2 (H2)** │ Major functional zones & primary facets             │
│                  │ - Entity Facets: Narrative, Kinship, Relationships  │
│                  │ - Provenance Surface: Claims & Textual Evidence     │
│                  │ - Global Shell: Lens Navigation, Search             │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Level 3 (H3)** │ Sub-facets, discrete episodes, and card modules     │
│                  │ - Ancestral Lineage, Marital Unions                 │
│                  │ - Individual Narrative Event Title                  │
│                  │ - Granular Evidence Proposition Item                │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Level 4 (H4)** │ Detailed attribute groupings and source records     │
│                  │ - Supporting Citation Item                          │
│                  │ - Bibliographic Source Record Details               │
└──────────────────┴─────────────────────────────────────────────────────┘
```

- **Non-Skipping Invariant**: Heading levels must never skip intermediate levels (for example, stepping from `<h2>` directly to `<h4>`).
- **Layout Independence**: Breakpoint adjustments and responsive reflow under Block F4 must never alter the underlying semantic heading hierarchy.

---

## 7. Keyboard Operability & Non-Pointer Interaction

Every user action, exploration pathway, inspection surface, and navigation link must be operable without requiring a pointing device.

```
┌────────────────────────────────────────────────────────────────────────┐
│                     KEYBOARD INTERACTION PATTERNS                      │
├──────────────────┬──────────────────┬──────────────────────────────────┤
│ Target Surface   │ Primary Key(s)   │ Action / Behavioral Result       │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Global Shell** │ `Tab` /          │ Traverses interactive controls   │
│                  │ `Shift+Tab`      │ in deterministic reading order.  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Skip Link**    │ `Tab` (at entry) │ Surfaces visible link to jump    │
│                  │ $\rightarrow$ `Enter` │ directly to main content.        │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Lens Switcher**│ `ArrowLeft` /    │ Moves selection across lens      │
│                  │ `ArrowRight`     │ controls when focused.           │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Facet Controls**│ `ArrowLeft` /   │ Navigates entity facets or       │
│                  │ `ArrowRight`     │ war sequence controls.           │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Autocomplete** │ `ArrowDown` /    │ Traverses search suggestions     │
│                  │ `ArrowUp`        │ without displacing query cursor. │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Overlay Dismiss│ `Escape`         │ Closes active modal dialog,      │
│                  │                  │ drawer, or search overlay and    │
│                  │                  │ restores invoking focus.         │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Companion View**│ `ArrowKeys` or  │ Navigates relational entities,   │
│                  │ Tabular listing  │ events, or geographic records    │
│                  │ navigation       │ without spatial canvas drag.     │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **Citation Pill**│ `Enter` /        │ Opens Block F15 Provenance       │
│                  │ `Space`          │ Surface focused on backing claim.│
└──────────────────┴──────────────────┴──────────────────────────────────┘
```

- **No Keyboard Traps**: Focus must never become trapped within any component, canvas container, or modal widget.
- **Shortcut Safety**: Character-key shortcuts, if provided, must allow disabling or remapping, or require a modifier key, satisfying normative WCAG success criteria for character key shortcuts.

---

## 8. Focus Management Capabilities

```
┌────────────────────────────────────────────────────────────────────────┐
│                       FOCUS MANAGEMENT LIFECYCLE                       │
├────────────────────────────────────────────────────────────────────────┤
│ 1. FOCUS ENTRY & SKIP MECHANISM                                        │
│    - First focusable element in DOM order provides immediate skip to   │
│      the primary workspace content.                                    │
│    - Secondary skip link available to jump directly to accessible      │
│      semantic companions on complex visualizer pages.                  │
├────────────────────────────────────────────────────────────────────────┤
│ 2. LOGICAL TAB ORDER                                                   │
│    - Sequential focus navigation strictly follows the visual and       │
│      reading layout of the active viewport.                            │
├────────────────────────────────────────────────────────────────────────┤
│ 3. FOCUS CONTAINMENT (MODAL SURFACES)                                  │
│    - When a modal dialog or overlay is open, focus containment is      │
│      enforced so keyboard navigation remains within the surface.       │
│    - Tab cycling wraps predictably within the active overlay.          │
├────────────────────────────────────────────────────────────────────────┤
│ 4. DETERMINISTIC FOCUS RESTORATION                                     │
│    - Dismissing a modal dialog, drawer, or sheet immediately restores  │
│      focus to the invoking element that triggered the surface.         │
├────────────────────────────────────────────────────────────────────────┤
│ 5. CLIENT-SIDE ROUTE TRANSITIONS                                       │
│    - Upon SPA route navigation governed by Block F7:                   │
│      a. Document title updates to reflect the new resource identity.   │
│      b. Focus moves logically to the primary content heading or        │
│         workspace container, preventing stranded focus.                │
│      c. Screen-reader receives polite route announcement.              │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 9. Contextual Surface Accessibility

In accordance with the Contextual Surface Taxonomy defined in Block F4 §9:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   CONTEXTUAL SURFACE ACCESSIBILITY                     │
├──────────────┬──────────────┬──────────────┬─────────────┬─────────────┤
│ Surface Type │ Semantic     │ Accessible   │ Focus       │ Escape Key  │
│              │ Role         │ Naming       │ Containment │ Dismissal   │
├──────────────┼──────────────┼──────────────┼─────────────┼─────────────┤
│ **Modal      │ Modal Dialog │ Named by     │ Required    │ Required    │
│ Dialog**     │              │ surface title│             │ (Restores   │
│ (Search/Box) │              │              │             │ prior focus)│
├──────────────┼──────────────┼──────────────┼─────────────┼─────────────┤
│ **Slide-Over │ Dialog or    │ Named by     │ Viewport-   │ Required    │
│ Drawer**     │ Region per   │ drawer title │ dependent   │ (When       │
│ (F15 / Insp) │ viewport     │              │ (F4 layout) │ focused)    │
├──────────────┼──────────────┼──────────────┼─────────────┼─────────────┤
│ **Bottom     │ Modal Dialog │ Named by     │ Required    │ Required    │
│ Sheet**      │              │ sheet title  │ on Compact  │ (Restores   │
│ (Compact)    │              │              │ viewports   │ prior focus)│
├──────────────┼──────────────┼──────────────┼─────────────┼─────────────┤
│ **Popover /  │ Tooltip or   │ Associated   │ Not         │ Required    │
│ Context HUD**│ Region       │ via reference│ applicable  │ (Dismisses  │
│              │              │              │             │ popover)    │
└──────────────┴──────────────┴──────────────┴─────────────┴─────────────┘
```

### 9.1 Background Inactivity
When a modal surface is active:
- Background inertness is enforced so assistive technologies ignore the occluded exploration workspace.
- Background pointer and touch interactions are guarded from accidental activation per F4 §9.

---

## 10. Dynamic Content & Status Announcements

Dynamic DOM updates must be communicated without visual dependency, while strictly preventing verbal flooding of assistive technology users:

```
┌────────────────────────────────────────────────────────────────────────┐
│                      DYNAMIC STATUS ANNOUNCEMENTS                      │
├──────────────────┬──────────────────┬───────────────┬──────────────────┤
│ Event Category   │ Announcement     │ Announcement  │ Message          │
│                  │ Trigger          │ Politeness    │ Intent           │
├──────────────────┼──────────────────┼───────────────┼──────────────────┤
│ **Search Results │ Autocomplete     │ Polite        │ Informs count of │
│ Updated**        │ query resolves   │               │ available items. │
├──────────────────┼──────────────────┼───────────────┼──────────────────┤
│ **Filter State   │ Facet or status  │ Polite        │ Confirms filter  │
│ Change**         │ filter applied   │               │ and item count.  │
├──────────────────┼──────────────────┼───────────────┼──────────────────┤
│ **Lens Context   │ Sequence step or │ Polite        │ Confirms active  │
│ Shift**          │ Parva activated  │               │ context shift.   │
├──────────────────┼──────────────────┼───────────────┼──────────────────┤
│ **System Error / │ Network failure  │ Assertive     │ Alerts user to   │
│ Network Alert**  │ or timeout (F6)  │               │ critical error.  │
├──────────────────┼──────────────────┼───────────────┼──────────────────┤
│ **Route Nav.     │ Client route     │ Polite        │ Confirms arrival │
│ Complete**       │ transition (F7)  │               │ at new resource. │
└──────────────────┴──────────────────┴───────────────┴──────────────────┘
```

- **Polite Default Rule**: Routine status and search updates use polite announcements. Assertive announcements are reserved strictly for critical errors or disruptive events.
- **Anti-Flooding Discipline**: High-frequency streaming interactions (continuous canvas pan/zoom coordinates, drag slider ticks) must never trigger live announcements.

---

## 11. Request Lifecycle & Epistemic State Separation

In accordance with the client request lifecycle (Block F6 §5) and canonical data architecture (Block B2 §6), accessible messaging must cleanly separate network transport states from canonical epistemic states:

```
┌────────────────────────────────────────────────────────────────────────┐
│               LIFECYCLE & EPISTEMIC MESSAGING SEPARATION               │
├──────────────────┬──────────────────┬──────────────────────────────────┤
│ Condition        │ Category         │ Accessible Presentation Meaning  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **`loading`**    │ Request Lifecycle│ Exposes transient loading status:│
│                  │ (Block F6)       │ "Loading content, please wait."  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **`error`**      │ Request Lifecycle│ Exposes operational error alert: │
│                  │ (Block F6)       │ "Unable to retrieve data. Retry."│
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **`known`**      │ Canonical Truth  │ "Epistemic status: known"        │
│                  │ (Block B2)       │                                  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **`conflicting`**│ Canonical Truth  │ "Epistemic status: conflicting"  │
│                  │ (Block B2)       │                                  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **`approximate`**│ Canonical Truth  │ "Epistemic status: approximate"  │
│                  │ (Block B2)       │                                  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **`unknown`**    │ Canonical Truth  │ "Epistemic status: unknown"      │
│                  │ (Block B2)       │                                  │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **`not_          │ Canonical Truth  │ "Epistemic status:               │
│   researched`**  │ (Block B2)       │  not_researched"                 │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ **`not_          │ Canonical Truth  │ "Epistemic status:               │
│   applicable`**  │ (Block B2)       │  not_applicable"                 │
└──────────────────┴──────────────────┴──────────────────────────────────┘
```

- **Strict Non-Inference Rule**: When data is absent or an attribute exhibits uncertainty, the accessible presentation reflects the canonical state directly. It must never fabricate or infer reasons for why data is missing.
- **Categorical Separation**: The architecture strictly maintains separation between `epistemic_status` (the six project-wide truth states), `certainty` (canonical certainty rating), and `provenance_origin` (B4-defined origin classification).

---

## 12. Search & Autocomplete Accessibility

Aligning strictly with Global Search UX Architecture (Block F8):

1. **Combobox Capabilities**:
   - The search input exposes standard combobox semantics, indicating expansion state, listbox association, and keyboard selection capabilities.
2. **Keyboard Traversal**:
   - Up and down arrow navigation traverses search suggestions without displacing the cursor in the search input.
   - Screen-reader users receive programmatic notification of currently highlighted suggestions.
3. **Status Feedback**:
   - Result counts are communicated politely upon completion of search queries.
4. **Escape Isolation**:
   - An initial Escape keystroke dismisses open suggestion lists; subsequent Escape keystrokes clear query text or dismiss the search surface.

---

## 13. Entity Views Accessibility

Aligning strictly with Character & Entity Views Architecture (Block F9):

1. **Entity Identity Header**:
   - Primary heading (H1) presents the canonical entity name in the representation supplied by the canonical backend and applicable project language/script architecture.
   - Where canonical data supplies original Sanskrit excerpts or titles, language tagging is applied to support accurate speech synthesizer pronunciation.
   - Attested epithets and aliases are presented in accessible semantic list or definition structures where supplied.
2. **Facet Navigation & Layout Flexibility**:
   - Entity facets (such as narrative episodes, relationships, and military context) must remain semantically structured and keyboard accessible regardless of whether rendered as tabs, stacked sections, or collapsible panels.
3. **Cross-Lens Navigation**:
   - Links connecting an entity to other lenses (Graph, Lineage, Timeline, Map, War) provide descriptive accessible names and follow standard route navigation.

---

## 14. Evidence & Provenance Accessibility

Aligning strictly with Block F15, the four-tier provenance chain must remain fully accessible without flattening its conceptual depth:

$$\text{Entity / Edge} \longrightarrow \text{Claim} \longrightarrow \text{Evidence} \longrightarrow \text{Source}$$

1. **Hierarchical Integrity**:
   - Claims, supporting evidence passages, and bibliographic sources are exposed through nested semantic structures where canonical provenance data is available.
2. **Distinct Attribute Presentation**:
   - The accessible representation communicates:
     - Canonical epistemic status: `epistemic_status: [canonical state]`
     - Canonical certainty: `certainty: [canonical value]`
     - Canonical provenance origin: `provenance_origin: [canonical B4 value]`
   - These attributes must never be merged into a single undifferentiated string.
3. **Evidence Excerpts**:
   - Canonical native locators, Sanskrit excerpts, and translations (where supplied) are rendered in semantic quote and citation structures.
   - If an excerpt is not present in the canonical record, the presentation renders a neutral notice without inferring why it is absent.
4. **Conflicting Claims Parity**:
   - Competing claims are presented as sibling items of equivalent structural depth and neutral labelling.

---

## 15. Visualization Accessibility Architecture

Graphical visualizers (Focus Graph, Lineage DAG, Timeline Stream, Geographic Map, Tactical Vyuha Canvas) are visual projections of the underlying knowledge graph. Making an SVG or Canvas element focusable does not make it accessible.

Every exploration lens provides an **Accessible Semantic Companion** that:
1. Derives from the exact **SAME canonical data** supplied to the visual lens.
2. Exposes the same canonical entities, relationships, events, and spatial records in structured semantic markup.
3. Is fully operable via keyboard navigation and assistive technologies.
4. Enables direct navigation to connected canonical resources and provenance citations.

```
┌────────────────────────────────────────────────────────────────────────┐
│               DUAL-PRESENTATION VISUALIZATION ARCHITECTURE             │
├────────────────────────────────────────────────────────────────────────┤
│                    CANONICAL BACKEND API PAYLOAD                       │
│             (B2 Data, B3 Knowledge Graph, B4 Provenance)               │
└──────────────────────────────────┬─────────────────────────────────────┘
                                   │
         ┌─────────────────────────┴─────────────────────────┐
         ▼                                                   ▼
┌─────────────────────────────────┐         ┌─────────────────────────────────┐
│ VISUAL GRAPHICAL PROJECTION     │         │ ACCESSIBLE SEMANTIC COMPANION   │
│ - Interactive Canvas / SVG      │         │ - Structured Semantic DOM Tree  │
│ - Spatial / Force layout        │◄───────►│ - Keyboard Navigable & Operable │
│ - Zoom, pan, visual halos       │  State  │ - Screen-Reader Accessible Text │
│ - Visual projection does not    │  Sync   │ - Direct Link Traversal         │
│   create a competing            │         │ - Zero Fabricated Data          │
│   inaccessible interaction path │         │                                 │
└─────────────────────────────────┘         └─────────────────────────────────┘
```

---

## 16. Graph Lens Accessibility

Aligning strictly with Graph Visualization Architecture (Block F10):

1. **Topology Scope**:
   - Reflects the canonical graph neighborhood parameters ($D \le 2$, $N \le 50$) without creating an alternate graph model.
2. **Accessible Semantic Representation**:
   - Structured as a hierarchical list or tree:
     - Focal entity identity and summary.
     - Grouped relationship categories as supplied by canonical relationship data.
     - Connected target entities, relationship labels, directionality, and canonical epistemic status.
3. **Interactive Parity**:
   - Activating a connected entity in the accessible companion enables identical exploratory actions to node activation:
     - Setting as new graph focus.
     - Navigating to the canonical entity destination governed by Block F7.
     - Inspecting backing provenance where canonical relationship and evidence data supports it.

---

## 17. Lineage / Family Tree Accessibility

Aligning strictly with Lineage / Family Tree Architecture (Block F11):

1. **Kinship Representation**:
   - The accessible representation communicates the generational structure derived from canonical parent-child relationships.
   - Structured as nested semantic lists or kinship trees:
     - Generational relationship to focal entity as derived from kinship traversal.
     - Marital unions and partner associations where supplied by canonical data.
     - Offspring and parentage links.
2. **Zero Inferred Kinship**:
   - Variant parentage claims are presented as explicit, distinct claim records.
   - The accessible view never invents intermediate generational nodes to bridge unrecorded lineage gaps.

---

## 18. Timeline & Chronology Accessibility

Aligning strictly with Timeline & Chronology Architecture (Block F12):

1. **Narrative Sequence Semantics**:
   - `sequence_index` represents canonical narrative progression across the epic narrative, **not** an absolute calendar date.
   - Accessible timeline items are presented as an ordered narrative sequence list (`<ol>`).
2. **Accessible Event Content**:
   - Exposes canonical event title and narrative sequence position.
   - Exposes participating entities, location associations, and narrative descriptions where supplied by canonical data.
   - Exposes canonical certainty and backing citation links where available.
   - Does not invent dates, date ranges, or historical timelines unless canonical data explicitly provides them.
3. **Sequential Navigation**:
   - Keyboard users can step sequentially through narrative events or navigate directly through the structured sequence listing.

---

## 19. Geographic Map Accessibility

Aligning strictly with Geographic Map Architecture (Block F13):

1. **Non-Spatial Parity**:
   - The accessible companion presents the same canonical location and entity associations represented by the visual map.
2. **Structured Location Directory**:
   - Structured listing or table containing:
     - Canonical Location Name (in the representation supplied by canonical data).
     - Associated narrative events and entities where supplied.
     - Canonical geographic status: `coordinate_status` exposes the canonical operational status defined by the geographic architecture (e.g., `unmapped`).
3. **Operational vs. Epistemic Separation**:
   - `coordinate_status` is an operational geographic status, not a project-wide epistemic state.
   - Epistemic status remains governed by the six project-wide states where applicable.
   - If modern identification is absent, conflicting, or unmapped, the accessible presentation communicates the canonical geographic status without inferring an explanation.

---

## 20. War & Tactical Vyuha Accessibility

Aligning strictly with War & Tactical Vyuha Architecture (Block F14):

1. **Fidelity Integrity**:
   - Accessibility must never make low-fidelity tactical information appear as high-precision metric coordinates.
   - The accessible representation exposes the same canonical information represented by the lens, without inventing troop numbers, movement paths, tactical outcomes, or battlefield perimeters.
2. **Three-Tier Fidelity Alignment**:
   - **Tier 1 (Descriptive)**: Renders canonical textual descriptions, faction affiliations, and participating warrior rosters where explicitly supplied by canonical data.
   - **Tier 2 (Schematic Order)**: Renders structured formation-role and topological outlines where supplied, without inventing metric spatial positions.
   - **Tier 3 (Authoritative Geometry)**: Renders accessible coordinate listings only when authoritative geometric data is explicitly supplied by canonical records.
3. **Neutral Tactical Scope**:
   - Accessible presentations describe participating groups, factions, and canonical war-day coverage without assuming uniform dual-army structures or unsupplied tactical metadata.

---

## 21. Color Independence & Visual Presentation

Aligning strictly with Design System Architecture (Block F3 §4 and §5):

1. **Normative Contrast Compliance**:
   - Text, UI components, and meaningful borders adhere to normative WCAG Level AA contrast requirements against adjacent surfaces across all supported themes.
2. **Non-Color Redundancy**:
   - Color is never used as the sole method of conveying information, indicating an action, prompting a response, or distinguishing a visual element.
   - Visual badges, status indicators, and faction affiliations are paired with:
     - Explicit textual labels.
     - Distinct iconography or typographic indicators.
     - Distinct shape or border treatments where visually presented.
3. **Focus Indicators**:
   - All interactive elements exhibit a visible focus indicator satisfying normative WCAG success criteria for focus appearance, ensuring clear distinction against adjacent backgrounds.

---

## 22. Motion & Sensory Characteristics

In alignment with system accessibility settings:

1. **Reduced Motion Adaptation**:
   - The application respects operating system and user preference settings for reduced motion (Block F5).
   - In reduced-motion mode:
     - Animated layout transitions, flight pans, and continuous force-directed canvas movements are disabled or replaced with immediate state transitions.
     - Overlays, drawers, and sheets appear without translation animations across the viewport.
     - Ambient or looping animations are suppressed.
2. **Sensory Independence**:
   - Instructions and navigation cues do not rely solely on shape, color, size, or spatial location (e.g., instructions must not say "click the round green button on the right").

---

## 23. Typography, Language & Multilingual Accessibility

1. **Language Identification**:
   - The root document establishes the primary language (`en`).
   - Where canonical metadata identifies Sanskrit passages or Devanāgarī text, explicit language tagging is provided to support correct screen-reader pronunciation.
2. **Character Encoding**:
   - UTF-8 encoding is enforced across all text streams to preserve IAST diacritical marks accurately. Assistive technologies must not strip or corrupt attested diacritics.
3. **No Frontend Transliteration Pipeline**:
   - Presentation uses canonical text representations as supplied by the backend and applicable project language architecture. F16 does not invent or assume client-side transliteration engines.
4. **Text Scaling**:
   - Content and typography scale smoothly up to 200% text enlargement without loss of content or functionality.

---

## 24. Responsive Layout & Reflow Invariants

Aligning with the four space-based layout classes established in Block F4 §4 (Compact, Medium, Expanded, Wide):

1. **Reflow Conformance**:
   - Content reflows without loss of information or functionality, and without requiring horizontal scrolling for vertical reading text, down to 320 CSS pixels width (corresponding to 400% zoom on a standard desktop display).
2. **Responsive Behavioral Parity**:
   - As layout components recompose across F4 viewport classes (such as slide-overs reflowing to bottom sheets, or navigation rails moving between sidebar and bottom bar):
     - Accessible landmark roles, names, and heading outlines remain stable.
     - Full keyboard operability and focus containment rules continue to apply.
     - Canonical information, controls, and available provenance inspectors represented by the active lens remain accessible.

---

## 25. Touch & Pointer Accessibility

1. **Target Sizing**:
   - Interactive targets satisfy normative WCAG Level AA target size requirements, ensuring controls can be easily activated on touch-enabled and fine-pointer surfaces.
2. **Pointer Gestures**:
   - Any functionality operable through multipoint or path-based gestures (such as canvas pinch-to-zoom or drag panning) is operable with a single-pointer alternative without dynamic gestures (e.g., dedicated zoom/pan buttons).
3. **Pointer Cancellation**:
   - For pointer activations, the down-event does not trigger unintended actions; completion occurs on up-event with options to abort by moving the pointer away.

---

## 26. URL, Deep-Linking & Route Navigation Accessibility

Aligning strictly with Routing & Navigation Architecture (Block F7):

1. **Document Title Updates**:
   - Every route transition updates the document `<title>` to convey current resource context and lens identity clearly.
2. **Logical Focus Placement**:
   - Client-side navigation places keyboard focus predictably at the primary workspace heading or container, preventing lost focus states.
3. **State Hydration**:
   - Entering the application via a canonical deep link hydrates both the visual lens and the accessible companion with identical state.
4. **URL Authority**:
   - Block F7 remains the sole authority for route definitions and query parameter grammar.

---

## 27. Zero Fabrication in Accessibility

Constitutional Rule 04 applies unconditionally to accessibility:

> **Accessibility mechanisms may reorganize, label, summarize, and describe canonical data for non-visual consumption, but they must NEVER invent or fabricate factual information.**

```
┌────────────────────────────────────────────────────────────────────────┐
│                   ZERO-FABRICATION ACCESSIBILITY RULES                 │
├───────────────────────────────────┬────────────────────────────────────┤
│ PERMITTED ACCESSIBILITY BEHAVIORS │ STRICTLY PROHIBITED BEHAVIORS      │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Organizing canonical entities   │ - Fabricating troop numbers, army  │
│   into structured semantic lists. │   sizes, or tactical casualties.   │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Exposing relationship tags and  │ - Inferring unrecorded parentage   │
│   directionality from graph data. │   to fill genealogical gaps.       │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Communicating canonical states: │ - Inventing narrative reasons why  │
│   `unknown`, `not_researched`.    │   an ancient source omits data.    │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Describing canonical excerpts   │ - Generating synthetic text or     │
│   with native locators.           │   verses to fill missing citations.│
└───────────────────────────────────┴────────────────────────────────────┘
```

---

## 28. Security & Untrusted Content in Accessible Surfaces

1. **Data Escaping**:
   - All text rendered into accessible descriptions, ARIA attributes, and live status messages is treated strictly as data and escaped to prevent script injection.
2. **Attribute Integrity**:
   - Dynamic values interpolated into accessible names and descriptions are sanitized to prevent attribute-breakout vulnerabilities.
3. **Outbound Citation Links**:
   - External links to bibliographic repositories enforce secure window isolation without leaking opener context.

---

## 29. Accessibility Testing & Verification Architecture

Block F16 establishes **WHAT** capabilities and criteria must be verified. Automated test frameworks, CI pipelines, and performance budgets are governed by Block F17:

```
┌────────────────────────────────────────────────────────────────────────┐
│                  ACCESSIBILITY VERIFICATION SCOPE                      │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Verification Tier│ Scope & Focus Area                                  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **1. Automated   │ - Landmark hierarchy and valid semantic outline.    │
│    Structural    │ - Valid ARIA roles, states, and relationships.      │
│    Validation**  │ - Non-empty accessible names on all controls.       │
│                  │ - Contrast compliance across all themes.            │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **2. Keyboard    │ - Non-pointer traversability across all views.      │
│    Operability   │ - Focus placement, containment, and restoration.    │
│    Verification**│ - Escape key dismissal of transient surfaces.       │
│                  │ - Zero keyboard traps across all interactive tools. │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **3. Screen-     │ - Readout verification with major assistive tools.  │
│    Reader        │ - Accurate rendering of IAST and Sanskrit passages. │
│    Audits**      │ - Polite dynamic announcements on state changes.    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **4. Reflow &    │ - 200% text enlargement without clipping.           │
│    Visual Zoom   │ - 400% zoom reflow without horizontal scrolling.    │
│    Audits**      │ - Full operational parity of accessible companion   │
│                  │   structures across all supported viewports.        │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 30. Traceable Requirements

```
┌────────────────────────────────────────────────────────────────────────┐
│                    ACCESSIBILITY REQUIREMENTS LIST                     │
├───────────────┬──────────────────┬─────────────────────────────────────┤
│ Requirement ID│ Source Document  │ Architectural Description           │
├───────────────┼──────────────────┼─────────────────────────────────────┤
│ **REQ-A11Y-01**│ Constitutional   │ Normative WCAG 2.2 Level AA         │
│               │ Rule 10; PRD §9.1│ conformance target across all routes│
├───────────────┼──────────────────┼─────────────────────────────────────┤
│ **REQ-A11Y-02**│ PRD §9.13; F4 §2 │ Semantic landmarks and heading      │
│               │                  │ hierarchy across shell & workspace. │
├───────────────┼──────────────────┼─────────────────────────────────────┤
│ **REQ-A11Y-03**│ PRD §9.13; F4 §8 │ Complete non-pointer operability    │
│               │                  │ with zero keyboard traps.           │
├───────────────┼──────────────────┼─────────────────────────────────────┤
│ **REQ-A11Y-04**│ PRD §9.13; F4 §9 │ Focus containment and restoration   │
│               │                  │ across contextual modal surfaces.   │
├───────────────┼──────────────────┼─────────────────────────────────────┤
│ **REQ-A11Y-05**│ PRD §9.13; F7 §3 │ SPA route transition focus and page │
│               │                  │ title announcement management.      │
├───────────────┼──────────────────┼─────────────────────────────────────┤
│ **REQ-A11Y-06**│ PRD §9.13; F3 §5 │ Non-color redundancy for all status,│
│               │                  │ relationship, and epistemic states. │
├───────────────┼──────────────────┼─────────────────────────────────────┤
│ **REQ-A11Y-07**│ PRD §9.13; F10–14│ Accessible semantic companion for   │
│               │                  │ all graphical visualizers.          │
├───────────────┼──────────────────┼─────────────────────────────────────┤
│ **REQ-A11Y-08**│ PRD §9.13; F15 §8│ Full accessibility of 4-tier        │
│               │                  │ provenance and citation structures. │
├───────────────┼──────────────────┼─────────────────────────────────────┤
│ **REQ-A11Y-09**│ PRD §9.13; F5 §4 │ Reduced-motion mode honoring system │
│               │                  │ and user preference settings.       │
├───────────────┼──────────────────┼─────────────────────────────────────┤
│ **REQ-A11Y-10**│ Constitutional   │ Zero fabrication in text-equivalent │
│               │ Rule 04; B4 §1   │ accessibility data representations. │
└───────────────┴──────────────────┴─────────────────────────────────────┘
```

---

## 31. Comprehensive Ownership Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                        SYSTEM OWNERSHIP MATRIX                         │
├──────────────────────────┬──────────────────┬──────────────────────────┤
│ Architectural Domain     │ Primary Owner    │ Governing Document       │
├──────────────────────────┼──────────────────┼──────────────────────────┤
│ Accessibility Arch.      │ Block F16        │ 15-accessibility-arch.md │
│ Visual Design Tokens     │ Block F3         │ 02-design-system.md      │
│ Shell & Viewport Classes │ Block F4         │ 03-responsive-layout.md  │
│ State Management Arch.   │ Block F5         │ 04-state-management.md   │
│ Data Fetching & Caching  │ Block F6         │ 05-api-client-caching.md │
│ Routing & Deep-Linking   │ Block F7         │ 06-routing-navigation.md │
│ Search Information Arch. │ Block F8         │ 07-search-autocomplete.md│
│ Entity Views IA          │ Block F9         │ 08-entity-views.md       │
│ Graph Visualization Arch.│ Block F10        │ 09-graph-visualization.md│
│ Lineage DAG Arch.        │ Block F11        │ 10-lineage-family-tree.md│
│ Timeline Chronology Arch.│ Block F12        │ 11-timeline-chronology.md│
│ Geographic Map Arch.     │ Block F13        │ 12-geographic-map.md     │
│ War & Tactical Vyuha Arch│ Block F14        │ 13-war-tactical-vyuha.md │
│ Evidence & Provenance IA │ Block F15        │ 14-evidence-provenance.md│
│ Quantitative Perf & Test │ Block F17        │ 16-performance-testing.md│
└──────────────────────────┴──────────────────┴──────────────────────────┘
```

---

## 32. Architectural Decision Records (ADRs)

### ADR-F16-01: Native Platform Semantics Priority
- **Context**: Rich Single Page Applications often rely excessively on generic layout containers paired with synthetic ARIA scripts, creating brittle and inconsistent assistive technology experiences.
- **Decision**: Prioritize native platform semantic elements for document landmarks, text blocks, lists, and interactive controls, introducing ARIA strictly to supplement dynamic state changes and relationships.
- **Consequences**: Ensures robust accessibility across assistive tools, simplifies ongoing maintenance, and minimizes assistive technology interpretation errors.

### ADR-F16-02: Synchronized Semantic Companion Architecture for Visualizations
- **Context**: The Explorer features dense visual projections (Graph, Lineage DAG, Map, Timeline, Vyuhas) that cannot be rendered perceivable to assistive technology users simply by attaching ARIA attributes to canvas pixels.
- **Decision**: Every graphical visualizer must be paired with an accessible semantic companion derived from the exact same canonical backend data payload.
- **Consequences**: Achieves genuine informational and operational parity for non-visual users without compromising high-performance graphical rendering for visual exploration.

### ADR-F16-03: Zero-Fabrication Integrity in Accessible Text
- **Context**: Accessibility implementations can sometimes interpolate synthetic dates, assumed family connections, or inferred reasons for missing data to make non-visual narratives appear complete.
- **Decision**: Strictly enforce Constitutional Rule 04 within accessibility: missing information is exposed directly via canonical states (`unknown`, `not_researched`); zero synthetic facts or explanations are generated.
- **Consequences**: Preserves epistemic integrity and academic credibility across all accessibility access channels.

### ADR-F16-04: Deterministic Route Transition Focus Management
- **Context**: Client-side single page navigation can strand keyboard focus in detached nodes or leave screen-reader users unaware that page context has changed.
- **Decision**: Establish coordinated post-navigation focus behavior: document title updates dynamically, focus shifts logically to the primary workspace landmark or heading, and a polite status update confirms route change.
- **Consequences**: Provides seamless mental orientation and predictable navigation flow across lens transitions.

---

## 33. Deferred Implementation Decisions

To preserve architectural focus, concrete implementation details are explicitly deferred to Stage 4:
1. **Concrete Accessibility Utility Libraries**: Specific focus-trap libraries, floating-ui primitives, or headless ARIA packages are deferred to implementation.
2. **Automated Test Harness Selection**: Specific test runners and CI pipeline configurations are governed by Block F17 and implemented in Stage 4.
3. **CSS Class Implementation**: Specific styling classes, utilities, and media-query tokens for focus rings are implemented in Stage 4 per Block F3 rules.
4. **Concrete Component Wiring**: DOM markup implementation within individual view components belongs to Stage 4.

---

## 34. Performance & Quantitative Budget Boundaries

In strict compliance with system architecture boundaries:
- Block F16 **does not introduce numerical performance budgets** (zero frame-rate targets, response-time milliseconds, bundle-size limits, or paint thresholds).
- Quantitative performance thresholds, memory consumption limits, and automated audit criteria are authoritatively owned and governed by **Block F17 (Performance & Testing Architecture)**.
- F16 requires accessible companion structures to remain usable and semantically equivalent regardless of the rendering strategy selected during implementation. Performance-sensitive rendering strategies and quantitative thresholds are deferred to F17 and Stage 4, provided they preserve accessibility and canonical-data parity.

---

## 35. F16 Acceptance Criteria

```
┌────────────────────────────────────────────────────────────────────────┐
│                      ACCEPTANCE CRITERIA STATUS                        │
├────────────────────────────────────────────────────────────────────────┤
│ Note: Architecture defined below; final acceptance pending review.     │
└────────────────────────────────────────────────────────────────────────┘
```

- [x] Architecture defined; final acceptance pending review: Authoritative accessibility architecture established across all application lenses and shell regions.
- [x] Architecture defined; final acceptance pending review: Normative conformance target defined as WCAG 2.2 Level AA without synthetic internal scoring systems.
- [x] Architecture defined; final acceptance pending review: Native platform semantics prioritized over synthetic ARIA across landmarks, headings, lists, and controls.
- [x] Architecture defined; final acceptance pending review: Complete non-pointer operability and keyboard access defined for all application pathways.
- [x] Architecture defined; final acceptance pending review: Focus entry, logical order, containment, restoration, and route navigation focus behaviors architected.
- [x] Architecture defined; final acceptance pending review: F4 contextual surfaces (modals, drawers, sheets, popovers) mapped to accessible roles and keyboard behaviors.
- [x] Architecture defined; final acceptance pending review: Dynamic content and status updates specified with strict anti-flooding constraints.
- [x] Architecture defined; final acceptance pending review: Request lifecycle states cleanly separated from canonical epistemic states in accessible text.
- [x] Architecture defined; final acceptance pending review: Search combobox and autocomplete accessibility aligned with Block F8.
- [x] Architecture defined; final acceptance pending review: Entity views, facets, and cross-lens launchpoints architected with semantic hierarchy per Block F9.
- [x] Architecture defined; final acceptance pending review: Four-tier provenance model (`Entity/Edge → Claim → Evidence → Source`) preserved without flattening per Block F15.
- [x] Architecture defined; final acceptance pending review: Dual-presentation semantic companion architecture defined for all graphical visualizers (Graph, Lineage, Timeline, Map, War, Vyuha).
- [x] Architecture defined; final acceptance pending review: Non-color redundancy enforced across all epistemic states, relationships, and interactive controls per Block F3.
- [x] Architecture defined; final acceptance pending review: Reduced-motion architecture defined honoring system and user preference settings per Block F5.
- [x] Architecture defined; final acceptance pending review: Multilingual text, IAST diacritics, and Sanskrit script accessibility architected without frontend transliteration pipelines.
- [x] Architecture defined; final acceptance pending review: Responsive accessibility parity preserved across all four Block F4 viewport classes.
- [x] Architecture defined; final acceptance pending review: Touch and pointer accessibility requirements defined without arbitrary numerical prescriptions.
- [x] Architecture defined; final acceptance pending review: Zero-fabrication principle strictly enforced: accessible alternatives derive from canonical data with zero invented facts or inferred reasons.
- [x] Architecture defined; final acceptance pending review: Security protections defined for untrusted text and attributes in accessible DOM nodes.
- [x] Architecture defined; final acceptance pending review: Traceable requirements table established (`REQ-A11Y-01` through `REQ-A11Y-10`).
- [x] Architecture defined; final acceptance pending review: Zero application source code, UI packages, or concrete component libraries selected or introduced.
- [x] Architecture defined; final acceptance pending review: Zero Stage 1 backend documents or Blocks F1–F15 documents modified.
