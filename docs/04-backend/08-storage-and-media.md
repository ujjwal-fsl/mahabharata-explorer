# Storage & Media Architecture (Block B8)

## 1. Purpose

This document establishes the **Storage & Media Architecture** for the **Mahābhārata Explorer** backend. It specifies the asset taxonomy, storage and delivery boundaries, URI reference conventions, image format handling, caching rules, and accessibility safeguards for static visual and cultural assets across the application.

In accordance with project principles:
- **Zero Fabrication (Rule 03, REQ-CHR-03, REQ-WAR-04)**: Historical and cultural visual assets must only be presented when their historical attribution or representation is adequately supported. Interface and purely presentational graphics must not be presented as historical artifacts or factual representations. Missing portraits or unvisualized formations must return `NULL` gracefully; the system must never generate speculative AI portraits or misleading placeholder graphics.
- **Modern First, Traditional Second (Rule 07, REQ-MED-02)**: Visual assets and cultural motifs (lotus, bows, wheels, emblems) must serve clean, semantic purposes rather than decorative clutter.
- **System Reuse & Scope Discipline (Rule 02, Rule 04, REQ-MED-03)**: Heavy 3D assets, video-game simulations, dynamic video rendering pipelines, and user-uploaded media are strictly excluded from V1.

---

## 2. Asset Taxonomy & Supported Media Categories

The V1 media subsystem manages four distinct categories of static visual and vector assets:

```
┌────────────────────────────────────────────────────────────────────────┐
│                          V1 ASSET TAXONOMY                             │
├──────────────────────────────────┬─────────────────────────────────────┤
│ 1. CHARACTER PORTRAITS           │ 2. SYMBOLIC & CULTURAL ICONS (SVG)  │
├──────────────────────────────────┼─────────────────────────────────────┤
│ - Curated historical paintings   │ - Dynastic emblems & faction badges │
│ - Traditional artwork scans      │ - Symbolic motifs (chakra, lotus)   │
│ - Modern web raster formats      │ - Role & navigational glyphs        │
├──────────────────────────────────┼─────────────────────────────────────┤
│ 3. BATTLEFIELD FORMATION ASSETS  │ 4. GEOGRAPHIC MAP ASSETS            │
├──────────────────────────────────┼─────────────────────────────────────┤
│ - Static vector diagrams (SVG)   │ - Static spatial overlays (GeoJSON) │
│ - Visual layout illustrations    │ - Map place marker icons & glyphs   │
└──────────────────────────────────┴─────────────────────────────────────┘
```

### 2.1. Character Portraits
- **Format**: Modern web raster formats (WebP with PNG/JPEG fallbacks).
- **Role**: Curated portraits for major historical/mythological characters (`Character.portrait_url`).
- **Sizing Strategy**: Responsive image variants may be generated where beneficial for layout performance, with exact dimensions and compression parameters deferred to asset pipeline implementation and B12 verification.

### 2.2. Symbolic & Cultural Vector Assets (SVG)
- **Format**: Clean, optimized XML-based SVGs.
- **Role**: Dynastic emblems, symbolic motifs, weapons, and navigational markers.
- **Design Philosophy**: Minimalist, semantic vector shapes adhering to Modern First, Traditional Second (Rule 07).

### 2.3. Battlefield Formation (Vyuha) Visual Assets
- **Format**: Static vector diagrams (SVG) representing curated tactical formations (`Formation.visualization_status`).
- **Canonical Data Boundary**: B8 governs only the storage and delivery of static visual diagram files. Canonical formation structure, tactical geometry, warrior positions, and textual descriptions must be represented according to the canonical domain/data model established by B2 and B3. B8 must not introduce or imply a separate presentation-data storage model for canonical formation information.

### 2.4. Geographic Map Assets
- **Format**: Static spatial files (e.g., GeoJSON boundary overlays) and SVG place markers required by the approved V1 geographic exploration experience.

---

## 3. Asset Ownership & Canonical Entity Decoupling

