# `.ai/` — Project-Agnostic AI Guidance

This directory holds only project-agnostic guidance for AI assistants: how they should code, what
skills they have, and which personas they can adopt. It carries no project-specific facts.

Project-specific facts live in canonical project files generated or curated after fork:

- Architecture style, layer rules, execution model → `docs/ARCHITECTURE.md` and
  `docs/architecture/<domain>.md` (created by the `init-architecture` skill).
- Module-specific contracts, owned state, and read-only paths → module blueprints under
  `docs/blueprints/`.
- Test gates and standard commands → `Makefile`.
- Persona model defaults per IDE → `.ai/models.defaults.toml` (projected by `make init <ide>`).

The generator at `.ai/scripts/ai_init.sh` reads `.ai/` and optionally enriches output from a project
manifest when one is detected at the repo root. Run `make init <ide>` without a manifest to project
governance only; re-run after adding a language/runtime manifest to refresh toolchain facts.
**Do not edit generated IDE files by hand.**

## Files in this directory

- `models.defaults.toml` — model and reasoning defaults for cursor, codex, claude.
- `personas/` — operating modes for AI agents.
  - `architect.md` — plans, work packages, DAG; never edits without instruction.
  - `implementer.md` — WP-scoped edits within layer rules.
  - `reviewer.md` — validates layer purity, spec, and WP `done_when`.
- `skills/` — task playbooks.
  - `init-architecture.md` — analyze repo and write `docs/ARCHITECTURE.md`.
  - `orchestrate-spec.md` — parallel implementer waves from spec work packages.
- `templates/` — spec drafting template; approval brief template (chat-only, for architect).
- `references/architectures/` — reference profiles for the `init-architecture` skill.
- `scripts/` — generator and maintenance scripts (`ai_init.sh`, `ai_update.sh`, `finalize_spec.sh`).

## Model defaults

Edit `.ai/models.defaults.toml` to change which model each IDE uses for each persona, then run
`make init <ide>` to regenerate agent files.

## Responsibility Boundaries

- **Personas define authority:** who plans, who changes files, who reviews, what each role must
  refuse, when work escalates, and which quality bar applies before handoff.
- **Skills define procedure:** reusable task playbooks. Skills do not define authority.
- **Docs define truth:** architecture, specs, ADRs, blueprints, and manifests hold project facts.

<!-- INIT:BEGIN -->
<!-- Everything below this marker is copied verbatim into per-IDE entrypoints by .ai/scripts/ai_init.sh. -->

## Main agent delegation

Portable governance for the main session. Persona bodies live in `.ai/personas/`. Skills live in
`.ai/skills/`. Architecture lives in `docs/ARCHITECTURE.md` and `docs/architecture/` once
`init-architecture` has run; before then there is no governed source tree.

### Clarify before acting

Before classifying or delegating, restate the request in your own terms and surface any ambiguity.
If the goal, scope, target files, success criteria, or tier is genuinely unclear — and the gap
would change which path you take below — ask the user a focused question first. Prefer one or two
sharp questions over a list; do not ask about details you can determine by reading the repo. Skip
clarification only when the request is unambiguous or the user has already answered. This step
runs before the spec gate and before any tool call that changes state.

### Spec gate

The main agent must **not** edit governed paths directly when tier is medium, large, or uncertain,
or when the user asks to implement, fix, add, refactor, or ship without an approved spec. User goal
verbs do **not** waive the spec gate.

### Before any state-changing tool call

1. **Classify** the request: trivial / small / medium / large / uncertain (see
   `docs/spec/README.md`).
2. **Choose the path:**
   - **Configuration / tooling / Q&A** (`Makefile` when not a contract, `.ai/`, IDE
     config, dependencies, exploration) → main agent may proceed.
   - **Governed code or contracts** (project source tree, `tests/`, `docs/blueprints/`,
     `docs/adr/`, `docs/spec/`, `docs/ARCHITECTURE.md`, `docs/architecture/`) → follow the persona
     chain below. The main session **must not** patch governed source or tests — only
     `implementer` (or a delegated WP implementer) applies those edits.
3. **When in doubt, delegate.**

### Persona chain

