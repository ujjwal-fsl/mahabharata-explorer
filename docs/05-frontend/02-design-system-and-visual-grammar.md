# Design System & Visual Grammar (Block F3)

## 1. Architectural Context

This document establishes the **Design System & Visual Grammar** for the **Mahābhārata Explorer** frontend. It defines the foundational visual philosophy, design-token architecture, semantic color roles, typography system, spacing scale, shape and surface grammar, component primitives, interaction states, visualization design language, motion guidelines, accessibility rules, and styling technology architecture for Stage 2.

```
┌────────────────────────────────────────────────────────────────────────┐
│                   STAGE 2 ARCHITECTURAL PLACEMENT                      │
├───────────────────────────────────┬────────────────────────────────────┤
│ UPSTREAM CONTRACTS & CONSTRAINTS  │ DOWNSTREAM FRONTEND SPECIFICATIONS │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Stage 1 Backend Architecture    │ - F4: Responsive Layout & Shell    │
│   (B2 Data, B3 Graph, B4 Provenance│ - F7: Routing & Navigation UX      │
│    B5 API, B7 Auth, B8 Media)     │ - F8: Global Search UX             │
│ - Block F1: Frontend Constitution │ - F9: Character & Entity Views     │
│ - Block F2: Tech Stack & Build    │ - F10–F14: Visualizations          │
│   (React 19+, TypeScript, Vite,   │ - F15: Evidence & Provenance UX    │
│    SPA, Three-Tier Rendering)     │ - F16: Accessibility & IAST        │
│                                   │ - F17: Testing & Budgets           │
└───────────────────────────────────┴────────────────────────────────────┘
```

### 1.1 Pure Architectural Specification
Block F3 is strictly an architectural specification. It defines visual rules, token hierarchies, and styling strategy. It does not create CSS stylesheets, generate React component JSX, install styling packages, or configure build scripts.

---

## 2. Design System Principles

The design system is governed by 15 foundational principles tailored to a scholarly, multi-lens knowledge exploration engine:

```
┌────────────────────────────────────────────────────────────────────────┐
│                     DESIGN SYSTEM CONSTITUTION                         │
├────────────────────────────────────────────────────────────────────────┤
│ 1. KNOWLEDGE-FIRST HIERARCHY: Typography, spacing, and layout prioritize│
│    scholarly content clarity over decorative ornamentation.            │
│ 2. RESTRAINED MODERN FIRST: A clean, contemporary digital aesthetic with│
│    subtle, authentic Sanātana cultural resonance (Rule 11).            │
│ 3. CLARITY OVER DECORATION: Every visual element (border, color, badge)│
│    must serve a semantic or informational purpose (Rule 12).           │
│ 4. THREE-TIER PROGRESSIVE DISCLOSURE: Visual density expands gracefully │
│    from summary card to full profile to deep citation drawer.          │
│ 5. EPISTEMIC HONESTY: Distinct visual treatments for verified, unknown,│
│    not researched, not applicable, conflicting, and approximate claims.│
│ 6. EVIDENCE-AWARE UI: Citations, primary sources, and verse locators   │
│    have first-class visual affordances rather than hidden footnotes.   │
│ 7. ACCESSIBLE BY DEFAULT: WCAG 2.1 AA contrast, visible focus rings,   │
│    and non-color-only encoding of all critical states.                 │
│ 8. RESPONSIVE PARITY: Visual hierarchy, touch target ergonomics, and   │
│    reading comfort are maintained across phone, tablet, and desktop.   │
│ 9. VISUAL CALM IN DENSE DATA: Low-noise surfaces and muted palettes     │
│    prevent cognitive fatigue during multi-hour exploration sessions.   │
│ 10. NON-COLOR REDUNDANCY: Color is always paired with iconography,     │
│     typography, borders, or text labels to convey status.              │
│ 11. CULTURAL SUBTLETY: Zero faux-parchment, zero burnt edges, zero fake │
│     manuscript kitsch, and zero ornamental temple frames.              │
│ 12. SHARED VISUAL GRAMMAR: Unified visual language across all 10 lenses │
│     while allowing domain-specific visualization adaptation.           │
│ 13. SEMANTIC TOKEN ISOLATION: UI components consume semantic tokens     │
│     rather than hardcoded color or spacing values.                     │
│ 14. VISUALIZATION-AWARE INTEGRATION: Graph, map, tree, and timeline    │
│     elements share identical semantic tokens with DOM UI components.   │
│ 15. LONG-TERM EXTENSIBILITY: Centralized token hierarchy enables       │
│     seamless theming (light/dark) without component refactoring.       │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Visual Identity & Cultural Expression

The Mahābhārata Explorer balances contemporary digital product design with authentic cultural dignity:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   CULTURAL EXPRESSION MATRIX                           │
├───────────────────────────────────┬────────────────────────────────────┤
│ AUTHENTIC & ENCOURAGED EXPRESSION │ STRICTLY PROHIBITED CLICHÉS        │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Clean, crisp modern typography  │ - Faux-parchment / aged paper bgs  │
│   with pristine IAST diacritics   │ - Burnt-edge / torn-paper effects  │
│ - Elegant, subtle geometric       │ - Heavy ornamental gold gradients  │
│   dividers inspired by classical  │ - Excessive brown / mud palettes   │
│   proportions (restrained 1px)    │ - Temple-style arched borders      │
│ - Warm saffron/vermilion accents  │ - Decorative Sanskrit text used    │
│   used strictly for active states │   as meaningless visual wallpaper  │
│ - Semantic monogram badges for    │ - Mythological fantasy game UI     │
│   characters without portraits    │   (glowing runes, ornate frames)   │
│ - High-contrast editorial layout  │ - Comic-book exaggerated fonts     │
└───────────────────────────────────┴────────────────────────────────────┘
```

