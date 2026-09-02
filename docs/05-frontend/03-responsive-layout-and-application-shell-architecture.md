# Responsive Layout & Application Shell Architecture (Block F4)

## 1. Architectural Context

This document establishes the **Responsive Layout & Application Shell Architecture** for the **Mahābhārata Explorer** frontend. It defines the structural layout hierarchy, navigation frame, workspace composition, responsive reflow behavior, orientation adaptations, contextual surface lifecycle, scroll ownership boundaries, and accessibility structure for Stage 2.

```
┌────────────────────────────────────────────────────────────────────────┐
│                   STAGE 2 ARCHITECTURAL PLACEMENT                      │
├───────────────────────────────────┬────────────────────────────────────┤
│ UPSTREAM CONTRACTS & CONSTRAINTS  │ DOWNSTREAM FRONTEND SPECIFICATIONS │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Stage 1 Backend Architecture    │ - F5: State Management             │
│   (B2 Data, B3 Graph, B5 API,     │ - F6: API Client & Data Fetching   │
│    B7 Auth, B8 Media, B9 Perf)    │ - F7: Routing & Navigation UX      │
│ - Block F1: Frontend Constitution │ - F8: Global Search UX             │
│ - Block F2: Tech Stack & Build    │ - F9: Character & Entity Views     │
│   (React 19+, TypeScript, Vite,   │ - F10–F14: Visualizations          │
│    SPA, Three-Tier Rendering)     │ - F15: Evidence & Provenance UX    │
│ - Block F3: Design System & Tokens│ - F16: Accessibility & IAST        │
│   (Semantic Tokens, Visual Rules) │ - F17: Testing & Budgets           │
└───────────────────────────────────┴────────────────────────────────────┘
```

### 1.1 Pure Architectural Specification
Block F4 is strictly an architectural specification. It establishes the spatial layout organization, responsive state transitions, and shell rules. It does not create CSS stylesheets, write React JSX component code, configure CSS frameworks, or implement route handlers.

---

## 2. Application Shell Architecture

The application shell provides the durable structural frame within which all exploration lenses, canonical profiles, and interactive visualizations operate.

```
┌────────────────────────────────────────────────────────────────────────┐
│                       GLOBAL SHELL HIERARCHY                           │
├────────────────────────────────────────────────────────────────────────┤
│ 1. GLOBAL FEEDBACK & SYSTEM LAYER (z-index: system)                    │
│    - Global offline / rate-limit alert toasts                          │
│    - Global loading progress indicator                                 │
├────────────────────────────────────────────────────────────────────────┤
│ 2. OVERLAY LAYER (z-index: overlay)                                   │
│    - Global Search Surface (Interaction & UX in F8)                    │
│    - Contextual Modals & Dialogs                                       │
├────────────────────────────────────────────────────────────────────────┤
│ 3. CONTEXTUAL & DETAIL LAYER (z-index: contextual)                     │
│    - Evidence & Provenance Surfaces (UX in F15)                        │
│    - Contextual Node / Entity Inspector Panels                         │
├────────────────────────────────────────────────────────────────────────┤
│ 4. PRIMARY APPLICATION FRAME (z-index: base)                           │
│    ├── Persistent Header Bar (Brand, Search trigger, Lens tabs, Theme) │
│    ├── Navigation Rail / Sidebar (Lens switcher on expanded layouts)   │
│    └── Content / Workspace Region (Active exploration lens / canvas)   │
└────────────────────────────────────────────────────────────────────────┘
```

### 2.1 Shell Region Responsibilities
1. **Header Bar**: Provides persistent brand orientation, entry point to global search, primary lens switching (or navigation menu trigger), and user preference controls (Light/Dark mode).
2. **Navigation Rail / Sidebar**: Dedicated structural lens navigation on medium-to-wide layouts, collapsible to maximize exploratory workspace area.
3. **Exploration Workspace**: The primary viewport region where the active lens (Character Profile, Knowledge Graph, Map, Timeline, War Day) renders.
4. **Contextual Drawer / Panel**: Secondary surface used for progressive disclosure (e.g., scholarly citations, verse locators, relationship inspector) without navigating away from the active exploration lens.
5. **Overlay Surface**: Focus-trapped dialogs and global search overlays.

---

## 3. Navigation Architecture

