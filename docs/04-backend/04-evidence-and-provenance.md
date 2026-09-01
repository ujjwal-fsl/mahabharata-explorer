# Evidence & Provenance Architecture (Block B4)

## 1. Purpose

This document establishes the **Evidence & Provenance Architecture** for the **Mahābhārata Explorer** backend. It provides the architectural specification for Layer 2 of the Five-Layer Architecture, detailing the data structures, citation standards, epistemic certainty models, and referential integrity rules governing:

$$\text{Entity / Edge} \longrightarrow \text{Claim} \longrightarrow \text{Evidence} \longrightarrow \text{Source}$$

In accordance with project principles:
- **Zero Fabrication (Rule 03, REQ-EVD-01–06)**: Every factual claim, relationship, and historical attribute must be anchored to verifiable scholarly evidence or explicitly tagged with its epistemic certainty. Missing or unknown details must never be populated with invented citations.
- **Conflicting Traditions Preservation (Rule 03, REQ-EVD-05)**: Competing recensions, critical editions, and traditional variants must be represented as distinct claim records; conflicting accounts must never be overwritten, averaged, or suppressed.
- **Human vs AI Distinction (Rule 03, REQ-EVD-06)**: Textual claims derived from primary scholarly texts must be strictly distinguishable from editorial synthesis, computational derivation, or AI-assisted summaries.

---

## 2. The Four-Tier Provenance Hierarchy

The architecture strictly separates physical texts from conceptual propositions and the entities they describe:

```
┌────────────────────────────────────────────────────────────────────────┐
│                      FOUR-TIER PROVENANCE MODEL                        │
├────────────────────────────────────────────────────────────────────────┤
│  1. ENTITY / GRAPH EDGE (What is represented)                          │
│     - Canonical domain node (Character, Event, Location, Group, War)   │
│     - First-class graph edge (FamilyRelationship, Relationship, etc.)  │
├────────────────────────────────────────────────────────────────────────┤
│  2. CLAIM (The discrete propositional assertion)                       │
│     - What is being asserted about the entity or relationship          │
│     - Epistemic status & scholarly certainty level                     │
│     - Subject entity type and subject entity identifier                │
├────────────────────────────────────────────────────────────────────────┤
│  3. EVIDENCE (The specific textual citation & excerpt)                 │
│     - Native locator text preserved faithfully                         │
│     - Normalized internal coordinates (Parva, Adhyaya, Shloka/Page)    │
│     - Sanskrit transliteration & English excerpt translation           │
│     - Scholarly assessment / context annotation                        │
├────────────────────────────────────────────────────────────────────────┤
│  4. SOURCE (The authoritative bibliographic work)                      │
│     - Critical edition, traditional recension, or commentary           │
│     - Bibliographic metadata, editors, identifiers (ISBN/DOI), URL     │
└────────────────────────────────────────────────────────────────────────┘
```

### 2.1. Definitions & Boundary Discipline