The architecture enforces strict decoupling between canonical database records and binary media files:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   CANONICAL DATA & MEDIA DECOUPLING                    │
├────────────────────────────────────────────────────────────────────────┤
│  POSTGRESQL DATABASE (Layer 1 Metadata & URIs)                         │
│  - Character.portrait_url = "/assets/portraits/arjuna.webp"           │
│  - Zero binary BLOBs inside PostgreSQL domain tables                   │
├────────────────────────────────────────────────────────────────────────┤
│  MEDIA STORAGE & DELIVERY (Static Server / Object Storage / CDN)       │
│  - High-performance binary file serving                                │
│  - Cache-Control headers & content-hash invalidation                   │
└────────────────────────────────────────────────────────────────────────┘
```

### 3.1. Invariants:
1. **Zero Binary Storage in Database**: PostgreSQL stores URI strings and asset references only; binary BLOBs are never stored in database columns.
2. **Metadata Decoupling**: Visual assets are addressable via standardized relative paths or base-URL parameterized URIs.
3. **Frontend Independence**: The backend API provides canonical asset URIs; the frontend never maintains hard-coded asset mapping tables.

---

## 4. Asset URI Model & Parameterized Origin

### 4.1. Parameterized Base URL Strategy
To allow seamless switching between local development and production hosting environments without database migrations, all media paths utilize an environment-configured asset origin:

$$\text{Asset URL} = \text{ASSET\_BASE\_URL} + \text{Canonical Asset Path}$$

- **Development Environment**: `ASSET_BASE_URL=""` (relative to application host) or `http://localhost:4000`.
- **Production Environment**: `ASSET_BASE_URL="https://<deployed-asset-host>"` (configured asset host / CDN domain).

### 4.2. Deterministic Naming Conventions

| Asset Category | Canonical Path Convention | Example (Illustrative) |
| :--- | :--- | :--- |
| **Character Portrait** | `/assets/portraits/{character-slug}.{ext}` | `/assets/portraits/arjuna.webp` |
| **Formation Diagram** | `/assets/vyuhas/{formation-slug}.svg` | `/assets/vyuhas/chakravyuha.svg` |
| **Faction / Group Emblem** | `/assets/emblems/{group-slug}.svg` | `/assets/emblems/kuru-dynasty.svg` |
| **Cultural Icon** | `/assets/icons/{icon-name}.svg` | `/assets/icons/chakra.svg` |
| **Map Spatial Overlay** | `/assets/maps/{layer-name}.geojson` | `/assets/maps/kuru-region.geojson` |

---

## 5. Storage & Delivery Strategy Evaluation

### 5.1. Evaluation of Storage Approaches

| Approach | Description | Tradeoffs & Evaluation | V1 Architectural Status |
| :--- | :--- | :--- | :--- |
| **Option 1: Local Static File Serving** | Assets served directly from local filesystem directory (`public/assets/`) by application server or reverse proxy. | **Pros**: Zero cloud setup, ideal for local development and self-contained deployments.<br>**Cons**: Couples binary traffic to server processes at large scale. | **SUPPORTED FOR LOCAL DEV & SELF-CONTAINED DEPLOYMENTS** |
| **Option 2: Cloud Object Storage + Edge Delivery** | Assets stored in cloud object storage and distributed via edge cache/CDN. | **Pros**: Offloads binary traffic from application server; global edge caching.<br>**Cons**: Requires cloud infrastructure configuration. | **PREFERRED PRODUCTION DIRECTION** |

### 5.2. Selected Storage & Delivery Decision: Environment-Configurable Asset Hosting
1. **Separation of Concerns**: Binary media is strictly externalized from PostgreSQL.
2. **Environment-Configurable Hosting**: The application supports serving assets from a local static directory in development or self-contained deployments, while production environments may use object storage and/or edge delivery configured through `ASSET_BASE_URL`.
3. **Infrastructure Independence**: Concrete object storage providers and CDN vendor selections remain deferred to infrastructure deployment planning.

---

## 6. Image Optimization, Formats, and SVG Security

### 6.1. Image Formats
- **Raster Formats**: Modern web raster formats (WebP, PNG, JPEG) supported based on visual requirements (e.g., transparency vs photographic fidelity).
- **Encoding Pipeline**: Concrete compression algorithms and optimization tooling are deferred to the ingestion pipeline (Block B10) and performance testing (Block B12).

