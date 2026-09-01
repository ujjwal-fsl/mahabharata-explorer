# Data Architecture (Block B2)

## 1. Purpose

This document establishes the canonical **Data Architecture** for the **Mahābhārata Explorer** backend. It translates the project requirements from [00-backend-architecture-context.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/00-backend-architecture-context.md) and the technology foundation from [01-technology-and-infrastructure.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/01-technology-and-infrastructure.md) into concrete database entities, field definitions, epistemic data states, normalization boundaries, and relational integrity constraints.

In accordance with project principles:
- **Zero Fabrication**: Missing historical data is never populated with invented values.
- **Epistemic Honesty**: Explicit states distinguish unknown facts from unresearched or conflicting data.
- **Referential Integrity**: Constraints protect the cross-cutting `Entity → Claim → Evidence → Source` provenance hierarchy.

---

## 2. Canonical Identifiers and Stable Slugs

Every core entity record requires two distinct identifying keys:

1. **Internal Primary Key (`id`)**: A unique, immutable, system-generated identifier for internal relational joins, foreign key enforcement, and graph edge binding.
2. **Canonical Slug (`slug`)**: A human-readable, URL-safe string identifier used for canonical routing, shareable links, and deep-link resolution (e.g., `arjuna`, `battle-of-kurukshetra-day-13`, `hastinapura`).

### Identifier Invariants
- `id` values are never exposed as the primary URL interface.
- **Slug Scope**: Canonical exploration routes in the application are scoped by entity type (e.g., `/character/:slug`, `/location/:slug`, `/event/:slug`, `/war/:slug`). Therefore, `slug` uniqueness is enforced per entity table (`UNIQUE` constraint on each entity table).
- Slugs use lowercase ASCII characters and hyphens only (`[a-z0-9-]+`) and are indexed for efficient indexed lookup.

---

## 3. Common Entity Fields & Audit Attributes

All primary entity tables inherit a consistent base schema pattern:

| Field Name | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `UUID` / `TEXT` | `PRIMARY KEY` | Unique internal identifier (concrete key format marked as Open Decision). |
| `slug` | `VARCHAR(255)` | `UNIQUE NOT NULL` | URL-safe routing slug (unique per table). |
| `status` | `entity_status` | `NOT NULL DEFAULT 'published'` | Curation lifecycle state (`draft`, `in_review`, `published`, `archived`). |
| `metadata` | `JSONB` | `NOT NULL DEFAULT '{}'` | Extensible key-value store for un-indexed annotations, display flags, and lens-specific hints. |
| `created_at` | `TIMESTAMPTZ` | `NOT NULL DEFAULT NOW()` | Record creation timestamp. |
| `updated_at` | `TIMESTAMPTZ` | `NOT NULL DEFAULT NOW()` | Record last modification timestamp. |

---

## 4. The 12 Core V1 Entities: Detailed Schema Definitions

### 4.1. `Character` (Layer 1: Core Knowledge Graph)
Represents historical, mythological, and narrative individuals within the epic.

| Field | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `UUID` / `TEXT` | `PRIMARY KEY` | Unique identifier. |
| `slug` | `VARCHAR(255)` | `UNIQUE NOT NULL` | Canonical route slug (e.g., `arjuna`). |
| `name` | `VARCHAR(255)` | `NOT NULL` | Primary canonical name. |
| `alternate_names` | `TEXT[]` | `NOT NULL DEFAULT '{}'` | Array of aliases, epithets, and transliterations (e.g., `['Partha', 'Phalguna', 'Dhananjaya']`). |
| `summary` | `TEXT` | `NOT NULL` | Curated encyclopedic summary. |
| `portrait_url` | `TEXT` | `NULL` | Static image URL (nullable; no placeholder fabrication). |
| `status` | `entity_status` | `NOT NULL DEFAULT 'published'` | Publication status. |
| `metadata` | `JSONB` | `NOT NULL DEFAULT '{}'` | Extensible attributes (e.g., gender, lineage context). |
| `created_at` / `updated_at` | `TIMESTAMPTZ` | `NOT NULL` | Standard audit timestamps. |

---

### 4.2. `Event` (Layer 1: Unified Chronology & War Actions)
Represents discrete chronological occurrences across both general narrative and the 18 days of Kurukshetra.

