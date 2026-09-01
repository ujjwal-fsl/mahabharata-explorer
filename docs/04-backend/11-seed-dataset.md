# Seed Dataset Strategy & Content Architecture (Block B11)

## 1. Purpose & Seed-Data Principles

This document establishes the **Seed Dataset Strategy & Content Architecture** for the **Mahābhārata Explorer** backend. It specifies the scope, entity coverage, graph topology, epistemic state distribution, provenance linkages, and validation expectations for the representative V1 seed dataset.

In accordance with project principles:
- **Verification over Completeness (Rule 08, REQ-ING-01)**: The V1 seed dataset is an **architectural verification corpus**, not an exhaustive encyclopedic capture of the entire Mahābhārata tradition. Its goal is to provide a structurally rich, multi-lens dataset capable of exercising every entity type, relationship constraint, exploration lens, epistemic certainty state, and provenance pathway established in B1–B10.
- **Zero Fabrication (Rule 03, PRD §11)**: Every factual record, name variant, relationship edge, and citation in the seed dataset must be derived from verified scholarly editions (such as the BORI Critical Edition) or academically documented variant traditions. The system must **never** fabricate historical dates, coordinates, relationships, or citations merely to fill out fields or make test graphs appear dense.
- **Epistemic Honesty (Rule 03, B2 §5, B4 §6)**: The seed dataset must deliberately include genuine historical ambiguities—unmapped locations, disputed chronologies, conflicting traditions, and unknown parentage—to rigorously exercise the backend's epistemic status handling.

---

## 2. Canonical Entity Coverage Matrix

The seed dataset must provide representative coverage for all canonical entity tables established in [02-data-architecture.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/02-data-architecture.md) and [03-knowledge-graph.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/03-knowledge-graph.md):

