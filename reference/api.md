# API reference

Public reads need no authentication. Pushing, pulling and account calls need an API token (`ramoira login` saves one) or a signed-in browser session. Making a version public is not an API call: only the signed-in owner does it, in the review on ramoira.com. The examples use `corvane`, a fictional brand.

---

## Public reads

Free, crawlable and readable from any origin (`Access-Control-Allow-Origin: *`). Responses may be cached for up to five minutes.

### GET /brands/{slug}/schema.summary.json

The brand's public summary: a [`SPEC.summary.schema.json`](https://github.com/ramoira/brand-schema-spec/blob/main/SPEC.summary.schema.json) 3.x document.

```json
{
  "ramoira": {
    "spec_version": "3.1.0",
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

Ramoira sets the `ramoira` block it serves from its own records, whatever the file says (for example `workflow_state` and `account_owner_verified`). `content_hash` is the hash of the full schema the summary was extracted from. `ratification` is `null` while the schema is a candidate.

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

## Push and pull

### POST /api/brands/{slug}/proposals

Called by `ramoira push`; also what a dashboard upload and a finished browser draft go through. Free. Stores a **private** proposed version for the owner's review. Nothing becomes public.

```
POST https://ramoira.com/api/brands/corvane/proposals
Authorization: Bearer <token>
Content-Type: application/json

{ "schema": { …the full 3.x brand.schema.json… }, "base_hash": "sha256:…" }
```

- `schema` must be a valid 3.x full schema with an up-to-date `content_hash`, `ramoira.brand_id` equal to the slug, and `ramoira.ratification` null (a ratification is recorded by Ramoira, not carried in a file).
- `base_hash` is the `content_hash` of the last version the owner reviewed, as pulled; `null` for a brand's first push.
- Rules marked `"affirmed": true`, examples judged by the brand, and fields marked `affirmed` are accepted only if the reviewed base holds exactly the same ones: only the owner affirms, signed in.
- Pushing to a slug no one holds claims it for your account. A newer push replaces a proposal still waiting for review.

**Response**

```json
{
  "proposal_id": "prp_…",
  "content_hash": "sha256:eaed480d…",
  "review_url": "https://ramoira.com/dashboard/corvane/review",
  "unchanged": false,
  "claimed": true
}
```

`201` for a new proposal; `200` with `unchanged: true` when this exact `content_hash` is already waiting.

**Errors**

| Status | Meaning |
|:---|:---|
| `400` | Not a 3.x full schema (a 2.0.0 file points to the migration guide), `brand_id` does not match the slug, a ratification pointer is present, or the body is not `{schema, base_hash}` |
| `401` | No valid token or session |
| `403` | The slug belongs to another account |
| `409` | `{"code": "stale_base", "reviewed_hash": "…"}`: the owner has reviewed a newer version than `base_hash`; pull and make the change on top of it. Or the slug was renamed; the response names the current one |
| `413` | The schema is over 1 MB |
| `422` | The schema is not valid 3.x (`issues` lists why), or `{"code": "unearned_affirmation", "rules": […], "examples": […], "fields": […]}` names the affirmations the owner never made |
| `429` | The account already holds the maximum number of brands |

### GET /api/brands/{slug}/reviewed

Called by `ramoira pull`. The owner's account only, since it carries the full schema.

```json
{
  "content_hash": "sha256:…",
  "schema": { …the reviewed full schema… },
  "state": "public",
  "reviewed_at": "2026-10-13T10:21:00Z",
  "brief": null
}
```

`state` is `public` when the owner made it public, `reviewed` when they sent it back. `brief` holds the owner's own requests and decisions when they sent it back (`{"requests": [{"path", "text"}], "decisions": [{"kind", "rule_id", "path", "note"}]}`), never suggestions of Ramoira's. `404` when nothing has been reviewed yet.

### POST /api/brands/{slug}/publish

Retired: answers `410`. Publishing is the owner's act in the review, because an agent holds every credential the CLI does.

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
| `DELETE /api/tokens/current` | `ramoira logout`, `ramoira login` | Revokes the token the request is made with: logout, and login retiring the computer's earlier token |

Send tokens as `Authorization: Bearer rmr_…`. A token acts for the account that created it, on every brand that account holds: it can push and pull, never make a version public. `RAMOIRA_TOKEN` in the environment takes precedence over the token `ramoira login` saved.
