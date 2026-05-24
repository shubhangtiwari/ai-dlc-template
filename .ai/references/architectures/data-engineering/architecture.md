# Data Engineering (reference)

Data flow and contract surfaces. Orchestration wires jobs; transforms live in dedicated layers.
The `init-architecture` skill adapts this profile into the project's generated architecture docs.

## Layers

| Layer | Role |
| --- | --- |
| **sources** | External datasets, APIs, landing definitions |
| **staging** | Raw-ish, append-only, minimal business rules |
| **transform** | Business rules, joins, slowly-changing dimensions |
| **publish** | Tables, views, topics consumed downstream |
| **orchestration** | DAGs, schedules, sensors — wiring only |
| **quality** | Tests, expectations, monitors |
| **config** | Parameters, environment mappings — no transform logic |

## Rules

1. Orchestration must not embed business transforms (same principle as "no logic in api").
2. **Publish contracts** (schema, grain, keys) are the "models" of data engineering — freeze in
   wave 0 for parallel work packages.
3. Downstream consumers depend only on **publish** artifacts, not staging internals.
4. Pipelines must be idempotent and backfill-safe unless an ADR documents otherwise.
5. PII handling and retention follow ADRs and blueprint owned-state sections.

## Test gates

| Gate | Scope |
| --- | --- |
| Compile / unit | Pure transform unit tests, model compilation |
| Integration | Pipeline dry-run on sample data |
| Quality | Column expectations, freshness, volume thresholds |

## Reviewer checklist

1. Grain and keys documented in spec and blueprint.
2. Staging not referenced from application code.
3. Quality tests on critical columns.
4. Publish schema matches spec blueprint deltas.

## Examples (informational)

Layer-to-tool mappings the init skill may adapt — not part of the layer law.

| Layer | Example paths / tools |
| --- | --- |
| sources | `ingest/`, `sources/` |
| staging | transform-tool staging models, ingest jobs |
| transform | mart models, batch transform jobs |
| publish | `publish/`, exposures, output tables |
| orchestration | DAG/scheduler definitions |
| quality | data-quality test suites, expectation libraries |
| config | `conf/`, `resources/` |
