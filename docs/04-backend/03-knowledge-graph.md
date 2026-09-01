# Knowledge Graph Architecture (Block B3)

## 1. Purpose

This document establishes the **Knowledge Graph Architecture** for the **Mahābhārata Explorer** backend. It specifies how the canonical data model defined in [02-data-architecture.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/02-data-architecture.md) functions as a single, unified, interconnected knowledge graph that powers all exploration lenses (Timeline, Geographic Map, Character Profiles, Relationships, War Explorer, and Global Focus).

In accordance with project principles:
- **One Knowledge Graph, Many Lenses (Rule 01, REQ-CORE-01)**: All exploration views query a single interconnected graph rather than maintaining siloed or duplicate datasets.
- **Zero Fabrication (Rule 03, REQ-GRP-05)**: Graph edges must never be inferred from text co-occurrence, narrative proximity, or layout algorithms. Every edge must be explicitly recorded and backed by evidence.
- **System Reuse (Rule 02, REQ-CORE-02)**: The graph traversal engine is built on standard PostgreSQL relational structures and recursive queries without requiring a separate graph database cluster for V1.

---

## 2. Graph Model: Nodes, Edges, and Roles

In the V1 architecture, graph elements map cleanly to the established 12-entity data model:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        KNOWLEDGE GRAPH TOPOLOGY                        │
├────────────────────────────────────────────────────────────────────────┤
│  LAYER 1: GRAPH NODES (Canonical Entities)                             │
│  - Character                                                           │
│  - Event                                                               │
│  - Location                                                            │
│  - Group                                                               │
│  - War                                                                 │
│  - WarDay                                                              │
│  - Formation                                                           │
├────────────────────────────────────────────────────────────────────────┤
│  LAYER 1: GRAPH EDGES (First-Class Relations & Associations)           │
│  - FamilyRelationship (Character ↔ Character genealogical kinship)     │
│  - Relationship (Generic typed multi-entity connections)               │
│  - EventParticipant (Character ↔ Event participation association)      │
│  - Structural Foreign Keys (Event → Location, Event → WarDay → War)    │
├────────────────────────────────────────────────────────────────────────┤
│  LAYER 2: PROVENANCE ANCHORS                                           │
│  - Claim (Epistemic assertions justifying nodes and edges)             │
│  - Evidence (Granular text locators & excerpts)                        │
│  - Source (Authoritative critical editions and recensions)             │
└────────────────────────────────────────────────────────────────────────┘
```

### 2.1. Graph Nodes
Every primary domain entity record in Layer 1 is a first-class graph node:
- **`Character`**: Individuals participating in narrative, lineage, alliance, and combat.
- **`Event`**: Chronological occurrences anchoring time, space, and action.
- **`Location`**: Geographic spaces anchoring events, realms, and battlegrounds.
- **`Group`**: Dynasties, clans, armies, and assemblies.
- **`War`**: Multi-day military campaigns.
- **`WarDay`**: Ordered daily subdivisions (1–18) of a war campaign.
- **`Formation`**: Tactical battlefield structures deployed during specific events.

### 2.2. Graph Edges
Edges represent typed, directed or symmetric relationships between nodes. Every edge carries:
- `source_node`: Source entity ID.
- `target_node`: Target entity ID.
- `relationship_type`: Controlled taxonomy identifier.
- `directionality`: `directed` (asymmetric) or `symmetric` (mutual).
- `epistemic_status`: Canonical epistemic classification (`known`, `conflicting`, `approximate`, `unknown`, `not_researched`, `not_applicable`).
- `claim_id`: Foreign key link to the backing scholarly Claim.

> **Entity Classification Note**: `EventParticipant` is strictly an **edge association record** linking `Character` to `Event`, NOT a 13th canonical entity node.

---

## 3. Relationship Architecture & Tradeoff Evaluation

In Block B2, the referential integrity mechanism for polymorphic relationships was identified as an open decision. Here we evaluate the three fundamental options:

### 3.1. Evaluation of Relationship Storage Strategies

| Criterion | Option 1: Pure Polymorphic Table | Option 2: Concrete Join Tables for Every Pair | Option 3: Hybrid Relational Graph Model (Recommended) |
| :--- | :--- | :--- | :--- |
| **Description** | Single `relationships` table storing `source_type + source_id` and `target_type + target_id`. | Separate tables for every combination (`character_allies`, `character_groups`, etc.). | Direct FKs for high-cardinality structural links + dedicated `FamilyRelationship` + `EventParticipant` association + controlled `Relationship` for remaining multi-entity edges. |
| **Referential Integrity** | **Poor**: Lacks native single-table foreign keys; high risk of orphaned records without rigorous trigger enforcement. | **High**: Native database foreign keys on all join tables. | **High**: Native foreign keys on all primary structural links, family trees, and event participants; database-level validation triggers on general relationships. |
| **Query Complexity** | **High**: Requires complex polymorphic `CASE` branches or multi-table UNION joins. | **High**: Graph traversal requires querying dozens of discrete join tables. | **Low**: High-frequency queries (family trees, event participants, war structures) hit dedicated indexed tables; Focus queries hit indexed edge tables. |
| **System Reuse** | **High**: Single table for all edges. | **Poor**: Fragmented across many ad-hoc join tables. | **High**: Unified query patterns across lenses with zero duplicate entity engines. |
| **V1 Scope & Maintainability** | **Medium**: Edge deletion risks orphan edges. | **Poor**: Schema explosion with high maintenance burden. | **Excellent**: Clean balance between data integrity, performance, and simplicity. |

---

### 3.2. Recommended Strategy: The Hybrid Relational Graph Architecture

To ensure strict referential integrity while avoiding schema fragmentation, the Knowledge Graph is organized into **four specialized, complementary edge layers**:

```
                                  ┌───────────────┐
                                  │   Character   │
                                  └───────┬───────┘
                                          │
                  ┌───────────────────────┼───────────────────────┐
                  │ (FamilyRelationship)  │ (EventParticipant)    │ (Relationship)
                  ▼                       ▼                       ▼
          ┌───────────────┐       ┌───────────────┐       ┌───────────────┐
          │   Character   │       │     Event     │       │     Group     │
          └───────────────┘       └───────┬───────┘       └───────────────┘
                                          │ (Structural FK)
                                          ▼
                                  ┌───────────────┐
                                  │   Location    │
                                  └───────────────┘