Navigation in the Mahābhārata Explorer is organized around **dimensional exploration lenses** rather than isolated database CRUD silos:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        NAVIGATION TOPOLOGY                             │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Navigation Tier  │ Purpose & Behavioral Contract                       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **1. Structural  │ Global lens switching (Characters, Lineage, Graph,  │
│ Navigation**     │ Timeline, Map, Wars, Vyuhas, Sources). Persistent   │
│                  │ across exploration; updates URL path via F7 router. │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **2. Contextual  │ In-lens sub-navigation (e.g., War Day stepper,      │
│ Navigation**     │ Parva timeline scrubbers, Character profile tabs).  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **3. Entity Deep │ Inter-entity graph jumps (clicking an entity node   │
│ Navigation**     │ navigates to the corresponding entity route defined │
│                  │ by Block F7).                                       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **4. Transient   │ Filter dropdowns, panel toggles, zoom/pan states.   │
│ UI Navigation**  │ Local interaction that does not break URL state.    │
└──────────────────┴─────────────────────────────────────────────────────┘
```

*(Note: Concrete URL route hierarchies, query parameter serializations, and path syntax are formally specified in Block **F7 (Routing, Navigation & Deep-Linking Architecture)**).*

---

## 4. Space-Based Responsive Layout Model

Rather than targeting physical hardware brands, the responsive architecture defines **four space-based layout classes** governed by available horizontal and vertical layout dimensions:

```
┌────────────────────────────────────────────────────────────────────────┐
│                     SPACE-BASED VIEWPORT TAXONOMY                      │
├──────────────┬──────────────────┬──────────────────────────────────────┤
│ Class Name   │ Layout Paradigm  │ Shell Composition Architecture       │
├──────────────┼──────────────────┼──────────────────────────────────────┤
│ **Compact**  │ Single-Column    │ Minimal header + bottom-anchored     │
│ (Illustrative:│ Stacked          │ navigation/lens bar; contextual info │
│ Mobile)      │                  │ opens in bottom sheets; search opens │
│              │                  │ in dedicated overlay.                │
├──────────────┼──────────────────┼──────────────────────────────────────┤
│ **Medium**   │ Adaptive Two-    │ Header + compact icon nav rail;      │
│ (Illustrative:│ Pane / Overlay   │ side-by-side or slide-over surface;  │
│ Tablet)      │                  │ collapsible panels for graph/map.    │
├──────────────┼──────────────────┼──────────────────────────────────────┤
│ **Expanded** │ Persistent Two-  │ Full header + icon/label sidebar;    │
│ (Illustrative:│ Pane Workspace   │ side-by-side workspace and persistent│
│ Laptop/PC)   │                  │ contextual inspector side-panel.     │
├──────────────┼──────────────────┼──────────────────────────────────────┤
│ **Wide**     │ Multi-Pane Studio│ Multi-column layout: Nav rail +      │
│ (Illustrative:│                  │ primary visualization + persistent   │
│ Ultra-wide)  │                  │ metadata sidebar + evidence surface. │
└──────────────┴──────────────────┴──────────────────────────────────────┘
```

---

## 5. Principled Breakpoint Strategy

Breakpoints represent **behavioral reflow boundaries** where the UI structural composition changes to preserve exploration usability:

```
┌────────────────────────────────────────────────────────────────────────┐
│                       BREAKPOINT ARCHITECTURE                          │
├──────────────────────────┬─────────────────────────────────────────────┤
│ Transition Boundary      │ Behavioral Layout Transformation            │
├──────────────────────────┼─────────────────────────────────────────────┤
│ **Boundary 1 (Compact)** │ Bottom-nav bar $\leftrightarrow$ Collapsed  │
│                          │ Navigation Rail; bottom sheet $\leftrightarrow$│
│                          │ slide-over drawer.                          │
├──────────────────────────┼─────────────────────────────────────────────┤
│ **Boundary 2 (Medium)**  │ Collapsed Rail $\leftrightarrow$ Expanded   │
│                          │ Sidebar; slide-over drawer $\leftrightarrow$│
│                          │ persistent contextual inspector.            │
├──────────────────────────┼─────────────────────────────────────────────┤
│ **Boundary 3 (Expanded)**│ Persistent Two-Pane $\leftrightarrow$ Multi- │
│                          │ Pane Studio layout.                         │
└──────────────────────────┴─────────────────────────────────────────────┘
```

*Architectural Invariant vs. Implementation Guidance*:
- **Architectural Invariant**: The shell must provide discrete transitions between Single-Column (Stacked/Sheet), Two-Pane (Slide-over/Split), and Multi-Pane (Studio) layouts.
- **Implementation Guidance**: Exact pixel values are implementation parameters configured in Tailwind/CSS tokens during Stage 4.

---

## 6. Responsive Reflow & Content Priority Rules

When available layout width decreases, the shell enforces strict content preservation rules:

```
┌────────────────────────────────────────────────────────────────────────┐
│                     RESPONSIVE CONTENT REFLOW RULES                    │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Layout Region    │ Reflow & Compression Behavior                       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Primary Nav**  │ Compresses from Text Sidebar $\rightarrow$ Icon     │
│                  │ Rail $\rightarrow$ Bottom Navigation Bar.           │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Search Bar**   │ Accommodates the responsive search interaction      │
│                  │ defined by Block F8.                                │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Contextual     │ Promotes from Persistent Split-Pane $\rightarrow$   │
│ Panels**         │ Slide-over Right Drawer $\rightarrow$ Bottom Sheet. │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Data Tables**  │ Adapt from Multi-Column Tables $\rightarrow$        │
│                  │ Stacked Property-Value Entity Cards.                │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Visualizations**│ Canvas/SVG expands to fill available workspace;     │
│                  │ floating controls collapse into overlay menus.      │
└──────────────────┴─────────────────────────────────────────────────────┘
```

### 6.1 Content Deference Hierarchy
When layout space is constrained, the shell establishes structural hierarchy rules:
1. **Primary Content**: Primary task or content required for understanding the active exploration state (e.g., active entity name, core narrative attributes, verified epistemic status).
2. **Supporting Contextual Information**: Secondary relationships, sub-branches, and navigational scrubbers (promoted to collapsible surfaces).
3. **Extended Detail & Supplementary Evidence**: Extended citations, full bibliographical data, and raw metadata records (accessed via on-demand contextual drawers or sheets).
*(Note: Detailed content prioritization within individual exploration lenses is determined by their respective lens blocks).*

---

## 7. Orientation & Constrained-Height Architecture

Responsive behavior must account for screen orientation and vertical constraints:

```
┌────────────────────────────────────────────────────────────────────────┐
│                      ORIENTATION ADAPTATIONS                           │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Viewport State   │ Architectural Adaptation                            │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Portrait       │ Vertical reading flow: Entity summary at top,       │
│ Flow**           │ expandable relationship tabs below, bottom-anchored │
│                  │ lens navigation, full-height modal overlays.        │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Landscape      │ Horizontal workspace flow: Minimalist top header,   │
│ Flow**           │ collapsed left-side icon rail, side-by-side         │
│                  │ workspace and mini-inspector to maximize height.    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Constrained    │ Dynamic Viewport Height (`dvh`) units prevent mobile│
│ Height (Short)** │ browser address bar clipping. Floating toolbars     │
│                  │ compress padding; sticky headers remain ultra-slim. │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 8. Workspace Composition Models

