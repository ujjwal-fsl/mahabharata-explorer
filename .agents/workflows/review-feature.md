# Workflow: Review Feature

## Purpose
A rigorous, read-only workflow for evaluating an existing implementation against architectural, functional, UX, responsive, and data-integrity standards.

---

## Operating Rules
- This workflow is strictly **READ-ONLY**. Do not modify code or create commits during a review unless the user explicitly requests fixes.
- Evaluate against `.agents/rules/` and authoritative project documents (`docs/`).

---

## Execution Steps

### 1. Requirements & Document Check
- Review the specific task prompt and relevant PRD requirements (`docs/03-prd/Mahabharata_Explorer_PRD.md`).
- Identify the governing architectural contracts in `docs/01-project-blueprint/` and `docs/02-detailed-reference/`.

### 2. Code & Architecture Inspection
- Inspect the modified or created files.
- Verify compliance with the **Five-Layer Architecture** (Data, Evidence, Exploration, Presentation, User State).
- Check for **System Reuse**: Ensure no duplicate event engines, timeline visualizers, search handlers, or map systems were introduced.

### 3. Data Integrity & Truth Verification
- Verify that no factual data, coordinates, dates, or relationships were fabricated or hardcoded into UI components.
- Check that missing or approximate data is represented transparently without false precision.
- Verify that provenance (`Entity → Claim → Evidence → Source`) is maintained.

### 4. UX, Responsive & Accessibility Inspection
- Check interaction models: No dead ends, progressive disclosure, and clear navigation paths.
- Check responsive adaptations: Verify that side panels become bottom sheets and complex graphs adapt to mobile.
- Check accessibility: Semantic markup, visible focus states, contrast, and non-reliance on color alone.
- Check mandatory edge states: Loading, empty, error, and partial-data behavior.
- Check state persistence: Global Focus preservation and canonical URL routing.

### 5. Regression & Scope Audit
- Verify that changes do not break existing lenses or traversal paths.
- Confirm that no unapproved V2/V3 features or speculative dependencies leaked into the codebase.

---

## Output Report Structure

Structure the review findings into three clear, prioritized categories:

```markdown
### Feature Review: [Feature Name]

#### 1. Required Fixes (Blocking)
- [List any bugs, architectural violations, data fabrication, or PRD requirement gaps that must be corrected before approval]

#### 2. Recommended Improvements (Non-Blocking)
- [List cleanups, performance optimizations, or minor styling refinements within current scope]

#### 3. Future Considerations (Deferred to V2/V3)
- [List ideas or enhancements identified during review that belong to later roadmap stages]
```
