# Global Search & Autocomplete UX Architecture (Block F8)

## 1. Architectural Context & Purpose

This document establishes the **Global Search & Autocomplete UX Architecture** for the **Mahābhārata Explorer** frontend. It defines the search entry points, autocomplete interaction lifecycle, suggestion information architecture, search results page UX (`/search`), query normalization boundaries, offset pagination integration, responsive recomposition, and accessibility standards for Stage 2.

```
┌────────────────────────────────────────────────────────────────────────┐
│                   STAGE 2 ARCHITECTURAL PLACEMENT                      │
├───────────────────────────────────┬────────────────────────────────────┤
│ UPSTREAM CONTRACTS & CONSTRAINTS  │ DOWNSTREAM FRONTEND SPECIFICATIONS │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Stage 1 Backend Architecture    │ - F9: Character & Entity Views     │
│   (B2 Data, B4 Provenance, B5 API,│ - F10: Knowledge Graph Canvas      │
│    B6 Search, B9 Perf/Rate Limits)│ - F11: Family Lineage Tree         │
│ - Block F1: Frontend Constitution │ - F12: Timeline Scrubber           │
│ - Block F2: Tech Stack (React SPA)│ - F13: Geographic Map Engine       │
│ - Block F3: Design System Tokens  │ - F14: War & Tactical Vyuhas       │
│ - Block F4: Responsive Shell      │ - F15: Evidence & Provenance UX    │
│ - Block F5: State Management Model│ - F16: Accessibility & IAST        │
│ - Block F6: API Client & Caching  │ - F17: Testing & Budgets           │
│ - Block F7: Routing & URL Grammar │                                    │
└───────────────────────────────────┴────────────────────────────────────┘
```

### 1.1 Pure Architectural Specification & Library Neutrality
Block F8 is strictly an architectural UX specification. It establishes user interaction contracts, state boundaries, information hierarchies, and behavioral state machines. It does not implement React search components, install search UI libraries, configure API fetching hooks, or implement backend search algorithms.

### 1.2 Explicit Ownership Boundaries
- **Block F8 Owns**:
  - Global search entry ergonomics across shell layouts.
  - Autocomplete suggestion UX and keyboard navigation orchestration.
  - Suggestion information architecture and epistemic status presentation.
  - Dedicated search results view (`/search`) layout and filtering UX.
  - Search interaction lifecycle and behavioral state transitions.
  - Responsive recomposition across the four F4 viewport classes.
  - Search accessibility contracts (combobox semantics, announcements, focus).
- **Block F8 Does NOT Own**:
  - Backend search indexing, ranking, and search algorithms (owned by B5/B6/Stage 4).
  - API client network transport, HTTP caching, and conditional revalidation (owned by F6).
  - Canonical URL route grammar, URL parsing, validation, canonicalization, and query serialization (owned by F7).
  - Internal view models and rendering engines for specific exploration lenses (owned by F9–F14).
  - Detailed scholarly citation and shloka evidence drawer UX (owned by F15).
  - Detailed system-wide accessibility implementation and typography engine (owned by F16).
  - Formal frontend numeric latency and bundle budgets (owned by F17).
  - Concrete search UI package selection and React hook implementation (owned by Stage 4).

---

## 2. Core Architectural Principles

The search and autocomplete UX architecture is governed by 10 foundational principles:

