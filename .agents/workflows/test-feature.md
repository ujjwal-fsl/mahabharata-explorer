# Workflow: Test Feature

## Purpose
A specialized, evidence-based verification workflow to validate that an implemented feature satisfies all explicit acceptance criteria, edge states, responsive layouts, and quality gates.

---

## Operating Rules
- Strictly adhere to Rule 08 (`.agents/rules/08-testing.md`).
- **Never claim a test or check passed if it was not executed.**
- If a test cannot be performed because dependencies or data do not exist, report it explicitly as `NOT TESTED / BLOCKED` rather than guessing or faking a pass.

---

## Execution Steps

### 1. Identify Acceptance Criteria
- Extract the feature-specific acceptance criteria from `docs/03-prd/Mahabharata_Explorer_PRD.md` (e.g., APP-xxx, TIM-xxx, CHR-xxx).
- List the expected behaviors, error handling paths, and state persistence rules.

### 2. Automated Test Execution
- Run relevant unit, component, or integration test suites.
- Record exact commands run and output results.

### 3. Interactive & User Flow Verification
- Using available browser capabilities, step through the primary user journeys:
  - Entry via search, timeline, map, or direct route
  - Traversal to related entities without dead ends
  - Activating and clearing Focus mode
  - Deep-linking and URL reload persistence

### 4. Edge-State Verification
- Systematically trigger and observe:
  - **Loading state** (spinner / visual transition)
  - **Empty state** (meaningful neutral message)
  - **Error state** (graceful error message with retry/back option)
  - **Partial-data state** (render available fields; neutral indicators for missing fields)
  - **No search results** (clear empty feedback)

### 5. Responsive & Cross-Device Verification
- Inspect viewport adaptations across:
  - Desktop landscape
  - Mobile portrait
  - Mobile landscape / Tablet (where relevant)
- Check that overlays (drawers vs bottom sheets), touch targets, and layout flow remain functional.

### 6. Accessibility & Console Verification
- Verify keyboard navigation (Tab, Enter, Escape, Arrow keys).
- Verify visible focus rings and semantic HTML controls.
- Check the browser console log for uncaught JavaScript errors, warnings, or network failures.

---

## Verification Report Format

```markdown
### Verification Report: [Feature Name]

| Verification Area | Status | Evidence / Notes |
| :--- | :--- | :--- |
| **Functional (PRD Criteria)** | PASS / FAIL / BLOCKED | [Summary of observed behavior] |
| **Automated Tests** | PASS / FAIL / NOT RUN | [Test command and pass/fail counts] |
| **Edge States (Load/Empty/Error)** | PASS / FAIL / NOT CHECKED | [Observations across edge states] |
| **Responsive (Desktop & Mobile)** | PASS / FAIL / NOT CHECKED | [Desktop vs mobile interaction notes] |
| **Accessibility (A11y)** | PASS / FAIL / NOT CHECKED | [Keyboard, focus, contrast notes] |
| **State & Canonical URLs** | PASS / FAIL / NOT CHECKED | [Deep link and reload persistence] |
| **Console & Runtime Errors** | PASS / FAIL / NOT CHECKED | [Console error/warning findings] |

#### Failures & Action Items
- [Detailed description of any failures with exact reproduction steps]
```
