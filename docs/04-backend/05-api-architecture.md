# API Architecture (Block B5)

## 1. Purpose

This document establishes the **API Architecture** for the **Mahābhārata Explorer** backend. It specifies the RESTful interface contracts, endpoint taxonomy, request/response envelopes, error formats, query parameters, pagination rules, and versioning standards that expose the canonical knowledge graph ([02-data-architecture.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/02-data-architecture.md), [03-knowledge-graph.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/03-knowledge-graph.md)) and evidence layer ([04-evidence-and-provenance.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/04-evidence-and-provenance.md)) to frontend exploration lenses.

In accordance with project principles:
- **One Knowledge Graph, Many Lenses (Rule 01, REQ-API-01)**: The API serves unified graph data tailored to client lenses without creating divergent backend representations.
- **Zero Fabrication & Epistemic Honesty (Rule 03, REQ-API-05)**: API response contracts always transmit explicit epistemic status and provenance links rather than omitting missing data or returning fabricated placeholders.
- **System Reuse & Consistency (Rule 02, REQ-API-02)**: Standardized response envelopes, deterministic error structures, and predictable resource naming apply across all routes.

---

## 2. Architectural Foundation & Protocol Standards

### 2.1. Core Protocol & Style
- **Protocol**: HTTP/1.1 and HTTP/2 over TLS (HTTPS).
- **Style**: Resource-oriented RESTful JSON API.
- **Data Format**: `application/json; charset=utf-8`.
- **Versioning Strategy**: URI Path Versioning (`/api/v1/...`).

### 2.2. Standard URL Conventions
- Resource collections use plural nouns in lowercase kebab-case (e.g., `/api/v1/characters`, `/api/v1/war-days`).
- Resource instances are resolved via their canonical URL-safe slugs (`/api/v1/characters/:slug`, `/api/v1/locations/:slug`).
- Sub-resources model containment or scoped graph traversals (e.g., `/api/v1/characters/:slug/family`, `/api/v1/wars/:slug/days/:day_number`).

---

## 3. Standard Request and Response Envelopes

Every API response follows a deterministic JSON envelope structure:

### 3.1. Success Envelope (Single Resource)
```json
{
  "data": {
    "id": "uuid-or-text-id",
    "slug": "canonical-slug",
    "name": "Primary Name",
    "status": "published",
    "metadata": {}
  }
}
```

### 3.2. Success Envelope (Paginated Collection)
```json
{
  "data": [
    {
      "id": "uuid-or-text-id",
      "slug": "canonical-slug",
      "name": "Primary Name"
    }
  ],
  "pagination": {
    "limit": 50,
    "offset": 0,
    "total_count": 142,
    "has_more": true
  }
}
```

### 3.3. Standard Error Envelope (RFC 7807 Compatible)
```json
{
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "Character with slug 'unknown-warrior' was not found.",
    "status": 404,
    "details": [],
    "timestamp": "2026-09-01T14:35:00Z"
  }
}
```

---

## 4. HTTP Status Code Discipline

| Status Code | Usage in Mahābhārata Explorer API |
| :--- | :--- |
| **`200 OK`** | Successful retrieval (`GET`), query execution, or Focus subgraph calculation. |
| **`201 Created`** | Successful ingestion/curation record creation (`POST`). |
| **`400 Bad Request`** | Malformed JSON, invalid query parameter, or unrecognized enum value. |
| **`404 Not Found`** | Requested slug or ID does not exist in the canonical dataset. |
| **`409 Conflict`** | Unique slug collision or duplicate edge submission. |
| **`422 Unprocessable Entity`**| Semantic validation failure (e.g., invalid relationship endpoint pair). |
| **`500 Internal Server Error`**| Unhandled backend exception (details sanitised in production). |

---

## 5. Comprehensive V1 Endpoint Taxonomy

### 5.1. Character Lens Endpoints
| Method | Endpoint Path | Description |
| :--- | :--- | :--- |
| `GET` | `/api/v1/characters` | List characters with filtering (`group_slug`, `has_portrait`, search query) and pagination. |
| `GET` | `/api/v1/characters/:slug` | Retrieve complete character profile, aliases, summary, and core metadata. |
| `GET` | `/api/v1/characters/:slug/family` | Retrieve generational ancestry, descendants, and siblings (`FamilyRelationship` graph). |
| `GET` | `/api/v1/characters/:slug/relationships` | Retrieve 1-hop inter-entity relationships (`allied_with`, `teacher_of`, `rival_of`, etc.). |
| `GET` | `/api/v1/characters/:slug/events` | Retrieve chronological narrative events where the character participated. |

---

