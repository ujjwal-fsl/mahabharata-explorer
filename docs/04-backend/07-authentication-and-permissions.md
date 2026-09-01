# Authentication & Permissions Architecture (Block B7)

## 1. Purpose

This document establishes the **Authentication, Authorization, and Security Architecture** for the **Mahābhārata Explorer** backend. It specifies the security boundaries, access controls, credential management policies, and mutation safeguards governing the canonical knowledge graph, evidence base, and public API.

In accordance with project principles:
- **Zero Fabrication & Information Integrity (Rule 03, REQ-SEC-01)**: The canonical dataset represents verified cultural and scholarly truth. Public endpoints must have zero ability to execute mutations on canonical datasets, and administrative mutations must pass strict schema and provenance validation before publication.
- **One Knowledge Graph, Anonymous Exploration (Rule 01, REQ-AUT-01, REQ-STA-01)**: Public exploration is 100% anonymous, stateless, and read-only.
- **System Reuse & Scope Discipline (Rule 02, Rule 04, REQ-AUT-02, REQ-STA-03)**: Complex multi-tenant RBAC, public user registration, user profiles, social features, bookmarks, and public editing are strictly excluded from V1.

---

## 2. Authentication vs. Authorization

The security architecture strictly separates the concepts of **Authentication** and **Authorization**:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   SECURITY BOUNDARY DEFINITION                         │
├───────────────────────────────────┬────────────────────────────────────┤
│ 1. AUTHENTICATION (Who are you?)  │ 2. AUTHORIZATION (What can you do?)│
├───────────────────────────────────┼────────────────────────────────────┤
│ Proving the caller's identity:    │ Determining permitted operations:  │
│ - Anonymous Public Visitor        │ - Public: Read-Only Queries        │
│ - Authenticated Administrator     │ - Admin: Ingest, Validate, Publish │
└───────────────────────────────────┴────────────────────────────────────┘
```

1. **Authentication**: Verifies the identity of the requesting agent (e.g., executing within the trusted deployment environment with database credentials vs identifying an unauthenticated public visitor).
2. **Authorization**: Determines whether an authenticated identity has permission to perform a specific action (e.g., granting read-only query execution to anonymous callers while restricting data ingestion and dataset publication to administrators).

---

## 3. Public Access Model & V1 Non-Goals

### 3.1. Public Exploration Access (100% Anonymous & Read-Only)
- In accordance with **REQ-AUT-01** and **REQ-STA-01**, all public exploration lenses (Timeline, Character Profiles, Geographic Map, War Explorer, Relationship Graph, Global Focus, and Search) require **zero authentication**.
- Public users make standard, unauthenticated HTTP `GET` requests.
- No session cookies, API keys, bearer tokens, or user logins are required or permitted for public exploration.

### 3.2. Explicit V1 Non-Goals (Scope Boundaries)
In accordance with **REQ-AUT-02** and **REQ-STA-03**, the following capabilities are **strictly excluded** from V1:
- Public user registration, logins, and passwords.
- User profiles, avatars, and personal dashboards.
- Public commenting, discussion forums, or social feeds.
- Public bookmarks, personal notes, or saved states (client-side localStorage is used for client UI preferences only).
- Public crowdsourcing, wiki-style editing, or open submissions.

---

## 4. Administrative Curation & Ingestion Security Boundary

Aligned with **REQ-ING-03** ("Ingestion pipeline must operate outside runtime user queries"), the backend establishes an explicit boundary separating public runtime traffic from administrative data curation:

```
                                  INCOMING TRAFFIC
                                         │
                 ┌───────────────────────┴───────────────────────┐
                 │                                               │
                 ▼                                               ▼
      [ Public HTTP Requests ]                       [ Administrative Curation ]
                 │                                               │
                 │ (No Credentials Required)                     │ (Trusted CLI / Ingestion Tooling)
                 ▼                                               ▼
      ┌──────────────────────┐                       ┌──────────────────────┐
      │  Public API Router   │                       │   Offline Ingestion  │
      │   (GET Routes Only)  │                       │   Pipeline (B10)     │
      └──────────┬───────────┘                       └──────────┬───────────┘
                 │                                               │
                 ▼ (Read-Only Queries)                           ▼ (Atomic Validated Mutation)
      ┌─────────────────────────────────────────────────────────────────────┐
      │                      POSTGRESQL PRIMARY DATABASE                    │
      │                  (Canonical Knowledge Graph & Evidence)             │
      └─────────────────────────────────────────────────────────────────────┘