---

## 4. Semantic Color Architecture

The color architecture is built entirely on semantic roles. Raw hex values are encapsulated within primitive tokens; components interact solely with semantic and interactive tokens.

```
┌────────────────────────────────────────────────────────────────────────┐
│                        SEMANTIC COLOR TAXONOMY                         │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Color Role       │ Semantic Purpose & Usage                            │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Brand Primary**│ Deep Indigo / Slate Navy (`color-brand-primary`):   │
│                  │ Core navigation, key structural headings, brand identity│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Brand Accent** │ Subtle Saffron / Ochre (`color-brand-accent`):      │
│                  │ Active lens indicator, focus halos, interactive highlights│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Surfaces**     │ Layered neutrals (`surface-base`, `surface-raised`, │
│                  │ `surface-overlay`, `surface-sunken`): Content layers│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Text / Content**│ High-contrast neutrals (`text-primary`,             │
│                  │ `text-secondary`, `text-muted`, `text-inverse`)     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Borders**      │ Subtle structural dividers (`border-subtle`,        │
│                  │ `border-muted`, `border-focus`): Structural frames  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Feedback**     │ Standard status tokens (`color-success`,            │
│                  │ `color-warning`, `color-error`, `color-info`)       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Epistemic**    │ Truth & certainty tokens across the 6 canonical     │
│                  │ B2 states (`epistemic-known`, `epistemic-unknown`,  │
│                  │ `epistemic-conflicting`, `epistemic-approximate`,   │
│                  │ `epistemic-not-researched`, `epistemic-not-applicable`)|
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Graph Types**  │ Semantic token category for relationship types      │
│                  │ (kinship, alliance, rivalry, mentorship, etc.)      │
└──────────────────┴─────────────────────────────────────────────────────┘
```

### 4.1 Non-Color Redundancy Rule
Color must never be the sole mechanism used to communicate state, relationship type, or epistemic certainty. Every color-coded element must be accompanied by text labels, distinct iconography, border styles (solid, dashed, dotted), or semantic badges.

---

## 5. Epistemic & Evidence Visual Language

