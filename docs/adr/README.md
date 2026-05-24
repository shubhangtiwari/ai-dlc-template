# Architecture Decision Records

Use ADRs for cross-cutting decisions that affect architecture, infrastructure, data ownership,
layering, public interfaces, or long-lived operational constraints.

## Naming

Use `docs/adr/<epoch>-<slug>.md`:

| Token | Meaning | Example |
| --- | --- | --- |
| **epoch** | Unix time in seconds at creation (`date +%s`) | `1748092800` |
| **slug** | Short kebab-case decision name | `use-postgres-for-events` |

Set the epoch once when creating the file (`date +%s`). The slug stays in the filename only; the
heading uses the epoch for uniqueness.

## Template

```markdown
# ADR-<epoch>: <Decision title>

- **Status:** Proposed | Accepted | Superseded
- **Date:** YYYY-MM-DD
- **Deciders:** <names or roles>

## Context

What forces, constraints, or problems required a decision?

## Decision

What did we decide?

## Consequences

What becomes easier, harder, required, or forbidden because of this?

## Alternatives Considered

What did we reject, and why?
```

## Deciders (optional)

When listing **Deciders**, default to the same identity as spec `owner`:

1. `git config user.name` (trimmed)
2. If empty, `whoami`