| Field | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `UUID` / `TEXT` | `PRIMARY KEY` | Unique identifier. |
| `slug` | `VARCHAR(255)` | `UNIQUE NOT NULL` | Canonical route slug (e.g., `dice-game-assembly`). |
| `title` | `VARCHAR(255)` | `NOT NULL` | Title of the event. |
| `summary` | `TEXT` | `NOT NULL` | Curated narrative description. |
| `date_value` | `VARCHAR(100)` | `NULL` | Absolute or relative chronological label (e.g., `Day 14 (Night)`, `Year 13 of Exile`). |
| `date_precision` | `date_precision` | `NOT NULL DEFAULT 'relative'` | Epistemic precision (`exact`, `approximate`, `relative`, `unknown`). |
| `chronology_status` | `epistemic_state` | `NOT NULL DEFAULT 'known'` | Epistemic state of the event's chronological placement. |
| `sequence_index` | `NUMERIC(10,2)` | `NOT NULL` | Global ordering float/numeric for relative chronological sorting. |
| `location_id` | `UUID` / `TEXT` | `NULL REFERENCES locations(id)` | Associated geographic location (if known). |
| `war_id` | `UUID` / `TEXT` | `NULL REFERENCES wars(id)` | Associated war (if war-related). |
| `war_day_id` | `UUID` / `TEXT` | `NULL REFERENCES war_days(id)` | Associated war day (if specific to a war day; must belong to `war_id`). |
| `status` | `entity_status` | `NOT NULL DEFAULT 'published'` | Publication status. |
| `metadata` | `JSONB` | `NOT NULL DEFAULT '{}'` | Extensible event metadata. |
| `created_at` / `updated_at` | `TIMESTAMPTZ` | `NOT NULL` | Standard audit timestamps. |

---

### 4.3. `Location` (Layer 1: Geographic Space)
Represents kingdoms, cities, battlefields, hermitages, forests, and geographical landmarks.

| Field | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `UUID` / `TEXT` | `PRIMARY KEY` | Unique identifier. |
| `slug` | `VARCHAR(255)` | `UNIQUE NOT NULL` | Canonical route slug (e.g., `kurukshetra`). |
| `name` | `VARCHAR(255)` | `NOT NULL` | Primary name of the location. |
| `alternate_names` | `TEXT[]` | `NOT NULL DEFAULT '{}'` | Historical / modern alternate names and transliterations. |
| `summary` | `TEXT` | `NOT NULL` | Curated geographic description. |
| `location_type` | `location_type` | `NOT NULL` | Type enum (`kingdom`, `city`, `forest`, `river`, `battlefield`, `hermitage`, `region`, `sacred_site`). |
| `latitude` | `DOUBLE PRECISION`| `NULL` | Geographic latitude (nullable if unmapped). |
| `longitude` | `DOUBLE PRECISION`| `NULL` | Geographic longitude (nullable if unmapped). |
| `coordinate_status` | `epistemic_state` | `NOT NULL DEFAULT 'known'` | Epistemic state of geographic coordinates (`known`, `approximate`, `unknown`, `conflicting`, `not_researched`). |
| `status` | `entity_status` | `NOT NULL DEFAULT 'published'` | Publication status. |
| `metadata` | `JSONB` | `NOT NULL DEFAULT '{}'` | Extensible spatial attributes (e.g., modern state/region). |
| `created_at` / `updated_at` | `TIMESTAMPTZ` | `NOT NULL` | Standard audit timestamps. |

---

### 4.4. `Group` (Layer 1: Factions & Lineages)
Represents clans, dynasties, military alliances, assemblies, and political groupings.

| Field | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `UUID` / `TEXT` | `PRIMARY KEY` | Unique identifier. |
| `slug` | `VARCHAR(255)` | `UNIQUE NOT NULL` | Canonical route slug (e.g., `pandava-faction`). |
| `name` | `VARCHAR(255)` | `NOT NULL` | Group or clan name. |
| `alternate_names` | `TEXT[]` | `NOT NULL DEFAULT '{}'` | Alternate spellings and titles. |
| `summary` | `TEXT` | `NOT NULL` | Encyclopedic summary of the group. |
| `group_type` | `group_type` | `NOT NULL` | Type enum (`dynasty`, `clan`, `military_unit`, `alliance`, `assembly`, `order`). |
| `status` | `entity_status` | `NOT NULL DEFAULT 'published'` | Publication status. |
| `metadata` | `JSONB` | `NOT NULL DEFAULT '{}'` | Extensible metadata. |
| `created_at` / `updated_at` | `TIMESTAMPTZ` | `NOT NULL` | Standard audit timestamps. |

