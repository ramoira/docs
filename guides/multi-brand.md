# Multi-brand

For multi-brand setups:

- keep one schema per brand ID
- resolve brand ID at request time (workspace, domain, user selection)
- cache trimmed schemas by (brandId, surface, intent, version)

The schema format is the same for every brand, so one integration serves all of them.
