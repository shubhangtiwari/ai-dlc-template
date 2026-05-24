# <Module> Blueprint

domain: data-science

Layer rows below match `.ai/references/architectures/data-science/architecture.md`. Adapt paths to
the project's actual layout; do not invent paths that do not exist.

## Module Purpose

One paragraph describing the model or experiment family this module owns.

## Layer Map

| Layer | Paths | Responsibility |
| --- | --- | --- |
| `datasets` | `<path>` | Curated, versioned inputs |
| `features` | `<path>` | Feature definitions (train + serve source of truth) |
| `research` | `<path>` | Notebooks, exploratory analysis — not production |
| `train` | `<path>` | Training scripts, configs, pipelines |
| `evaluate` | `<path>` | Metrics, holdouts, bias checks |
| `package` | `<path>` | Model bundle, inference code, contracts |
| `deploy` | `<path>` | Registration, endpoints, batch scoring |

## Contracts

Document the model bundle contract: input schema, output schema, feature dependencies, version
scheme, and the eval gates that must pass before promotion.

## Workflow or Topology

Describe the experiment → train → evaluate → package → deploy flow, including reproducibility
inputs (data revision, seed, config, environment) and promotion gates.

## Owned State

List datasets, feature stores, model registry entries, eval result stores, and retention rules.

## Read-only Paths

List `research/` paths and any frozen eval sets that must not be modified mid-experiment.

## Integration Boundaries

List feature stores, model registries, serving systems, and the wrappers that isolate them.

## Test Gates

| Gate | Scope |
| --- | --- |
| Unit | Pure feature/transform logic |
| Integration | Train + eval pipeline on sample data |
| Evals | Metrics on frozen holdout, bias and fairness checks |

## Status

Last verified against code: <YYYY-MM-DD>