Exploration lenses organize their viewports using four standardized architectural workspace models:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        WORKSPACE COMPOSITIONS                          │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Workspace Model  │ Description & Lens Application (Routes in F7)       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Model A:       │ Standard editorial reading flow with sticky sub-nav.│
│ Editorial Flow** │ Used for Character Profiles, Groups, and Sources.   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Model B:       │ Viewport-filling interactive canvas with floating   │
│ Immersive Canvas**│ HUD controls and contextual side-drawer. Used for    │
│                  │ Knowledge Graph and Geographic Map lenses.          │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Model C:       │ Master-detail layout (scrubber/stepper + content).  │
│ Split Stepper**  │ Used for War Explorer and Timeline Chronology.      │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Model D:       │ Centered tactical diagram with synchronized text    │
│ Diagram Explorer**│ description and event links. Used for Vyuhas.       │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 9. Contextual Surfaces: Panels, Drawers & Sheets

Contextual surfaces support progressive disclosure without navigation disruption:

```
┌────────────────────────────────────────────────────────────────────────┐
│                    CONTEXTUAL SURFACE TAXONOMY                         │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Surface Type     │ Architectural Scope & Placement                     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Slide-Over     │ Bounded right-anchored panel (width refined in      │
│ Drawer**         │ Stage 4). Accommodates Evidence (F15), Node         │
│                  │ Inspector, and Filter Panes on expanded layouts.    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Bottom Sheet** │ Bottom-anchored sliding sheet on compact layouts.   │
│                  │ Supports drag-to-dismiss and snap-points            │
│                  │ (half-height summary $\leftrightarrow$ full-height).│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Modal Dialog** │ Centered overlay dialog with backdrop dimming and   │
│                  │ strict focus containment. Accommodates Global       │
│                  │ Search (F8), Confirmation dialogs, and Lightboxes.  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Popover / HUD**│ Floating lightweight contextual card anchored to a  │
│                  │ graph node, map marker, or citation pill.           │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 10. Scroll Ownership & Virtualization Boundaries

To avoid nested-scroll trapping (where inner elements swallow page gestures), scroll ownership is strictly partitioned:

```
┌────────────────────────────────────────────────────────────────────────┐
│                       SCROLL OWNERSHIP MODEL                           │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Layout Region    │ Scroll Behavior & Containment Rule                  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Application    │ `overflow: hidden` on root to prevent window-level  │
│ Frame**          │ bounce/jitter during canvas interaction.            │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Editorial Flow │ `overflow-y: auto` on workspace container. Owns the │
│ Workspace**      │ primary vertical scrollbar with smooth momentum.    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Active Canvas  │ Visualization surfaces own internal gestures (zoom, │
│ Viewports**      │ pan) when active; surrounding page scrolls where    │
│                  │ the compositional model allows.                     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Drawers /      │ Isolated `overflow-y: auto` inside drawer content;  │
│ Sheets**         │ scrolling background is locked while open.          │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Virtual Lists**│ Dedicated windowed scroll viewport for large        │
│                  │ event feeds and search result collections.          │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 11. Touch, Pointer & Interaction Ergonomics

