# AI DLC Template

MIT License - Copyright (c) 2026 [Shubhang Tiwari](mailto:shubh.bitsmith@gmail.com).

This repository is a forkable governance template for AI-assisted repositories. It contains
project-neutral operating rules, personas, skills, documentation templates, and initialization
tooling for common coding assistants.

It intentionally does not include project code, dependency manifests, architecture profiles, quality
gate definitions, or placeholder scaffolding. Fork the template, run the initializer, then document
the real project structure as it takes shape.

## Contents

- `.ai/` - portable AI personas, skills, templates, delegation, scripts, and optional model defaults.
- `docs/spec/` - spec-first workflow documentation and tracker.
- `docs/adr/` - architecture decision record guidance.
- `docs/blueprints/` - module blueprint guidance and template.
- `.ai/scripts/ai_init.sh` - shell generator for assistant-specific files.
- `.ai/scripts/ai_update.sh` - syncs `.ai/` from the upstream template repo.
- `.ai/scripts/finalize_spec.sh` - post-merge spec finalization helper.

## Initialize

Governance projection comes entirely from `.ai/` and `docs/`:

```sh
make init codex
make init claude
make init cursor
make init copilot
make init windsurf
make init all
```

This writes assistant-specific files such as `AGENTS.md`, `.codex/agents/*.toml`,
`.agents/skills/*/SKILL.md`, `CLAUDE.md`, or `.cursor/rules/` (gitignored and not committed in
this template). Cursor output includes an always-applied AI DLC discovery rule so Composer, Claude,
Gemini, and other Cursor Agent models can find generated personas and skills. Regenerate after
changing `.ai/`; do not edit generated IDE files by hand.

Generated assistant files do not infer project, dependency, or toolchain facts. Forked projects
should document those facts explicitly in their own architecture, blueprint, ADR, and spec files when
they become relevant.

## Customize Architecture

Use the `init-architecture` skill in `.ai/skills/` after forking. It analyzes the repo and creates
`docs/ARCHITECTURE.md` from real project evidence. Module contracts live in `docs/blueprints/` as
the project grows.