1. **Entity / Edge**: The canonical domain object in Layer 1 (e.g., an individual Character node or a typed Relationship edge). Entities and edges do not store raw citation strings directly; they reference backing Claims.
2. **Claim**: A single, discrete propositional assertion regarding an entity, attribute, event, or relationship (e.g., a statement describing parentage, an event's chronological placement, or a combat outcome).
3. **Evidence**: An individual passage or citation within a Source that substantiates a Claim.
4. **Source**: The authoritative physical or scholarly work containing the Evidence (e.g., a critical edition, traditional recension, or academic commentary).
5. **Epistemic Status**: The project-wide epistemic state of knowledge (`known`, `conflicting`, `approximate`, `unknown`, `not_researched`, `not_applicable`).
6. **Scholarly Certainty**: The level of academic or traditional consensus regarding the claim (`established`, `traditional_consensus`, `disputed`, `approximate`, `unresolved`).
7. **Editorial Assessment**: Structured scholarly commentary evaluating textual variants, manuscript discrepancies, or interpolation history.

---

## 3. Detailed Data Schemas for Layer 2

### 3.1. `Source` (Authoritative Edition & Bibliography)
Represents critical editions, traditional recensions, regional retellings, and scholarly reference works.

| Field | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `UUID` / `TEXT` | `PRIMARY KEY` | Unique internal identifier. |
| `slug` | `VARCHAR(255)` | `UNIQUE NOT NULL` | Canonical routing slug (e.g., `bori-critical-edition`). |
| `title` | `VARCHAR(255)` | `NOT NULL` | Full bibliographic title. |
| `short_title` | `VARCHAR(100)` | `NOT NULL` | Standard scholarly abbreviation (e.g., `BORI CE`, `Southern Recension`). |
| `author` | `VARCHAR(255)` | `NULL` | Author, translator, or general editor. |
| `source_type` | `source_type` | `NOT NULL` | Enum: `critical_edition`, `traditional_recension`, `scholarly_analysis`, `commentary`, `regional_retelling`. |
| `tradition_context`| `VARCHAR(100)` | `NULL` | Regional or recension context (e.g., `Northern Recension`, `Southern Recension`, `Vulgate`). |
| `publication_info`| `TEXT` | `NULL` | Publisher, volume count, city, and publication year details. |
| `identifier` | `VARCHAR(100)` | `NULL` | Standard identifier (ISBN, DOI, URN, Worldcat ID). |
| `url` | `TEXT` | `NULL` | Authoritative academic digital repository or open-access URL. |
| `summary` | `TEXT` | `NULL` | Bibliographic and critical overview of this edition's methodology. |
| `status` | `entity_status` | `NOT NULL DEFAULT 'published'` | Curation lifecycle state. |
| `metadata` | `JSONB` | `NOT NULL DEFAULT '{}'` | Extensible bibliographic metadata. |
| `created_at` / `updated_at` | `TIMESTAMPTZ` | `NOT NULL` | Standard audit timestamps. |

---

### 3.2. `Claim` (Epistemic Proposition)
Represents a discrete factual assertion regarding an entity, event, or graph edge.

| Field | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `UUID` / `TEXT` | `PRIMARY KEY` | Unique internal identifier. |
| `subject_entity_type`| `claim_subject_type`| `NOT NULL` | Enum: `character`, `event`, `location`, `group`, `war`, `war_day`, `relationship`, `family_relationship`, `formation`. |
| `subject_entity_id` | `UUID` / `TEXT` | `NOT NULL` | Foreign identifier of the subject entity or graph edge. |
| `claim_text` | `TEXT` | `NOT NULL` | Clear, unambiguous statement of the factual proposition. |
| `claim_type` | `claim_type` | `NOT NULL` | Enum: `genealogy`, `event_chronology`, `location_identification`, `combat_outcome`, `relationship`, `moral_philosophical`, `theological_attribute`. |
| `certainty` | `certainty_level` | `NOT NULL DEFAULT 'established'` | Scholarly certainty: `established`, `traditional_consensus`, `disputed`, `approximate`, `unresolved`. |
| `epistemic_status` | `epistemic_state` | `NOT NULL DEFAULT 'known'` | Canonical six-state classification: `known`, `conflicting`, `approximate`, `unknown`, `not_researched`, `not_applicable`. |
| `provenance_origin`| `VARCHAR(50)` | `NOT NULL DEFAULT 'human_source'` | Origin classification (exact enum taxonomy marked as Open Decision). |
| `status` | `entity_status` | `NOT NULL DEFAULT 'published'` | Publication status. |
| `metadata` | `JSONB` | `NOT NULL DEFAULT '{}'` | Extensible claim annotations. |
| `created_at` / `updated_at` | `TIMESTAMPTZ` | `NOT NULL` | Standard audit timestamps. |

---

### 3.3. `Evidence` (Granular Textual Citation & Excerpt)
Directly anchors a `Claim` to a specific passage in a `Source`. Multiple Evidence records may link to a single Claim (multi-source corroboration), and a single Source contains multiple Evidence passages.

| Field | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `UUID` / `TEXT` | `PRIMARY KEY` | Unique internal identifier. |
| `claim_id` | `UUID` / `TEXT` | `NOT NULL REFERENCES claims(id) ON DELETE CASCADE` | Parent claim reference (Foreign Key enforced). |
| `source_id` | `UUID` / `TEXT` | `NOT NULL REFERENCES sources(id) ON DELETE RESTRICT` | Authoritative source reference (Foreign Key enforced). |
| `locator` | `VARCHAR(255)` | `NOT NULL` | Exact native citation string preserved faithfully from the source work. |
| `parva_number` | `INTEGER` | `NULL CHECK (parva_number BETWEEN 1 AND 18)` | Internal normalized Parva (1–18) for indexing and sorting. |
| `adhyaya_number` | `INTEGER` | `NULL CHECK (adhyaya_number >= 1)` | Internal normalized Adhyaya (Chapter) number. |
| `shloka_range` | `VARCHAR(50)` | `NULL` | Internal normalized Shloka / verse range (e.g., `12-15`, `5a-b`). |
| `page_reference` | `VARCHAR(50)` | `NULL` | Internal normalized volume / page reference. |
| `sanskrit_excerpt` | `TEXT` | `NULL` | Sanskrit passage (transliteration / Devanagari) as presented in the source. |
| `excerpt_translation`| `TEXT` | `NULL` | Translation of the cited passage. |
| `evidence_type` | `evidence_type` | `NOT NULL DEFAULT 'direct_mention'` | Enum: `direct_mention`, `primary_narrative`, `commentary_gloss`, `geographic_reference`, `variant_manuscript`. |
| `assessment` | `TEXT` | `NULL` | Editorial assessment (notes on textual variants, manuscript support, or commentary). |
| `metadata` | `JSONB` | `NOT NULL DEFAULT '{}'` | Extensible citation metadata. |
| `created_at` / `updated_at` | `TIMESTAMPTZ` | `NOT NULL` | Standard audit timestamps. |

---

## 4. Claim Subject Anchoring & Referential Integrity

In Block B2, the referential integrity mechanism for polymorphic Claim subjects was identified as an open decision.

### 4.1. Evaluation of Claim Anchoring Strategies

| Criterion | Option 1: Polymorphic Subject Columns (`subject_entity_type` + `subject_entity_id`) | Option 2: Concrete Foreign Key Columns (`character_id`, `event_id`, etc.) | Option 3: Reverse-Only References (Entities/Edges store `claim_id`) |
| :--- | :--- | :--- | :--- |
| **Description** | Single `claims` table storing type and ID, validated via database triggers. | `claims` table has 9 nullable foreign key columns (`character_id`, `event_id`, etc.). | Entities store `claim_id`; `claims` table does not store subject back-references. |
| **Database FK Integrity** | **High** (via database validation triggers matching B3's polymorphic relationship pattern). | **High** (standard PostgreSQL single-column FKs). | **Poor** (cannot support multi-claim entities or reverse entity lookups cleanly). |
| **Schema Cleanliness** | **High**: Single consistent schema pattern for all 9 claim subjects. | **Poor**: 9 nullable columns; high risk of multi-column invalidity. | **Medium**: Limits claims to single-field attachment. |
| **System Reuse** | **High**: Reuses the validated B3 trigger pattern. | **Poor**: Requires altering `claims` table whenever a new entity type is added. | **Poor**: Lacks bidirectional claim queries. |
| **Recommendation** | **WINNER (RECOMMENDED)** | **REJECTED (SPARSE / RIGID)** | **REJECTED (LIMITED)** |

---

### 4.2. Dual-Link Provenance Model

The architecture establishes a **Dual-Link Provenance Model**:

```
1. Subject Anchor (Claim → Entity/Edge):
   Claim.subject_entity_type + Claim.subject_entity_id ──► Validated against canonical entity table

2. Direct Edge Anchor (Edge → Claim):
   Relationship.claim_id         ──REFERENCES──► Claim.id
   FamilyRelationship.claim_id   ──REFERENCES──► Claim.id
   EventParticipant.claim_id     ──REFERENCES──► Claim.id
```

#### Database Validation Trigger Architecture for Claims:
- **`BEFORE INSERT OR UPDATE ON claims`**: Dynamic PostgreSQL trigger validates that `subject_entity_id` exists in the table designated by `subject_entity_type` (`characters`, `events`, `locations`, `groups`, `wars`, `war_days`, `formations`, `relationships`, `family_relationships`).
- **`BEFORE DELETE` on Canonical Tables**: Rejects entity or edge deletion if active `Claim` records reference it (`RESTRICT` behavior).
- **Edge FKs**: `Relationship`, `FamilyRelationship`, and `EventParticipant` enforce standard PostgreSQL foreign keys on `claim_id REFERENCES claims(id) ON DELETE RESTRICT`.

---

## 5. Citation Locators & Text Preservation Standard

### 5.1. Native Citation Fidelity vs Internal Normalized Coordinates
A central rule of the citation architecture is that **the source's original locator text must be preserved faithfully without mutation**:
- **Native Locator (`locator`)**: Stores the verbatim citation string as published in the source text (e.g., `Ādi Parva 1.216.5–12`, `Vol. 4, p. 142`, `MBh (K) 2.5.12`). This is the authoritative citation coordinate shown to users.
- **Internal Normalized Fields (`parva_number`, `adhyaya_number`, `shloka_range`, `page_reference`)**: Structured numeric columns extracted to enable fast database indexing, sorting, filtering, and sequence verification.
- **Non-Replacement Rule**: Internal normalized coordinates serve database indexing and query optimization only; they must **never** overwrite, reinterpret, or replace the source's native citation identity.

### 5.2. Canonical Parva Enumeration (1–18)
The `parva_number` column is strictly constrained by `CHECK (parva_number BETWEEN 1 AND 18)`:
1. Ādi Parva | 2. Sabhā Parva | 3. Vana / Āraṇyaka Parva | 4. Virāṭa Parva | 5. Udyoga Parva
6. Bhīṣma Parva | 7. Droṇa Parva | 8. Karṇa Parva | 9. Śalya Parva | 10. Sauptika Parva
11. Strī Parva | 12. Śānti Parva | 13. Anuśāsana Parva | 14. Āśvamedhika Parva | 15. Āśramavāsika Parva
16. Mausala Parva | 17. Mahāprasthānika Parva | 18. Svargārohaṇa Parva

*(Decisions regarding supplemental texts, such as the Harivaṃśa, are maintained as an Open Decision for future corpus expansion).*

---

## 6. Conflicting Traditions & Multi-Claim Architecture

In accordance with Rule 03 (Data Integrity) and [PRD §9.10 (SRC-005)](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/03-prd/Mahabharata_Explorer_PRD.md#L305), **conflicting traditions and variant manuscript readings must never be flattened, averaged, or overwritten**:

```
┌────────────────────────────────────────────────────────────────────────┐
│               CONFLICTING TRADITIONS SCHEMA ILLUSTRATION               │
├────────────────────────────────────────────────────────────────────────┤
│                                                                        │
│               ┌───────────────────────────────────────┐                │
│               │      Subject Entity / Attribute       │                │
│               └──────────────────┬────────────────────┘                │
│                                  │                                     │
│         ┌────────────────────────┴────────────────────────┐            │
│         │                                                 │            │
│         ▼ (Claim 1)                                       ▼ (Claim 2)  │
│  ┌──────────────────────────────┐          ┌─────────────────────────┐ │
│  │ Proposition: Tradition A     │          │ Proposition: Tradition B│ │
│  │ Epistemic: 'conflicting'     │          │ Epistemic: 'conflicting'│ │
│  └──────────────┬───────────────┘          └────────────┬────────────┘ │
│                 │                                       │              │
│                 ▼ (Evidence 1)                          ▼ (Evidence 2) │
│  ┌──────────────────────────────┐          ┌─────────────────────────┐ │
│  │ Native Locator: Citation A   │          │ Native Locator: Citat. B│ │
│  │ Excerpt: Text A              │          │ Excerpt: Text B         │ │
│  └──────────────┬───────────────┘          └────────────┬────────────┘ │
│                 │                                       │              │
│                 ▼                                       ▼              │
│  ┌──────────────────────────────┐          ┌─────────────────────────┐ │
│  │ Source: Critical Edition A   │          │ Source: Recension B     │ │
│  └──────────────────────────────┘          └─────────────────────────┘ │
└────────────────────────────────────────────────────────────────────────┘
```

### 6.1. Architectural Rules for Conflicting Accounts
1. **Distinct Claim Rows**: Competing traditional accounts or manuscript variants are stored as distinct rows in `claims`, each anchored to its respective `Evidence` and `Source`.
2. **Epistemic State**: Conflicting claims are tagged with `epistemic_status = 'conflicting'`.
3. **Multi-Source Corroboration**: When multiple sources agree on the same claim, multiple `Evidence` rows attach to that single `Claim` row.
4. **Non-Destructive Curation**: Ingestion of a variant reading never mutates existing claims.

---

## 7. Epistemic Status vs Scholarly Certainty

The data model intentionally separates two distinct dimensions of factual classification:

### 7.1. Semantic Distinction

| Attribute | Semantic Role | Vocabulary | Description |
| :--- | :--- | :--- | :--- |
| **`epistemic_status`** | **Objective State of Knowledge** in the Corpus | `known`, `conflicting`, `approximate`, `unknown`, `not_researched`, `not_applicable` | Describes whether information exists, is missing, is approximate, or is subject to textual conflict across sources. |
| **`certainty`** | **Scholarly / Academic Consensus** Level | `established`, `traditional_consensus`, `disputed`, `approximate`, `unresolved` | Describes the degree of consensus among scholars and traditional commentators regarding the interpretation of the proposition. |

### 7.2. Evidence Requirements per State
- **Source-derived factual claims** (`epistemic_status = 'known'` or `'conflicting'`): Require complete `Evidence → Source` citation chains.
- **Derived / editorial syntheses**: Must be explicitly flagged in `provenance_origin` and must not masquerade as primary textual evidence.
- **Negative / missing states** (`unknown`, `not_researched`, `not_applicable`): May legitimately lack `Evidence` rows when representing the absence of textual mentions or unresearched fields.
- *(A full validation matrix mapping allowable combinations of `certainty` and `epistemic_status` is explicitly deferred to Block B10 / implementation).*

---

## 8. Provenance Origin & Content Classification

To protect scholarly integrity (Rule 03, REQ-EVD-06), the architecture mandates that the origin of all curated content remains strictly distinguishable:

1. **Human Source-Derived Claims**: Directly extracted from published critical editions, traditional recensions, or peer-reviewed scholarship. Must link to concrete `Evidence` and `Source` records.
2. **Editorial Curation / Syntheses**: Curated summaries, narrative structuring, or thematic glosses written for user exploration. Must be marked as synthesized and cannot claim direct scriptural authority without backing evidence.
3. **Computational / AI-Assisted Outputs**: Any algorithmically generated layout suggestions, automated cross-references, or synthetic summaries must be classified distinctly so they are never conflated with primary manuscript data.
4. **Open Decision**: The final concrete enum taxonomy for `provenance_origin` (e.g., `human_source`, `editorial_synthesis`, `computational_inference`, `ai_assisted`) is marked as an **Open Decision** for Block B10.

---

## 9. Database Integrity Constraints & Orphan Prevention

To guarantee that the provenance graph remains self-validating and consistent:

1. **Foreign Key Constraints**:
   - `evidence.claim_id REFERENCES claims(id) ON DELETE CASCADE` (Deleting a claim cleanly cascades to its granular evidence citations).
   - `evidence.source_id REFERENCES sources(id) ON DELETE RESTRICT` (A source cannot be deleted if active evidence records cite it).
   - `relationships.claim_id REFERENCES claims(id) ON DELETE RESTRICT` (A claim cannot be deleted if an active graph edge depends on it).
   - `family_relationships.claim_id REFERENCES claims(id) ON DELETE RESTRICT`.
   - `event_participants.claim_id REFERENCES claims(id) ON DELETE RESTRICT`.
2. **Uniqueness Constraints**:
   - `sources.slug` is `UNIQUE`.
   - `(claim_id, source_id, locator)` is `UNIQUE` on `evidence` (prevents duplicate citations for the same passage).
3. **Check Constraints**:
   - `evidence.parva_number BETWEEN 1 AND 18` (when populated).
   - `evidence.adhyaya_number >= 1` (when populated).

---

## 10. Capability-Level Query Requirements for Provenance

The backend data architecture must support the following capability-level query access patterns (concrete API endpoints, HTTP routes, and serialization formats are explicitly deferred to Block B5):

1. **Entity Provenance Retrieval Capability**:
   - Ability to fetch all `Claim` propositions associated with a given entity, including attached `Evidence` passages and `Source` bibliographic summaries.
2. **Graph Edge Provenance Retrieval Capability**:
   - Ability to fetch the specific backing `Claim` and all supporting `Evidence` records for any given graph relationship or family edge.
3. **Source Bibliography & Citation Aggregation Capability**:
   - Ability to fetch a `Source` record with its bibliographic metadata along with all `Claim` records supported by that source across the epic.
4. **Variant Tradition Comparison Capability**:
   - Ability to retrieve conflicting claims for a single subject entity/attribute and compare variant excerpts and sources side-by-side.

---

## 11. Requirement Traceability Matrix

| Provenance Requirement | Source Document | Implemented Architectural Mechanism |
| :--- | :--- | :--- |
| **Four-Tier Provenance Model** | Blueprint §10; PRD §6.1, §9.10 (SRC-004) | Section 2: `Entity → Claim → Evidence → Source` hierarchy. |
| **Source Bibliographic Metadata** | Blueprint §11; PRD §9.10 (SRC-001) | Section 3.1: Complete `Source` schema with identifiers and URLs. |
| **Granular Locators & Excerpts** | Blueprint §11; PRD §9.10 (SRC-001) | Section 3.3, Section 5: Native locator preservation + internal normalized fields. |
| **Conflicting Traditions Preservation** | Blueprint §12; PRD §9.10 (SRC-005); Rule 03 | Section 6: Distinct claim rows per variant tradition, side-by-side representation. |
| **Human vs Derived/AI Distinction** | Detailed Ref §19; PRD §10; Rule 03 | Section 8: Mandatory origin classification and non-fabrication boundary. |
| **Edge Provenance Integration** | B3 Knowledge Graph §6; PRD §9.5 | Section 4.2: Direct `claim_id` foreign keys on `Relationship`, `FamilyRelationship`, `EventParticipant`. |
| **Orphan Prevention & Integrity** | B2 Open Decision #3; Rule 03 | Section 4.2, Section 9: PostgreSQL validation triggers and `RESTRICT` deletion rules. |
| **Multi-Source Corroboration** | Blueprint §10; PRD §9.10 | Section 3.3: 1:N `Evidence` records per `Claim`. |

---

## 12. Decisions Resolved & Deferred

### Decisions Resolved in Block B4:
1. **RESOLVED B4-01**: Established the **Dual-Link Provenance Model** (Claims reference subject entities via validated polymorphic attributes; graph edges reference Claims via direct foreign keys).
2. **RESOLVED B4-02**: Established the **Native Locator Preservation Standard** (native locator string preserved verbatim; internal Parva 1–18 / Adhyaya / Shloka fields serve indexing only).
3. **RESOLVED B4-03**: Established the **Multi-Claim Conflict Architecture** for representing competing traditions and recensions without data overwriting.
4. **RESOLVED B4-04**: Defined the semantic separation between **`epistemic_status`** (objective corpus state) and **`certainty`** (scholarly consensus level).
5. **RESOLVED B4-05**: Defined PostgreSQL cascade and restriction constraints guaranteeing zero orphaned citations.

### Decisions Deferred to Subsequent Blocks:
1. **API Endpoint Signatures, HTTP Methods & Response Payloads** → *Deferred to Block B5 (API Architecture)*.
2. **Full-Text Search Indexing over Excerpts and Citation Locators** → *Deferred to Block B6 (Search Architecture)*.
3. **Final Concrete Enum Taxonomy for `provenance_origin`** → *Deferred to Block B10 (Data Ingestion)*.
4. **Formal Epistemic-Certainty Combination Validation Matrix** → *Deferred to Block B10 (Data Ingestion)*.
5. **Supplemental Text Indexing Policies (e.g., Harivaṃśa)** → *Deferred to Future Corpus Expansion*.
6. **Citation Formatting & UI Drawer Rendering in Frontend** → *Deferred to Stage 2 (Frontend Architecture)*.
