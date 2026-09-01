# Rule 07: Visual Language & Cultural Philosophy

## Purpose
This rule defines the core aesthetic philosophy, cultural asset integration standards, and visual constraints for the Mahābhārata Explorer.

---

## 1. Core Visual Philosophy

**MODERN FIRST. TRADITIONAL SECOND.**

The goal is to build an exceptional modern digital atlas of an ancient epic. The application must look, feel, and perform like a sophisticated modern information product, embedding Indic and Mahābhārata cultural motifs deliberately and meaningfully into components, transitions, and micro-moments.

---

## 2. Explicit Anti-Patterns & Visual Traps

Agents and designers must strictly **AVOID**:
- Recreating an ancient parchment, scroll, or manuscript as a website.
- Mythology-blog or generic Indian cultural website aesthetics.
- Temple-themed or fantasy RPG video-game interfaces.
- Burnt paper, excessive brown, sepia, heavy warmth, or excessive gold.
- Heavy textures, ornamental borders on every container, or decorative clutter.
- Random cultural decoration (e.g., lotuses, Om symbols, mandalas, or Devanagari text inserted without semantic meaning).
- Decorative weapons or symbols placed solely for decoration rather than semantic role.
- Excessive glow, cinematic particle effects, or unnecessary 3D elements.

---

## 3. Semantic Cultural Enrichment & Abstraction

1. **Semantic Relevance**: Cultural references must directly reinforce the meaning of the interaction (e.g., using the circular geometry of the Sudarshan Chakra for a rotational loading spinner, or arrow geometry for directional causal traversal).
2. **Modular Asset Strategy**: Systems must follow `Generic Component → Asset Slot → Mahābhārata Cultural Asset`. Components must function cleanly with neutral modern assets and permit progressive cultural asset enhancement.
3. **Abstraction Levels**:
   - **Level 0 (Modern)**: Completely modern (standard search inputs, form fields, checkboxes, settings toggles remain modern).
   - **Level 1 (Indic Geometry)**: Abstract symmetry, radial geometry, subtle line work, and rhythm. *(Preferred)*
   - **Level 2 (Symbolic Mahābhārata)**: Recognizable, restrained symbols (e.g., chakra, bow, conch, banner). *(Preferred)*
   - **Level 3 (Literal Illustration)**: Detailed illustrations or character representations. *(Use selectively)*
   - **Level 4 (Narrative Interaction)**: Interaction physics or transitions inspired by epic concepts. *(Preferred)*
4. **Token Agnosticism in Stage 0**: Concrete color palettes, font families, and asset formats will be specified during Stage 2; do not invent speculative design tokens during Stage 0.

---

## 4. Rule Priority & Conflict Resolution

1. **Human Instruction**: Explicit instructions provided by the user for the current task take immediate precedence.
2. **Domain Documentation**: The Detailed Master Reference (`docs/02-detailed-reference/`) is authoritative for visual language and component styling rules.
3. **Persistent Rules**: These rules enforce visual discipline and prevent ornamental degradation.
4. **Technical Specifications**: Future design system and frontend specifications (`docs/05-frontend/`) will formalize theme tokens and asset pipelines.
5. **Conflict Escalation**: When in doubt between a modern neutral component and a cultural enhancement, default to **modern neutral** and request human guidance.