---

### 4.5. `Relationship` (Layer 1: General Knowledge Graph Edge)
Represents typed, directed or symmetric edges between core entities.

> **Polymorphic Reference Note**: In this logical model, `source_entity_id` and `target_entity_id` refer dynamically to records across entity tables (`characters`, `events`, `locations`, `groups`, `wars`) based on `source_entity_type` and `target_entity_type`. Consequently, standard single-table PostgreSQL foreign key constraints cannot directly enforce referential integrity on these polymorphic columns at the database level. The concrete graph representation (polymorphic table with trigger/application validation vs typed join tables) is marked as an **Open Decision for Block B3**.

| Field | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `UUID` / `TEXT` | `PRIMARY KEY` | Unique edge identifier. |
| `source_entity_type`| `entity_type` | `NOT NULL` | Source entity type (`character`, `event`, `location`, `group`, `war`). |
| `source_entity_id` | `UUID` / `TEXT` | `NOT NULL` | Identifier of source entity record (polymorphic). |
| `target_entity_type`| `entity_type` | `NOT NULL` | Target entity type (`character`, `event`, `location`, `group`, `war`). |
| `target_entity_id` | `UUID` / `TEXT` | `NOT NULL` | Identifier of target entity record (polymorphic). |
| `relationship_type` | `VARCHAR(100)` | `NOT NULL` | Relationship taxonomy code (`ally`, `rival`, `teacher_of`, `disciple_of`, `allegiance_to`, `participant_in`, `occurred_at`, `member_of`). |
| `directionality` | `directionality` | `NOT NULL DEFAULT 'directed'` | Edge symmetry (`directed`, `symmetric`). |
| `epistemic_status` | `epistemic_state` | `NOT NULL DEFAULT 'known'` | Epistemic certainty of this relationship edge. |
| `summary` | `TEXT` | `NULL` | Contextual annotation describing the relationship. |
| `claim_id` | `UUID` / `TEXT` | `NULL REFERENCES claims(id)` | Backing epistemic claim justifying this relationship. |
| `status` | `entity_status` | `NOT NULL DEFAULT 'published'` | Publication status. |
| `metadata` | `JSONB` | `NOT NULL DEFAULT '{}'` | Edge-specific metadata. |
| `created_at` / `updated_at` | `TIMESTAMPTZ` | `NOT NULL` | Standard audit timestamps. |

---

### 4.6. `FamilyRelationship` (Layer 1: Ancestral & Generational Hierarchy)
Specialized first-class character-to-character genealogical connections supporting tree generation. Because both endpoints are strictly characters, standard PostgreSQL foreign keys are fully enforced.

| Field | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `UUID` / `TEXT` | `PRIMARY KEY` | Unique identifier. |
| `source_character_id`| `UUID` / `TEXT` | `NOT NULL REFERENCES characters(id)` | Subject character (Foreign Key enforced). |
| `target_character_id`| `UUID` / `TEXT` | `NOT NULL REFERENCES characters(id)` | Relative character (Foreign Key enforced). |
| `relationship_type` | `family_relation`| `NOT NULL` | Strict kinship type (`father`, `mother`, `son`, `daughter`, `brother`, `sister`, `husband`, `wife`, `grandfather`, `grandmother`, `grandson`, `granddaughter`). |
| `directionality` | `directionality` | `NOT NULL DEFAULT 'directed'` | Directionality (`directed` for parent/child, `symmetric` for sibling/spouse). |
| `kinship_status` | `epistemic_state` | `NOT NULL DEFAULT 'known'` | Epistemic status (e.g., `conflicting` or `approximate` for disputed parentage). |
| `summary` | `TEXT` | `NULL` | Specific narrative notes (e.g., biological vs adoptive parentage). |
| `claim_id` | `UUID` / `TEXT` | `NULL REFERENCES claims(id)` | Backing epistemic claim. |
| `status` | `entity_status` | `NOT NULL DEFAULT 'published'` | Publication status. |
| `metadata` | `JSONB` | `NOT NULL DEFAULT '{}'` | Extensible kinship metadata. |
| `created_at` / `updated_at` | `TIMESTAMPTZ` | `NOT NULL` | Standard audit timestamps. |

---

### 4.7. `War` (Layer 1: Conflict Structure)
Represents major multi-day war campaigns (principally the Kurukshetra War).

