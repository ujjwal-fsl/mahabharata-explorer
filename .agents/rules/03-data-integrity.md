# Rule 03: Data Integrity & Epistemic Truth

## Purpose
This rule establishes strict epistemic standards and zero-fabrication policies for all data ingestion, model structures, AI-assisted tasks, and UI representations.

---

## 1. Zero-Fabrication Mandate (CRITICAL)

Agents must **NEVER fabricate, extrapolate, or invent**:
- Historical or narrative facts
- Dates, calendar years, or sequence indices
- Chronological certainty
- Geographic coordinates or map placements
- Familial, social, or political relationships
- Character identities, parentage, or epithets
- Source citations, book/chapter/verse locators, or URLs
- Direct quotations or textual excerpts
- Troop counts, formation geometry, or battle statistics
- Epistemic certainty or consensus where none exists

---

## 2. Epistemic Data States

When handling data, agents and systems must distinguish between explicit data states:

- **Known**: Supported by curated, verified structured knowledge.
- **Unknown**: The fact is unrecorded or not established in canonical sources.
- **Not Researched**: The project research workflow has not yet evaluated the fact.
- **Not Applicable**: The attribute does not apply to this entity type or context.
- **Conflicting**: Different authoritative sources or traditions provide differing accounts.
- **Approximate**: The data is supported as an approximation, not exact precision.

---

## 3. Strict Truthfulness Principles

1. **Missing Data is Not Permission to Invent**: If an attribute (date, coordinate, portrait, source) is missing, render the appropriate neutral empty/unknown state. Never generate speculative data to make a screen "look complete."
2. **No False Precision**: If an event lacks an exact date, place it using relative narrative sequence; do not invent a specific year or day.
3. **No False Coordinates**: If a location lacks verifiable coordinates, display its details and indicate map placement is unavailable; do not invent geographic points.
4. **No False Consensus**: Conflicting traditions or accounts must be preserved as distinct claims or variants, never silently flattened into one arbitrary choice.
5. **No False Authority**: Never present an AI summary as an ancient quote, an illustration as an authoritative map, or an editorial inference as canonical text.
6. **Provenance Tracking**: Inferences and derived relationships must identify their inputs, derivation rule, and evidence chain.
7. **Stop on Missing Critical Data**: If factual information is required to complete a task but is missing from authoritative project sources, **STOP and report the missing data** instead of guessing.

---

## 4. Rule Priority & Conflict Resolution

1. **Human Instruction**: Explicit instructions provided by the user for the current task take immediate precedence.
2. **Domain Documentation**: The PRD and Master Blueprint govern data integrity rules (`docs/00-documentation-context.md`).
3. **Persistent Rules**: This data integrity rule is non-negotiable and strictly enforced across all development.
4. **Technical Specifications**: Future data schemas and ingestion pipelines must strictly enforce these constraints at the data layer.
5. **Conflict Escalation**: If uncertain about factual veracity or source attribution, agents must **STOP and request human clarification**.
