# Tiered Service (reference)

Application code organized by horizontal technical tiers (entry → orchestration → data). Circular dependencies between
layers are forbidden. The `init-architecture` skill copies this profile (with paths adapted) into
the project's generated `docs/ARCHITECTURE.md` and `docs/architecture/<domain>.md`.

## Layers

### `api` — entry points

- Controllers, handlers, routes, request parsing, validation, response shaping.
- No business logic.
- Delegates behavior to `services`.

### `services` — orchestration and business logic

- Workflows, application services, coordination, agentic behavior.
- May call `models`, `integrations`, `utils`, and read `resources`.
- Must not import from `api`.

### `models` — data shapes

- Entities, schemas, interfaces, DTOs, validation models.
- No I/O, external service calls, or orchestration.
- Independent of `api` and `services`.

### `integrations` — external boundaries

- Third-party SDKs, database clients, message brokers, external HTTP services.
- Wrap vendor APIs behind project-shaped interfaces.
- Services depend on these wrappers, not raw external APIs.

### `utils` — pure helpers

- Stateless helpers. No I/O, no SDK calls, no global state.

### `resources` — static runtime data

- Config defaults, prompt templates, golden datasets, SQL files, schemas.
- Data only. No executable business logic.

## Layer-root convention

Layer rules are universal. The physical layer root follows the language's idiomatic package
layout so native tooling works without remapping.

## Cross-cutting rules

1. Tests partitioned by gate (unit / integration / evals) — directory or naming, per language norms.
2. `resources` is a peer of code layers at the layer root.
3. Language ceremony above the layer root contains declarations only, not business logic.
4. Monorepos apply the rule per package; the monorepo root is structural, not architectural.

## Test gates

| Gate | Scope |
| --- | --- |
| Unit | Pure logic, isolated from databases and external services |
| Integration | End-to-end flow across layers |
| Evals | Probabilistic quality (LLM, retrieval, agents) |

## Reviewer checklist

1. Layer purity: no business logic in `api`; no `api` imports in `services`; no I/O in `models`.
2. Test placement: unit vs integration vs evals.
3. Naming: file name matches primary export; no stale compatibility suffixes unless spec requires.
4. Imports: project namespace; no removed namespaces.
5. Spec compliance for medium/large changes.

## Examples (informational)

Concrete layouts the init skill may adapt — not part of the layer law.

| Language | Layer root | Example layout |
| --- | --- | --- |
| Python | `src/<package_name>/` | `src/<package_name>/api/app.py` |
| TypeScript / Node | `src/` | `src/api/server.ts` |
| Go | module root or `internal/` | `api/handler.go`, `services/workflow.go` |
| Java / Kotlin | `src/main/<lang>/<pkg>/` | `src/main/kotlin/<pkg>/api/Server.kt` |
| Rust | `src/` | `src/api/mod.rs`, `src/services/mod.rs` |
