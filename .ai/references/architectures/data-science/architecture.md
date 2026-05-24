# Data Science (reference)

Experiment → reproducible artifact → promotion. Research code must not leak into production paths.
The `init-architecture` skill adapts this profile into the project's generated architecture docs.

## Layers

| Layer | Role |
| --- | --- |
| **datasets** | Curated, versioned inputs |
| **features** | Feature definitions |
| **research** | Notebooks, exploratory analysis — not production |
| **train** | Training scripts, configs, pipelines |
| **evaluate** | Metrics, holdouts, bias checks |
| **package** | Model bundle, inference code, contracts |
| **deploy** | Registration, endpoints, batch scoring |

## Rules

1. **Research** must not be imported by **train** or **package** — promote code explicitly.
2. **Evaluate** gates before **package** promotion.
3. Reproducibility: pinned data revision, seed, config, environment (ADR when cross-team).
4. No train/serve skew: feature definitions in **features** are the source of truth for serving.
5. Eval sets must be frozen and referenced in the governing spec.

## Test gates

| Gate | Scope |
| --- | --- |
| Unit | Pure feature/transform functions |
| Integration | Training pipeline on sample data |
| Evals | Metric thresholds, bias checks, holdout performance |

## Reviewer checklist

1. Spec cites dataset revision and eval split.
2. No leakage from future data in features or labels.
3. Evaluate ran before package promotion.
4. Blueprint reflects model contract and serving boundary.

## Examples (informational)

Layer-to-path mappings the init skill may adapt — not part of the layer law.

| Layer | Example paths |
| --- | --- |
| datasets | `data/`, versioned data refs, feature store |
| features | `features/` |
| research | `notebooks/`, `research/` |
| train | `train/`, `ml/pipelines/` |
| evaluate | `evaluate/`, `tests/ml/` |
| package | `ml/package/`, `serving/` |
| deploy | ties to `infra/` and software `integrations` |
