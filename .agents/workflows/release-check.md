# Workflow: Release Check

## Purpose
A comprehensive, holistic pre-release verification workflow to evaluate system-wide readiness, data integrity, cross-lens stability, responsive health, and scope compliance across the entire project.

---

## Operating Rules
- This is an overarching quality audit. Do not automatically apply code fixes during a release check unless explicitly requested by the user.
- Every check must be transparently reported as `PASS`, `FAIL`, `BLOCKED`, or `NOT CHECKED`. Never fabricate verification.

---

## Audit Checklist

### 1. Functional & Critical User Journeys
- [ ] Primary navigation functions across all primary lenses (Explore, Timeline, Map, War, Search).
- [ ] Cross-lens traversal works without dead ends (Character → Event → Location → Source).
- [ ] Global Focus functions across lenses and clears cleanly.
- [ ] Global Search returns ranked results across supported entity types with direct canonical routing.

### 2. Data Integrity & Provenance
- [ ] No fabricated facts, coordinates, dates, or relationships.
- [ ] Missing, unknown, approximate, or conflicting data states render correctly.
- [ ] Source provenance (`Entity → Claim → Evidence → Source`) is intact and verifiable.

### 3. Architecture & System Reuse
- [ ] Five-layer architecture is cleanly maintained without cross-layer violations.
- [ ] Shared systems (Event, Timeline, Map, Search, Focus, Evidence) remain unified without duplicate engines.
- [ ] Relationships are modeled as first-class data, not hardcoded into UI components.

### 4. Responsive Design
- [ ] Desktop landscape layout is clean and functional.
- [ ] Mobile portrait layout adapts properly (bottom sheets, compact navigation, touch targets).
- [ ] Tablet and mobile landscape layouts adapt cleanly where applicable.

### 5. Accessibility (A11y)
- [ ] Complete keyboard navigability across core exploration loops.
- [ ] Visible focus indicators and accessible color contrast.
- [ ] Meaning is not communicated by color or cultural symbolism alone.

### 6. Performance, Console & Runtime Stability
- [ ] Zero unhandled runtime exceptions or severe console errors.
- [ ] Progressive loading functions correctly (shell → primary data → secondary visualizers).
- [ ] No broken asset links, broken deep routes, or failed API calls.

### 7. Scope & Roadmap Discipline
- [ ] No unapproved V2/V3 features or speculative infrastructure present in current release build.
- [ ] Current implementation aligns strictly with the approved PRD and stage goals.

---

## Release-Check Report Format

```markdown
# Release Health Check: [Release / Milestone Name]

## Executive Summary
- **Overall Status**: READY FOR RELEASE / ACTION REQUIRED / BLOCKED
- **Key Highlights**: [Brief 2-3 sentence overview]

## Detailed Audit Results

| Audit Category | Status | Notes & Observations |
| :--- | :--- | :--- |
| **1. Functional Journeys** | PASS / FAIL / BLOCKED / NOT CHECKED | [Notes on core flows & lenses] |
| **2. Data Integrity** | PASS / FAIL / BLOCKED / NOT CHECKED | [Zero-fabrication & provenance audit] |
| **3. Architecture & Reuse** | PASS / FAIL / BLOCKED / NOT CHECKED | [Five-layer & system reuse check] |
| **4. Responsive Layout** | PASS / FAIL / BLOCKED / NOT CHECKED | [Desktop & mobile verification] |
| **5. Accessibility** | PASS / FAIL / BLOCKED / NOT CHECKED | [A11y compliance observations] |
| **6. Stability & Performance** | PASS / FAIL / BLOCKED / NOT CHECKED | [Console, runtime, & route health] |
| **7. Scope Compliance** | PASS / FAIL / BLOCKED / NOT CHECKED | [V1 scope boundary verification] |

## Blocking Issues (Must resolve before release)
- [Issue description with affected components]

## Non-Blocking Observations & Recommendations
- [Improvement suggestions for upcoming iterations]
```