### 6.2. SVG Handling & Security Safeguards
Because SVGs are XML documents capable of containing executable scripts and external references, the media subsystem enforces strict SVG security standards:
1. **Sanitization**: All SVGs must pass automated sanitization to strip:
   - `<script>` elements.
   - Inline event handlers (`onload`, `onclick`, `onerror`).
   - External entity definitions (`<!ENTITY>`, `<!DOCTYPE>` with external URNs) to prevent XML External Entity (XXE) attacks.
2. **MIME Type Enforcement**: Served with strict `Content-Type: image/svg+xml; charset=utf-8`.
3. **Execution Isolation**: Frontend renders SVGs via `<img>` tags or sandboxed vector elements, preventing script execution within the document context.

---

## 7. Caching, Versioning, and Invalidation Strategy

### 7.1. Conditional Cache-Control Policy
- **Versioned / Content-Hashed Assets**:
  - `Cache-Control: public, max-age=31536000, immutable` (long-lived immutable caching is permitted strictly when URLs contain deterministic content hashes or version query parameters).
- **Unversioned / Mutable Assets**:
  - Assets subject to updates without URL changes must use standard HTTP revalidation headers (`Cache-Control: public, no-cache`, `ETag`, or `stale-while-revalidate`).
  - Detailed numeric latency and byte budgets are formally deferred to Block B9 (Performance) and Block B12 (Verification).

### 7.2. Asset Versioning Strategy
- **Build-Time Content Hashing**: Production asset pipelines apply content hashes (e.g., `/assets/portraits/arjuna.a1b2c3d4.webp`) or version tags to ensure reliable cache busting upon asset updates.

---

## 8. Missing Assets, Provenance & Zero Fabrication

In accordance with Rule 03 (Zero Fabrication) and **REQ-CHR-03**:

```
┌────────────────────────────────────────────────────────────────────────┐
│                 ZERO FABRICATION IN ASSET HANDLING                     │
├────────────────────────────────────────────────────────────────────────┤
│  CURATED ASSET AVAILABLE:                                              │
│  - portrait_url: "/assets/portraits/arjuna.webp"                       │
│  - UI: Renders curated historical painting with attribution metadata   │
├────────────────────────────────────────────────────────────────────────┤
│  UNRESEARCHED / UNVERIFIED ASSET:                                      │
│  - portrait_url: NULL                                                  │
│  - UI: Renders clean semantic monogram / badge (e.g., initials "AR")   │
│  - PROHIBITED: Generating speculative AI portraits or misleading       │
│    generic warrior silhouettes that pretend to be historical facts.    │
└────────────────────────────────────────────────────────────────────────┘
```

### 8.1. Artwork Provenance & Attribution Model
- In strict adherence to [04-evidence-and-provenance.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/04-evidence-and-provenance.md), historical artwork provenance and source attribution must use the **established B4 Evidence/Provenance architecture** (linking artwork claims to canonical `Source` and `Evidence` records).
- Operational media metadata (e.g., MIME type, file byte size, image dimensions) is managed separately at the asset delivery layer and must not be conflated with historical provenance.

---

## 9. Accessibility & Text Alternatives (Rule 06, REQ-ACC-01, REQ-ACC-02)

To satisfy universal accessibility requirements without inventing non-canonical API fields:
1. **Descriptive Text Alternatives**: Visual assets are accompanied by meaningful text descriptions derived from canonical entity fields (`Character.name`, `Location.name`, `Formation.description`) in accordance with [05-api-architecture.md](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/05-api-architecture.md).
2. **Accessible Formation Descriptors (REQ-ACC-02)**: For battlefield vyuhas, `Formation.description` is mandatory and must provide a complete textual breakdown of the formation's structure, warrior positions, and tactical purpose, ensuring users have full access to strategic information regardless of visual diagram rendering.
3. **Contrast & Theme Support**: SVG icons and diagrams must maintain clear contrast ratios and render legibly across dark, light, and high-contrast accessibility themes.

---

## 10. Security, Privacy & CORS Posture

