# Rule 02: Architecture & System Boundaries

## Purpose
This rule governs system architecture, layer separation, graph data models, system reuse, and architectural discipline across the codebase.

---

## 1. Foundational Architecture

Agents must adhere strictly to the established **Five-Layer Architecture**:

1. **Data Layer**: What exists (Characters, Events, Locations, Groups, Relationships, FamilyRelationships, Wars, WarDays, Formations).
2. **Evidence Layer**: Why it is represented (Claims, Evidence, Sources, provenance, uncertainty).
3. **Exploration Layer**: How users navigate (Timeline, Map, Explore, War, Search, Focus).
4. **Presentation Layer**: How it looks and behaves (Design tokens, components, layouts, motion, cultural accents).
5. **User State Layer**: What the user is doing or prefers (Focus, active filters, selected day/event, theme, density, motion settings).

---

## 2. Core Architectural Rules

1. **Unified Knowledge Graph**: The knowledge graph is the shared foundation for all views. Do not construct isolated data silos for individual pages or features.
2. **First-Class Relationships**: Relationships between entities must be modeled as structured data (`source_entity`, `target_entity`, `relationship_type`, `directionality`, `claim_id`). Never hardcode factual relationships directly into UI component logic.
3. **No Inferred Proximity**: Never infer a factual relationship merely because two entities appear together in narrative proximity or on-screen layout.
4. **Mandatory System Reuse**:
   - **One Event System**: A War Event is an Event with War/WarDay context—do not create a separate War Event entity or engine.
   - **One Map System**: Main Map, War Map, Location Map, and Journey routes must share one underlying Map system with contextual layers.
   - **One Timeline Architecture**: Main timeline, character chronology, and war day timelines must share one timeline visualization system.
   - **One Global Search**: Global search handles all entity types with type ranking and filters; do not create fragmented per-page search engines.
   - **One Global Focus**: Focus operates as a cross-cutting state across all lenses.
   - **One Evidence Model**: All entities link to sources via the unified `Entity → Claim → Evidence → Source` model.
5. **Architectural Discipline**: Do not introduce new major systems, entities, or subsystems without explicit approval.
6. **Technology Stack Neutrality**: Architecture rules define system structure and boundaries; specific technology choices (frameworks, databases, libraries) must remain undecided until formally planned in Stage 1 and Stage 2.

---

## 3. Rule Priority & Conflict Resolution

1. **Human Instruction**: Explicit instructions provided by the user for the current task take immediate precedence.
2. **Domain Documentation**: The Master Consolidated Blueprint governs overall architecture; the PRD governs functional scope (`docs/00-documentation-context.md`).
3. **Persistent Rules**: These agent rules govern architectural integrity and system reuse.
4. **Technical Specifications**: Once approved, future technical specifications govern concrete implementation details.
5. **Conflict Escalation**: If architectural ambiguities or contradictions arise, agents must **STOP and request human clarification**.