### 5.2. Timeline & Narrative Event Endpoints
| Method | Endpoint Path | Description |
| :--- | :--- | :--- |
| `GET` | `/api/v1/events` | List events in global chronological sequence (`sequence_index` order). Supports Parva filtering. |
| `GET` | `/api/v1/events/:slug` | Retrieve full event details, location link, war associations, and participants. |
| `GET` | `/api/v1/events/:slug/participants` | Retrieve structured roster of participating characters with their specific roles. |

---

### 5.3. Geographic Map Endpoints
| Method | Endpoint Path | Description |
| :--- | :--- | :--- |
| `GET` | `/api/v1/locations` | List geographic locations. Supports filtering by `location_type` and `coordinate_status`. |
| `GET` | `/api/v1/locations/:slug` | Retrieve location details, modern region notes, and geographic coordinates. |
| `GET` | `/api/v1/locations/:slug/events` | Retrieve sequential events that occurred at this specific location. |

---

### 5.4. Groups & Dynasties Endpoints
| Method | Endpoint Path | Description |
| :--- | :--- | :--- |
| `GET` | `/api/v1/groups` | List dynasties, clans, military units, and factions. |
| `GET` | `/api/v1/groups/:slug` | Retrieve group summary, type, and dynastic metadata. |
| `GET` | `/api/v1/groups/:slug/members` | Retrieve characters belonging to or allied with this group. |

---

### 5.5. War Explorer Endpoints
| Method | Endpoint Path | Description |
| :--- | :--- | :--- |
| `GET` | `/api/v1/wars` | List major conflicts (principally the Kurukshetra War). |
| `GET` | `/api/v1/wars/:slug` | Retrieve war campaign overview, commanders, and casualty summaries. |
| `GET` | `/api/v1/wars/:slug/days` | Retrieve all 18 structured war days in chronological sequence. |
| `GET` | `/api/v1/wars/:slug/days/:day_number` | Retrieve specific war day (1–18), summary, formations, and daily events. |

---

### 5.6. Battle Formations (Vyuhas) Endpoints
| Method | Endpoint Path | Description |
| :--- | :--- | :--- |
| `GET` | `/api/v1/formations` | List battlefield vyuhas with `visualization_status` filtering. |
| `GET` | `/api/v1/formations/:slug` | Retrieve formation description, associated event, and coordinate geometry payload. |

---

### 5.7. Knowledge Graph & Global Focus Endpoints
| Method | Endpoint Path | Description |
| :--- | :--- | :--- |
| `GET` | `/api/v1/graph/focus/:entity_type/:slug` | Retrieve bounded Focus mode subgraph ($D_0$ node, $D_1$ neighbors, optional $D_2$ nodes). |
| `GET` | `/api/v1/relationships/:id` | Retrieve single relationship edge record with attached metadata. |

---

### 5.8. Evidence & Provenance Endpoints
| Method | Endpoint Path | Description |
| :--- | :--- | :--- |
| `GET` | `/api/v1/sources` | List authoritative bibliographic sources, critical editions, and recensions. |
| `GET` | `/api/v1/sources/:slug` | Retrieve source overview, critical methodology, and citation index. |
| `GET` | `/api/v1/claims/:id` | Retrieve claim details, certainty level, and all attached evidence passages. |
| `GET` | `/api/v1/entities/:entity_type/:slug/provenance` | Retrieve all claims and citation evidence supporting a specific entity. |
| `GET` | `/api/v1/relationships/:id/evidence` | Retrieve the backing claim and textual evidence for a specific graph edge. |

---

### 5.9. Search Endpoint Contract (High-Level Interface)
| Method | Endpoint Path | Description |
| :--- | :--- | :--- |
| `GET` | `/api/v1/search` | Global search across entity names, aliases, summaries, and topics. |

*Query Parameters*:
- `q`: Search query string (minimum 2 characters).
- `types`: Comma-separated entity type filter (e.g., `character,location,event`).
- `limit`: Maximum number of results per type.
*(Search ranking, trigram matching, and fuzzy matching details are formalized in Block B6).*

---

## 6. Global Focus Mode API Contract