| Field | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `UUID` / `TEXT` | `PRIMARY KEY` | Unique identifier. |
| `slug` | `VARCHAR(255)` | `UNIQUE NOT NULL` | Canonical route slug (e.g., `kurukshetra-war`). |
| `name` | `VARCHAR(255)` | `NOT NULL` | War name. |
| `summary` | `TEXT` | `NOT NULL` | Overview of the war. |
| `start_date_value` | `VARCHAR(100)` | `NULL` | Narrative or approximate start date. |
| `start_date_precision`| `date_precision`| `NOT NULL DEFAULT 'relative'` | Start date precision. |
| `end_date_value` | `VARCHAR(100)` | `NULL` | Narrative or approximate end date. |
| `end_date_precision` | `date_precision`| `NOT NULL DEFAULT 'relative'` | End date precision. |
| `status` | `entity_status` | `NOT NULL DEFAULT 'published'` | Publication status. |
| `metadata` | `JSONB` | `NOT NULL DEFAULT '{}'` | Extensible war metadata (e.g., total casualties summary). |
| `created_at` / `updated_at` | `TIMESTAMPTZ` | `NOT NULL` | Standard audit timestamps. |

---

### 4.8. `WarDay` (Layer 1: Daily War Subdivision)
Represents discrete days (1–18) of the Kurukshetra War.

| Field | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `UUID` / `TEXT` | `PRIMARY KEY` | Unique identifier. |
| `war_id` | `UUID` / `TEXT` | `NOT NULL REFERENCES wars(id)` | Parent war reference (Foreign Key enforced). |
| `day_number` | `INTEGER` | `NOT NULL CHECK (day_number BETWEEN 1 AND 18)` | Sequence day number (1 to 18 for V1 Kurukshetra War). |
| `title` | `VARCHAR(255)` | `NOT NULL` | Descriptive title (e.g., `Day 13: The Chakravyuha`). |
| `summary` | `TEXT` | `NOT NULL` | Daily narrative summary. |
| `date_value` | `VARCHAR(100)` | `NULL` | Specific day label. |
| `date_precision` | `date_precision` | `NOT NULL DEFAULT 'relative'` | Date precision. |
| `status` | `entity_status` | `NOT NULL DEFAULT 'published'` | Publication status. |
| `metadata` | `JSONB` | `NOT NULL DEFAULT '{}'` | Daily commanders, key fallen warriors, faction state. |
| `created_at` / `updated_at` | `TIMESTAMPTZ` | `NOT NULL` | Standard audit timestamps. |

---

### 4.9. `Formation` (Layer 1: Military Vyuha)
Represents strategic battlefield formations (e.g., Chakravyuha, Krauncha Vyuha).

| Field | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `UUID` / `TEXT` | `PRIMARY KEY` | Unique identifier. |
| `slug` | `VARCHAR(255)` | `UNIQUE NOT NULL` | Canonical slug (e.g., `chakravyuha`). |
| `name` | `VARCHAR(255)` | `NOT NULL` | Formation name. |
| `summary` | `TEXT` | `NOT NULL` | Curated overview of the formation structure. |
| `description` | `TEXT` | `NOT NULL` | Detailed textual description (ensures accessibility without visual rendering). |
| `event_id` | `UUID` / `TEXT` | `NULL REFERENCES events(id)` | Associated battlefield event (Foreign Key enforced). |
| `visualization_status`| `formation_status`| `NOT NULL DEFAULT 'described_only'` | Status (`visualized`, `described_only`, `unsupported`). |
| `claim_id` | `UUID` / `TEXT` | `NULL REFERENCES claims(id)` | Backing scholarly claim. |
| `status` | `entity_status` | `NOT NULL DEFAULT 'published'` | Publication status. |
| `metadata` | `JSONB` | `NOT NULL DEFAULT '{}'` | Geometric coordinate payload (if visualized). |
| `created_at` / `updated_at` | `TIMESTAMPTZ` | `NOT NULL` | Standard audit timestamps. |

---

### 4.10. `Source` (Layer 2: Scholarly Text & Edition)
Represents authoritative primary texts, critical editions, and scholarly reference works.