```

### 4.1. Required V1 Administrative Mechanism: Trusted Offline/CLI Pipeline
- In accordance with **REQ-ING-03** and **REQ-AUT-03**, the **required V1 administrative mechanism** is the trusted offline/CLI curation pipeline (Block B10).
- Curators and maintainers execute ingestion and validation scripts within the secure deployment boundary with direct database credentials (`DATABASE_URL`).
- This eliminates public-facing HTTP mutation attack surfaces entirely for core V1 dataset operations.

---

## 5. Administrative Authentication Evaluation & Optional Remote API

### 5.1. Evaluation of Administrative Authentication Approaches

| Approach | Description | Tradeoffs & Evaluation | V1 Architectural Status |
| :--- | :--- | :--- | :--- |
| **Option 1: Trusted CLI / Ingestion Tooling Boundary** | Ingestion executed exclusively via trusted CLI scripts connecting directly to PostgreSQL (no HTTP mutation routes exposed). | **Pros**: Eliminates public-facing HTTP attack surface; minimal operational overhead; matches batch ingestion model.<br>**Cons**: Requires access to deployment execution environment. | **REQUIRED V1 MECHANISM (SELECTED)** |
| **Option 2: Pre-Shared Cryptographic Bearer Token (`ADMIN_API_KEY`)** | High-entropy random token supplied via environment variable; validated via constant-time comparison for optional remote admin HTTP endpoints. | **Pros**: Simple, stateless.<br>**Cons**: Shared credential, no individual curator attribution, requires secret rotation on staff change. | **OPTIONAL / DEFERRED MECHANISM** |
| **Option 3: Database-Backed User Accounts + JWT Sessions** | PostgreSQL `users` table with password hashing and JWT sessions. | **Pros**: Multi-user auditability.<br>**Cons**: Unnecessary architectural complexity for a project with no public users and a small curation team. Violates System Reuse / Scope Discipline. | **REJECTED FOR V1** |
| **Option 4: External Identity Provider (OAuth2 / OIDC)** | Third-party identity management platform (Auth0, Clerk, etc.). | **Pros**: Enterprise SSO.<br>**Cons**: Unnecessary external dependency, recurring cost, complex headless integration. | **REJECTED FOR V1** |

### 5.2. Limitations of Shared Token Authentication (`ADMIN_API_KEY`)
If an administrative HTTP API is introduced as an optional remote interface, the architecture documents the following explicit limitations of shared bearer token authentication:
- **Shared Credential**: A single key is shared across curators; individual curator actions cannot be cryptographically attributed to specific individuals.
- **Revocation & Rotation Overhead**: If a maintainer leaves, the key must be rotated in the environment, invalidating access for all tools simultaneously.
- **Accountability Rule**: If granular, non-repudiable individual administrator accountability becomes a strict requirement, the authentication model must be revisited and upgraded to individual administrative credentials.

---

## 6. Authorization Model

Because V1 does not support public user accounts, the authorization model is intentionally lean and deterministic:

### 6.1. The Two Canonical V1 Authorization Scopes

| Scope | Identity Mechanism | Permitted Capabilities | Prohibited Capabilities |
| :--- | :--- | :--- | :--- |
| **`Public (Anonymous)`** | Unauthenticated request | `GET`, `HEAD`, `OPTIONS` on all public exploration routes (`/characters`, `/events`, `/locations`, `/wars`, `/search`, etc.). | Any HTTP mutation (`POST`, `PUT`, `PATCH`, `DELETE`); any access to administrative tooling. |
| **`Administrator / Curator`** | Trusted CLI execution (or validated admin token if optional API is used) | Execute dataset validation; run batch seed ingestion; publish curated datasets. | Direct bypassing of database integrity and schema validation gates. |

---

## 7. Secret Management Architecture

To satisfy **REQ-SEC-02** and **REQ-SEC-03**, credentials and private keys are strictly managed according to the following invariants:

```
┌────────────────────────────────────────────────────────────────────────┐
│                       SECRET MANAGEMENT INVARIANTS                     │
├────────────────────────────────────────────────────────────────────────┤
│ 1. NEVER IN SOURCE CONTROL: Zero secrets or keys in Git repositories.  │
│ 2. ENVIRONMENT INJECTION: Injected at runtime via process.env.         │
│ 3. ZERO FRONTEND EXPOSURE: Frontend bundle contains ZERO server secrets│
│ 4. ZERO RESPONSE LEAKAGE: Secrets are never returned in API payloads.  │
│ 5. INDEPENDENT ROTATION: Tokens rotate via environment without code    │
│    changes.                                                            │
└────────────────────────────────────────────────────────────────────────┘
```

### 7.1. Canonical Secret Inventory

| Secret Identifier | Usage & Scope | Storage & Supply Mechanism |
| :--- | :--- | :--- |
| `DATABASE_URL` | PostgreSQL connection string with credentials. | Injected via environment variable on backend runtime / hosting environment. |
| `ADMIN_API_KEY` | High-entropy secret for optional administrative endpoints. | Injected via environment variable on backend runtime. |
| `STORAGE_SECRET_KEY` | Storage/CDN credentials (if applicable in B8). | Injected via environment variable on backend runtime. |

---

## 8. Public API Security & Abuse Protections

Although public endpoints are read-only, the API enforces defensive protections to prevent denial-of-service, scraping abuse, and data tampering:

### 8.1. HTTP Method Restriction & Mutation Blocking (REQ-SEC-01)
- Public routes strictly permit read-only HTTP verbs: `GET`, `HEAD`, and `OPTIONS`.
- Any attempt to send `POST`, `PUT`, `PATCH`, or `DELETE` to public routes is rejected immediately at the routing layer with standard `405 Method Not Allowed`.

### 8.2. Strict Input & Query Parameter Validation
- All query parameters (`q`, `limit`, `offset`, `types`, `slug`, `parva`) are validated against strict type, format, and range constraints before executing database queries.
- Bounded pagination constraints (enforced in [05-api-architecture.md §7.1](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/05-api-architecture.md#L243): `limit <= 100`, `offset >= 0`) prevent memory exhaustion and table-scan denial of service.

### 8.3. Transport Security (HTTPS)
- Production traffic requires TLS 1.3 (or TLS 1.2 minimum).
- HTTP requests are redirected to HTTPS.
- Standard security headers (`Strict-Transport-Security`, `X-Content-Type-Options: nosniff`, `X-Frame-Options: DENY`) are enforced.

---

## 9. Failure & Error Behavior

All security-related failures follow the standardized RFC 7807-compatible error format established in [05-api-architecture.md §3.3](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/05-api-architecture.md#L66):

### 9.1. Standard Error Responses

1. **Missing Authentication Credentials (`401 Unauthorized`)**:
   ```json
   {
     "error": {
       "code": "UNAUTHORIZED",
       "message": "Authentication credentials are required to access this resource.",
       "status": 401
     }
   }
   ```
2. **Invalid or Revoked Credentials (`401 Unauthorized`)**:
   ```json
   {
     "error": {
       "code": "INVALID_CREDENTIALS",
       "message": "The provided authentication token is invalid or has expired.",
       "status": 401
     }
   }
   ```
3. **Prohibited Mutation Verb (`405 Method Not Allowed`)**:
   ```json
   {
     "error": {
       "code": "METHOD_NOT_ALLOWED",
       "message": "HTTP mutation methods are prohibited on public exploration endpoints.",
       "status": 405
     }
   }
   ```
4. **Sanitized Internal Errors (`500 Internal Server Error`)**:
   - Internal database errors, SQL syntax failures, and system stack traces are **never** returned to callers. Production errors return a generic `INTERNAL_SERVER_ERROR` with a unique error tracking ID.

---

## 10. Browser Security, CORS, and CSRF Boundaries

### 10.1. Cross-Origin Resource Sharing (CORS)
- **Public Exploration API**: Configured to permit cross-origin `GET` requests strictly from authorized, deployed frontend origins.
- Wildcard CORS with credentials (`Access-Control-Allow-Credentials: true` with `*`) is strictly prohibited.
- Permitted methods: `GET, HEAD, OPTIONS`.
- Permitted headers: `Content-Type, Accept`.

### 10.2. Cross-Site Request Forgery (CSRF)
- The public exploration API is strictly read-only and uses stateless HTTP requests.
- Cookie-based CSRF protection is **not required** for stateless Bearer authentication or unauthenticated `GET` endpoints because web browsers do not automatically attach custom `Authorization` headers.
- If cookie-based authentication or browser sessions are ever introduced in future phases, CSRF protection requirements must be formally reassessed.

---

## 11. Database Privilege & Least Privilege Boundary

To ensure defense-in-depth against accidental data corruption or potential injection vulnerabilities:

1. **Public Runtime Database User**:
   - Granted `SELECT` privileges only on canonical domain tables (`characters`, `events`, `locations`, `groups`, `wars`, `war_days`, `formations`, `sources`, `claims`, `evidence`, `relationships`, `family_relationships`, `event_participants`).
   - Prohibited from executing `DROP`, `ALTER`, `TRUNCATE`, `INSERT`, `UPDATE`, or `DELETE` on canonical tables.
2. **Administrative Ingestion Database User**:
   - Granted `INSERT`, `UPDATE`, `DELETE` privileges for data ingestion and curation workflows.
   - Operates strictly within transactional migration and ingestion scripts.
   - *(Exact database user names and role DDL are implementation-level decisions deferred to database setup).*

---

## 12. Administrative Auditing & Traceability

In accordance with **REQ-AUT-03** and **REQ-SEC-02**, administrative operations maintain security traceability:
- All administrative data mutations, dataset seed publications, and batch ingestion runs produce structured audit logs containing:
  - Timestamp of operation.
  - Operation type (e.g., `seed_ingest`, `source_add`, `status_publish`).
  - Affected record counts and entity slugs.
  - Success/failure result status.

---

## 13. Threat & Risk Mitigation Matrix

| Threat / Risk | Target Asset | Architectural Mitigation |
| :--- | :--- | :--- |
| **Public Data Tampering** | Canonical knowledge graph & evidence | Public routes are strictly read-only (`GET` only); runtime DB user lacks `UPDATE/DELETE` privileges. |
| **Generative / Fabricated Content** | Truth integrity (Rule 03) | Administrative ingestion requires schema and provenance gates; no open public submission APIs. |
| **Timing Attacks on Admin API** | `ADMIN_API_KEY` | Validation uses `crypto.timingSafeEqual` constant-time string comparison. |
| **Query Exhaustion / Scraping DoS** | PostgreSQL database | Bounded pagination limits (`limit <= 100`, B5 §7.1), input validation, rate limiting (B9). |
| **Credential Leakage** | Database and Admin secrets | Zero secrets in git; zero secrets in frontend; environment variable injection only. |
| **Stack Trace / Schema Leaks** | Backend implementation | RFC 7807 error formatting; sanitized 500 error envelopes in production. |

---

## 14. Requirement Traceability Matrix

| Security / Auth Requirement | Source Document | Implemented Architectural Mechanism |
| :--- | :--- | :--- |
| **Anonymous Public Read Access** | Context §4.L (REQ-AUT-01); PRD §17 | Section 3.1: 100% anonymous, unauthenticated `GET` access for all exploration lenses. |
| **Exclusion of Public User Accounts / Editing** | Context §4.L (REQ-AUT-02); PRD §17, §19 | Section 3.2: Strict V1 exclusion of registration, comments, user profiles, and public wiki edits. |
| **Secured Administrative Curation** | Context §4.L (REQ-AUT-03); PRD §17 | Section 4.1, Section 12: Protected CLI boundary and administrative audit logging. |
| **Zero Public Data Tampering** | Context §4.Q (REQ-SEC-01); PRD §17 | Section 8.1, Section 11: Mutation verb blocking and read-only database role. |
| **Robust Secret Management** | Context §4.Q (REQ-SEC-02, REQ-SEC-03); Rule 03 | Section 7: Environment injection, zero secrets in git, zero frontend exposure. |
| **Stateless Exploration** | Context §4.K (REQ-STA-01); PRD §12 | Section 3.1: URL-based state resolution without session dependencies. |
| **Deferred User Personalization** | Context §4.K (REQ-STA-03); PRD §18.2 | Section 3.2: User accounts and server-side bookmarks deferred to V2/V3. |
| **Curated Offline Ingestion Pipeline** | Context §4.N (REQ-ING-03); PRD §10 | Section 4.1, Section 11: Ingestion executed via trusted administrative CLI tools. |

---

## 15. Decisions Resolved & Deferred

### Decisions Resolved in Block B7:
1. **RESOLVED B7-01**: Established **100% Anonymous Public Exploration** (no login, registration, or session cookies).
2. **RESOLVED B7-02**: Designated **Trusted CLI Ingestion Tooling** as the required V1 administrative curation boundary.
3. **RESOLVED B7-03**: Established a lean **2-Scope Authorization Model** (`Public (Anonymous)` vs `Administrator / Curator`).
4. **RESOLVED B7-04**: Mandated **Public Mutation Verb Blocking** (`405 Method Not Allowed` on public endpoints).
5. **RESOLVED B7-05**: Defined **Secret Management Invariants** (environment variable injection, zero git commits, zero frontend leakage).
6. **RESOLVED B7-06**: Established **Database Principle of Least Privilege** (read-only runtime user vs administrative curation role).
7. **RESOLVED B7-07**: Formulated **Origin-Restricted CORS** and defined stateless CSRF posture.

### Decisions Deferred to Subsequent Blocks:
1. **Optional Remote Administrative HTTP API Endpoints** → *Deferred / Optional (CLI is primary V1 mechanism)*.
2. **Media Asset CDN Token Signing & Access Controls** → *Deferred to Block B8 (Storage & Media)*.
3. **Specific Rate Limiting Algorithms & IP Throttling Thresholds** → *Deferred to Block B9 (Performance & Caching)*.
4. **Data Ingestion Script Implementations & Pre-Ingestion Validation Gates** → *Deferred to Block B10 (Data Ingestion)*.
5. **Automated Security & Access-Control Verification Tests** → *Deferred to Block B12 (Backend Testing)*.
6. **User Personalization Accounts (Bookmarks / Custom Notes)** → *Deferred to V2/V3 Roadmap*.
