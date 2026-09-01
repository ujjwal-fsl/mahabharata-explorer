# Rule 05: Frontend Architecture & Engineering

## Purpose
This rule governs frontend development principles, component design, data-driven rendering, state hierarchy, and UX standards without prematurely binding the project to a specific framework.

---

## 1. Core Frontend Principles

1. **Reusable Component System**: Avoid component proliferation. Use base components with structured variants (e.g., Base Entity Card, Context Drawer, Context Bottom Sheet).
2. **Data-Driven UI**: UI components must render based on available structured data. If a field (e.g., formation, coordinates, portrait) is null, render the corresponding neutral state rather than an empty broken container.
3. **No Hardcoded Knowledge in Views**: Factual knowledge, entity records, and relationship networks must reside in structured datasets or state stores, never hardcoded inside UI view components.
4. **Separation of Presentation and Truth**: User customization (themes, text scaling, density, motion) changes presentation only. Settings must never alter factual data, chronological ordering, geographic coordinates, or source provenance.
5. **Mandatory Edge States**: Every view, lens, and card component must explicitly handle:
   - Loading states
   - Empty states
   - Error states
   - Partial / missing data states
   - No search results
6. **State Hierarchy & Navigation Persistence**:
   - **Global State**: Global Focus (e.g., Focused Entity).
   - **Page State**: Selected view, active tab, active war day.
   - **Local State**: Selected marker, hovered node, active drawer.
   - **Temporary State**: Search queries, filter inputs.
   - Changing page-level state (e.g., selecting a war day) must not accidentally wipe global entity focus unless explicitly requested.
7. **Canonical Routes & Deep Linking**: Major entities and exploration views must support shareable, bookmarkable canonical URLs (e.g., `/character/arjuna`, `/war/kurukshetra/day/13`).
8. **Framework Agnosticism in Stage 0**: Do not select or commit to specific frontend frameworks (React, Next.js, Vue, Svelte), styling solutions, or state managers until Stage 2.

---

## 2. Rule Priority & Conflict Resolution

1. **Human Instruction**: Explicit instructions provided by the user for the current task take immediate precedence.
2. **Domain Documentation**: The Detailed Master Reference (`docs/02-detailed-reference/`) governs UX and component inventories; the PRD governs functional frontend requirements.
3. **Persistent Rules**: These rules govern frontend quality, component reusability, and state discipline.
4. **Technical Specifications**: The future Frontend Master Plan (`docs/05-frontend/`) will govern framework choices and implementation details once approved in Stage 2.
5. **Conflict Escalation**: If UI requirements and architectural reusability conflict, agents must **STOP and request human clarification**.
