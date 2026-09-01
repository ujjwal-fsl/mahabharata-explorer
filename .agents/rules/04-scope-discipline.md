# Rule 04: Scope Discipline & Stage Boundaries

## Purpose
This rule enforces strict task scope control, prevents premature feature creep, and guarantees adherence to the project's phased roadmap.

---

## 1. Phased Project Roadmap

Development proceeds strictly through the defined project stages:

- **STAGE 0** — Antigravity Project Setup *(Current)*
- **STAGE 1** — Backend Architecture
- **STAGE 2** — Frontend Architecture
- **STAGE 3** — Integration Architecture
- **STAGE 4** — Implementation
- **STAGE 5** — Validation

Agents must **not rename, reorder, split, merge, or insert additional top-level stages** without explicit human approval.

---

## 2. Scope Boundaries

1. **Current Task Only**: Implement only the specific stage, sub-stage, or task explicitly requested by the user.
2. **No Speculative Building**: Do not proactively build speculative infrastructure, helper functions, or abstractions "just in case" they might be needed later.
3. **Respect V1/V2/V3 Release Boundaries**: The PRD (`docs/03-prd/Mahabharata_Explorer_PRD.md`) is authoritative for explicit V1, V2, and V3 release-scope definitions. Agents must implement only features approved for the current release scope and never pull future release features into current development.
4. **No Premature Technology Commitments**: Do not introduce package dependencies, frameworks, or database tools until reaching their designated architectural stages.
5. **Defer Discovered Non-Blocking Issues**: If an issue or idea belonging to a later stage is identified during work, record it clearly in the task report and continue with the current scope.
6. **Stop on Requirement Ambiguity**: If a requirement is ambiguous or underspecified, ask for clarification rather than assuming or inventing features.

---

## 3. Rule Priority & Conflict Resolution

1. **Human Instruction**: Explicit instructions provided by the user for the current task take immediate precedence.
2. **Domain Documentation**: The PRD (`docs/03-prd/Mahabharata_Explorer_PRD.md`) is authoritative for V1/V2/V3 scope definitions.
3. **Persistent Rules**: These agent rules govern stage execution and scope boundaries.
4. **Technical Specifications**: Approved technical specifications govern implementation details within their assigned stage.
5. **Conflict Escalation**: When in doubt about whether a feature belongs in the current scope, agents must **STOP and request human confirmation**.