The application shell accommodates both coarse touch and fine pointer modalities:
1. **Coarse Pointer (Touch) Ergonomics**:
   - Interactive touch targets maintain a minimum hit area of **$\ge 44 \times 44\text{ px}$**.
   - Generous spacing between adjacent navigation links to prevent accidental miss-clicks.
   - Shell provides appropriate layout clearance for touch interactions across all exploration lenses.
2. **Fine Pointer (Mouse) Ergonomics**:
   - Compact table density, instant hover tooltips, and keyboard accelerator hints.
*(Note: Detailed gesture algorithms, physics simulations, and engine-specific interactions belong to Blocks **F10 through F14**).*

---

## 12. Accessibility & Semantic Shell Structure

Block F4 establishes the structural foundation for application accessibility:

```
┌────────────────────────────────────────────────────────────────────────┐
│                      SEMANTIC LANDMARK STRUCTURE                       │
├────────────────────────────────────────────────────────────────────────┤
│ `<div id="app-root">`                                                  │
│   ├── `<a href="#main-content" class="skip-link">Skip to Content</a>`  │
│   ├── `<header role="banner">` (Global Brand, Search trigger)          │
│   ├── `<nav aria-label="Exploration Lenses">` (Lens switcher)          │
│   ├── `<main id="main-content" role="main">` (Active Lens Workspace)   │
│   │     └── `<section aria-labelledby="...">`                          │
│   ├── `<aside aria-label="Evidence and Details" aria-hidden="...">`    │
│   └── `<footer role="contentinfo">` (Scholarly source attribution)     │
└────────────────────────────────────────────────────────────────────────┘
```

### 12.1 Focus & Keyboard Navigation Flow
1. **Skip Mechanisms**: Prominent "Skip to Main Content" link appears on initial `Tab` keypress, bypassing navigation bars.
2. **Logical Focus Order**: Focus order strictly follows the logical semantic and interaction flow of the active composition rather than purely arbitrary visual coordinates.
3. **Modal & Drawer Focus Trapping**: When an overlay modal or slide-over drawer opens, keyboard focus moves to the first focusable element inside the panel; `Escape` key immediately closes the surface and restores focus to the invoking trigger.
*(Note: Detailed accessibility architecture is specified in Block **F16**; empirical accessibility verification is owned by Block **F17**).*

---

## 13. Shell-Level Loading, Error & Empty States

