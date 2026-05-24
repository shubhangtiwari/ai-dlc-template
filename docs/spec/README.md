# Specs

A spec is a forward-looking blueprint for a single feature, fix, or refactor. One spec maps to one
PR or MR. Medium and large changes are approved before implementation begins.

## Human vs agent audiences

Specs under `docs/spec/` are optimized for **agents**: YAML frontmatter, work packages, dependency
graphs, and precise file lists. Humans should **not** need to read the full spec to approve work.

At draft stage, the **architect** posts an **approval brief** in chat (see
`.ai/templates/approval-brief.md`): medium detail covering why, what changes, and which files. After
approval, implementer and reviewer use the **spec file only** as the source of truth.

## When to write a spec

- **Trivial** changes: typo, rename, comment, or single-line fix. No spec.
- **Small** changes: single-file fix, single test, or dependency bump. No spec file; the main agent
  states intent inline, the user confirms, then **`implementer`** applies the change.
- **Medium / Large** changes: more than one file, contract or topology changes, integration
  additions, or cross-cutting behavior. A full spec file is required and approved before code is
  written.

No spec does **not** skip governance: trivial and small work still delegate **`implementer`** for
governed paths. The implementer runs a **blueprint sanity** check and updates `docs/blueprints/` when
the change affects contracts, owned state, integrations, topology, or read-only paths.

## Frontmatter fields

| Field | Required | Meaning |
| --- | --- | --- |
| `id` | yes | `spec-<epoch>-<slug>` (matches filename) |
| `status` | yes | `draft` → `approved` → `implemented` → `stale` |
| `owner` | yes | Spec author: `git config user.name`, else OS login (`whoami`) |
| `tier` | yes | trivial / small / medium / large |
| `domain` | yes | `software`, `data-engineering`, `data-science`, or `mixed` |
| `work_packages` | medium/large | Delegable units with waves and dependencies |

Optional sidecar: `docs/spec/<epoch>-<slug>.work-packages.yaml` when YAML frontmatter is too large.

## Work packages

Medium and large specs should define `work_packages` in frontmatter plus markdown sections for the
dependency tree and parallel execution plan. See `.ai/templates/spec.md`.

- **Wave 0** freezes shared contracts and shared pure helpers.
- **One writer per path** per active wave.
- Apply skill `orchestrate-spec` for parallel implementer delegation.

## Lifecycle

```text
draft → approved → implemented → stale
```

1. Draft from `.ai/templates/spec.md`.
2. Fill all required sections. `Open questions` must end empty.
3. Architect posts approval brief in chat; user approves and spec `status` becomes `approved`.
4. Implementer applies the spec (per work package when defined), including blueprint deltas. For
   trivial/small work without a spec, implementer still runs blueprint sanity in the same PR.
5. Open a PR or MR; add an entry to `docs/spec/.in-flight.yaml` (manual or automation).
6. After merge, run `make finalize-spec` or rely on CI — removes the in-flight entry and sets
   `status: implemented`.

## Naming

Use `docs/spec/<epoch>-<slug>.md`:

| Token | Meaning | Example |
| --- | --- | --- |
| **epoch** | Unix time in seconds at creation (`date +%s`) | `1748092800` |
| **slug** | Short kebab-case feature name | `add-oauth-login` |

Set the epoch once when creating the file. Do not reuse another branch’s epoch. The slug is the
readable name only (2–6 words, kebab-case).

Frontmatter `id:` must be `spec-<epoch>-<slug>` (e.g. `spec-1748092800-add-oauth-login`).

## Owner

Set `owner:` when drafting — do not leave a placeholder. Resolution order:

1. `git config user.name` (trimmed)
2. If empty, `whoami` (OS login name)

Override in frontmatter before approval if needed.

## Artifact Taxonomy

| Artifact | Level | Path |
| --- | --- | --- |
| PRD | Product motivation | `docs/PRD_*.md` |
| ADR | Architecture decision | `docs/adr/<epoch>-*.md` |
| Blueprint | Module-level living design | `docs/blueprints/<module>.md` |
| Spec | Feature change | `docs/spec/<epoch>-*.md` |

Specs may span modules. They update blueprints; they do not replace ADRs.

## In-flight Tracker

`docs/spec/.in-flight.yaml` lists specs whose PRs or MRs are open but not merged.

```yaml
in-flight:
  - spec: docs/spec/1748092800-add-oauth-login.md
    branch: feature/add-oauth-login
```

- **Add** an entry when a spec-backed PR is opened (manual or future automation).
- **Remove** on merge via `make finalize-spec` or a post-merge CI job (see `init-architecture`).

Projects may maintain the file manually or wire CI to call `make finalize-spec` with `--branch` set
to the merged PR head ref.