| Field | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `UUID` / `TEXT` | `PRIMARY KEY` | Unique identifier. |
| `slug` | `VARCHAR(255)` | `UNIQUE NOT NULL` | Canonical slug (e.g., `bori-critical-edition`). |
| `title` | `VARCHAR(255)` | `NOT NULL` | Bibliographic title. |
| `author` | `VARCHAR(255)` | `NULL` | Author, translator, or editorial committee (e.g., `V.S. Sukthankar et al.`). |
| `source_type` | `source_type` | `NOT NULL` | Classification (`critical_edition`, `traditional_recension`, `scholarly_analysis`, `commentary`, `regional_retelling`). |
| `publication_info`| `TEXT` | `NULL` | Publisher, year, volume details. |
| `identifier` | `VARCHAR(100)` | `NULL` | Scholarly standard code (ISBN, DOI, BORI identifier). |
| `url` | `TEXT` | `NULL` | Public digital repository or academic URL. |
| `summary` | `TEXT` | `NULL` | Bibliographic annotation. |
| `status` | `entity_status` | `NOT NULL DEFAULT 'published'` | Publication status. |
| `metadata` | `JSONB` | `NOT NULL DEFAULT '{}'` | Bibliographic metadata. |
| `created_at` / `updated_at` | `TIMESTAMPTZ` | `NOT NULL` | Standard audit timestamps. |

---

### 4.11. `Claim` (Layer 2: Epistemic Proposition)
Represents discrete factual assertions made regarding entities, relationships, or events.

> **Polymorphic Subject Note**: `subject_entity_id` dynamically references records across different entity tables (`characters`, `events`, `locations`, `groups`, `wars`, `relationships`, `family_relationships`, `formations`) based on `subject_entity_type`. Standard PostgreSQL foreign key constraints cannot enforce referential integrity across multiple tables on this column. The concrete evidence and claim anchoring architecture is marked as an **Open Decision for Block B4**.

| Field | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `UUID` / `TEXT` | `PRIMARY KEY` | Unique identifier. |
| `subject_entity_type`| `claim_subject_type`| `NOT NULL` | Subject entity type (`character`, `event`, `location`, `group`, `war`, `relationship`, `family_relationship`, `formation`). |
| `subject_entity_id` | `UUID` / `TEXT` | `NOT NULL` | Subject entity ID (polymorphic). |
| `claim_text` | `TEXT` | `NOT NULL` | Clear, unambiguous statement of the claim. |
| `claim_type` | `claim_type` | `NOT NULL` | Type (`genealogy`, `event_chronology`, `location_identification`, `combat_outcome`, `relationship`, `moral_philosophical`). |
| `certainty` | `certainty_level` | `NOT NULL DEFAULT 'established'` | Epistemic certainty (`established`, `traditional_consensus`, `disputed`, `approximate`, `unresolved`). |
| `epistemic_status` | `epistemic_state` | `NOT NULL DEFAULT 'known'` | Epistemic state classification. |
| `status` | `entity_status` | `NOT NULL DEFAULT 'published'` | Publication status. |
| `metadata` | `JSONB` | `NOT NULL DEFAULT '{}'` | Claim annotations. |
| `created_at` / `updated_at` | `TIMESTAMPTZ` | `NOT NULL` | Standard audit timestamps. |

---

### 4.12. `Evidence` (Layer 2: Granular Citation & Excerpt)
Directly anchors a `Claim` to a specific textual passage in a `Source`.

| Field | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `UUID` / `TEXT` | `PRIMARY KEY` | Unique identifier. |
| `claim_id` | `UUID` / `TEXT` | `NOT NULL REFERENCES claims(id) ON DELETE CASCADE` | Parent claim reference (Foreign Key enforced). |
| `source_id` | `UUID` / `TEXT` | `NOT NULL REFERENCES sources(id)` | Authoritative source reference (Foreign Key enforced). |
| `locator` | `VARCHAR(255)` | `NOT NULL` | Precise citation coordinate (e.g., `Sabhā Parva 2.5.12` or `Vol. 4, p. 112`). |
| `excerpt` | `TEXT` | `NULL` | Direct textual translation or Sanskrit verse excerpt. |
| `evidence_type` | `evidence_type` | `NOT NULL DEFAULT 'direct_mention'` | Evidence nature (`direct_mention`, `primary_narrative`, `commentary_gloss`, `geographic_reference`, `variant_manuscript`). |
| `assessment` | `TEXT` | `NULL` | Scholarly assessment or contextual commentary. |
| `metadata` | `JSONB` | `NOT NULL DEFAULT '{}'` | Extensible citation metadata. |
| `created_at` / `updated_at` | `TIMESTAMPTZ` | `NOT NULL` | Standard audit timestamps. |