In accordance with [PRD §9.9 (FOC-001–005)](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/03-prd/Mahabharata_Explorer_PRD.md#L295) and [03-knowledge-graph.md §9](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/03-knowledge-graph.md#L266):

### Request:
`GET /api/v1/graph/focus/:entity_type/:slug?depth=1&limit=50`

### Response Payload Structure:
```json
{
  "data": {
    "focus_node": {
      "id": "char-uuid-1",
      "slug": "arjuna",
      "entity_type": "character",
      "name": "Arjuna",
      "summary": "Third Pandava prince..."
    },
    "nodes": [
      {
        "id": "char-uuid-2",
        "slug": "krishna",
        "entity_type": "character",
        "name": "Krishna",
        "depth": 1
      },
      {
        "id": "event-uuid-1",
        "slug": "gita-discourse",
        "entity_type": "event",
        "title": "Discourse of the Bhagavad Gita",
        "depth": 1
      }
    ],
    "edges": [
      {
        "id": "rel-uuid-1",
        "source_entity_id": "char-uuid-1",
        "target_entity_id": "char-uuid-2",
        "relationship_type": "allied_with",
        "directionality": "symmetric",
        "epistemic_status": "known",
        "claim_id": "claim-uuid-10"
      }
    ]
  },
  "meta": {
    "depth": 1,
    "node_count": 2,
    "edge_count": 1
  }
}
```

---

## 7. Pagination, Filtering, and Sorting Standards

### 7.1. Pagination Standard
All collection endpoints default to limit/offset pagination:
- `limit`: Integer between `1` and `100` (default `50`).
- `offset`: Integer $\ge 0$ (default `0`).

### 7.2. Standard Filtering Parameters
- `status`: Filter by curation status (default `published`).
- `epistemic_status`: Filter by certainty state (`known`, `conflicting`, `approximate`, etc.).
- `parva`: Filter events or evidence by Parva number (`1` to `18`).

### 7.3. Deterministic Sorting Parameters
- `sort`: Field name (e.g., `sequence_index`, `name`, `day_number`, `created_at`).
- `order`: `asc` (default) or `desc`.

---

## 8. OpenAPI 3.1 Specification Strategy

To guarantee frontend-backend contract type safety:
- The backend API will be formally documented using an **OpenAPI 3.1** specification.
- API route schemas will be authored using type-safe TypeScript validation libraries (e.g., Zod / TypeBox) that generate OpenAPI definitions and TypeScript client types synchronously.
- Concrete tooling and documentation hosting are deferred to implementation.

---

## 9. Requirement Traceability Matrix

| API Architecture Requirement | Source Document | Implemented Architectural Mechanism |
| :--- | :--- | :--- |
| **RESTful JSON Foundation** | Context §5; B1 §5 | Section 2: Standard HTTP/REST over TLS with URI versioning. |
| **Unified Envelope & Error Contracts** | Rule 02, Rule 05; PRD §8 | Section 3: Standard data, pagination, and RFC 7807 error envelopes. |
| **Complete 12-Entity Route Coverage** | B2 §4; PRD §6; Blueprint §11 | Section 5: Endpoints for Character, Event, Location, Group, War, WarDay, Formation, Source, Claim, Evidence. |
| **Global Focus Subgraph Endpoint** | PRD §9.9 (FOC-001–005); B3 §9 | Section 5.7, Section 6: Dedicated `/api/v1/graph/focus/...` contract. |
| **Epistemic Status Transparency** | Rule 03; PRD §11; B2 §5 | Section 3, Section 6: Mandatory `epistemic_status` fields in entity and edge envelopes. |
| **Evidence & Provenance Routes** | PRD §9.10; B4 §10 | Section 5.8: Endpoints for entity provenance cards, edge evidence, and source citations. |
| **Deterministic Pagination & Sorting** | Rule 05; PRD §8 | Section 7: Bounded limit/offset pagination with deterministic order fields. |

---

## 10. Decisions Resolved & Deferred

### Decisions Resolved in Block B5:
1. **RESOLVED B5-01**: Established **URI Path Versioning (`/api/v1/...`)** and resource-oriented REST conventions.
2. **RESOLVED B5-02**: Standardized **Universal Response Envelopes** (`data`, `pagination`, `error`) across all routes.
3. **RESOLVED B5-03**: Established the complete **Endpoint Taxonomy for the 12 Core Entities and Graph Lenses**.
4. **RESOLVED B5-04**: Defined the **Focus Mode API Request and Response Contract**.
5. **RESOLVED B5-05**: Standardized HTTP status code usage and error reporting.

### Decisions Deferred to Subsequent Blocks:
1. **Full-Text Search Query Internals & Ranking** → *Deferred to Block B6 (Search Architecture)*.
2. **Authentication, Authorization & Curation Endpoints** → *Deferred to Block B7 (Auth & Permissions)*.
3. **Media URL Delivery & Asset CDN Caching** → *Deferred to Block B8 (Storage & Media)*.
4. **Rate Limiting, Compression & HTTP Cache Headers** → *Deferred to Block B9 (Performance & Caching)*.
5. **Concrete API Framework Code & Routing Implementation** → *Deferred to Stage 4 (Backend Implementation)*.