The shell maintains structural stability during data-fetching lifecycles:
1. **Shell Skeleton Loading**: While an exploration lens loads its primary API payload, the persistent header and navigation frame remain intact; the workspace displays matching geometric skeleton placeholders.
2. **Global Error Boundary**: If an uncaught rendering error occurs within an exploration lens, the global shell isolates the failure, displaying a recovery card without crashing the global navigation header.
3. **Empty Lens Workspace**: Displays a clear, culturally restrained empty state with guidance prompts when active filters return zero results.

---

## 14. Visualization Shell Accommodation

Large-scale visual viewports (Knowledge Graph, Map, Timeline, Vyuhas) require dedicated shell accommodation:
1. **Full-Bleed Canvas Mode**: Canvas viewports occupy $100\%$ width and height of the workspace region (`radius-none`).
2. **Floating HUD Controls**: Zoom controls, depth selectors, and legend toggles float over the canvas in accessible overlay cards.
3. **Viewport Resize Observers**: Shell triggers resize events when contextual drawers open or close, allowing visualization engines to update projection matrices smoothly.
*(Note: Concrete visualization rendering technologies and layout algorithms are governed by Blocks **F10 through F14**).*

---

## 15. Deep-Linking Shell Compatibility

In accordance with [00-frontend-architecture-context.md §12](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/05-frontend/00-frontend-architecture-context.md#L210) and **REQ-STA-01**:
1. **URL-Driven Surface Hydration**: The shell reads URL state upon initial load to mount the correct exploration lens, set active tab states, and open contextual drawers if deep-linked.
2. **Stateless Navigation**: Direct entry or browser refresh at any deep link reconstructs the exact workspace layout and open drawer state without requiring client-side session state.

---

## 16. Performance-Aware Layout Architecture

To satisfy Stage 1 performance standards (B9 alignment):
1. **Architectural Layout Principles**:
   - Minimize unnecessary layout coupling across multi-pane views.
   - Avoid unnecessary shell-wide rendering upon local component state updates.
   - Isolate expensive visual and contextual panel regions into independent rendering trees.
2. **Implementation Guidance (Refined during Stage 4)**:
   - Hardware-accelerated transitions via CSS `transform` and `opacity` for drawer and modal animations.
   - Off-screen panel containment leveraging modern CSS containment (`contain: layout style`) and `content-visibility: auto`.
*(Note: Formal numeric frontend performance budgets and verification suites are defined in Block **F17**).*

---

## 17. Upstream Dependency & Downstream Ownership Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                     DOWNSTREAM OWNERSHIP MATRIX                        │
├────────────────────────────────────────┬───────────────────────────────┤
│ Architectural Domain                   │ Authoritative Target Block    │
├────────────────────────────────────────┼───────────────────────────────┤
│ Client State Management & Store Model  │ **Block F5**                  │
│ API Client, Data Fetching & Caching    │ **Block F6**                  │
│ Route Syntax, URL Schema & Router      │ **Block F7**                  │
│ Global Search Modal & Command UX       │ **Block F8**                  │
│ Character Profile Layouts & Tabs       │ **Block F9**                  │
│ Knowledge Graph Visualization Engine   │ **Block F10**                 │
│ Family Lineage Tree DAG Layout         │ **Block F11**                 │
│ Timeline Scrubber & Chronology Engine  │ **Block F12**                 │
│ Geographic Map Visualization Engine    │ **Block F13**                 │
│ Tactical Battle Formations (Vyuhas)    │ **Block F14**                 │
│ Evidence Drawer Citation & Text UX     │ **Block F15**                 │
│ Accessibility Architecture & IAST/Dev  │ **Block F16**                 │
│ Performance Budgets & A11y Verification│ **Block F17**                 │
│ Concrete Component JSX & CSS Tokens    │ **Stage 4 (Implementation)**  │
└────────────────────────────────────────┴───────────────────────────────┘
```

---

## 18. Architectural Decision Record (ADR)

| Decision ID | Architectural Decision | Chosen Approach | Alternatives Evaluated | Rationale & Trade-offs | Owner | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **ADR-FE-18** | Global Shell Structure | **Four-Tier Layered Frame** | Monolithic single-pane, multi-window floating | Clean separation between system, overlay, contextual, and base workspace layers. | F4 | **DECIDED** |
| **ADR-FE-19** | Responsive Layout Model| **Space-Based 4-Class Viewport**| Device-specific hardcoding | Delivers first-class layout parity across compact, medium, expanded, and wide spaces. | F4 | **DECIDED** |
| **ADR-FE-20** | Breakpoint Strategy | **Behavioral Transition Scale**| Fixed pixel assumptions | Focuses on structural reflow boundaries rather than chasing device models. | F4 | **DECIDED** |
| **ADR-FE-21** | Contextual Surfaces | **Drawers / Bottom Sheets**     | Inline accordion, separate page nav | Preserves exploration context while progressively disclosing deep citations. | F4 | **DECIDED** |
| **ADR-FE-22** | Scroll Ownership | **Isolated Scroll Containers**  | Global window scrolling | Prevents scroll trapping on dense lists and active visualization viewports. | F4 | **DECIDED** |
| **ADR-FE-23** | Touch Ergonomics | **$\ge 44\text{px}$ Hit Targets**| Desktop-only click targets | Ensures smooth, error-free touch navigation on touch-enabled viewports. | F4 | **DECIDED** |
| **ADR-FE-24** | Semantic Landmarks | **HTML5 Landmarks + Skip Links** | Generic `<div>` soup | Provides the structural foundation for project accessibility requirements. | F4 | **DECIDED** |

---

## 19. Requirement Traceability Matrix

| Requirement / Principle | Source Document | Implementing Block F4 Section | Coverage Status |
| :--- | :--- | :--- | :--- |
| **Knowledge Explorer First** | F1 §5.1, PRD §2 | §2 (Shell Architecture), §8 (Workspace Models) | **SATISFIED** |
| **One Unified Graph, Many Lenses**| F1 §5.2, B3 §1 | §3 (Navigation Architecture), §8 (Workspace) | **SATISFIED** |
| **Evidence-Aware UI** | F1 §5.3, B4 §1 | §9 (Contextual Surfaces), §15 (Deep Linking) | **SATISFIED** |
| **Responsive by Design** | F1 §5.5, Rule 09 | §4 (Space-Based Viewports), §6 (Reflow Rules)| **SATISFIED** |
| **Progressive Disclosure** | F1 §5.6, B9 §5 | §6.1 (Deference Hierarchy), §9 (Drawers) | **SATISFIED** |
| **Stateless Deep Linking** | F1 §5.8, B5 §5 | §15 (Deep-Linking Compatibility) | **SATISFIED** |
| **Accessibility Foundation** | F1 §5.9, Rule 10 | §11 (Touch Ergonomics), §12 (Landmarks & Focus)| **SATISFIED** |
| **Visual Restraint (Modern First)**| F1 §5.10, F3 §2 | §2 (Flat Layers), §14 (Canvas Accommodation) | **SATISFIED** |
| **Performance-Aware Rendering** | F2 §6, B9 §2 | §10 (Scroll Model), §16 (Layout Principles)  | **SATISFIED** |

---

## 20. F4 Exit Criteria Checklist

- [x] Global application shell hierarchy and region responsibilities are formally defined.
- [x] Navigation topology is specified without pre-empting Block F7 route syntax.
- [x] Space-based 4-class viewport taxonomy (Compact, Medium, Expanded, Wide) is established.
- [x] Breakpoint transition architecture is defined without arbitrary hardcoded device lock-in.
- [x] Responsive reflow and generalized content priority rules are specified.
- [x] Orientation adaptations (Portrait, Landscape, Dynamic Viewport Height `dvh`) are addressed.
- [x] Four workspace composition models are defined without pre-empting lens blocks.
- [x] Contextual surface lifecycles (Drawers, Bottom Sheets, Modals, Popovers) are specified without hardcoded pixel widths.
- [x] Scroll ownership boundaries are established to prevent nested scroll traps while allowing active canvas gestures.
- [x] Touch ergonomics ($\ge 44\times 44\text{ px}$) are formalized without visualization-specific gesture mandates.
- [x] Semantic HTML5 landmarks, skip links, and logical focus flow are specified without claiming universal WCAG compliance.
- [x] Visualization shell accommodations and resize observer rules are defined without naming specific engines.
- [x] Deep-linking compatibility is confirmed.
- [x] Performance principles are distinguished from Stage 4 implementation techniques.
- [x] Downstream ownership matrix across F5–F17 and Stage 4 is explicitly catalogued.
- [x] Zero application source code, component JSX, CSS stylesheets, or package installations were introduced.
- [x] Zero Stage 1 backend documents, Block F1, Block F2, or Block F3 documents were modified.
