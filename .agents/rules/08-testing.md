# Rule 08: Testing, Verification & Quality Standards

## Purpose
This rule defines the verification standards and completion criteria required before declaring any task, feature, or subsystem complete.

---

## 1. Definition of "Done"

A feature or task is **NEVER complete merely because code compiles or runs without immediate crashing**.

Before declaring any feature complete, agents must verify:
1. **Functional Requirements**: Satisfies all explicit requirements and acceptance criteria in the PRD (`docs/03-prd/`).
2. **Data Integrity & Provenance**: Respects zero fabrication, handles missing/approximate data without inventing facts, and preserves source linkage.
3. **Mandatory Edge States**: Verified behavior across Loading, Empty, Error, Partial Data, and No Results states.
4. **Responsive Verification**: Verified on both Desktop and Mobile viewport contexts.
5. **Accessibility Standards**: Verified semantic structure, keyboard navigation, focus visibility, contrast, and non-reliance on color alone.
6. **State & URL Persistence**: Verified that Focus, active filters, and deep-link canonical routes behave correctly upon reload and navigation.
7. **Regression Impact**: Existing lenses, shared components, and graph traversal remain unbroken.

---

## 2. Verification Workflows

1. **Browser Capabilities**: For interactive and visual features, leverage Antigravity's browser testing capabilities to verify rendering, responsiveness, and user flows.
2. **Automated Testing**: Run unit, component, or integration test suites whenever automated tests exist.
3. **Transparent Reporting**: If a test or verification step fails, fix the underlying defect or report it clearly with exact reproduction steps. Never conceal test failures, skip validation, or mark unverified work as complete.

---

## 3. Rule Priority & Conflict Resolution

1. **Human Instruction**: Explicit instructions provided by the user for the current task take immediate precedence.
2. **Domain Documentation**: The PRD (`docs/03-prd/`) defines authoritative acceptance criteria; the Blueprint and Reference define system behavior.
3. **Persistent Rules**: These rules govern quality control and verification gates.
4. **Technical Specifications**: Future integration and build specifications (`docs/06-integration/`, `docs/07-build/`) will define automated CI test pipelines.
5. **Conflict Escalation**: If verification criteria cannot be satisfied due to upstream ambiguities, **STOP and report the obstacle**.
