# Multi-brand

For multi-brand setups:

- keep one schema per brand ID
- resolve brand ID at request time (workspace, domain, user selection)
- cache trimmed schemas by (brandId, surface, intent, version)

The platform repo is designed as a single-brand SaaS, but the schema contracts are reusable.