| Step | Persona | When |
| --- | --- | --- |
| 1 | `architect` | Planning, tiering, drafting or updating `docs/spec/<epoch>-<slug>.md` |
| 2 | `implementer` | After spec `status: approved`, or after trivial/small intent confirmed with user |
| 3 | `reviewer` | Finished diff ready for merge; input is diff + governing spec (or inline intent for trivial/small) |

**Medium / large / uncertain:** delegate to `architect`. The architect writes the spec to disk and
posts an **approval brief** in chat (see `.ai/templates/approval-brief.md`). Return the spec path
and brief summary to the user, then **stop** until they explicitly approve. Do not call
`implementer` in the same turn. Do not paste the full spec into chat — the brief is the human gate;
the spec file is the machine gate.

**Trivial / small:** No spec file. Main agent states intent inline (short summary: what, which
files, expected outcome). User confirms. Delegate **`implementer`** — do not apply governed edits
from the main session. Optional: invoke `architect` only when the user asks for planning or tier is
unclear.

**Blueprint sanity (all tiers):** Every `implementer` run ends with a blueprint check. If the
change touches anything blueprints document (contracts, owned state, read-only paths, integrations,
topology, layer map, test gates), update `docs/blueprints/<module>.md` in the same branch. If not,
leave blueprints unchanged — do not add noise. Medium/large work still lists deltas in the spec;
trivial/small rely on this check instead of a spec file.

### Workflow

```text
Medium/large:
  architect → spec (file) + approval brief (chat)
       ↓ user approves
  main agent → implementer (per WP / orchestrate-spec) → reviewer → merge

Trivial/small:
  main agent → inline intent → user confirms → implementer (+ blueprint sanity) → reviewer
```

**Human gate:** approval brief in chat for medium/large (~250–500 words); short inline intent for
trivial/small. **Machine gate:** spec file on disk when tier requires it.

### Escalation

- **Minor discovery** → spec `Implementation notes` with date; continue.
- **Material change** → stop; architect amends spec.

### Work packages and parallel execution

For medium/large specs with `work_packages` in frontmatter:

1. Read waves from the spec's **Parallel execution plan**.
2. Execute **one wave at a time**. All work packages in wave *N* must finish before wave *N+1*.
3. Spawn **one implementer per work package** in the current wave (cap concurrency at 3–6; queue
   the rest in the same wave).
4. Each implementer receives: spec path, WP id, allowed `files`, `gates`, `done_when`, and
   `domain`.
5. **One writer per path** per active wave — refuse overlapping file ownership.
6. **Wave 0** freezes shared contracts: models/DTOs, integration interfaces, shared pure utils
   (2+ consumers), shared test fixtures. Not feature-complete services.
7. **WP-INT** (final wave): wire-up, integration tests, blueprint sync, cross-cutting barrels —
   `depends_on: [*]`.
8. Apply skill `orchestrate-spec` for the step-by-step playbook.

### Human vs machine artifacts

| Audience | Artifact | Location |
| --- | --- | --- |
| Human (approval) | Approval brief | Chat only — architect synthesis |
| Agents (execution) | Spec | `docs/spec/<epoch>-<slug>.md` on disk |

The main agent may restate the brief for clarity but must not regenerate a full plan or duplicate
the spec body in chat.

### Review

Before merge, run `reviewer` against the diff. Medium/large: diff + governing spec.
Trivial/small: diff + inline intent from chat; verify blueprint sanity when module contracts may
have changed.

## Hard Rules

1. Layer purity follows `docs/architecture/` for the spec's `domain` once initialized.
2. Execute through `Makefile` targets only.
3. Cross-cutting decisions require an ADR in `docs/adr/` before implementation.
4. Medium and large changes require an approved spec before code is written.
5. The main agent must enforce the spec gate before any state-changing action.

## IDE hints (optional)

These are invocation hints only. Governance truth is this file and `.ai/personas/`.

| IDE | Delegate persona |
| --- | --- |
| Cursor | Task tool with `subagent_type` matching persona name |
| Codex | Spawn named custom agent from `.codex/agents/` |
| Claude Code | Invoke subagent from `.claude/agents/` |