---

## 5. Epistemic Data States & Zero Fabrication Model

The schema implements a structured epistemic model to guarantee historical truthfulness and eliminate false certainty.

### 5.1. The 6 Epistemic States

| State | Semantic Definition | Application in Schema |
| :--- | :--- | :--- |
| **`known`** | Supported by unambiguous primary text or scholarly consensus. | Default state for corroborated entity attributes and standard edges. |
| **`unknown`** | Explicitly declared unknown or lost in traditional sources. | Field value is `NULL`, backed by an explicit Claim stating text is silent. |
| **`not_researched`** | Curation is incomplete for this specific field. | Field value is `NULL`; UI displays empty state without asserting absence. |
| **`not_applicable`** | Attribute does not apply to this entity category. | Explicitly flags irrelevant structural attributes. |
| **`conflicting`** | Multiple credible traditions or manuscript recensions disagree. | Supported via multiple linked Claims rather than flattening or averaging. |
| **`approximate`** | Estimated or relative value without false precision. | Applied to relative event chronologies or broad geographic areas. |

> **Epistemic Vocabulary Rule**: The six-state vocabulary is project-wide; individual epistemic fields may support only the subset of states semantically applicable to that field. For example, a coordinate status supports `known`, `approximate`, `unknown`, `conflicting`, and `not_researched`, whereas `not_applicable` is only used when a geographic attribute is structurally irrelevant.

### 5.2. Where Epistemic States Apply in V1
Rather than polluting every database column with redundant state tracking, the V1 data model targets epistemic state tracking to fields with historical uncertainty:
1. **Geographic Coordinates (`locations.coordinate_status`)**: Identifies whether coordinates are exact, approximate, conflicting, or unknown.
2. **Event Chronology (`events.chronology_status`, `events.date_precision`)**: Distinguishes relative narrative sequences from disputed or unknown chronological placements.
3. **Kinship & Parentage (`family_relationships.kinship_status`)**: Distinguishes clear genealogies from disputed, divine, or adoptive parentage.
4. **General Graph Edges (`relationships.epistemic_status`)**: Indicates whether a relationship is settled consensus or disputed.
5. **Claims (`claims.certainty`, `claims.epistemic_status`)**: Primary vehicle for detailed scholarly dispute modeling and variant traditions.

### 5.3. Missing vs Unknown vs Not Researched Values
- **Missing / Unresearched (`NULL` + `not_researched`)**: Editorial curation has not yet populated this field. The frontend must display no claim rather than implying a negative fact.
- **Affirmatively Unknown (`NULL` + `unknown` + Claim)**: The primary texts explicitly note the information is missing or unknowable.
- **Conflicting (`conflicting` + multiple Claims)**: Competing claims are anchored to distinct `Source` records.

---

## 6. Alias & Multilingual Transliteration Representation

Entity names across Sanskrit literature appear in multiple transliterations and epithets (e.g., *Arjuna*, *Pārtha*, *Phalguna*, *Jishnu*, *Savyasācī*).