A signature capability of the Mahābhārata Explorer is its visual representation of historical and textual certainty. Block F3 establishes the visual grammar representing the exact six canonical epistemic states defined by Stage 1 ([02-data-architecture.md §6](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/02-data-architecture.md#L180)):

```
┌────────────────────────────────────────────────────────────────────────┐
│                   EPISTEMIC VISUAL GRAMMAR MATRIX                      │
├──────────────────┬──────────────────┬───────────────┬──────────────────┤
│ Epistemic State  │ Visual Treatment │ Border / Icon │ Semantic Role    │
├──────────────────┼──────────────────┼───────────────┼──────────────────┤
│ **known**        │ Neutral / Solid  │ Solid border  │ Affirmatively    │
│                  │ High-contrast    │ Check glyph   │ established fact │
├──────────────────┼──────────────────┼───────────────┼──────────────────┤
│ **unknown**      │ Muted Slate /    │ Dashed border │ Affirmatively    │
│                  │ De-emphasized    │ Question mark │ unknown in texts │
├──────────────────┼──────────────────┼───────────────┼──────────────────┤
│ **not_researched**│ Low-contrast    │ Dotted border │ Field not yet    │
│                  │ Neutral badge    │ Clock / Pause │ curated in corpus│
├──────────────────┼──────────────────┼───────────────┼──────────────────┤
│ **not_applicable**│ Ghosted badge   │ Diagonal hash │ Attribute does   │
│                  │ Dimmed text      │ Null glyph    │ not apply        │
├──────────────────┼──────────────────┼───────────────┼──────────────────┤
│ **conflicting**  │ Dual-Tone Split  │ Split border  │ Competing claims │
│                  │ Multi-claim card │ Forked branch │ across traditions│
├──────────────────┼──────────────────┼───────────────┼──────────────────┤
│ **approximate**  │ Muted Amber /    │ Dotted border │ Inherently       │
│                  │ Subtle warm tint │ Tilde (`~`)   │ approximate date │
└──────────────────┴──────────────────┴───────────────┴──────────────────┘
```
*(Note: Operational status `coordinate_status = unmapped` for locations with unverified coordinates uses a dashed pinless visual badge; detailed geographic rendering is governed by Block F13).*

### 5.1 Evidence & Citation Visual Affordances
- **Source Citation Pill (Illustrative Example)**: Compact interactive tag (e.g., `[Source Locator 1.57.1]`) indicating verifiable backing evidence.
- **Disputed Claim Card (Illustrative Example)**: Split visual container presenting Claim A (Tradition 1) alongside Claim B (Tradition 2) without forced harmonization.
- **Missing Attribute Display**: Renders explicit semantic labels (*"Parentage: Unknown in source tradition"*) rather than empty whitespace or generic "N/A".
*(Note: Final interaction models, citation drawers, and textual excerpt views are formally specified in Block **F15 (Evidence, Citation & Provenance UX Architecture)**).*

---

## 6. Typography Architecture

The typography system supports dense scholarly exploration, multi-lingual Sanskrit transliteration (IAST), and high editorial readability:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        TYPOGRAPHY SYSTEM MATRIX                        │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Category         │ Typeface Strategy & Architectural Purpose           │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **UI & Body**    │ Clean, high-legibility geometric/humanist sans-serif│
│                  │ with complete Latin-Extended / IAST diacritic glyphs│
│                  │ (e.g., ā, ī, ū, ṛ, ṝ, ḷ, ṅ, ñ, ṭ, ḍ, ṇ, ś, ṣ, ḥ, ṃ)  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Display / H1** │ Refined, editorial serif or crisp display sans for  │
│                  │ canonical character names and major lens headings.  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Devanāgarī**   │ High-quality Unicode Devanāgarī font for native     │
│                  │ Sanskrit terms (e.g., अर्जुन, कुरुक्षेत्र, चक्रव्यूह)   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Monospace**    │ High-clarity tabular monospace for verse locators,  │
│                  │ sequence indices, and numerical references.         │
└──────────────────┴─────────────────────────────────────────────────────┘
```

### 6.1 Modular Scale & Guidance
- **Modular Scale Philosophy**: Employs a structured typographic scale (such as Minor Third $1.200$ for dense data views and Major Third $1.250$ for display headlines) to establish clear hierarchy.
- **Scale Hierarchy Guidance (Refined during Stage 4 implementation)**:
  - `text-xs`: Metadata, verse locator tags, timeline ticks.
  - `text-sm`: Table cells, body descriptions, secondary labels.
  - `text-base`: Standard narrative body text, input controls.
  - `text-lg`: Card titles, drawer headers, lead summaries.
  - `text-xl`: Section headers, lens panel titles.
  - `text-2xl`: Entity profile titles, war day headings.
  - `text-3xl`: Major lens titles, application hero.
- **Line Heights**: Tight ($1.2$) for headlines, Standard ($1.5$) for UI labels, Relaxed ($1.65$) for long-form scholarly narrative reading.

---

## 7. Spacing & Layout Token Scale

The spacing architecture utilizes a base-8 ($8\text{ px}$) grid with a 4px sub-grid for fine-grained component density:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        SPACING TOKEN SCALE                             │
├──────────────┬──────────────┬──────────────────────────────────────────┤
│ Token Name   │ Base Value   │ Common Application Scope                 │
├──────────────┼──────────────┼──────────────────────────────────────────┤
│ `space-1`    │ 4px (0.25rem)│ Fine icon-to-text gap, badge padding     │
│ `space-2`    │ 8px (0.5rem) │ Input internal padding, chip gaps        │
│ `space-3`    │ 12px (0.75rem│ Compact card padding, button side gap    │
│ `space-4`    │ 16px (1.0rem)│ Standard component padding, list spacing │
│ `space-6`    │ 24px (1.5rem)│ Card header spacing, grid column gutters │
│ `space-8`    │ 32px (2.0rem)│ Major section margins, panel padding     │
│ `space-12`   │ 48px (3.0rem)│ Page section vertical dividers           │
│ `space-16`   │ 64px (4.0rem)│ Major page hero top/bottom padding       │
└──────────────┴──────────────┴──────────────────────────────────────────┘
```
*(Note: Application shell layouts, breakpoints, and responsive positioning are formally specified in Block **F4**).*

---

## 8. Shape, Border & Elevation Language

The visual language is modern, crisp, and predominantly flat, avoiding heavy 3D neumorphic or glossy visual noise:

```
┌────────────────────────────────────────────────────────────────────────┐
│                         SHAPE & RADIUS SYSTEM                          │
├──────────────┬──────────────┬──────────────────────────────────────────┤
│ Token Name   │ Base Value   │ UI Element Application                   │
├──────────────┼──────────────┼──────────────────────────────────────────┤
│ `radius-none`│ 0px          │ Full-bleed viewport panels, canvas borders│
│ `radius-sm`  │ 4px          │ Status badges, citation tags, inputs     │
│ `radius-md`  │ 8px          │ Standard entity cards, dropdown menus    │
│ `radius-lg`  │ 12px         │ Modals, floating drawers, search panels  │
│ `radius-full`│ 9999px       │ Avatars, filter pills, round icon buttons│
└──────────────┴──────────────┴──────────────────────────────────────────┘
```

### 8.1 Elevation & Surface Layering
Elevation is expressed through **subtle structural borders** ($1\text{px}$) paired with low-blur directional shadows:
- `elevation-0`: Flat surface, separated strictly by `border-subtle` ($1\text{px}$ solid).
- `elevation-1`: Raised cards, subtle hover states.
- `elevation-2`: Floating panels, sticky navigation header.
- `elevation-3`: Modal dialogs, search overlay, evidence drawers.

---

## 9. Surface & Layering Architecture

Surfaces establish depth and visual containment across complex multi-pane views:

```
┌────────────────────────────────────────────────────────────────────────┐
│                         SURFACE HIERARCHY                              │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Surface Tier     │ Purpose & Visual Treatment                          │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `surface-canvas` │ Viewport background behind graph/map canvas         │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `surface-base`   │ Default application page background                 │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `surface-raised` │ Individual cards, content containers, list rows     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `surface-overlay`│ Flyout menus, search modal, evidence drawer         │
├──────────────────┼─────────────────────────────────────────────────────┤
│ `surface-sunken` │ Inset data wells, code snippets, citation quote box │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 10. Iconography Strategy

1. **Style & Weight**: Outlined vector iconography with consistent visual stroke weight and rounded join caps (recommended initial guidance: $16\text{ px}$, $20\text{ px}$, and $24\text{ px}$ viewports).
2. **Semantic Meaning**: Icons must reinforce, not replace, text labels (e.g., search magnifying glass, map pin, timeline clock, kinship tree nodes, book citation).
3. **Accessibility**: Standalone icon buttons must include explicit `aria-label` attributes; decorative icons paired with visible text must be flagged with `aria-hidden="true"`.
4. **Cultural Restraint**: Religious and sacred motifs are prohibited from being used as generic UI controls.

---

## 11. Component Visual Primitives

F3 defines the visual grammar for common UI primitives:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        COMPONENT VISUAL GRAMMAR                        │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Component        │ Visual Specification & States                       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Buttons**      │ Primary (solid brand), Secondary (outlined),        │
│                  │ Ghost (text-only with hover surface). Touch-friendly│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Search Field** │ Uses standard input/search visual primitive. Global │
│                  │ search interaction, shortcuts, and overlay in F8.   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Tabs & Pills** │ Underlined active tab indicator or pill shape with  │
│                  │ subtle saffron active background.                   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Entity Cards** │ Clean surface-raised container, semantic portrait   │
│                  │ or monogram, role tags, and hover elevation lift.   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Drawers**      │ Slide-in panel with subtle dimmed backdrop where    │
│                  │ overlay separation is required (positioning in F4). │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Skeletons**    │ Low-contrast shimmering placeholder blocks matching │
│                  │ typography and card geometries during loading.      │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 12. Tripartite State Architecture: Interaction, System & Epistemic

To eliminate ambiguity between operational UI states and historical truth states, the visual architecture establishes three distinct state categories:

```
┌────────────────────────────────────────────────────────────────────────┐
│                    TRIPARTITE STATE ARCHITECTURE                       │
├──────────────────┬─────────────────────────────────────────────────────┤
│ State Category   │ States & Visual Manifestation                       │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Category A:    │ - Default: Base token color, standard border        │
│ Interaction**    │ - Hover: Subtle surface tint, border contrast lift  │
│                  │ - Focus: High-contrast 2px outline focus ring       │
│                  │ - Active / Pressed: Depressed scale, darkened tint  │
│                  │ - Selected: Accent border, active indicator glyph   │
│                  │ - Disabled: 40% opacity, cursor not-allowed         │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Category B:    │ - Loading: Skeleton shimmer or non-blocking spinner │
│ System / Content**│ - Error: Contextual alert banner (RFC 7807 problem) │
│                  │ - Empty: Clean empty-state prompt with search hint  │
│                  │ - Unavailable: Temporary rate-limit / offline card  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Category C:    │ - known: Solid border, verified check glyph         │
│ Epistemic (B2)** │ - unknown: Dashed border, question glyph            │
│                  │ - not_researched: Dotted border, clock glyph        │
│                  │ - not_applicable: Ghosted badge, diagonal hash glyph│
│                  │ - conflicting: Split-tone dual card, branch glyph   │
│                  │ - approximate: Dotted border, tilde glyph           │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 13. Data-Dense UI Grammar

To present extensive historical records without visual clutter:
1. **Property-Value Pairs**: Key in `text-xs text-muted font-medium uppercase`, value in `text-sm text-primary font-normal`.
2. **Compact Data Tables**: Clean horizontal dividing rules, zero vertical column rules, right-aligned numbers, tabular monospace numerals.
3. **Collapsible Detail Disclosures**: Progressive accordion blocks for long genealogical branches and extensive event participant listings.

---

## 14. Shared Visualization Design Grammar

Blocks **F10 through F14** will select concrete visualization engines. All graphical viewports must inherit this unified design grammar:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   VISUALIZATION DESIGN GRAMMAR                         │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Visual Element   │ Standardized Visual Representation                  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Graph Nodes**  │ Circular glyphs with high-contrast borders; size    │
│                  │ reflects centrality; color encodes entity type.     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Graph Edges**  │ Solid line = direct relationship;                   │
│                  │ Dashed line = disputed / conflicting relationship;  │
│                  │ Directional arrow for asymmetrical links.           │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Focus Halo**   │ $D=1$ nodes full opacity; $D=2$ nodes $70\%$ opacity;│
│                  │ background graph de-emphasized to $20\%$ opacity.   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Map Pins**     │ Minimalist vector pins with type icon; cluster badge│
│                  │ for overlapping regional sites.                     │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Timeline Bars**│ Horizontal sequence tracks segmented by Parvas 1–18 │
│                  │ with milestone diamonds for major critical events.  │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Legends**      │ Clean floating corner card defining node types,     │
│                  │ edge styles, and epistemic border meanings.         │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 15. Relationship & Network Semantics

Block F3 establishes the **semantic token category** for relationship types across the knowledge graph (e.g., `color-rel-kinship`, `color-rel-alliance`, `color-rel-rivalry`, `color-rel-mentorship`, `color-faction-pandava`, `color-faction-kaurava`).

*Authority Delegation*: Final color assignments, edge stroke mappings, and visual contrast optimizations are under the authority of Blocks **F10 (Knowledge Graph Visualization)** and **F11 (Family Lineage Tree Visualization)** based on graph density and visualization engine capabilities.

---

## 16. Motion & Animation Architecture

Motion provides spatial orientation and feedback without decorative delay:
1. **Motion Principles**:
   - *Purposeful & Brief*: Animations communicate state changes or spatial continuity without delaying user access to information.
   - *Non-Decorative*: No gratuitous continuous looping animations or decorative bounce effects.
   - *Interruptible*: Fast user interactions interrupt running transitions cleanly.
2. **Reduced-Motion Mandate**:
   - Under `prefers-reduced-motion: reduce`, continuous visualization animation, smooth scrolling transitions, and drawer sliding animations are suppressed or replaced with instant visual state updates.
   - Concrete graph-engine simulation and layout behavior under reduced motion is specified in Block **F10**.
3. **Implementation Refinement**: Exact microsecond durations and easing curves are implementation-level parameters refined during Stage 4 and validated in Block **F17**.

---

## 17. Responsive Visual Grammar

Block F3 establishes the visual-system rules governing responsive adaptation:
1. **Typography & Spacing Fluidity**: Type sizes and layout padding scale gracefully across viewport widths.
2. **Touch-Friendly Hit Targets**: Interactive controls maintain touch-friendly target dimensions ($\ge 44\times 44\text{ px}$) on touch-enabled viewports.
3. **Information Density Adaptation**: Compact density presentation on small screens without truncating critical epistemic badges or citation locators.
*(Note: Application shell layouts, responsive grid breakpoints, and drawer placements are formally specified in Block **F4**).*

---

## 18. Accessibility Visual Requirements

1. **Contrast Standards**: All body text achieves $\ge 4.5:1$ contrast against surface backgrounds; large headings and interactive icons achieve $\ge 3.0:1$.
2. **Focus Visibility**: High-visibility outline focus ring ($2\text{ px}$ with $2\text{ px}$ offset) is never suppressed on keyboard navigation.
3. **Zoom & Text Reflow**: The visual layout remains fully functional when scaled up to $200\%$ browser zoom without horizontal text clipping.

---

## 19. Light & Dark Theme Architecture

The design system supports **dual Light and Dark modes** via CSS Custom Property token transformations:
- **Light Mode (Default)**: Clean, high-contrast scholarly reading environment with crisp neutral surfaces.
- **Dark Mode**: Low-luminance backgrounds optimized for deep night-time graph exploration, preserving exact epistemic and relationship hue relationships.
- **Token Transformation**: Themes transform semantic tokens (`--surface-base`, `--text-primary`) rather than modifying component markup. Exact primitive hex values are implementation-level palette refinements.

---

## 20. Three-Tier Design Token Architecture

The design token hierarchy isolates primitive values from component usage:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        DESIGN TOKEN HIERARCHY                          │
├────────────────────────────────────────────────────────────────────────┤
│ 1. GLOBAL PRIMITIVES (Raw palette values, font stacks, base units)     │
├────────────────────────────────────────────────────────────────────────┤
│ 2. SEMANTIC TOKENS (`--surface-base`, `--text-primary`,                │
│    `--border-subtle`, `--epistemic-unknown`, `--color-brand-accent`)   │
├────────────────────────────────────────────────────────────────────────┤
│ 3. COMPONENT TOKENS (`--card-bg`, `--drawer-width`, `--node-border`)   │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 21. Styling Technology Architecture Evaluation & Selection

To implement the design token architecture and styling rules, modern CSS methodologies were evaluated:

```
┌────────────────────────────────────────────────────────────────────────┐
│                     STYLING ARCHITECTURE EVALUATION                    │
├───────────────────┬──────────────────┬───────────────┬─────────────────┤
│ Evaluation Factor │ Utility CSS      │ CSS Modules   │ CSS-in-JS       │
│                   │ (Tailwind CSS v4)│ (Scoped CSS)  │ (Emotion/Styled)│
├───────────────────┼──────────────────┼───────────────┼─────────────────┤
│ Performance       │ **Optimal**      │ Optimal       │ Suboptimal      │
│ & Runtime Cost    │ Zero runtime, minimal purged CSS│ Zero runtime │ Runtime style injection overhead│
├───────────────────┼──────────────────┼───────────────┼─────────────────┤
│ Token Integration │ **First-Class**  │ Manual via    │ Dynamic via     │
│ & Theme Swapping  │ Native CSS vars & theme mapping │ CSS custom vars│ JS ThemeProvider│
├───────────────────┼──────────────────┼───────────────┼─────────────────┤
│ Developer Velocity│ **Highest**      │ Moderate      │ High            │
│ & Refactoring     │ Co-located classes, zero naming friction│ File context switching│ High abstraction│
├───────────────────┼──────────────────┼───────────────┼─────────────────┤
│ Antigravity       │ **Optimal**      │ Moderate      │ Moderate        │
│ Inspectability    │ Explicit utility classes in TSX│ Disconnected CSS files│ Opaque wrapper components│
└───────────────────┴──────────────────┴───────────────┴─────────────────┘
```

### 21.1 Selected Styling Architecture
- **Selected Approach**: **Utility-First CSS (Tailwind CSS v4 / Modern CSS Engine)** paired with **Semantic CSS Custom Properties** for the design token hierarchy.
- **Architectural Clarification**: Tailwind/utility classes serve as the **implementation mechanism**; centralized semantic design tokens remain the **authoritative source of visual meaning and governance**.
- **Rationale**:
  1. *Zero Runtime Overhead*: Compiles to a lightweight, static CSS stylesheet during Vite builds.
  2. *Antigravity Code Clarity*: Co-located utility classes allow AI subagents to inspect and modify component visual styling directly within TSX files without CSS file switching.
  3. *Fluid Design Token Synchronization*: Seamlessly maps semantic tokens (`--surface-base`, `--text-primary`, `--color-brand-accent`) to clean utility classes (`bg-surface-base`, `text-primary`).

---

## 22. Icon & Font Library Boundaries

- **Iconography Library**: Evaluation and selection deferred to Stage 4 implementation (recommending lightweight SVG icon sets such as Lucide Icons).
- **Webfont Packaging**: Concrete webfont distribution (self-hosted WOFF2 via Vite) is defined in Block **F16 (Accessibility, Internationalization & Diacritic Architecture)**.

---

## 23. Design System Governance

1. **Centralized Semantic Token Governance**: Components must consume semantic tokens (`bg-surface-raised`, `text-primary`); raw arbitrary visual values (e.g., hardcoded hex colors or inline pixel margins) are prohibited in component code unless explicitly justified.
2. **Lens Extension Rules**: Domain lenses (e.g., War Day battlefield map) may define scoped component tokens but must inherit base semantic colors and typography scales.
3. **Accessibility Gating**: Every new component primitive must undergo keyboard navigation and contrast ratio verification before inclusion in the system.

---

## 24. Deferred Decisions & Ownership Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                     DEFERRED DECISION MATRIX                           │
├────────────────────────────────────────┬───────────────────────────────┤
│ Architectural Domain                   │ Authoritative Target Block    │
├────────────────────────────────────────┼───────────────────────────────┤
│ Application Shell, Grid & Breakpoints  │ **Block F4**                  │
│ Global Search Modal & Shortcut UX      │ **Block F8**                  │
│ Character Profile Layout Hierarchy     │ **Block F9**                  │
│ Knowledge Graph Visualization Engine   │ **Block F10**                 │
│ Family Lineage Tree DAG Layout         │ **Block F11**                 │
│ Timeline Scrubber Visualization Engine │ **Block F12**                 │
│ Geographic Map Tile Engine & Layers    │ **Block F13**                 │
│ Battlefield Vyuha SVG Component UX     │ **Block F14**                 │
│ Evidence Drawer & Citation UX          │ **Block F15**                 │
│ Webfont Packaging & IAST Layouts       │ **Block F16**                 │
│ Visual Regression & A11y Test Suites   │ **Block F17**                 │
│ Concrete Icon Pack & Package Install   │ **Stage 4 (Implementation)**  │
└────────────────────────────────────────┴───────────────────────────────┘
```

---

## 25. Architectural Decision Record (ADR)

| Decision ID | Architectural Decision | Chosen Approach | Alternatives Evaluated | Rationale & Trade-offs | Owner | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **ADR-FE-12** | Visual Philosophy | **Restrained Modern First** | Faux-parchment, ornate manuscript | Prioritizes scholarly legibility, digital clarity, and avoids cultural clichés. | F3 | **DECIDED** |
| **ADR-FE-13** | Design Token Model | **Three-Tier Semantic Tokens** | Hardcoded values, single flat tokens | Encapsulates raw primitives, enables seamless light/dark theme switching. | F3 | **DECIDED** |
| **ADR-FE-14** | Styling Architecture | **Utility-First CSS + CSS Vars** | CSS Modules, CSS-in-JS | Zero runtime cost, optimal Antigravity code inspectability, instant Vite bundling. | F3 | **DECIDED** |
| **ADR-FE-15** | Epistemic Visual System| **Non-Color Multi-Coded Badges** | Color-only badges, hidden tooltips | Explicit visual states for all 6 B2 epistemic states. | F3 | **DECIDED** |
| **ADR-FE-16** | Elevation System | **Subtle Borders + Low-Blur Shadow** | Heavy drop shadows, neumorphism | Clean, flat modern layering without visual clutter. | F3 | **DECIDED** |
| **ADR-FE-17** | Theme Strategy | **Dual Light & Dark Mode** | Light-only, multi-theme | Supports daytime scholarly reading and dark room immersive graph exploration. | F3 | **DECIDED** |

---

## 26. Requirement Traceability Matrix

| Requirement / Principle | Source Document | Implementing Block F3 Section | Coverage Status |
| :--- | :--- | :--- | :--- |
| **Knowledge Explorer First** | F1 §5.1, PRD §2 | §2 (Principles), §13 (Data-Dense Grammar) | **SATISFIED** |
| **Evidence-Aware UI** | F1 §5.3, B4 §1 | §5 (Epistemic Visuals), §11 (Citation Pills) | **SATISFIED** |
| **Zero Data Fabrication** | F1 §5.4, Rule 03 | §3 (Cultural Integrity), §5 (Epistemic States) | **SATISFIED** |
| **Visual Restraint (Modern First)**| F1 §5.10, Rule 11 | §2 (Principles), §3 (Prohibited Clichés) | **SATISFIED** |
| **Accessible by Default** | F1 §5.9, Rule 10 | §4.1 (Non-Color), §12 (Focus Rings), §18 (A11y)| **SATISFIED** |
| **Responsive Parity** | F1 §5.5, Rule 09 | §7 (Spacing), §17 (Responsive Grammar) | **SATISFIED** |
| **Shared Visualization Grammar** | F1 §5.2, B3 §1 | §14 (Visualization Grammar), §15 (Graph Tokens)| **SATISFIED** |

---

## 27. F3 Exit Criteria Checklist

- [x] Visual philosophy is explicitly defined (Modern First, Traditional Second, Zero Faux-Parchment).
- [x] Cultural identity is authentic, subtle, and free from stereotypical clichés.
- [x] Three-tier design token architecture (Primitives $\rightarrow$ Semantic $\rightarrow$ Component) is established.
- [x] Semantic color architecture and non-color redundancy rules are formalized.
- [x] All six canonical B2 epistemic states (`known`, `unknown`, `not_researched`, `not_applicable`, `conflicting`, `approximate`) are visually mapped.
- [x] Tripartite state separation (Interaction, System, Epistemic) is formalized.
- [x] Typography, spacing, shape, elevation, and surface systems are specified.
- [x] Component visual primitives are defined without preempting F4/F8.
- [x] Shared visualization design grammar is established without pre-empting Blocks F10–F14.
- [x] Styling architecture (Utility-First CSS + CSS Variables) is evaluated and selected with token governance.
- [x] Ownership boundaries across F4, F8, F10–F17, and Stage 4 are explicitly catalogued.
- [x] Zero application source code, CSS files, package installations, or build configurations were introduced.
- [x] Zero Stage 1 backend documents, Block F1, or Block F2 documents were modified.
