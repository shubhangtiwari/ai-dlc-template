# Polyglot Monorepo (reference)

Multiple packages or top-level roots with different domains under one repository. The
`init-architecture` skill adapts this profile into the project's generated architecture docs.

## Rules

1. Each package gets its own blueprint under `docs/blueprints/<module>.md` with a `domain` field.
2. Apply the layer laws from the matching domain profile **per package**, not repo-wide generic
   rules.
3. The monorepo root is structural (CI, shared `Makefile`, governance). It is not an architectural
   layer.
4. Cross-package dependencies must go through **published contracts** (APIs, tables, model
   bundles), not through staging or research paths.
5. Path prefixes map to domains in the project's generated `docs/ARCHITECTURE.md`.

## Work packages

When a spec spans packages, assign one writer per path per wave. Wave 0 freezes shared contracts
(models, publish schemas, feature definitions) before parallel implementers run.

## Reviewer checklist

1. Changes stay within the declared package and domain.
2. No illegal cross-imports between domains.
3. Blueprint updated for each touched module.

## Examples (informational)

Path-prefix layouts the init skill may adapt — not part of the layer law.

- `apps/<name>/` → application/software domain
- `packages/<name>/` → shared library
- `pipelines/<name>/` → data-engineering domain
- `ml/<name>/` → data-science domain