### 6.1. Deliberate Scope of `alternate_names`
Based strictly on the authoritative [Master Blueprint §11](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/01-project-blueprint/Mahabharata_Explorer_Master_Consolidated_Blueprint.md#L145) and [PRD §9.8 (SRCH-001)](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/03-prd/Mahabharata_Explorer_PRD.md#L274), `alternate_names TEXT[]` is explicitly defined only for entities with extensive scholarly alias/epithet systems:
- **`Character`**: Epithets, patronymics, and transliterations (e.g., *Dhananjaya*, *Kaunteya*).
- **`Location`**: Historical vs modern region names (e.g., *Indraprastha*, *Khandavaprastha*).
- **`Group`**: Clan names and lineage variants (e.g., *Bharatas*, *Kurus*, *Kauravas*).

For `Event`, `War`, `Formation`, and `Source`, titles/names are distinct narrative headings, and global search operates directly over their canonical `title`/`name` and `summary` fields without requiring a separate alias array.

### 6.2. Data Storage vs Search Architecture
- The data model defines `alternate_names TEXT[]` as the canonical storage field.
- All decisions regarding specific index types (e.g., GIN), operator classes (`gin_trgm_ops`), and similarity thresholds belong to **Block B6 (Search Architecture)**.

---

## 7. Metadata Strategy (`JSONB`)

Every core entity table includes a `metadata JSONB NOT NULL DEFAULT '{}'` column.

### Governed Rules for `metadata`:
1. **Never use `metadata` for core relational entities or foreign keys**: All primary relationships, claims, events, and locations must use typed relational columns.
2. **Promotion Rule**: If any metadata attribute becomes **searchable, filterable, sortable, evidence-bearing, or graph-significant**, it **MUST** be promoted to a typed relational/schema column rather than remaining hidden in JSONB.
3. **Permitted uses of `metadata`**:
   - Visual presentation hints (e.g., formation vertex coordinates, UI color accents).
   - Un-indexed lens-specific UI flags.
   - Minor historical annotations that do not participate in relational graph queries or filtering.

---

## 8. Database-Level Integrity Constraints

To guarantee that the database remains self-validating, PostgreSQL schema constraints are enforced where direct relational foreign keys exist:

1. **Foreign Key Integrity**:
   - `events.location_id REFERENCES locations(id) ON DELETE SET NULL`
   - `events.war_id REFERENCES wars(id) ON DELETE SET NULL`
   - `events.war_day_id REFERENCES war_days(id) ON DELETE SET NULL`
   - `family_relationships.source_character_id REFERENCES characters(id) ON DELETE RESTRICT`
   - `family_relationships.target_character_id REFERENCES characters(id) ON DELETE RESTRICT`
   - `evidence.claim_id REFERENCES claims(id) ON DELETE CASCADE`
   - `evidence.source_id REFERENCES sources(id) ON DELETE RESTRICT`
2. **Event War / War-Day Consistency Invariant**:
   - Whenever an `Event` record contains both `war_id` and `war_day_id`, `war_day_id` **must** belong to that specified `war_id` (`war_days.war_id = events.war_id`). The concrete database-level enforcement (composite foreign key `(war_id, war_day_id) REFERENCES war_days(war_id, id)` vs validation trigger) is marked as an **Open Decision**.
3. **Uniqueness Constraints**:
   - `slug` is unique per entity table (`characters`, `events`, `locations`, `groups`, `wars`, `formations`, `sources`).
   - `(war_id, day_number)` is unique on `war_days`.
   - `(source_character_id, target_character_id, relationship_type)` is unique on `family_relationships`.
4. **Check Constraints**:
   - `war_days.day_number BETWEEN 1 AND 18` (for V1 Kurukshetra War)
   - `locations`: `latitude BETWEEN -90.0 AND 90.0` (when not null)
   - `locations`: `longitude BETWEEN -180.0 AND 180.0` (when not null)

---

## 9. Data-Access Layer & Schema Evolution Boundaries

- **Type-Safe Data-Access**: Aligned with Block B1, database operations will utilize a type-safe SQL layer. The specific library and DDL migration tooling are deferred to implementation.
- **Schema Evolution**: Core schemas defined in this document are forward-compatible. Adding auxiliary annotations will utilize `metadata JSONB` to minimize migration overhead during curation.

---

## 10. Open Decisions for Future Blocks

The following items are deliberately maintained as **Open Decisions** to be resolved in their dedicated Stage 1 blocks:

1. **Internal Primary Key Format (UUID v4 vs CUID2 vs Text ID)**: The concrete primary key format is deferred to technical implementation setup (*Stage 4 / Block B12*); the schema currently requires an immutable, unique identifier.
2. **Polymorphic Relationship Referential Integrity Enforcement**: Block B3 (Knowledge Graph) will determine whether polymorphic edge references (`source_entity_type` + `source_entity_id`) are maintained with application/trigger validation or mapped to concrete typed join tables.
3. **Polymorphic Claim Subject Integrity Enforcement**: Block B4 (Evidence & Provenance) will determine the exact foreign-key anchoring mechanism for claims across entities and relationships.
4. **Event War / War-Day Composite FK Enforcement**: Concrete SQL enforcement mechanism for `events(war_id, war_day_id)` consistency (composite foreign key vs trigger) will be resolved during database implementation.
5. **Canonical Citation Locator Grammar**: The exact parsing grammar for canonical locator strings (e.g., `Parva.Adhyaya.Shloka`) will be defined in **Block B4 (Evidence & Provenance)**.
6. **Search Indexing Strategy & Operator Classes**: Specific search indexes (e.g., GIN trigram indexes) and matching thresholds on `alternate_names` and `name` will be formalized in **Block B6 (Search Architecture)**.