```

1. **Structural Foreign Key Associations**:
   - `events.location_id REFERENCES locations(id)` (Event ↔ Location)
   - `events.war_id REFERENCES wars(id)` (Event ↔ War)
   - `events.war_day_id REFERENCES war_days(id)` (Event ↔ WarDay)
   - `war_days.war_id REFERENCES wars(id)` (WarDay ↔ War)
   - `formations.event_id REFERENCES events(id)` (Formation ↔ Event)
   *Enforces 100% database-level integrity for core spatial and chronological containment.*

2. **Genealogical Edge Model (`FamilyRelationship`)**:
   - Dedicated table strictly linking `Character ↔ Character`.
   - `source_character_id REFERENCES characters(id)`
   - `target_character_id REFERENCES characters(id)`
   *Exclusively owns all kinship semantics (parents, children, siblings, spouses) with strict native foreign keys and recursive CTE tree traversal.*

3. **Event Participation Edge Model (`EventParticipant`)**:
   - Dedicated association table linking `Character ↔ Event`.
   - `character_id REFERENCES characters(id)`
   - `event_id REFERENCES events(id)`
   - `role`: (`commander`, `warrior`, `speaker`, `listener`, `mediator`, `victim`, `witness`).
   - `claim_id REFERENCES claims(id)`
   *Enforces strict database foreign keys and powers character timeline presences and battlefield rosters.*

4. **General Inter-Entity Edge Model (`Relationship`)**:
   - Handles all non-genealogical inter-entity connections:
     - `Character ↔ Character` non-familial (e.g., `guru_of`, `allied_with`, `rival_of`, `slayer_of`).
     - `Character ↔ Group` (e.g., `leader_of`, `member_of`, `allegiance_to`).
     - `Group ↔ Group` (e.g., `allied_with`, `rival_of`, `allegiance_to`).
     - `Location ↔ Location` (e.g., `capital_of`, `located_within`).

---

## 4. Polymorphic Referential Integrity Architecture

Because the generic `Relationship` table stores polymorphic endpoints (`source_entity_type` + `source_entity_id` and `target_entity_type` + `target_entity_id`), database-level integrity cannot rely on standard single-table foreign keys. To guarantee absolute referential integrity at the database layer, the following mechanisms are mandated:

### 4.1. Endpoint Existence Validation (INSERT / UPDATE)
- A PostgreSQL database-level validation trigger (`BEFORE INSERT OR UPDATE ON relationships`) executes dynamic existence checks:
  - Validates that `source_entity_id` exists in the table corresponding to `source_entity_type` (`characters`, `events`, `locations`, `groups`, `wars`, `war_days`).
  - Validates that `target_entity_id` exists in the table corresponding to `target_entity_type`.
  - Rejects the transaction with an explicit constraint error if either endpoint is missing or of the wrong type.

### 4.2. Source/Target Compatibility Matrix Enforcement
The validation trigger strictly enforces the permitted endpoint combinations:

| Relationship Type | Directionality | Permitted Source Entity | Permitted Target Entity | Semantic Definition & Invariant |
| :--- | :--- | :--- | :--- | :--- |
| `teacher_of` | `directed` | `Character` | `Character` | Pedagogical/spiritual mentorship (guru $\rightarrow$ shishya). |
| `leader_of` | `directed` | `Character` | `Group` | Executive leadership or supreme command of a clan, army, or faction. |
| `member_of` | `directed` | `Character` | `Group` | Formal belonging or recruitment into a dynasty, order, or faction. |
| `allegiance_to` | `directed` | `Character` | `Character` | Personal feudatory or chivalric oath of loyalty to another individual. |
| `allegiance_to` | `directed` | `Character` | `Group` | Oath of allegiance sworn by an individual to a royal dynasty, realm, or faction. |
| `allegiance_to` | `directed` | `Group` | `Group` | Vassalage or subordinate treaty obligation between two distinct groups/lineages. |
| `allied_with` | `symmetric` | `Character` | `Character` | Mutual military or political alliance between individuals. |
| `allied_with` | `Group` | `Group` | Mutual coalition or confederacy between distinct factions. |
| `rival_of` | `symmetric` | `Character` | `Character` | Mutual personal enmity or factional feud between individuals. |
| `rival_of` | `symmetric` | `Group` | `Group` | Hostility, conflict, or state of war between factions. |
| `slayer_of` | `directed` | `Character` | `Character` | Fatal combat outcome (victor $\rightarrow$ deceased). |
| `capital_of` | `directed` | `Location` | `Location`, `Group` | Administrative or dynastic seat of a realm, kingdom, or clan. |
| `located_within` | `directed` | `Location` | `Location`, `Group` | Geographic containment within a broader region or sovereign realm. |

### 4.3. Orphan Prevention on Entity Deletion
- PostgreSQL triggers on parent entity tables (`BEFORE DELETE ON characters, events, locations, groups, wars, war_days`) check for referencing records in `relationships`:
  - If referencing edges exist, deletion is rejected with a foreign-key-equivalent restriction (`RESTRICT` behavior).

### 4.4. Concurrency & Transactional Isolation
- All ingestion and curation mutations execute within `READ COMMITTED` or `SERIALIZABLE` database transactions. Row-level locks (`SELECT 1 FROM target_table WHERE id = :id FOR SHARE`) during edge insertion ensure that referenced entities cannot be concurrently deleted during edge creation.
- Pre-ingestion validation in TypeScript (Block B10) acts as an early defense-in-depth gate, but the database triggers are the authoritative integrity enforcer.

---

## 5. Controlled Relationship Taxonomy, Directionality & Conflicting Claims

### 5.1. Directionality & Canonicalization Semantics
Every relationship edge is explicitly typed as **directed** or **symmetric**:

- **Directed Edges (`directionality = 'directed'`)**:
  - The semantic meaning is strictly asymmetric ($A \rightarrow B \neq B \rightarrow A$).
  - **Canonical Direction Convention**: Redundant inverse types are eliminated from storage. Only the primary canonical direction is stored, and the inverse is derived dynamically during query traversal:
    - Store `teacher_of` $\rightarrow$ derive `disciple_of` via inbound query.
    - Store `slayer_of` $\rightarrow$ derive `slain_by` via inbound query.
    - Store `leader_of` $\rightarrow$ derive `led_by` via inbound query.
    - Store `allegiance_to` $\rightarrow$ derive `allegiance_from` via inbound query.
    - Store `capital_of` $\rightarrow$ derive `has_capital` via inbound query.
- **Symmetric / Mutual Edges (`directionality = 'symmetric'`)**:
  - The relationship applies equally in both directions ($A \leftrightarrow B \equiv B \leftrightarrow A$).
  - Canonical examples: `allied_with`, `rival_of`.
  - **Canonical Storage Invariant**: To prevent duplicate inverse records ($A \leftrightarrow B$ and $B \leftrightarrow A$), symmetric edges are canonically stored such that the composite tuple `(source_entity_type, source_entity_id)` is lexicographically less than `(target_entity_type, target_entity_id)`. Traversal queries evaluate forward and reverse lookups uniformly.

### 5.2. Representation of Conflicting Traditional Accounts

In accordance with Rule 03 (Data Integrity) and [PRD §9.10 (SRC-005)](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/03-prd/Mahabharata_Explorer_PRD.md#L305):
- Every graph edge record contains exactly **one** `claim_id` foreign key.
- When competing scholarly recensions or regional traditions assert **conflicting variants** of the same relationship (e.g., conflicting accounts of who slew a warrior, or whether an alliance existed):
  - The backend stores **distinct, separate edge records**, each carrying its own specific `claim_id`.
  - Both edges carry `epistemic_status = 'conflicting'`.
  - Each `claim_id` anchors to its respective `Claim` record, which cites the differing primary text in its linked `Evidence` and `Source`.
  - The uniqueness constraint enforces: `UNIQUE(source_entity_type, source_entity_id, target_entity_type, target_entity_id, relationship_type, claim_id)`.

---

## 6. Provenance & Epistemic Status Policy

The Knowledge Graph enforces a strict provenance policy distinguishing edge records from structural associations:

### 6.1. Provenance on Graph Edge Records
- All first-class graph edge records (`Relationship`, `FamilyRelationship`, `EventParticipant`) **MUST** carry a `claim_id` foreign key referencing a valid `Claim` record.
- Every edge record carries an `epistemic_status` classified using the project-wide six-state vocabulary:
  - `known`: Settled traditional and scholarly consensus.
  - `conflicting`: Competing traditions assert different relationships (anchored to distinct edge rows with individual Claims/Sources).
  - `approximate`: Relationship or role is estimated or narrative context is vague.
  - `unknown` / `not_researched` / `not_applicable`: Handled per field-level semantics defined in B2.

### 6.2. Provenance on Structural Foreign Key Associations
- Structural foreign keys (`Event.location_id`, `Event.war_id`, `Event.war_day_id`, `WarDay.war_id`, `Formation.event_id`) represent structural data containment rather than distinct edge rows.
- Provenance for structural associations is maintained by anchoring a `Claim` to the entity record (e.g., a `Claim` with `subject_entity_type = 'event'`, `subject_entity_id = :event_id`, and `claim_type = 'location_identification'`, which links to `Evidence` and `Source`).

---

## 7. Graph Integrity & Anti-Fabrication Safeguards

To strictly uphold Rule 03 (Data Integrity) and Rule 04 (Scope Discipline), the backend enforces strict graph invariants:

1. **Anti-Duplicate Edge Invariant**:
   - Unique composite constraint on `(source_entity_type, source_entity_id, target_entity_type, target_entity_id, relationship_type, claim_id)`.
2. **Anti-Self-Reference Invariant**:
   - Check constraint prohibiting `source_entity_id = target_entity_id` when `source_entity_type = target_entity_type`.
3. **Strict Prohibition of Inferred Edges (Rule 03)**:
   - Graph edges must **never** be synthesized by automated co-occurrence analysis, narrative proximity, or layout coordinates.
   - If Character $A$ and Character $B$ both participate in Event $E$, they are **not** automatically linked as allies or rivals unless an explicit scholarly `Relationship` record is curated.
4. **Mandatory Provenance Attachment**:
   - Graph edges must attach a `claim_id` referencing a valid `Claim` record.
5. **Orphan Prevention**:
   - Deleting an entity record is blocked (`ON DELETE RESTRICT`) if active family relationships, event participations, or evidence citations depend on it.

---

## 8. Graph Traversal Architecture & Query Patterns

All graph queries operate over indexed relational tables and recursive SQL Common Table Expressions (CTEs).

```
┌────────────────────────────────────────────────────────────────────────┐
│                      SUPPORTED TRAVERSAL PATTERNS                      │
├──────────────────┬─────────────────────────────────────────────────────┤
│ 1-Hop Neighbors  │ Direct outbound and inbound edges from an entity.   │
│ Focus Mode Subgraph│ Bounded 1–2 hop subgraph with deterministic limits. │
│ Family Ancestry  │ Multi-generational directed tree traversal via CTE. │
│ Narrative Path   │ Chronological sequence of Location-linked Events.   │
│ War Campaign     │ War → WarDay (1–18) → Event hierarchical traversal. │
└──────────────────┴─────────────────────────────────────────────────────┘
```

### 8.1. 1-Hop Neighbor Traversal (Direct Connections)
- Retrieves all immediate relationships connected to a node, combining:
  - Outbound edges (`source_id = :entity_id`).
  - Inbound edges (`target_id = :entity_id`).
  - Symmetric edges (`(source_id = :entity_id OR target_id = :entity_id)`).
- Supports filtering by `relationship_type`, `target_entity_type`, and `epistemic_status`.

### 8.2. Family Tree Traversal (Ancestry & Descendants)
- Executes recursive CTE queries over `family_relationships` to construct multi-generational trees (ancestors, descendants, siblings).
- Includes cycle detection guards (`WHERE depth < 10` and path array tracking) to prevent infinite loops.

### 8.3. Narrative Chronological Traversal (Event ↔ Location Paths)
- Queries `Event` records ordered by `sequence_index` where `location_id` is populated.
- Provides sequential spatial movement (e.g., Pandava exile route) derived purely from ordered event data without requiring a speculative `Journey` entity.

### 8.4. War Campaign Hierarchy Traversal
- Traverses the 4-level containment hierarchy: `War → WarDay (1..18) → Event → EventParticipant → Character`.

---

## 9. Global Focus Mode Query Semantics

Focus Mode ([PRD §9.9, FOC-001–FOC-005](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/03-prd/Mahabharata_Explorer_PRD.md#L295)) allows a user to center the entire application on an entity, highlighting directly related entities across all exploration lenses.

### Canonical Focus Subgraph Query Semantics:
1. **Focal Node ($D_0$)**: The focused entity profile and metadata.
2. **First-Degree Neighbors ($D_1$)**:
   - All direct family members (`FamilyRelationship`).
   - All direct inter-entity relationships (`Relationship`).
   - All events where the entity participated (`EventParticipant`).
   - All groups to which the entity belonged.
3. **Optional Second-Degree Neighbors ($D_2$)**:
   - Retrieved only when explicitly requested by client lenses (e.g., Extended Relationship Network).
   - Constrained by strict deterministic limits ($N$ total nodes, default $N \le 50$) to prevent payload explosion.
4. **Deterministic Sorting & Pagination**:
   - Focus results are deterministically ordered:
     1. Traversal depth ($D_0 \rightarrow D_1 \rightarrow D_2$).
     2. Relationship type ascending.
     3. Target entity type ascending.
     4. Target entity slug ascending.
     5. Target entity ID ascending.
   - Algorithmic relevance scoring or dynamic graph-weight ranking is deferred to later blocks.
5. **Cycle & Duplication Suppression**: SQL queries maintain visited node ID arrays to ensure each entity appears exactly once in the returned subgraph payload.
6. **Provenance Inclusion**: Every edge in the Focus subgraph payload includes its `claim_id` and canonical `epistemic_status`.

---

## 10. Family Graph Architecture & Kinship Semantics

Genealogy in the Mahābhārata is foundational (Kuru dynasty, Pandavas, Kauravas, Yadavas).

### 10.1. Exclusivity of `FamilyRelationship`
- All kinship connections are strictly isolated in `family_relationships` to guarantee clean generational layering and eliminate noise from social or military alliances.
- Kinship types are restricted to direct parentage, sibling, spousal, and grandparental links: `father`, `mother`, `son`, `daughter`, `brother`, `sister`, `husband`, `wife`, `grandfather`, `grandmother`, `grandson`, `granddaughter`.

### 10.2. Disputed & Divine Parentage (Epistemic Handling)
- Where parentage involves divine invocation (e.g., Karna born to Kunti and Surya; Pandavas born via Niyoga/devas), both biological/divine parentage and social parentage (Pandu / Adhiratha) are stored as distinct typed edges with explicit `kinship_status` tags (`known`, `approximate`, `conflicting`) and backing `Claim` citations.

---

## 11. Performance & Indexing Requirements for Relational Graphing

To ensure fast graph queries over PostgreSQL without specialized graph hardware, the database schema mandates the following indexing strategy:

1. **Composite B-Tree Indexes on Foreign Key Columns**:
   - `family_relationships(source_character_id, relationship_type)`
   - `family_relationships(target_character_id, relationship_type)`
   - `event_participants(character_id, event_id)`
   - `event_participants(event_id, character_id)`
   - `relationships(source_entity_type, source_entity_id, relationship_type)`
   - `relationships(target_entity_type, target_entity_id, relationship_type)`
2. **Structural Indexing**:
   - `events(location_id, sequence_index)`
   - `events(war_id, war_day_id, sequence_index)`
   - `war_days(war_id, day_number)`
3. **Deferred Latency Budgets**:
   - Explicit query execution time budgets, memory caches, and empirical query plan benchmarks remain deferred to **Block B9 (Performance & Caching)** and **Block B12 (Backend Testing)**.

---

## 12. Requirement Traceability Matrix

| Knowledge Graph Requirement | Source Document | Implemented Architectural Mechanism |
| :--- | :--- | :--- |
| **Unified Graph across All Lenses** | Blueprint §1, §5; PRD §2; Rule 01 | Section 2: Shared Layer 1 nodes and edges queried across lenses. |
| **System Reuse / No Duplicate Engines** | Blueprint §9.3; PRD §8; Rule 02 | Section 3.2: Single shared event model for War, Map, and Timeline. |
| **First-Class Typed Relationships** | Blueprint §11; PRD §9.5 (REL-002) | Section 4.2, Section 5: Controlled taxonomy with canonical directionality and claim links. |
| **Generational Family Trees** | Blueprint §11; PRD §9.5 (REL-004) | Section 3.2, Section 10: Dedicated `FamilyRelationship` with recursive CTEs. |
| **Focus Mode Subgraph Queries** | PRD §9.9 (FOC-001–005); Detailed Ref §7.1 | Section 9: Bounded 1–2 hop subgraph query semantics with deterministic limits. |
| **Zero Inferred / Fabricated Edges** | Blueprint §12; PRD §6.3, §9.5; Rule 03 | Section 7: Anti-fabrication rules; no automatic co-occurrence edges. |
| **Polymorphic Referential Integrity** | B2 Open Decision #2; Rule 03 | Section 4: Database triggers for endpoint existence, compatibility, and orphan prevention. |
| **Disputed / Variant Kinship** | Blueprint §12; PRD §9.10 (SRC-005) | Section 5.2, Section 10.2: Multiple claim-backed edges with canonical epistemic status tags. |
| **Event ↔ Location Paths (No V1 Journey)**| Blueprint §9.1; PRD §6.2, §9.6 | Section 8.3: Chronological event-location sequence queries. |

---

## 13. Decisions Resolved & Deferred

### Decisions Resolved in Block B3:
1. **RESOLVED B3-01**: Adopted the **Hybrid Relational Graph Architecture** (Direct FKs for structural containment + dedicated `FamilyRelationship` + dedicated `EventParticipant` association + controlled `Relationship` for remaining multi-entity links).
2. **RESOLVED B3-02**: Established database-level **Validation Triggers and Restriction Triggers** to enforce referential integrity and orphan prevention on polymorphic `Relationship` endpoints.
3. **RESOLVED B3-03**: Established strict **Kinship Exclusivity** in `FamilyRelationship`, removing kinship types from generic `Relationship`.
4. **RESOLVED B3-04**: Established **Canonical Directionality Convention** (storing canonical forward direction only, deriving inverse queries) and composite symmetric canonicalization.
5. **RESOLVED B3-05**: Formulated canonical query semantics for **Focus Mode Subgraphs** (bounded depth $\le 2$, deterministic tie-breaking, cycle suppression).
6. **RESOLVED B3-06**: Defined anti-fabrication constraints prohibiting synthetic co-occurrence edges.
7. **RESOLVED B3-07**: Established multi-claim representation for **Conflicting Relationship Variants** as separate claim-backed edge records with `epistemic_status = 'conflicting'`.

### Decisions Deferred to Subsequent Blocks:
1. **Granular Citation Parsing & Claim Validation Rules** → *Deferred to Block B4 (Evidence & Provenance)*.
2. **API Endpoint Signatures for Graph & Focus Payloads** → *Deferred to Block B5 (API Architecture)*.
3. **Search Ranking over Graph Nodes & Epithets** → *Deferred to Block B6 (Search Architecture)*.
4. **Empirical Query Benchmarks & Latency Budgets** → *Deferred to Block B9 (Performance & Caching) & Block B12 (Verification)*.
5. **Graph Ingestion Scripts & Integrity Validation Tooling** → *Deferred to Block B10 (Data Ingestion)*.
