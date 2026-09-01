# Search Architecture (Block B6)

## 1. Purpose

This document establishes the **Search Architecture** for the **Mahābhārata Explorer** backend. It specifies the search retrieval strategies, indexing models, ranking heuristics, and transliteration-handling rules that power the unified global search interface across primary canonical entities, aliases, narrative events, and textual evidence.

In accordance with project principles:
- **One Knowledge Graph, Many Lenses (Rule 01, PRD §9.8)**: Search acts as a universal navigation and discovery lens across the unified knowledge graph. Derived search indexes are secondary access structures over the single canonical dataset and do not create duplicate domain stores.
- **Zero Fabrication (Rule 03, PRD §11)**: Search queries return only curated, verified entities and textual passages; results must never be hallucinated, inferred from co-occurrence, or synthetically generated via generative fallback.
- **System Reuse & Scope Discipline (Rule 02, Rule 04)**: For V1, search is implemented natively within PostgreSQL using `pg_trgm`, `unaccent`, and Full-Text Search (`tsvector`/GIN), eliminating the operational complexity of external search clusters (Elasticsearch, Meilisearch) or vector databases.

---

## 2. Searchable Entity Scope & Result Categorization

### 2.1. Searchable Domain Scope in V1
Aligned with [PRD §9.8 (SRCH-001)](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/03-prd/Mahabharata_Explorer_PRD.md#L274), V1 search provides discovery across specific top-level domain entities and their supporting narrative/evidence texts:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        V1 SEARCH DOMAIN SCOPE                          │
├──────────────────────────────────┬─────────────────────────────────────┤
│ 1. PRIMARY DISCOVERABLE ENTITIES │ 2. NARRATIVE & CITATION TEXTS       │
├──────────────────────────────────┼─────────────────────────────────────┤
│ - Character (name, aliases)      │ - Event summaries                   │
│ - Event (title, summary)         │ - Claim proposition text            │
│ - Location (name, aliases, type) │ - Evidence excerpts & locators      │
│ - Group (name, aliases, type)    │ - Source bibliographic summaries    │
│ - War (name, summary)            │                                     │
│ - Formation (name, description)  │                                     │
│ - Source (title, short title)    │                                     │
└──────────────────────────────────┴─────────────────────────────────────┘
```

> **Entity Scope Clarification**: Not all 12 core database tables are top-level search targets. Graph edges (`Relationship`, `FamilyRelationship`, `EventParticipant`) and daily subdivisions (`WarDay`) are structured associations discovered through their parent entities (Characters, Wars, Events). `Claim` and `Evidence` texts are searchable as textual content, but their search results resolve directly to their parent entity or source.

### 2.2. Three Functional Search Discovery Modes
1. **Entity Discovery**: Finding specific characters, places, groups, wars, formations, or sources by primary name, title, or alias (e.g., searching *Arjuna*, *Dhananjaya*, *Hastinapura*, *Chakravyuha*, *BORI*).
2. **Narrative & Content Discovery**: Finding chronological events, narrative milestones, and philosophical claims matching keyword queries across summaries and claim texts.
3. **Evidence & Citation Discovery**: Locating specific textual passages, Sanskrit verse excerpts, or commentary assessments across authoritative sources.

---

## 3. PostgreSQL-Native Search Foundation

Aligned with the infrastructure direction in [01-technology-and-infrastructure.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/01-technology-and-infrastructure.md), the search architecture utilizes a **Dual-Tier Search Engine** backed by a dedicated text normalization preprocessing capability:

```
┌────────────────────────────────────────────────────────────────────────┐
│             PREPROCESSING & NORMALIZATION (PostgreSQL unaccent)        │
│  - Strips Latin diacritical accents (ā→a, ī→i, ś→s) for input tolerance│
└───────────────────────────────────┬────────────────────────────────────┘
                                    │
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│                     DUAL-TIER RETRIEVAL ENGINE                         │
├───────────────────────────────────┬────────────────────────────────────┤
│ TIER A: TRIGRAM SIMILARITY        │ TIER B: FULL-TEXT SEARCH           │
│ (pg_trgm Extension)               │ (PostgreSQL tsvector / tsquery)    │
├───────────────────────────────────┼────────────────────────────────────┤
│ - Primary names, titles & slugs   │ - Long-form narrative summaries    │
│ - Curated alternate name arrays   │ - Event descriptions               │
│ - Typo-tolerant fuzzy matching    │ - Textual excerpts & commentary    │
│ - Prefix & substring matching     │ - Propositional claim text         │
└───────────────────────────────────┴────────────────────────────────────┘
```

1. **Retrieval Mechanism 1: `pg_trgm` (Trigram Similarity Engine)**:
   - Operates over structured identity attributes (`name`, `title`, `slug`, `alternate_names`).
   - Supports fuzzy similarity matching, prefix completion, and typo tolerance.
2. **Retrieval Mechanism 2: PostgreSQL FTS (`tsvector` / `tsquery` with GIN Indexes)**:
   - Operates over long-form prose (`summary`, `description`, `claim_text`, `excerpt`).
   - Supports stemming, lexeme parsing, boolean query operators, and structured relevance ranking (`ts_rank_cd`).
3. **Preprocessing Dependency: `unaccent` (Diacritic Stripping Extension)**:
   - Serves strictly as an input/text normalization dependency. It strips standard Latin diacritical accents to enable matching between academic IAST inputs and plain ASCII keyboard queries. It is not an independent search engine or transliteration converter.

---

## 4. Transliteration Handling & Safe Normalization Boundaries

Sanskrit proper nouns in scholarship carry diverse transliteration conventions. To uphold Rule 03 (Zero Fabrication), **the system must never perform algorithmic consonant collapses that could cause false-positive identity collisions between distinct entities**:

### 4.1. Precise Scope of `unaccent`
- `unaccent` provides **basic Latin diacritic stripping only** (e.g., *ā* $\rightarrow$ *a*, *ī* $\rightarrow$ *i*, *ū* $\rightarrow$ *u*, *ṛ* $\rightarrow$ *r*, *ś/ṣ* $\rightarrow$ *s*, *ṭ/ḍ/ṇ* $\rightarrow$ *t/d/n*).
- This allows *Bhīṣma* to match *Bhisma*, and *Ādi Parva* to match *Adi Parva*.
- `unaccent` is **not** a general Sanskrit transliteration engine and does not attempt phonetic translation.

### 4.2. Safe Normalization Rules
1. **Case Normalization**: All comparison vectors are evaluated in lowercase.
2. **Whitespace Normalization**: Trimming trailing/leading spaces, collapsing whitespace.
3. **Prohibition of Unsafe Phonetic Heuristics**: Automated consonant substitutions (e.g., `v ↔ w`, `sh ↔ s`, `ri ↔ r`) are **strictly prohibited** because they risk merging distinct Sanskrit names into false identities.
4. **Explicit Curated Aliases**: All genuine transliteration variants, patronymics, and epithets are explicitly curated in the canonical `alternate_names TEXT[]` array on `Character`, `Location`, and `Group`.

---

## 5. Search Ranking & Execution Pipeline

Search queries are evaluated through a multi-tier pipeline that prioritizes exact canonical identity matches over partial and full-text matches:

### 5.1. Ranking Hierarchy & Weight Classes

| Field Category | Target Columns | Weight Class | Relative Importance |
| :--- | :--- | :--- | :--- |
| **Primary Canonical Identifier** | `name`, `title`, `slug` | **Weight A** | Highest Priority |
| **Curated Aliases & Epithets** | `alternate_names` | **Weight B** | High Priority |
| **Narrative Summaries & Prose** | `summary`, `description`, `claim_text` | **Weight C** | Medium Priority |
| **Citation Excerpts & Locators** | `evidence.excerpt`, `evidence.locator` | **Weight D** | Secondary Priority |

### 5.2. Multi-Tier Execution Pipeline

```
Query Input (:q)
      │
      ▼
┌─────────────────────────────────────────────────────────────┐
│ Tier 1: Exact & Prefix Matching                             │
│ - Exact slug match                                          │
│ - Exact name/title match                                    │
│ - Prefix name match                                         │
└─────────────────────────────┬───────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│ Tier 2: Trigram Similarity Matching (pg_trgm)               │
│ - Trigram similarity over unaccent(name), unaccent(title),   │
│   and unaccent(array_to_string(alternate_names, ' '))       │
│ - Default similarity threshold: > 0.30                      │
└─────────────────────────────┬───────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│ Tier 3: Full-Text Search (tsvector / ts_rank_cd)           │
│ - websearch_to_tsquery match over summaries & claim text    │
│ - Lexeme stemming and positional ranking                    │
└─────────────────────────────┬───────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│ Result Merging, Deduplication & Deterministic Tie-Breaking  │
│ - Order: Calculated Score DESC, Entity Type ASC, Slug ASC   │
└─────────────────────────────────────────────────────────────┘
```

> **Tuning Parameter Note**: Specific numerical scoring weights and similarity thresholds (e.g., $0.30$ trigram threshold, scoring multipliers) are defined as **initial default tuning parameters**. These constants are subject to empirical recall/precision tuning and regression verification in Block B12. The relative ranking hierarchy (Tier 1 $\rightarrow$ Tier 2 $\rightarrow$ Tier 3) remains the invariant architectural requirement.

---

## 6. Indexing Architecture & Database Configuration

To ensure search queries remain responsive across the dataset, the database schema mandates the following specialized GIN index models:

### 6.1. Trigram GIN Indexes (Names & Aliases)
1. **Characters**:
   - `name` (with `gin_trgm_ops`)
   - `array_to_string(alternate_names, ' ')` (with `gin_trgm_ops`)
2. **Locations**:
   - `name` (with `gin_trgm_ops`)
   - `array_to_string(alternate_names, ' ')` (with `gin_trgm_ops`)
3. **Groups**:
   - `name` (with `gin_trgm_ops`)
   - `array_to_string(alternate_names, ' ')` (with `gin_trgm_ops`)
4. **Events, Wars, Formations, Sources**:
   - `events(title)` (with `gin_trgm_ops`)
   - `wars(name)` (with `gin_trgm_ops`)
   - `formations(name)` (with `gin_trgm_ops`)
   - `sources(title, short_title)` (with `gin_trgm_ops`)

### 6.2. Full-Text Search GIN Indexes (Summaries & Evidence)
- Generated `tsvector` columns with GIN indexes on `events(summary)`, `formations(description)`, `claims(claim_text)`, and `evidence(excerpt)`.

---

## 7. Search Interface Contracts & API Design

In strict alignment with [05-api-architecture.md §5.9](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/05-api-architecture.md#L171), the search subsystem provides two standardized REST interfaces:

### 7.1. Fast Autocomplete / Suggestions (`GET /api/v1/search/suggest`)
Optimized for search-as-you-type dropdown navigation:

- **Query Parameter**: `q` (required, minimum 2 characters).
- **Target Route**: Direct canonical exploration link (`/characters/:slug`, `/locations/:slug`, `/events/:slug`, `/wars/:slug`, `/formations/:slug`, `/sources/:slug`).

#### Response Envelope:
```json
{
  "data": [
    {
      "id": "char-uuid-1",
      "slug": "arjuna",
      "entity_type": "character",
      "title": "Arjuna",
      "matched_term": "Partha",
      "match_type": "alias",
      "summary_snippet": "Third Pandava prince...",
      "target_url": "/characters/arjuna"
    }
  ]
}
```

### 7.2. Full Global Search (`GET /api/v1/search`)
Full-faceted search across discoverable entities and content:

- **Query Parameters**:
  - `q`: Search query string (minimum 2 characters).
  - `types`: Comma-separated entity type filter (e.g., `character,location,event,source`).
  - `limit`: Integer between `1` and `100` (default `20`).
  - `offset`: Integer $\ge 0$ (default `0`).

#### Response Envelope:
```json
{
  "data": [
    {
      "id": "char-uuid-1",
      "slug": "arjuna",
      "entity_type": "character",
      "title": "Arjuna",
      "match_type": "exact_name",
      "matched_term": "Arjuna",
      "summary_snippet": "Third Pandava prince, celebrated archer...",
      "score": 95.4,
      "target_url": "/characters/arjuna",
      "epistemic_status": "known"
    }
  ],
  "pagination": {
    "limit": 20,
    "offset": 0,
    "total_count": 1,
    "has_more": false
  },
  "meta": {
    "query": "Arjuna"
  }
}
```

> **Contextual Epistemic Status Note**: The `epistemic_status` field in search responses is returned where semantically applicable (e.g., `locations` reflecting `coordinate_status`, `events` reflecting `chronology_status`, `claims` reflecting `epistemic_status`). For entity types without field-level uncertainty (e.g., `sources`), this field is omitted or returned as `null`.

---

## 8. Anti-Fabrication Safeguards in Search (Rule 03)

To ensure search results strictly maintain scholarly truthfulness:
1. **Curated Corpus Boundary**: Search indexes only index verified records present in the canonical PostgreSQL tables.
2. **Deterministic Empty State**: When no match is found, the API returns `{ "data": [], "pagination": { "total_count": 0 } }`. The backend must never execute generative fallback, LLM interpolation, or uncurated external search.
3. **Fuzzy Search Boundary**: Trigram fuzzy matching aids in discovering existing curated records with alternate spellings or typos; it **never** synthesizes new records, relationships, or attributes.

---

## 9. Requirement Traceability Matrix

| Search Requirement | Source Document | Implemented Architectural Mechanism |
| :--- | :--- | :--- |
| **Global Multi-Entity Search** | PRD §9.8 (SRCH-001); Blueprint §11 | Section 2, Section 6: Universal search across Characters, Events, Locations, Groups, Wars, Formations, Sources. |
| **Transliteration & Alias Matching** | PRD §9.8 (SRCH-001); Rule 03 | Section 4: `unaccent` normalization + GIN trigram indexing on curated `alternate_names`. |
| **Typo-Tolerant Fuzzy Search** | PRD §9.8 (SRCH-002) | Section 3, Section 5: `pg_trgm` similarity threshold matching. |
| **Full-Text Narrative Search** | PRD §9.8 (SRCH-003) | Section 3, Section 5.2: PostgreSQL FTS (`tsvector` / `ts_rank_cd`). |
| **Autocomplete / Quick Navigation** | PRD §9.8 (SRCH-004) | Section 7.1: Dedicated `/api/v1/search/suggest` endpoint. |
| **No External Search Clusters in V1** | Context §4.H; B1 §5; Rule 02 | Section 3: Pure PostgreSQL implementation (`pg_trgm` + `unaccent` + FTS). |
| **Zero Fabrication in Search** | Rule 03; PRD §11 | Section 8: Strict curation boundaries, zero generative fallback. |

---

## 10. Decisions Resolved & Deferred

### Decisions Resolved in Block B6:
1. **RESOLVED B6-01**: Selected **PostgreSQL Native Dual-Tier Search** (`pg_trgm` + `tsvector`) with **`unaccent` preprocessing** as the unified search engine for V1.
2. **RESOLVED B6-02**: Clarified the **Searchable Domain Scope** (primary discoverable entities vs content/evidence text).
3. **RESOLVED B6-03**: Established the **Safe Transliteration Normalization Standard** (`unaccent` Latin diacritic stripping + explicit curated `alternate_names`, prohibiting unsafe consonant substitutions).
4. **RESOLVED B6-04**: Formulated the **Multi-Tier Search Execution Pipeline** and established the relative ranking hierarchy.
5. **RESOLVED B6-05**: Aligned **Search API Contracts** (`/search` and `/search/suggest`) with Block B5 contracts.
6. **RESOLVED B6-06**: Defined anti-fabrication rules prohibiting generative fallback in search.

### Decisions Deferred to Subsequent Blocks:
1. **Authentication & Rate Limiting on Search Endpoints** → *Deferred to Block B7 & B9*.
2. **Search Cache Invalidation & Query Acceleration** → *Deferred to Block B9 (Performance & Caching)*.
3. **Empirical Precision/Recall Tuning & Latency Benchmarks** → *Deferred to Block B12 (Backend Testing)*.
4. **Semantic Vector Search & Embeddings** → *Deferred to V2/V3 Roadmap*.
