# API Reference

The Ramoira Brand API has two categories of endpoints: public (no auth required) and CLI-facing (token required).

---

## Public endpoints

These require no authentication. They serve the published summary schema for any brand that has run `ramoira publish`.

### GET /brands/[slug]/schema.summary.json

Returns the public summary schema for a brand.

```
GET https://ramoira.com/brands/little-rituals/schema.summary.json
```

**Response** — `200 OK`, `application/json`

A valid `SPEC.summary.schema.json` document. See [schema-fields.md](schema-fields.md) for the field list.

```json
{
  "meta": {
    "brandId": "little-rituals",
    "brandName": "Little Rituals",
    "schemaVersion": "2.0.0",
    "schemaType": "summary",
    "canonicalURL": "https://ramoira.com/brands/little-rituals/schema.summary.json",
    "certified": false,
    "confidence": 0
  },
  "identity": { ... },
  "narrative": { ... },
  "voice": { ... }
}
```

**Errors**

| Status | Meaning |
|:---|:---|
| `404` | Brand not found or not published |

---

### GET /brands/[slug]/status

Returns the current publication state for a brand.

```
GET https://ramoira.com/brands/little-rituals/status
```

**Response** — `200 OK`, `application/json`

```json
{
  "workflowState": "published",
  "certified": false,
  "confidence": 0,
  "canonicalUrl": "https://ramoira.com/brands/little-rituals/schema.summary.json"
}
```

| Field | Type | Notes |
|:---|:---|:---|
| `workflowState` | string | `"draft"` \| `"published"` |
| `certified` | boolean | `true` only for Studio tier |
| `confidence` | number | 0–1, set by certification analysis |
| `canonicalUrl` | string \| null | Null if not yet published |

**Errors**

| Status | Meaning |
|:---|:---|
| `404` | Brand not found |

---

## CLI-facing endpoints

These require a Bearer token. Get a token at [ramoira.com/tokens](https://ramoira.com/tokens) or via `ramoira login`.

### POST /api/brands/[slug]/publish

Publishes a full brand schema. Called by `ramoira publish`.

```
POST https://ramoira.com/api/brands/little-rituals/publish
Authorization: Bearer <token>
Content-Type: application/json
```

**Request body**

```json
{
  "schema": { ...full brand.schema.json content... }
}
```

The full schema must pass local validation (`SPEC.schema.json`) before sending. `ramoira publish` validates locally first and will not call this endpoint if validation fails.

**Response** — `200 OK`

```json
{
  "versionId": "ver_abc123",
  "workflowState": "published",
  "canonicalUrl": "https://ramoira.com/brands/little-rituals/schema.summary.json",
  "certified": false,
  "confidence": 0
}
```

**Response** — `207 Multi-Status`

Returned when the schema was published but summary extraction encountered a non-fatal issue. The schema is published; the summary may be incomplete.

**Errors**

| Status | Meaning |
|:---|:---|
| `401` | Token missing or invalid |
| `403` | Token does not have access to this brand slug |
| `400` | Schema failed structural validation |

---

## Authentication

All CLI-facing endpoints use Bearer token authentication:

```
Authorization: Bearer <token>
```

Tokens are scoped to a brand owner account. A token grants access to all brand slugs owned by that account.

**Getting a token:**
1. Create an account at [ramoira.com](https://ramoira.com)
2. Visit [ramoira.com/tokens](https://ramoira.com/tokens) to generate a token
3. Run `ramoira login` to save it locally, or set `RAMOIRA_TOKEN` in your environment

Environment variable takes precedence over the saved config file.
