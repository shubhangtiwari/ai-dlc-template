# <Module> Blueprint

domain: data-engineering

Layer rows below match `.ai/references/architectures/data-engineering/architecture.md`. Adapt paths
to the project's actual layout; do not invent paths that do not exist.

## Module Purpose

One paragraph describing what this pipeline owns end-to-end.

## Layer Map

| Layer | Paths | Responsibility |
| --- | --- | --- |
| `sources` | `<path>` | External datasets, APIs, landing definitions |
| `staging` | `<path>` | Raw-ish, append-only, minimal business rules |
| `transform` | `<path>` | Business rules, joins, slowly-changing dimensions |
| `publish` | `<path>` | Tables, views, topics consumed downstream |
| `orchestration` | `<path>` | DAGs, schedules, sensors — wiring only |
| `quality` | `<path>` | Tests, expectations, monitors |
| `config` | `<path>` | Parameters, environment mappings |

## Contracts

Document **publish contracts**: schema, grain, primary keys, partitioning, SLAs, freshness. These
are the source of truth downstream consumers depend on. Cite the schema or model file.

## Workflow or Topology

Describe the DAG: source → staging → transform → publish, plus schedule triggers, sensors,
backfill semantics, and idempotency guarantees.

## Owned State

List published tables/topics, intermediate materializations, watermarks, and retention rules.

## Read-only Paths

List staging or upstream source paths consumers must not bypass `publish` to read.

## Integration Boundaries

List external systems (warehouses, lakes, brokers) and the connectors that isolate them.

## Test Gates

| Gate | Scope |
| --- | --- |
| Unit | Pure transform unit tests, model compilation |
| Integration | Pipeline dry-run on sample data |
| Quality | Column expectations, freshness, volume thresholds |

## Status

Last verified against code: <YYYY-MM-DD>
