# Publishing

Publishing makes a summary schema available for agents and systems that need a stable public reference.

## CLI

```bash
ramoira publish
```

Publishing typically:

- validates the full schema
- produces/updates the summary schema
- pushes the summary schema to the publication endpoint

## Validation

```bash
ramoira validate
```

For the canonical platform policy checks, see the preflight system:

- `platform/lib/brand-schema/index.ts`
