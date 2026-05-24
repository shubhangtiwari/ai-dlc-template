# <Package> Blueprint

domain: <tiered-service|data-engineering|data-science>

In a polyglot monorepo, each package gets its own blueprint and uses the layer map from its
**own domain**, not a repo-wide template. Pick the matching domain template and copy its layer
map here:

- Tiered service → `.ai/references/architectures/tiered-service/blueprint-template.md`
- Data engineering → `.ai/references/architectures/data-engineering/blueprint-template.md`
- Data science → `.ai/references/architectures/data-science/blueprint-template.md`

The sections below are the monorepo-specific concerns every package blueprint must cover, in
addition to the per-domain layer map.

## Package Purpose

One paragraph describing what this package owns and which domain profile applies.

## Package Boundary

| Concern | Value |
| --- | --- |
| Package root | `<path>` |
| Domain profile | `<id>` |
| Owns published contracts | `<list>` |
| Consumes published contracts | `<list>` |

## Layer Map

<!-- Paste from the matching per-domain blueprint template and adapt paths -->

## Cross-package Contracts

List APIs, tables, topics, or model bundles this package publishes for other packages, and the
ones it consumes. Cross-package dependencies must go through these published contracts only.

## Owned State

List durable state owned by this package.

## Read-only Paths

List paths in this package that ordinary feature work must not modify without explicit scope.
Other packages are read-only by default — call them out only if a cross-package read is allowed.

## Integration Boundaries

List external systems this package wraps and any sibling packages it integrates with via their
published contracts.

## Test Gates

Map behavior to the gates defined by the package's domain profile.

## Status

Last verified against code: <YYYY-MM-DD>
