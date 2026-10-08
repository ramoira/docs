# API reference

Public reads need no authentication. Publishing and account calls need an API token (`ramoira login` saves one) or a signed-in browser session. The examples use `corvane`, a fictional brand.

---

## Public reads

Free, crawlable and readable from any origin (`Access-Control-Allow-Origin: *`). Responses may be cached for up to five minutes.

### GET /brands/{slug}/schema.summary.json

The brand's public summary: a [`SPEC.summary.schema.json`](https://github.com/ramoira/brand-schema-spec/blob/main/SPEC.summary.schema.json) 3.0.0 document.

```json
{
  "ramoira": {
    "spec_version": "3.0.0",
    "schema_type": "summary",
    "brand_id": "corvane",
    "schema_version": "1.0.0",
    "content_hash": "sha256:eaed480d…",
    "workflow_state": "published",
    "ratification": null,
    "account_owner_verified": false,
    "canonical_url": "https://ramoira.com/brands/corvane/schema.summary.json",
    "summary_opt_in": []
  },
  "rules": [ … ],
  "identity": { … },
  "narrative": { … },
  "voice": { … }
}
```

Ramoira sets the `ramoira` block it serves. `content_hash` is the hash of the full schema the summary was extracted from. `ratification` is `null` while the schema is a candidate.

### GET /brands/{slug}/status

What is true for the brand. Facts, never a score.

```json
{
  "brand": "corvane",
  "workflowState": "published",
  "canonicalUrl": "https://ramoira.com/brands/corvane/schema.summary.json",
  "published": { "at": "2026-10-07T13:51:19Z", "schema_version": "1.0.0", "content_hash": "sha256:eaed480d…" },
  "ratified": null,
  "conformance": { "active": false, "coverage": null, "last_checked": null },
  "faithfulness": null,
  "density": null,
  "account_owner_verified": false
}
```

| Field | Meaning |
|:---|:---|
| `workflowState` | `published`, or `not published` for a claimed slug with nothing published yet |
| `published` | The current published version, or `null` |
| `ratified` | The ratification (hash, date, role), or `null` while the schema is a candidate |
| `conformance` | Whether independent conformance checking is active, at what coverage (`sampled` or `complete`), and when it last ran |
| `faithfulness` | `null` until independent faithfulness attestation exists |
| `density` | A diagnostic, reported separately when it exists; never a quality score |
| `account_owner_verified` | The account has proven control of the brand's domain. Never read as ratification. |

Both reads are also served under `/api/brands/{slug}/…`.

**Errors and redirects**

| Status | Meaning |
|:---|:---|
| `404` | No brand at this slug, or nothing published yet |
| `308` | The brand renamed its slug; `Location` has the new one. A slug is never reassigned to another brand. |

---

## Publishing

### POST /api/brands/{slug}/publish

Called by `ramoira publish`. Free.

```
POST https://ramoira.com/api/brands/corvane/publish
Authorization: Bearer <token>
Content-Type: application/json

{ "schema": { …the full 3.0.0 brand.schema.json… } }
```

The schema must be a valid 3.0.0 full schema, its `ramoira.brand_id` must equal the slug, and `ramoira.ratification` must be `null` (a ratification is recorded by Ramoira, not carried in a file). Publishing to a slug no one holds claims it for your account. Ramoira keeps the full schema privately, bound to its `content_hash`, and serves the summary.

**Response**

```json
{
  "versionId": "ver_…",
  "workflowState": "published",
  "canonicalUrl": "https://ramoira.com/brands/corvane/schema.summary.json",
  "contentHash": "sha256:eaed480d…",
  "schemaVersion": "1.0.0",
  "unchanged": false,
  "claimed": true
}
```

`201` when a new version is published; `200` with `unchanged: true` when this exact `content_hash` is already current.

**Errors**

| Status | Meaning |
|:---|:---|
| `400` | Not a 3.0.0 full schema (a 2.0.0 file points to the migration guide), `brand_id` does not match, or a ratification pointer is present |
| `401` | No valid token or session |
| `403` | The slug belongs to another account |
| `409` | The slug was renamed; the response names the current one |
| `413` | The schema is over 1 MB |
| `422` | The schema is not valid 3.0.0; `issues` lists why |
| `429` | The account already holds the maximum number of brands |

---

## Accounts and tokens

| Call | Used by | What it does |
|:---|:---|:---|
| `POST /api/auth/device/code` | `ramoira login` | Starts a device login: returns a code to show and a URL to open |
| `POST /api/auth/device/token` | `ramoira login` | Polled with `{"device_code": "…"}`: `202` while waiting, `200 {"token": "…"}` once approved, `400` when expired or used |
| `GET /api/auth/me` | `ramoira whoami` | The account's email and brand slugs |
| `GET /api/tokens` | dashboard | Live tokens (labels and dates, never the values) |
| `POST /api/tokens` | `ramoira create-token` | `{"label": "ci-deploy"}` → `{"token": "rmr_…"}`, shown once |
| `DELETE /api/tokens/{id}` | dashboard | Revokes a token |

Send tokens as `Authorization: Bearer rmr_…`. A token acts for the account that created it, on every brand that account holds. `RAMOIRA_TOKEN` in the environment takes precedence over the token `ramoira login` saved.
