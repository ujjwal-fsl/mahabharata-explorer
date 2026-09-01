# Rule 06: Responsive Design & Cross-Device Behavior

## Purpose
This rule establishes mandatory responsive design principles, multi-device support requirements, and interaction-adaptation standards across all views.

---

## 1. Supported Form Factors

The product must natively and intentionally support five distinct viewport contexts:

1. **Desktop Landscape**
2. **Tablet Landscape**
3. **Tablet Portrait**
4. **Mobile Portrait**
5. **Mobile Landscape**

---

## 2. Interaction Adaptation (Not Just Scaling)

Responsive design requires **adapting the interaction model**, not merely shrinking desktop columns:

1. **Drawers & Overlays**: Desktop side panels and contextual drawers adapt to mobile bottom sheets or full-height overlay panels.
2. **Navigation**: Desktop persistent navigation rails/headers adapt to compact top navigation with bottom action navigation.
3. **Complex Visualizations**:
   - Expansive relationship graphs on desktop adapt to focused node-and-connection views on mobile.
   - Horizontal timelines on desktop adapt to touch-friendly mobile timeline views.
4. **Touch & Usability**: All interactive elements (buttons, markers, tabs, cards) must maintain accessible touch target sizes and gesture ergonomics on touch devices.
5. **Information Parity**: Essential information and source verification capabilities must remain accessible on mobile, not discarded for convenience.
6. **Feature Definition Requirement**: Every user-facing UI feature must define and verify both desktop and mobile behaviors before being declared complete.
7. **Breakpoint Agnosticism in Stage 0**: Concrete pixel breakpoints and media-query tokens will be formalized during Stage 2 design token planning; do not hardcode speculative breakpoints during Stage 0.

---

## 3. Rule Priority & Conflict Resolution

1. **Human Instruction**: Explicit instructions provided by the user for the current task take immediate precedence.
2. **Domain Documentation**: The PRD (`docs/03-prd/`) and Detailed Master Reference (`docs/02-detailed-reference/`) govern responsive requirements.
3. **Persistent Rules**: These rules govern cross-device compatibility and interaction adaptation.
4. **Technical Specifications**: Future frontend specifications (`docs/05-frontend/`) will define exact CSS breakpoints and layout engines once approved.
5. **Conflict Escalation**: If a visual layout cannot be cleanly adapted across mobile and desktop, agents must **STOP and request human clarification**.
