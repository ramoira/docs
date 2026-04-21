# API reference (Platform)

This document describes the API endpoints used by the **platform** implementation.

Canonical source docs:

- `platform/docs/brand-schema-generation.md`

## Schema generation

- `POST /api/create-brand/schema`
  - Synthesizes a draft schema from:
    - brand profile
    - archetype seed
    - divergence interview answers

## Governance endpoints (example)

Schemas can declare governance webhooks (examples in the platform schemas):

- `POST /api/brand-schema/violations` (violation webhook)
- `POST /api/brand-schema/overrides` (override audit log)

## Notes

Exact request/response contracts are implementation-specific.
Treat the TypeScript schema contracts in `platform/lib/validation` as the authoritative runtime input shapes.
