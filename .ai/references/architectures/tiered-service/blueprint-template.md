# <Module> Blueprint

domain: tiered-service

Layer rows below match `.ai/references/architectures/tiered-service/architecture.md`. Adapt paths
to the project's actual layout; do not invent paths that do not exist.

## Module Purpose

One paragraph describing what this module owns.

## Layer Map

| Layer | Paths | Responsibility |
| --- | --- | --- |
| `api` | `<path>` | Entry points and request/response shaping |
| `services` | `<path>` | Business logic and orchestration |
| `models` | `<path>` | Data shapes and contracts |
| `integrations` | `<path>` | External dependency wrappers |
| `utils` | `<path>` | Pure helpers |
| `resources` | `<path>` | Static runtime data |

## Contracts

Document public inputs, outputs, state updates, events, or persistence rules. Cite source files or
schemas that are the executable source of truth.

## Workflow or Topology

Describe the main request flow or lifecycle. Include edge cases that affect contract boundaries.

## Owned State

List durable state, transient state, caches, and ownership rules.

## Read-only Paths

List paths that ordinary feature work must not modify without explicit scope.

## Integration Boundaries

List external systems and the wrappers in `integrations` that isolate them.

## Test Gates

Map behavior to unit, integration, and eval gates.

## Status

Last verified against code: <YYYY-MM-DD>