```
┌────────────────────────────────────────────────────────────────────────┐
│                      SEARCH CORE PRINCIPLES                            │
├────────────────────────────────────────────────────────────────────────┤
│ 1. KNOWLEDGE EXPLORER FIRST: Search is an exploration gateway into the │
│    interconnected epic graph, not merely a detached text box.          │
│ 2. REPRODUCIBLE SEARCH STATE: Committed search queries and filters are │
│    strictly governed by URL state (`/search?q=...`) under F5 and F7.   │
│ 3. TRANSIENT AUTOCOMPLETE LOCALITY: Suggestion dropdowns, active item  │
│    highlights, and typing buffers remain strictly local UI state.      │
│ 4. CANONICAL ENTITY NAVIGATION: Selecting an autocomplete suggestion   │
│    navigates directly to the canonical entity destination defined by   │
│    Block F7, bypassing unnecessary intermediary search-results screens.│
│ 5. EPISTEMIC FIDELITY: Search results present certainty states using   │
│    the six canonical B2 values only; certainty is never fabricated.    │
│ 6. MULTILINGUAL & SCRIPT RESPECT: Query processing transparently       │
│    preserves Unicode characters, IAST diacritics, and Devanāgarī.      │
│ 7. OFFSET PAGINATION FIDELITY: Search result collections consume the   │
│    B5 public offset-pagination contract; zero cursors are introduced.  │
│ 8. RESPONSIVE RECOMPOSITION: Search transitions gracefully from inline │
│    desktop headers to focused mobile overlays without losing query.   │
│ 9. ACCESSIBILITY-FIRST ORCHESTRATION: Keyboard and assistive-technology│
│    accessibility is a first-class requirement across all search surfaces│
│ 10. BOUNDED & SAFE INPUT: Query inputs are treated as untrusted        │
│     data, safely rendered without dynamic script execution.           │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Search Entry Architecture & Shell Integration

Search entry is integrated directly into the application shell established in Block F4 ([03-responsive-layout-and-application-shell-architecture.md §3](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/05-frontend/03-responsive-layout-and-application-shell-architecture.md#L45)):

```
┌────────────────────────────────────────────────────────────────────────┐
│                   SEARCH ENTRY ERGONOMICS BY VIEWPORT                  │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Viewport Class   │ Search Entry Visual Placement & Activation          │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Expanded /     │ Persistent search bar in top shell header; visual   │
│ Wide**           │ shortcut badge (`Ctrl+K` / `⌘K` or `/`); inline     │
│                  │ input expands on focus to reveal autocomplete panel.│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Medium**       │ Prominent search button/bar in top header; activates│
│                  │ a contextual overlay that visually separates the    │
│                  │ active search interaction from the underlying shell.│
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Compact**      │ Persistent search icon in top utility bar and bottom│
│                  │ navigation bar; activates full-screen search overlay│
└──────────────────┴─────────────────────────────────────────────────────┘
```

### 3.1 Interaction & Activation Mechanics
1. **Global Keyboard Activation**: Pressing `/` (when focus is outside input elements) or `Ctrl+K` / `⌘K` immediately shifts focus to the search input, regardless of the active lens.
2. **Focus & Dismissal**: Clicking outside the autocomplete container or pressing `Escape` dismisses the suggestion panel and restores focus to the previously active element.
3. **Touch Ergonomics**: In Compact viewports, search activation launches a focused overlay with appropriately sized touch targets consistent with Block F16's accessibility architecture, placing the input at the top with the native on-screen keyboard engaged.

---

## 4. Autocomplete Architecture & Interaction Lifecycle

Autocomplete provides rapid discovery as the user types, orchestrating queries through the Block F6 API client:

```
┌────────────────────────────────────────────────────────────────────────┐
│                     AUTOCOMPLETE LIFECYCLE MODEL                       │
├────────────────────────────────────────────────────────────────────────┤
│ [Idle]                                                                 │
│   │ (User types character in search input)                             │
│   ▼                                                                    │
│ [Editing / Buffering] (Local UI state buffers raw string)              │
│   │ (Query normalized: trimmed, length ≥ minimum threshold)            │
│   ▼                                                                    │
│ [Request Dispatch] (F8 requests suggestions from F6 API client)        │
│   │                                                                    │
│   ├── [Loading] (Subtle progress indicator; previous results dimmed)  │
│   │                                                                    │
│   ├── [Suggestions Ready] (Render sorted, grouped suggestion list)     │
│   │     │                                                              │
│   │     ├── (User navigates with Up/Down arrows $\rightarrow$ Highlighted item)│
│   │     ├── (User selects item $\rightarrow$ Canonical F7 Entity Destination)  │
│   │     └── (User submits query $\rightarrow$ `/search?q=...`)                 │
│   │                                                                    │
│   ├── [No Results] (Render "No entities matching '{query}' in epic")   │
│   │                                                                    │
│   └── [Error] (Render clear user-facing search error notice)           │
└────────────────────────────────────────────────────────────────────────┘
```

### 4.1 Obsolescence & Concurrency Defense
- **Stale Response Protection**: Every suggestion request is associated with the current input sequence. When the user modifies the input, previous in-flight suggestion responses are discarded upon arrival, ensuring that obsolete asynchronous results never overwrite current suggestions.
- **Request Coordination**: Network deduplication, HTTP caching, and debounce execution are delegated to Block **F6**.

### 4.2 Keyboard Navigation Contract
- `Down Arrow` / `Up Arrow`: Moves focus cyclically through the suggestion items without modifying the text in the input buffer.
- `Enter` on Highlighted Suggestion: Immediately invokes canonical entity navigation to the target resource defined by Block F7.
- `Enter` on Input (No Suggestion Highlighted): Submits the full query and navigates to the dedicated search page: `/search?q={normalized_query}`.
- `Escape`: Closes the suggestion dropdown and preserves the current input text.

---

## 5. Search Suggestion Information Architecture

Suggestions present high-density, disambiguated metadata enabling scholarly precision at a glance:

```
┌────────────────────────────────────────────────────────────────────────┐
│                     SUGGESTION ITEM ANATOMY                            │
├────────────────────────────────────────────────────────────────────────┤
│ [Entity Name (IAST)]  [Devanāgarī Script]    [Category Badge: Character]│
│ Alternate: "Pārtha, Dhanañjaya"              [Epistemic: Known]        │
│ Context: "Third Pāṇḍava brother; son of Kuntī and Indra"               │
└────────────────────────────────────────────────────────────────────────┘
```

### 5.1 Metadata Attributes
1. **Canonical Display Name**: Rendered in Roman script with accurate IAST transliteration (e.g., *Arjuna*, *Bhīṣma*, *Droṇa*).
2. **Original Script Representation**: Secondary Devanāgarī rendering (e.g., अर्जुन) displayed in accordance with client script preferences governed by Block F5 and Block F16.
3. **Resource Category**: Semantic badge indicating domain classification (*Character*, *Location*, *Group*, *Event*, *War*, *Formation*).
4. **Disambiguation Context**: Brief ancestral or narrative tag to differentiate duplicate names (e.g., *Droṇa [Achārya]* vs. *Droṇa [Kāka]*).
5. **Epistemic Certainty Badge**: Visual non-color indicator (Block F3 §12) presenting one of the six canonical B2 epistemic states:
   - `known`: Established epic figure or event.
   - `conflicting`: Disputed lineage or conflicting narrative accounts.
   - `approximate`: Approximate chronological or spatial attribution.
   - `unknown`, `not_researched`, `not_applicable`: Presented where affirmatively recorded in domain data.
   *Rule*: Suggestions must never imply historical or narrative certainty unsupported by backend data.

---

## 6. Dedicated Search Results UX (`/search`)

When a query is formally submitted, the application navigates to the dedicated `/search` route established in Block F7 ([06-routing-navigation-and-deep-linking-architecture.md §6.8](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/05-frontend/06-routing-navigation-and-deep-linking-architecture.md#L230)):

```
┌────────────────────────────────────────────────────────────────────────┐
│                      SEARCH RESULTS PAGE STRUCTURE                     │
├────────────────────────────────────────────────────────────────────────┤
│ [Search Header: Input Bar + Query Summary: "N results for '{query}'"]  │
├────────────────────────────────────────────────────────────────────────┤
│ [Filter Bar: Domain Categories | Epistemic Status                      │
│              Sort Ordering (Relevance / Chronological)]                │
├────────────────────────────────────────────────────────────────────────┤
│ [Results Collection: Stacked Scholarly Result Cards]                   │
│   ├── Result Card 1 (Title, Category, Summary, Claims, Link to Profile)│
│   ├── Result Card 2 (Title, Category, Summary, Claims, Link to Profile)│
│   └── ...                                                              │
├────────────────────────────────────────────────────────────────────────┤
│ [Pagination Controls: "Showing 1–20 of N" | [Prev] Page 1 of M [Next]] │
└────────────────────────────────────────────────────────────────────────┘
```

### 6.1 Progressive Disclosure on Result Cards
1. **Primary Layer**: Canonical title, IAST transliteration, category badge, and one-sentence primary identification.
2. **Secondary Context Layer**: Narrative summary, primary Parva occurrences, and key kinship/alliance connections.
3. **Evidence Preview**: Scholarly citation locator count (e.g., *"{count} verified Critical Edition references"*); clicking opens the Evidence Drawer (Block F15) without leaving the search context.
4. **Primary Navigation Action**: Prominent link navigating to the target entity route defined by Block F7 (`/characters/:slug`, `/geography/:slug`, etc.).

### 6.2 Filter and Facet Integration
- Search facets (e.g., domain categories, epistemic status, sort order) allow users to refine results.
- Concrete query parameter naming and serialization adhere strictly to the URL grammar established by Block **F7**.

---

## 7. Searchable Resource Domain Coverage & Route Boundary

Search resource coverage bridges the backend search domains established in Stage 1 with the frontend exploration lenses defined in Block F7:

```
┌────────────────────────────────────────────────────────────────────────┐
│               SEARCH DOMAIN TO FRONTEND ROUTE MAPPING                  │
├──────────────────────────┬─────────────────────────────────────────────┤
│ Backend Search Domain    │ Authoritative Frontend Route (Block F7)     │
│ (Canonical B2 / B6)      │                                             │
├──────────────────────────┼─────────────────────────────────────────────┤
│ **Character**            │ `/characters/:slug`                         │
├──────────────────────────┼─────────────────────────────────────────────┤
│ **Location**             │ `/geography/:slug`                          │
├──────────────────────────┼─────────────────────────────────────────────┤
│ **Group**                │ `/factions/:slug`                           │
├──────────────────────────┼─────────────────────────────────────────────┤
│ **Event**                │ `/timeline/:slug`                           │
├──────────────────────────┼─────────────────────────────────────────────┤
│ **War**                  │ `/wars/:war_slug`                           │
├──────────────────────────┼─────────────────────────────────────────────┤
│ **Formation**            │ `/vyuhas/:slug`                             │
├──────────────────────────┼─────────────────────────────────────────────┤
│ **Source**               │ Contextual Evidence Drawer (Block F15)      │
└──────────────────────────┴─────────────────────────────────────────────┘
```

### 7.1 Separation of Concerns
1. **Backend Search Representations**: The frontend consumes high-level search domain representations exposed by the backend search API ([06-search-architecture.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/06-search-architecture.md)). It does not map directly to underlying database tables or SQL schemas.
2. **Frontend Destination Authority**: Autocomplete and search selections navigate strictly to the canonical frontend destinations defined by Block **F7**. If backend search DTOs provide `target_url` hints, they are treated as informative backend data subject to F7's authoritative route contract.
3. **Exploration Lenses vs. Domain Entities**: Top-level exploration lenses (such as Lineage or Focus Graph) represent analytical perspectives over the knowledge graph rather than independent backend entity types.

---

## 8. Query Input, Normalization & Multilingual Handling

In alignment with Block F7 (§14):
1. **Canonical Parameter**: Search text is serialized strictly under the `q` query parameter (`/search?q={query}`).
2. **Unicode & Transliteration Preservation**: The search input fully supports Latin text, IAST diacritics (e.g., `ā`, `ī`, `ū`, `ṛ`, `ṣ`, `ś`, `ñ`, `ṭ`, `ḍ`), and Devanāgarī script. Characters are preserved during normalization and safely URL-encoded.
3. **Whitespace Normalization**: Leading and trailing whitespaces are trimmed; consecutive internal spaces are collapsed to a single space for querying.
4. **Bounded Input Length**: Query input is bounded to a maximum of $200\text{ characters}$ according to Block F7. Queries exceeding this length are handled according to F7's parameter validation and canonicalization rules without arbitrary semantic truncation.
5. **Untrusted Input Invariant**: Raw query strings are treated as untrusted user input; string interpolation directly into DOM structures or dynamic evaluation is prohibited.

---

## 9. URL & State Integration Architecture

The separation of search state is strictly coordinated across Blocks F5, F6, F7, and F8:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        SEARCH STATE GOVERNANCE                         │
├──────────────┬──────────────┬──────────────────────────────────────────┤
│ Architectural│ Authoritative│ State Responsibility & Scope             │
│ Layer        │ Owner        │                                          │
├──────────────┼──────────────┼──────────────────────────────────────────┤
│ **URL State**│ **Block F7** │ Canonical search query (`q`), active     │
│              │              │ search facets/filters, and collection    │
│              │              │ pagination (`offset`, `limit`)           │
├──────────────┼──────────────┼──────────────────────────────────────────┤
│ **Server     │ **Block F6** │ Cached search DTOs, autocomplete cache,  │
│ State**      │              │ HTTP conditional revalidation, inflight  │
├──────────────┼──────────────┼──────────────────────────────────────────┤
│ **UI State** │ **Block F8** │ Raw input text buffer, suggestion open/  │
│              │              │ closed, highlighted suggestion index,    │
│              │              │ search overlay visibility                │
├──────────────┼──────────────┼──────────────────────────────────────────┤
│ **Client     │ **Block F5** │ Display and script preferences (governed │
│ Prefs**      │              │ by Block F5 and Block F16)               │
└──────────────┴──────────────┴──────────────────────────────────────────┘
```

