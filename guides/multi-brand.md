# Multi-brand

For multi-brand setups:

- keep one schema per brand, identified by `ramoira.brand_id` (its slug);
- resolve the brand at request time (workspace, domain, user selection);
- cache what you load by brand, surface and `content_hash`, so a change to the schema invalidates the cache.

The schema format is the same for every brand, so one integration serves all of them. One account can hold several brands.
