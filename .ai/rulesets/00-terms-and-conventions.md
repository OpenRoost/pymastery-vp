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

### Project Administrator

Identifier: `project-administrator`

Responsible for:

- Repository structure and organization
- CI/CD pipelines configuration
- Static site generator setup and maintenance
- Access control documentation
- ADR documentation

### Content Editor

Identifier: `content-editor`

Responsible for:

- Grammar and spelling corrections
- Formatting consistency
- Documentation updates and accuracy
- Metadata validation

### DevOps

Identifier: `devops`

Responsible for:

- CI/CD pipeline implementation and maintenance
- Build automation and optimization
- Deployment workflows
- Infrastructure-as-code configurations
- Monitoring and alerting setup

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
