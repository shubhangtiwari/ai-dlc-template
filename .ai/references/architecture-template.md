# Architecture Template (custom / option C)

Use when defining a new profile. Replace placeholders; save domain statutes under
`docs/architecture/<domain>.md` and link from `docs/ARCHITECTURE.md`.

---

primary_domain: <software|data-engineering|data-science|mixed>

## AI Agent Initialization Protocol

1. Read `.ai/` for personas, skills, delegation.
2. Read `docs/ARCHITECTURE.md` and applicable `docs/architecture/*.md`.
3. Execute via Makefile only.
4. Treat `infra/` as explicit scope.

## Directory map

```text
/
├── .ai/
├── docs/
│   ├── ARCHITECTURE.md
│   ├── architecture/
│   ├── adr/
│   ├── blueprints/
│   └── spec/
├── <source-root>/          # describe actual layout
└── tests/
```

## Domain profiles

| Domain | Document | Path prefixes |
| --- | --- | --- |
| <domain> | `docs/architecture/<domain>.md` | `<paths>` |

## Layer laws

<!-- Pull from closest reference in .ai/references/architectures/ and adapt -->

## Testing standards

| Gate | Command | Scope |
| --- | --- | --- |
| lint | `make lint` | |
| unit | `make test-unit` | |
| integration | `make test-integration` | |
| evals | `make evals` | |

## Artifact taxonomy

ADR → `docs/adr/<epoch>-<slug>.md`. Blueprint → `docs/blueprints/<module>.md`. Spec →
`docs/spec/<epoch>-<slug>.md`. Epoch = `date +%s` at creation; slug = kebab-case name.

## Initialization record

- Date:
- Option: C (define new)
- Reference ids merged:
- Analyst summary:
