# Workflow: Build Feature

## Purpose
A standardized, disciplined lifecycle for implementing an approved feature, task, or bug fix within the current project stage.

---

## Operating Rules
- Follow all persistent rules in `.agents/rules/`.
- Consult authoritative domain documents per `docs/00-documentation-context.md`.
- Never fabricate data (Rule 03) or exceed current stage scope (Rule 04).

---

## Execution Steps

### 1. Understand
- Parse the user's task description and identify the exact requested outcome.
- Identify the governing acceptance criteria in `docs/03-prd/Mahabharata_Explorer_PRD.md`.

### 2. Inspect
- Inspect existing codebase files, shared components, models, and docs related to the task.
- Check for existing systems to reuse before creating any new abstractions.

### 3. Plan
- Formulate a clear implementation plan before modifying files:
  - Files to create or modify
  - Shared systems, components, or stores to reuse
  - Potential edge states and dependencies
  - Verification strategy

### 4. Confirm Scope
- Verify the planned change against the current project stage and V1 release scope.
- **STOP CONDITION**: If the task requires an architectural change, new external dependency, or a feature belonging to a later stage/release (V2/V3), **STOP and request explicit human approval**.

### 5. Implement
- Implement the minimal, clean solution for the requested scope only.
- Reuse established shared systems (Navigation, Timeline, Map, Entity, Relationship, Event, Search, Focus, Evidence).
- Adhere to the "Modern First, Traditional Second" visual language and modular asset slots.
- Ensure data-driven rendering with support for all epistemic states (known, unknown, approximate, conflicting).

### 6. Test
- Run relevant automated test suites (unit, integration, component) if tests are present.
- Fix any broken test cases or regressions immediately.

### 7. Verify
- For user-facing or interactive changes, verify across:
  - **Functional Behavior**: Meets PRD acceptance criteria.
  - **Edge States**: Loading, empty, error, partial-data, and no-results states.
  - **Responsive Layout**: Desktop, tablet, and mobile viewport interaction models.
  - **Accessibility**: Keyboard navigation, semantic elements, visible focus, color contrast.
  - **State & URLs**: Canonical routes, deep-link persistence, and Focus preservation.
  - **Browser Console**: No unhandled exceptions or console errors (using browser tools where applicable).

### 8. Review
- Self-review diffs to ensure no unintended files, debug logs, or speculative abstractions were introduced.
- Confirm zero fabrication and documentation consistency.

### 9. Report
- Deliver a concise report to the user summarizing:
  - Changes made (files created/modified)
  - Existing systems reused
  - Automated tests executed and results
  - Interactive/visual verifications performed (including browser checks)
  - Any limitations or non-blocking observations deferred to future stages

> [!WARNING]
> **Strict Verification Truthfulness**: Never claim an automated test, browser check, or accessibility check was performed if it was not executed.