```
┌────────────────────────────────────────────────────────────────────────┐
│                   CANONICAL ENTITY COVERAGE MATRIX                     │
├──────────────────┬──────────────────┬──────────────────────────────────┤
│ Entity Domain    │ Canonical Table  │ Minimum Architectural Coverage   │
├──────────────────┼──────────────────┼──────────────────────────────────┤
│ 1. Character     │ characters       │ Major heroes, elders, teachers,  │
│                  │                  │ monarchs, allies, and rivals.    │
│ 2. Event         │ events           │ Chronological milestones: peace, │
│                  │                  │ exile, diplomacy, war days.      │
│ 3. Location      │ locations        │ Capitals, forests, pilgrimage    │
│                  │                  │ sites, and battlefield regions.  │
│ 4. Group         │ groups           │ Dynasties, kingdoms, factions,   │
│                  │                  │ and military confederations.     │
│ 5. War           │ wars             │ The Kurukshetra War structure.   │
│ 6. WarDay        │ war_days         │ Selected tactical war days       │
│                  │                  │ (e.g., Days 1, 13, 14, 18).      │
│ 7. Formation     │ formations       │ Curated tactical Vyuhas          │
│                  │                  │ (e.g., Chakravyuha, Krauncha).   │
│ 8. Source        │ sources          │ Critical edition, Vulgate, and   │
│                  │                  │ regional scholarly translations. │
│ 9. Claim         │ claims           │ Propositional claims: standard,  │
│                  │                  │ approximate, and conflicting.    │
│ 10. Evidence     │ evidence         │ Shloka locators and Sanskrit/    │
│                  │                  │ English textual excerpts.        │
│ 11. Relationship │ relationships    │ Typed non-familial edges (ally,  │
│                  │                  │ teacher, rivalry, combatant).    │
│ 12. FamilyRel.   │ family_rel.      │ Generational lineage DAG (parent,│
│                  │                  │ child, spouse, sibling).         │
├──────────────────┴──────────────────┴──────────────────────────────────┤
│ Staging Association: event_participants (Character ↔ Event links)       │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Exploration-Lens Traceability Matrix

The seed dataset is explicitly designed to supply the necessary data structures to exercise the major exploration capabilities represented by the V1 API architecture ([05-api-architecture.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/05-api-architecture.md)):

| Exploration Lens | Target Endpoints | Required Seed Data Capabilities |
| :--- | :--- | :--- |
| **Character Profiles** | `/api/v1/characters/:slug` | Full biographical summaries, portraits (both present and `NULL`), aliases, and role tags. |
| **Timeline / Chronology** | `/api/v1/events` | Strict sequence indices (`sequence_index`), Parva numbers (1–18), date approximations, and relative ordering. |
| **Geographic Map** | `/api/v1/locations` | Mapped coordinates (lat/lng), unmapped locations (`coordinate_status = 'unmapped'`), and Event ↔ Location links. |
| **Dynasties & Factions** | `/api/v1/groups/:slug` | Group hierarchies, member associations, and factional allegiances. |
| **Family Lineage Tree** | `/api/v1/characters/:slug/family` | Multi-generational branching parent-child DAGs, sibling groups, and spousal relationships. |
| **Global Focus Graph** | `/api/v1/graph/focus/:type/:slug` | High-degree hub nodes (e.g., Krishna, Arjuna, Bhishma) and $D_1/D_2$ bounded traversal networks. |
| **War Explorer** | `/api/v1/wars/:slug/days/:day` | Day-by-day partitioned events, battlefield commanders, fallen heroes, and troop movements. |
| **Battlefield Vyuhas** | `/api/v1/formations/:slug` | Tactical descriptions, SVG diagram asset links, event associations, and accessibility text. |
| **Search & Autocomplete** | `/api/v1/search`, `/suggest` | Curated `alternate_names`, epithets, Sanskrit IAST diacritics, and multi-entity text indexing. |
| **Evidence & Provenance** | `/api/v1/claims/:id`, `/provenance` | Complete Entity $\rightarrow$ Claim $\rightarrow$ Evidence $\rightarrow$ Source chains, conflicting claims, and native locators. |

---

## 4. Knowledge Graph Topology & Representativeness

To validate the graph engine ([03-knowledge-graph.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/03-knowledge-graph.md)), the seed graph must exhibit distinct topological structures:

```
┌────────────────────────────────────────────────────────────────────────┐
│                      SEED GRAPH TOPOLOGY MODEL                         │
├────────────────────────────────────────────────────────────────────────┤
│ 1. DENSE HUB NODES: Key focal characters with high degree centrality   │
│    (e.g., Krishna, Arjuna, Karna, Bhishma, Duryodhana).                │
│ 2. DIRECTED RELATIONSHIPS: Asymmetric typed edges (teacher_of,         │
│    rival_of, combatant_in, allegiance_to).                             │
│ 3. SYMMETRIC RELATIONSHIPS: Stored canonical symmetric edges           │
│    (allied_with) with bidirectional query projections.                 │
│ 4. BRANCHING GENERATIONAL LINEAGE DAG: Multi-generational parent-child │
│    DAG branching across the Kuru dynasty (Shantanu's lineage branching │
│    to Vichitravirya and Chitrangada; then Dhritarashtra, Pandu, and    │
│    Vidura; branching to Pandavas and Kauravas; continuing through      │
│    Abhimanyu to Parikshit) without inaccurate biological shortcuts.    │
│ 5. MULTI-ENTITY EVENT EDGES: Events connecting multiple Characters,    │
│    a Location, a WarDay, and supporting Claims.                        │
└────────────────────────────────────────────────────────────────────────┘
```

> **Anti-Fabrication Rule**: Relationship edges must never be introduced merely to artificially balance graph density. Every edge in the seed dataset must link to an underlying factual claim.

---

## 5. Epistemic-State Coverage Strategy (Rule 03, B2 §5)

To ensure the backend handles epistemic uncertainty robustly without data corruption, the seed dataset must contain representative instances of all six epistemic certainty states defined in [02-data-architecture.md §5](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/02-data-architecture.md#L173), clearly distinguishing them from operational status fields:

| Epistemic State | Applicable Data Field / Entity Context | Representative Seed Context | Verification Intent |
| :--- | :--- | :--- | :--- |
| **`known`** | `Claim.epistemic_status` | Established canonical claims with consensus textual testimony in Critical Edition. | Verifies standard claim display and undisputed evidence rendering. |
| **`approximate`** | `Event.chronology_status`, `Claim.epistemic_status` | Relative sequential ordering or estimated durations without false calendar precision. | Verifies timeline ordering and approximation indicators. |
| **`unknown`** | `Claim.epistemic_status` | Affirmatively unknown attributes (e.g., exact unrecorded parentage or unanchored ancient sites), distinct from unresearched fields. | Verifies system displays explicit "Unknown in Tradition" badges rather than empty blanks. |
| **`conflicting`** | `Claim.epistemic_status` | Competing traditions (e.g., variant parentage or differing accounts of a warrior's death between Critical Edition and regional recensions). | Verifies that both claims are stored as separate records and rendered with conflict flags. |
| **`not_researched`** | `Character.epistemic_status` / metadata | Curated records where secondary attributes have not yet undergone full academic curation in V1. | Verifies system distinguishes unresearched data from affirmatively proven absences. |
| **`not_applicable`** | Canonical entity attribute fields | Attributes that do not apply to specific entity categories (e.g., death-related fields for immortal/Chiranjivi characters). | Verifies domain boundaries and omission of irrelevant attributes. |

> **Operational Status Distinction**: The operational field `Location.coordinate_status = 'unmapped'` is an architectural spatial status indicating an unmapped site, distinct from the propositional epistemic state `unknown`.

---

## 6. Provenance & Evidence Verification Chains (B4 Alignment)

Every factual assertion in the seed dataset must trace through the complete provenance chain established in [04-evidence-and-provenance.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/04-evidence-and-provenance.md):

```
┌────────────────────────────────────────────────────────────────────────┐
│                   FOUR REPRESENTATIVE PROVENANCE CASES                 │
├───────────────────────────────────┬────────────────────────────────────┤
│ CASE A: WELL-SUPPORTED FACT       │ CASE B: CONFLICTING TRADITIONS     │
├───────────────────────────────────┼────────────────────────────────────┤
│ Character: Arjuna                 │ Event: Karna's Final Battle        │
│ └── Claim: Wields Gandiva bow     │ ├── Claim 1 (Critical Edition)     │
│     └── Evidence: [Exact CE       │ │   └── Evidence: [Exact CE        │
│         locator verified in seed] │ │       locator verified in seed]  │
│         └── Source: BORI CE       │ └── Claim 2 (Vulgate / Regional)   │
│                                   │     └── Evidence: [Exact Vulgate   │
│                                   │         locator verified in seed]  │
├───────────────────────────────────┼────────────────────────────────────┤
│ CASE C: APPROXIMATE CHRONOLOGY    │ CASE D: AFFIRMATIVELY UNKNOWN      │
├───────────────────────────────────┼────────────────────────────────────┤
│ Event: Pandava Forest Exile       │ Character / Location Attribute     │
│ └── Claim: Duration / relative span│ └── Claim: Precise ancient anchor │
│     └── Evidence: [Exact CE       │     └── Epistemic: unknown         │
│         locator verified in seed] │         └── Source: BORI CE        │
│         └── Source: BORI CE       │                                    │
└───────────────────────────────────┴────────────────────────────────────┘
```

> **Citation Integrity Rule**: Exact native citation locators (e.g., Critical Edition Parva.Adhyaya.Shloka numbers) will be populated only after verified against authoritative primary text during seed authoring. No placeholder citation numbers are fabricated in this architectural specification.

---

## 7. Search, Alias, and Transliteration Test Cases (B6 Alignment)

The seed dataset must include rich linguistic and alias variants to thoroughly exercise the dual-tier search engine ([06-search-architecture.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/06-search-architecture.md)):

```
┌────────────────────────────────────────────────────────────────────────┐
│                     SEARCH VERIFICATION TEST CORPUS                    │
├─────────────────┬───────────────────────────┬──────────────────────────┤
│ Canonical Name  │ Curated Aliases & Epithets│ Transliteration Variants │
├─────────────────┼───────────────────────────┼──────────────────────────┤
│ **Arjuna**      │ *Pārtha*, *Dhanañjaya*,   │ *Arjuna*, *Arjun*        │
│                 │ *Savyasācī*, *Jiṣṇu*      │                          │
│ **Yudhiṣṭhira** │ *Dharmarāja*, *Ajātaśatru*│ *Yudhisthira*,           │
│                 │                           │ *Yudhishthir*,           │
│                 │                           │ *Yudhiṣṭhira*            │
│ **Karṇa**       │ *Rādheya*, *Vaikartana*,  │ *Karna*, *Radheya*,      │
│                 │ *Aṅgarāja*                │ *Karṇa*                  │
│ **Bhīṣma**      │ *Devavrata*, *Pitāmaha*   │ *Bhisma*, *Bheeshma*,    │
│                 │                           │ *Bhīṣma*                 │
│ **Kṛṣṇa**       │ *Vāsudeva*, *Govinda*,    │ *Krishna*, *Vasudeva*,   │
│                 │ *Keśava*                  │ *Kṛṣṇa*                  │
│ **Hastināpura** │ *Gajasāhvaya*, *Nāgasāhvaya*│ *Hastinapur*,          │
│                 │                           │ *Hastinapura*            │
└─────────────────┴───────────────────────────┴──────────────────────────┘
```

- **Prefix Autocomplete**: Queries like `Arj`, `Dhan`, `Hast` must return instant autocomplete candidates.
- **Diacritic Normalization (`unaccent`)**: Searching `Bhisma` matches `Bhīṣma`; searching `Karna` matches `Karṇa`.
- **Alias Resolution**: Searching `Partha` returns `Arjuna` with `match_type = 'alias'`.

---

## 8. War Explorer & Battlefield Formations Strategy

To validate the War Explorer ([02-data-architecture.md §4.7](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/02-data-architecture.md#L141)):
1. **Curated WarDays**: The seed dataset focuses on key representative days of the 18-day war:
   - **Day 1**: Commencement, Bhishma commander, initial skirmishes.
   - **Day 13**: Drona commander, Chakravyuha formation, Abhimanyu's heroic stand.
   - **Day 14**: Arjuna's vow against Jayadratha, night battle.
   - **Day 18**: Shalya commander, final duel between Bhima and Duryodhana.
2. **Formations (Vyuhas)**:
   - **Chakravyuha** (`chakravyuha`): Complete tactical description, event link (Day 13), SVG diagram URI (`/assets/vyuhas/chakravyuha.svg`), accessibility description text, and BORI source citation.
   - **Krauncha Vyuha** (`krauncha-vyuha`): Heron formation on Day 2/Day 6 with textual description and accessibility fallback.

---

## 9. Architectural Seed Dataset Size Range & Rationale

Rather than mandating arbitrary volume, the seed dataset targets an optimal **architectural verification range** designed to provide maximum structural variety while maintaining 100% human verification:

```
┌────────────────────────────────────────────────────────────────────────┐
│                 RECOMMENDED SEED DATASET SIZE RANGES                   │
├───────────────────────────────────┬────────────────────────────────────┤
│ Canonical Entity / Record Type    │ Target Seed Record Range           │
├───────────────────────────────────┼────────────────────────────────────┤
│ Characters                        │ $25 - 40\text{ records}$           │
│ Events                            │ $20 - 35\text{ records}$           │
│ Locations                         │ $12 - 20\text{ records}$           │
│ Groups / Dynasties                │ $5 - 10\text{ records}$            │
│ Wars                              │ $1 - 2\text{ records}$             │
│ WarDays                           │ $4 - 6\text{ tactical days}$       │
│ Formations (Vyuhas)               │ $2 - 4\text{ formations}$          │
│ Sources                           │ $6 - 12\text{ sources}$            │
│ Propositional Claims              │ $40 - 80\text{ claims}$            │
│ Evidence Passages                 │ $50 - 100\text{ evidence items}$   │
│ General Relationships             │ $40 - 70\text{ relationship edges}$│
│ Family Relationships              │ $30 - 50\text{ family edges}$      │
├───────────────────────────────────┴────────────────────────────────────┤
│ Staging Association: event_participants ($40 - 80\text{ associations}$)│
└────────────────────────────────────────────────────────────────────────┘
```

### 9.1. Rationale for Seed Dataset Sizing
- **Sufficient Topology**: $25 - 40$ characters allow for 4-generation deep lineage DAGs, complex alliance/rivalry networks, and high-degree hub nodes ($\text{degree} \ge 10$) for Focus Mode stress testing.
- **Rapid Execution & Verification**: Small enough to support rapid local validation and CI execution; actual performance is measured against applicable B9/B12 budgets.
- **Scholarly Provenance Requirement**: Every populated factual record must be verified against appropriate authoritative source material and, where applicable, a verified native source locator before publication.

---

## 10. Staging File Directory Structure (B10 Alignment)

In accordance with [10-data-ingestion.md §3.2](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/10-data-ingestion.md#L59), the seed dataset files reside in `data/seeds/`:

```
data/seeds/
├── characters.json              # Canonical character profiles & aliases
├── events.json                  # Chronological events with sequence_index
├── locations.json               # Mapped & unmapped geographic places
├── groups.json                  # Dynasties, clans, and kingdoms
├── wars.json                    # Major war structures
├── war_days.json                # Ordered WarDay records (Days 1, 13, 14, 18)
├── formations.json              # Battlefield Vyuhas with tactical prose
├── sources.json                 # Academic sources & critical editions
├── claims.json                  # Propositional claims with epistemic tags
├── evidence.json                # Shloka locators & textual excerpts
├── relationships.json           # Typed non-familial graph edges
├── family_relationships.json    # Generational kinship DAG edges
└── event_participants.json      # Staging association for Character ↔ Event
```

---

## 11. Test Fixtures & Quality Gate Verification Strategy

To validate the quality gates defined in [10-data-ingestion.md §4](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/10-data-ingestion.md#L77), the testing subsystem will maintain separate, isolated **Negative Test Fixture Bundles** (stored in `tests/fixtures/invalid_seeds/`):

1. **`invalid_cyclic_ancestry.json`**: Contains intentional parent-child cycles $\rightarrow$ Verifies Gate 4 halts ingestion.
2. **`invalid_broken_fk.json`**: Contains dangling `source_id` references $\rightarrow$ Verifies Gate 2 halts ingestion.
3. **`invalid_asymmetric_alliance.json`**: Contains invalid directionality on symmetric `allied_with` edge $\rightarrow$ Verifies Gate 5 halts ingestion.
4. **`invalid_empty_locator.json`**: Contains evidence without native locator text $\rightarrow$ Verifies Gate 6 halts ingestion.

---

## 12. Requirement Traceability Matrix

| Seed Dataset Requirement | Source Document | Implemented Architectural Mechanism |
| :--- | :--- | :--- |
| **Representative Dataset Scope** | Context §4.N (REQ-ING-01); PRD §20 | Section 2, Section 9: 12-entity coverage within 25–40 entity range. |
| **Complete Lens Coverage** | PRD §9.1–§9.10; B5 §5 | Section 3: Traceability mapping to all major exploration capabilities. |
| **Epistemic States Verification** | Rule 03; B2 §5 | Section 5: Intentional representation of all 6 epistemic states. |
| **Four-Tier Provenance Chain** | B4 §2; PRD §9.10 | Section 6: Entity $\rightarrow$ Claim $\rightarrow$ Evidence $\rightarrow$ Source verification chains. |
| **Search & Transliteration Testing**| PRD §9.8; B6 §4 | Section 7: IAST transliterations, epithets, and alias test cases. |
| **War Explorer & Formations** | PRD §9.7; B2 §4.7 | Section 8: Key tactical war days (1, 13, 14, 18) and Chakravyuha Vyuha. |
| **Staging & Canonical Distinction** | B10 §3.2; Rule 02 | Section 2, Section 10: Explicit staging role of `event_participants.json`. |
| **Deterministic Ingestion Testing**| Context §4.N (REQ-ING-02); B10 §4 | Section 11: Negative test fixture strategy for CI/CD test gates. |

---

## 13. Decisions Resolved & Deferred

### Decisions Resolved in Block B11:
1. **RESOLVED B11-01**: Established the **Architectural Seed Dataset Size Range** ($25 - 40$ characters, $20 - 35$ events, $12 - 20$ locations, $40 - 80$ claims).
2. **RESOLVED B11-02**: Formulated the **Exploration-Lens Traceability Model** ensuring full lens verification.
3. **RESOLVED B11-03**: Established the **Epistemic-State Distribution Plan** (testing all 6 epistemic certainty states).
4. **RESOLVED B11-04**: Defined **Representative Provenance Chains** (well-supported, approximate, conflicting, and unknown cases).
5. **RESOLVED B11-05**: Selected **Key Tactical War Days (1, 13, 14, 18)** and **Chakravyuha** for War Explorer verification.
6. **RESOLVED B11-06**: Formulated the **Negative Test Fixture Strategy** for automated quality gate verification.

### Open Decisions & Deferred Items:
- **Architectural Open Decisions**: None identified at B11.
- **Content Population Items**: Exact seed records, source selection, citation locators, and final population remain subject to source verification during seed-data authoring in Stage 4 and testing in Block B12.
- **Full Encyclopedic Production Dataset Expansion**: Deferred to post-V1 content curation roadmap.
