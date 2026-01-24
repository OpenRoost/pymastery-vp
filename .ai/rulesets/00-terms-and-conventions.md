[//]: # (.ai/rulesets/00-terms-and-conventions.md)

# Terms and Conventions

## Project Identity

Name
: Python for Web-Developers

Identifier
: pymastery-vp

Repositories
: [origin](https://github.com/OpenRoost/pymastery-vp.git) and
[upstream](https://github.com/PonomaryovVladyslav/PythonCourses.git)

## Roles

| Role                    | Identifier               | Primary Scope                  |
|-------------------------|--------------------------|--------------------------------|
| Project Owner           | `project-owner`          | Strategic oversight            |
| Project Manager         | `project-manager`        | Planning & coordination        |
| Project Administrator   | `project-administrator`  | Infrastructure & tooling       |
| Content Editor          | `content-editor`         | Pre-release content validation |
| DevOps Engineer         | `devops-engineer`        | CI/CD & deployment automation  |

For comprehensive role definitions, boundaries, responsibilities, and escalation paths,
see [`docs/ROLES.md`](../../docs/ROLES.md).

## Scopes

### In Scope

| Area            | Description                                  |
|-----------------|----------------------------------------------|
| Grammar checks  | Spelling, punctuation, style consistency     |
| SSG setup       | Configuration, themes, build optimization    |
| File structure  | Directory organization, naming conventions   |
| CI/CD pipelines | Build, test, deploy automation               |
| Documentation   | README, contributing guides, AI rules, docs/ |

### Out of Scope

| Area                 | Condition                               |
|----------------------|-----------------------------------------|
| Content modification | Allowed ONLY with explicit user request |
| Curriculum changes   | Requires human approval                 |
| Learning objectives  | Requires human approval                 |

## Content Formats

`*.markdown`, `*.md`
: Markdown; primary content format

`.rst`
: reStructuredText; legacy format

## Languages

| Primary | Multilingual support | Style guide                      |
|---------|----------------------|----------------------------------|
| English | Yes                  | Agent-determined, ADR documented |

## File Naming Conventions

| Group   | Format               | Example                     | Description                                       |
|---------|----------------------|-----------------------------|---------------------------------------------------|
| Ruleset | `xx-ruleset-name.md` | 00-terms-and-conventions.md | Two-digit ordering, lowercase, hyphen-separated   |
| ADR     | `ADR-xxx-slug.md`    | ADR-0001-example-name.md    | Three-digit ordering, lowercase, hyphen-separated |
| Content | `xx-title-slug.md`   | 01-basics.md                | Two-digit ordering, lowercase, hyphen-separated   |
| Partial | `_*`                 | _changelog.md               | Leading underscore, lowercase, hyphen-separated   |