In strict alignment with [07-authentication-and-permissions.md §10.1](file:///d:/CODING/MAHABHARATA%20WEBSITE/docs/04-backend/07-authentication-and-permissions.md#L206):
- **Read-Only Public Distribution**: Media assets are publicly accessible via HTTP `GET` without authentication.
- **Zero Public Uploads**: V1 contains no user-upload endpoints, avatar creators, or binary mutation APIs.
- **Origin-Restricted CORS**: Cross-origin asset requests follow the origin-restricted policy established in B7, allowlisting deployed frontend origins where cross-origin access is required. Credentialed wildcard CORS is strictly prohibited.

---

## 11. Explicit V1 Non-Goals (Scope Discipline)

In accordance with **REQ-MED-03** and PRD §19, the following are strictly excluded from V1:
- 3D meshes, GLTF/GLB models, and WebGL video-game simulation assets.
- Video streaming, audio narration pipelines, or dynamic video transcoding servers.
- User-uploaded avatars, custom profile banners, or public image submissions.
- Generative AI asset synthesis pipelines.

---

## 12. Requirement Traceability Matrix

| Storage & Media Requirement | Source Document | Implemented Architectural Mechanism |
| :--- | :--- | :--- |
| **Optimized Asset References/URIs** | Context §4.M (REQ-MED-01); Blueprint §11 | Section 3, Section 4: Parameterized `ASSET_BASE_URL` with deterministic path conventions. |
| **Lightweight SVG & Cultural Assets** | Context §4.M (REQ-MED-02); Rule 07 | Section 2.2, Section 6.2: Semantic SVG emblems with XML sanitization. |
| **Prohibition of Heavy 3D / Video Assets** | Context §4.M (REQ-MED-03); PRD §19 | Section 11: Explicit V1 non-goals excluding 3D meshes and video pipelines. |
| **Graceful Null / Zero Fabrication for Portraits** | Context §4.F (REQ-CHR-03); Rule 03; PRD §11 | Section 8: `portrait_url = NULL` handling; semantic monograms; zero AI placeholders. |
| **Battlefield Formation Data & Accessibility** | Context §4.H (REQ-WAR-04); Context §4.P (REQ-ACC-02) | Section 2.3, Section 9: `Formation.description` canonical text fallback for all vyuhas. |
| **Accessible Text Labels & Imagery** | Context §4.P (REQ-ACC-01); PRD §15 | Section 9: Text descriptions derived from canonical entity fields; high-contrast support. |
| **Origin-Restricted CORS & Security** | Context §4.Q (REQ-SEC-03); B7 §10.1 | Section 10: Origin-restricted CORS; zero credentials in frontend. |

---

## 13. Decisions Resolved & Deferred

### Decisions Resolved in Block B8:
1. **RESOLVED B8-01**: Established **Environment-Configurable Asset Hosting** (local static files in development/self-contained deployments; object storage / edge delivery in production via `ASSET_BASE_URL`).
2. **RESOLVED B8-02**: Established **Deterministic Asset Path Conventions** for portraits, vyuhas, emblems, and spatial overlays.
3. **RESOLVED B8-03**: Mandated **Zero Binary Database Storage** (database stores URIs only).
4. **RESOLVED B8-04**: Formulated **Zero-Fabrication Missing Asset Policy** (`NULL` portraits rendered as semantic monograms, prohibiting speculative AI avatars).
5. **RESOLVED B8-05**: Defined **SVG Security & Sanitization Rules** (stripping scripts, XXE prevention).
6. **RESOLVED B8-06**: Established **Conditional Immutable Caching Standards** (long-lived caching conditional on content hashing/versioning).
7. **RESOLVED B8-07**: Aligned **Media CORS Policy** strictly with B7 origin-restricted rules.

### Decisions Deferred to Subsequent Blocks:
1. **Concrete Object Storage & CDN Provider Selection** → *Deferred to Deployment / Infrastructure Setup*.
2. **Detailed Numeric Latency & Byte Budgets** → *Deferred to Block B9 (Performance) & Block B12 (Verification)*.
3. **Asset Ingestion & Image Pipeline Scripts** → *Deferred to Block B10 (Data Ingestion)*.
4. **Dynamic Edge Image Resizing & Transcoding Infrastructure** → *Deferred to V2/V3 Scale Optimization*.
