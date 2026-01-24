# AI Agent Entry Point

This repository uses AI agents for infrastructure, tooling, and documentation tasks.
**Course content is human-authored and protected.**

## Quick Start

1. Read `.ai/config.yaml` for project context and file index
2. Review rulesets in `.ai/rulesets/` before taking action
3. Follow the guidelines — when uncertain, ask

## Project Summary

| Attribute        | Value                                                   |
|------------------|---------------------------------------------------------|
| Name             | Python for Web-Developers                               |
| ID               | `pymastery-vp`                                          |
| Audience         | Complete beginners                                      |
| Goal             | Prepare learners for junior/trainee developer positions |
| Stack            | Python, Django, Django REST Framework                   |
| Primary language | English (en-US)                                         |
| Translations     | Ukrainian (uk-UA)                                       |

## Critical Rules

1. **Content is immutable** — Do NOT modify course content in `src/` unless explicitly instructed
2. **You support, not author** — Organize, advise, build tooling. Do not write curriculum.
3. **Confirm destructive actions** — File changes, deletions, or structural modifications require explicit approval

## Configuration Index

```
.ai/
├── config.yaml                      # Project context, roles, file index
└── rulesets/
    ├── 00-terms-and-conventions.md  # Roles, scopes, naming conventions
    ├── 01-general-guidelines.md     # Core principles, commit standards
    ├── 02-grammar-and-style.md      # American English, formatting rules
    └── 03-documentation.md          # Documentation types, ADR process
```

## Allowed Autonomous Actions

- Grammar and spelling corrections (per style guide)
- Formatting fixes (whitespace, line endings)
- Broken link identification and reporting
- Build configuration updates
- File organization per established conventions

## Actions Requiring Approval

- Any modification to `content/` content
- Structural changes to repository
- Tool/technology selection (requires ADR)
- Process modifications

## Documentation

- ADRs and docs live in `/docs/` (flat structure)
- Templates in `/templates/`
- Use `ADR-xxx-slug.md` naming for Architecture Decision Records

## Git Identity

See [`config.yaml`](.ai/config.yaml) for full git configuration.