### 9.1 State Flow Rules
1. **Typing is Transient**: While the user types in the search bar, the characters live in local UI state. The URL is not updated per keystroke.
2. **Submission Commits to URL**: Submitting the query initiates navigation to `/search?q={query}` governed by Block F7.
3. **Browser History Restorability**: Navigating `Back` or `Forward` restores the URL-representable search exploration state from the URL.

---

## 10. Pagination & Result Navigation Architecture

In accordance with Stage 1 API standards ([05-api-architecture.md §7.1](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/05-api-architecture.md#L242)) and Block F6 (§11):

```
┌────────────────────────────────────────────────────────────────────────┐
│                     OFFSET PAGINATION CONTRACT                         │
├────────────────────────────────────────────────────────────────────────┤
│ 1. CANONICAL PARAMETERS: `offset` (integer $\ge 0$, default `0`) and   │
│    `limit` (integer $1\text{--}100$, default `20`).                    │
├────────────────────────────────────────────────────────────────────────┤
│ 2. DEFAULT OMISSION: When `offset=0`, it is omitted from the URL.      │
├────────────────────────────────────────────────────────────────────────┤
│ 3. FILTER PRESERVATION: Navigating to page 2 (`offset=20`) preserves   │
│    the active search query and applicable search facets/filters.       │
├────────────────────────────────────────────────────────────────────────┤
│ 4. ZERO CURSOR PAGINATION: Public search collections never expose or   │
│    consume cursor tokens, keysets, or opaque pagination markers.       │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 11. Search Behavioral State Machine

Search UX transitions are governed by a deterministic state machine cleanly separating interaction, data lifecycle, and epistemic states:

```
┌────────────────────────────────────────────────────────────────────────┐
│                      SEARCH BEHAVIORAL MATRIX                          │
├───────────────────┬──────────────────┬─────────────────────────────────┤
│ State Identifier  │ Dimension        │ Description & UI Manifestation  │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **`idle`**        │ Interaction (UI) │ Input inactive; dropdown closed.│
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **`editing`**     │ Interaction (UI) │ User typing; suggestions shown. │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **`auto_loading`**│ Data (F6)        │ Fetching suggestions; spinner.  │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **`auto_results`**│ Data (F6)        │ Suggestions displayed to user.  │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **`auto_empty`**  │ Data (F6)        │ 0 suggestions matching query.   │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **`auto_error`**  │ Data (F6)        │ Suggestion fetch failed; notice.│
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **`search_load`** │ Data (F6)        │ Query submitted; page skeletons.│
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **`search_ready`**│ Data (F6)        │ Results rendered on `/search`.  │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **`search_empty`**│ Data (F6)        │ 0 results found for query.      │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **`search_error`**│ Data (F6)        │ Search API failed; error view.  │
├───────────────────┼──────────────────┼─────────────────────────────────┤
│ **`epistemic_*`** │ Domain (B2)      │ Certainty of individual items   │
│                   │                  │ (`known`, `conflicting`, etc.). │
└───────────────────┴──────────────────┴─────────────────────────────────┘
```

---

## 12. Responsive Search Recomposition

Search UX adapts structurally across Block F4's four viewport classes:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   RESPONSIVE RECOMPOSITION MATRIX                      │
├──────────────────┬─────────────────────────────────────────────────────┤
│ Viewport Class   │ Search UX Presentation & Behavior                   │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Wide &         │ Persistent search box in header; autocomplete drops │
│ Expanded**       │ down directly below input; result cards render in a │
│                  │ comfortable two-column or wide list grid.           │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Medium**       │ Header search input expands on focus; contextual    │
│                  │ overlay focuses attention; single-column stack.    │
├──────────────────┼─────────────────────────────────────────────────────┤
│ **Compact**      │ Search button triggers a dedicated full-screen      │
│                  │ overlay; auto-focuses input; suggestion list fills  │
│                  │ remaining screen height with accessible touch rows. │
└──────────────────┴─────────────────────────────────────────────────────┘
```

---

## 13. Accessibility Architecture (A11y Contract)

In coordination with Block F16 standards:
1. **ARIA Combobox Pattern**: The search input implements `role="combobox"`, `aria-autocomplete="list"`, `aria-expanded="true/false"`, and `aria-controls="search-suggestions-list"`.
2. **Active Descendant Navigation**: Suggestion items use `role="option"` with a unique ID; the active suggestion is conveyed to screen readers via `aria-activedescendant`.
3. **Live Region Announcements**:
   - Number of available suggestions announced via `aria-live="polite"` (e.g., *"{count} suggestions available for {query}"*).
   - Search page results announced upon load (e.g., *"{count} results loaded for query {query}"*).
4. **Visible Focus**: Interactive suggestion items and search inputs maintain a prominent $2\text{ px}$ semantic focus ring adhering to Block F3 contrast requirements.
5. **Reduced Motion**: All dropdown animations and transitions respect `prefers-reduced-motion: reduce`.

---

## 14. Error, Resilience & Untrusted Input Handling

1. **URL Validation Authority**: Block F7 authoritatively parses, validates, and canonicalizes all URL parameters. Block F8 consumes canonical URL state to render the search interface.
2. **Network Failures**: When search requests fail (HTTP 5xx, network timeout), the UI renders an understandable user-facing search error notice with a clear retry action, preserving meaningful backend error semantics.
3. **Graceful Zero Results**: When a search yields 0 matches, the UI provides helpful exploratory suggestions (e.g., *"Try searching by alternate name, Parva, or kinship"*), avoiding dead-end states.

---

## 15. Performance Architecture Principles

In alignment with Block B9 backend targets and Block F17 budget governance:
1. **Debounced Suggestion Dispatch**: User typing is buffered locally; network requests are dispatched only after input stabilization, minimizing unnecessary API queries.
2. **Client-Side Result Caching**: Block F6 caching and conditional revalidation may reduce redundant requests and support efficient restoration of previously visited search states.
3. **Backend Compatibility**: Search queries respect the backend B9 target limits (Autocomplete $\le 20\text{ KB}$, Search Results $\le 150\text{ KB}$) and rate limits ($120\text{ req/min}$). Formal frontend performance budgets are governed by Block **F17**.

---

## 16. Security & Privacy Architecture

1. **Untrusted Data Boundary**: All search input strings are treated as untrusted. They are never rendered using unescaped HTML injection.
2. **Zero Client Secret Leaks**: Search queries do not contain authentication tokens or session identifiers.
3. **Public Exploration Scope**: Search operates entirely within the anonymous public read-only boundary; administrative records are completely inaccessible via search endpoints.

---

## 17. Upstream Dependency & Downstream Ownership Matrix

```
┌────────────────────────────────────────────────────────────────────────┐
│                     DOWNSTREAM OWNERSHIP MATRIX                        │
├────────────────────────────────────────┬───────────────────────────────┤
│ Architectural Domain                   │ Authoritative Target Block    │
├────────────────────────────────────────┼───────────────────────────────┤
│ Canonical Entity Domain & Data Types   │ **Stage 1 (B2 / B5 / B6)**    │
│ Design Tokens & Visual Badges          │ **Block F3**                  │
│ Application Shell & Header Landmarks   │ **Block F4**                  │
│ State Management & Client Store Model  │ **Block F5**                  │
│ API Client, Network & Cache Layer      │ **Block F6**                  │
│ Route Grammar & URL Parameter Schema   │ **Block F7**                  │
│ Search UX, Autocomplete & Results View │ **Block F8 (This Document)**  │
│ Character Profile Layouts & Tabs       │ **Block F9**                  │
│ Knowledge Graph Visualization Canvas   │ **Block F10**                 │
│ Evidence Drawer Citation & Text UX     │ **Block F15**                 │
│ Accessibility Standards & Script Engine│ **Block F16**                 │
│ Performance Budgets & Bundle Audits    │ **Block F17**                 │
│ Concrete Component Setup & Packages    │ **Stage 4 (Implementation)**  │
└────────────────────────────────────────┴───────────────────────────────┘
```

---

## 18. Architectural Decision Record (ADR)

| Decision ID | Architectural Decision | Chosen Approach | Alternatives Evaluated | Rationale & Trade-offs | Owner | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **ADR-FE-43** | Search Entry Role | **Exploration Entry Point** | Detached utility box | Seamlessly routes users directly into graph, lineage, or profile lenses. | F8 | **DECIDED** |
| **ADR-FE-44** | Autocomplete State | **Transient Local UI State** | Synchronized to URL | Prevents polluting browser history with every single keystroke. | F8 | **DECIDED** |
| **ADR-FE-45** | Suggestion Action | **Direct Canonical Entity Route**| Intermediate search page | Reduces friction; allows immediate navigation to target characters or places.| F8 | **DECIDED** |
| **ADR-FE-46** | Search Pagination | **Public Offset Pagination** | Cursor pagination | Preserves Stage 1 B5 contracts across all search result collections. | F8 | **DECIDED** |
| **ADR-FE-47** | Multilingual Search | **Unicode & IAST Preservation** | Strip diacritics | Respects Indian epic scholarly standards, preserving IAST and Devanāgarī.| F8 | **DECIDED** |
| **ADR-FE-48** | Epistemic Display | **Strict B2 Epistemic Mapping** | Arbitrary certainty scores | Prevents data fabrication; surfaces disputed or approximate facts accurately.| F8 | **DECIDED** |

---

## 19. Requirement Traceability Matrix

| Requirement / Principle | Source Document | Implementing Block F8 Section | Coverage Status |
| :--- | :--- | :--- | :--- |
| **Global Search Capability** | PRD §8, B5 §5.5, B6 §1| §4 (Autocomplete), §6 (Search Results UX) | **SATISFIED** |
| **Multilingual IAST Support**| PRD §15, F3 §9 | §5.1 (Suggestion Anatomy), §8 (Query Normalization) | **SATISFIED** |
| **Zero Data Fabrication** | Rule 03, F1 §5.4 | §5.1 (Epistemic Display), §18 (ADR-FE-48) | **SATISFIED** |
| **Stateless Deep-Linking** | F1 §5.8, F7 §6.8 | §9 (URL & State Integration) | **SATISFIED** |
| **Public Offset Pagination** | B5 §7.1, F6 §11 | §10 (Pagination Contract) | **SATISFIED** |
| **Responsive Shell Layout** | F4 §3, §9 | §3 (Search Entry), §12 (Responsive Recomposition) | **SATISFIED** |
| **Backend Payload Limits** | B9 §5 | §15 (Performance Architecture) | **SATISFIED** |
| **Accessibility Combobox** | F1 §5.10, F16 | §13 (Accessibility Architecture) | **SATISFIED** |

---

## 20. F8 Exit Criteria Checklist

- [x] Global search entry ergonomics defined across Expanded, Medium, and Compact layouts.
- [x] Autocomplete interaction lifecycle (idle $\rightarrow$ input $\rightarrow$ suggestions $\rightarrow$ selection) is formalized.
- [x] Suggestion information architecture includes canonical IAST names, categories, and B2 epistemic states.
- [x] Dedicated search results page (`/search`) layout, progressive disclosure, and cards are specified.
- [x] Search resource domain coverage follows the searchable resource domains established by B2/B6.
- [x] Query input boundaries support Unicode, IAST diacritics, and Devanāgarī up to 200 characters.
- [x] State ownership strictly coordinated across F5 (state), F6 (data), F7 (routing), and F8 (UX).
- [x] Public collection pagination strictly preserves the offset contract without cursor pagination.
- [x] Search behavioral state machine distinguishes interaction, data lifecycle, and epistemic states.
- [x] Responsive recomposition defined across the four F4 viewport classes.
- [x] Accessibility contracts formalized for ARIA combobox patterns, live regions, and focus management.
- [x] Error handling, untrusted input boundaries, and resilience behaviors are specified.
- [x] Downstream ownership matrix across F9–F17 and Stage 4 is explicitly catalogued.
- [x] Zero application source code, UI packages, or component files were introduced.
- [x] Zero Stage 1 backend documents or Blocks F1–F7 documents were modified.
