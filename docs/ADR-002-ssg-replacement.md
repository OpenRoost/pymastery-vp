# ADR-002: Static Site Generator Replacement

[//]: # (@formatter:off)
<!-- document status badges -->
[draft]: https://img.shields.io/badge/document_status-draft-orange.svg
[accepted]: https://img.shields.io/badge/document_status-accepted-green.svg
[deprecated]: https://img.shields.io/badge/document_status-deprecated-lightgrey.svg
[rejected]: https://img.shields.io/badge/document_status-rejected-red.svg
[final]: https://img.shields.io/badge/document_status-final-blue.svg
[//]: # (@formatter:on)
![status][draft]

<details>
<summary>Document Changelog</summary>

[//]: # (order by version number descending)

| ver. | Date       | Author                                    | Changes description |
|------|------------|-------------------------------------------|---------------------|
| 0.2  | 2026-01-25 | Serhii Horodilov                          | Fix typos           |
| 0.1  | 2026-01-25 | Claude Sonnet 4.5 <noreply@anthropic.com> | Initial draft       |

</details>

## Context

The project currently uses Sphinx as its static site generator. While Sphinx is powerful and widely used for technical
documentation, several issues have emerged that motivate reconsidering this choice:

**Current Pain Points:**

- **Authoring complexity**: Sphinx requires reStructuredText (reST), which adds a learning curve for contributors
  compared to Markdown
- **Perceived overkill**: Sphinx's extensive feature set (designed for large technical documentation projects like
  Python's official docs) may exceed the project's actual needs
- **Contributor friction**: New contributors familiar with Markdown face an additional barrier when contributing
  content
- **Maintenance overhead**: reST syntax requires more careful formatting and is less forgiving than Markdown

**Current Setup:**

- Static site generator: Sphinx
- Content format: reStructuredText (.rst)
- Deployment target: GitHub Pages
- Localization: English (primary), Ukrainian (translation)
- Content location: `/src/` directory

**Project Requirements:**

- Must support English/Ukrainian localization workflow
- Must be compatible with GitHub Pages deployment
- Should prioritize Markdown authoring for contributor accessibility
- Should maintain professional documentation quality
- Should support code highlighting, admonitions, and other common documentation features

**Note:** This decision is interdependent with ADR-003 (Repository File Structure). The chosen SSG may have specific
expectations about directory structure, and the migration effort will vary based on how legacy content is handled.

## Decision Drivers

- **Author experience**: Ease of writing and maintaining course content (Markdown vs. reST)
- **Contributor accessibility**: Lower barrier to entry for community contributions
- **Localization workflow**: Support for English/Ukrainian content with a maintainable translation process
- **Migration effort**: Feasibility of converting existing Sphinx/reST content to a new format
- **Feature completeness**: Support for documentation features needed by course content (admonitions, code blocks,
  cross-references, etc.)
- **GitHub Pages compatibility**: Deployment workflow simplicity
- **Community familiarity**: Common adoption in educational/documentation projects
- **Maintenance burden**: Long-term effort required to maintain the SSG setup
- **Build performance**: Site generation speed for rapid iteration during content development

## Considered Options

### Option 1: MkDocs with Material Theme

**Description**: MkDocs is a static site generator designed specifically for project documentation, with native
Markdown support and an extensive plugin ecosystem. The Material theme provides a modern UI and includes i18n support
via plugin.

**Pros**:

- Native Markdown support (CommonMark + extensions)
- Excellent documentation-focused design
- Active development and a strong community
- Material theme provides professional appearance out-of-box
- i18n plugin available for localization
- Simple configuration (single `mkdocs.yml` file)
- Good GitHub Pages integration
- Fast build times
- Lower a learning curve for contributors

**Cons**:

- i18n workflow is plugin-based (not a first-class feature)
- Less powerful than Sphinx for complex cross-referencing scenarios
- Localization workflow may require additional tooling/conventions
- Some advanced Sphinx features may not have direct equivalents

### Option 2: Docusaurus

**Description**: Facebook's modern documentation framework built on React, with first-class i18n support and optimized
for technical documentation sites.

**Pros**:

- First-class internationalization support (built-in, not plugin)
- Modern, polished UI with excellent UX
- MDX support (Markdown + React components) for interactive content
- Strong versioning support (useful if the course evolves)
- Very active development and community
- Excellent documentation and ecosystem
- Built-in search functionality
- Optimized performance (SPA architecture)

**Cons**:

- More complex than simpler SSGs (React-based)
- Requires Node.js ecosystem (additional dependency)
- Heavier build process
- Maybe overkill for linear course content
- Steeper learning curve for content authors who want to use React features

### Option 3: Hugo

**Description**: Fast, flexible static site generator written in Go, with strong i18n support and extensive theming
ecosystem.

**Pros**:

- Extremely fast build times (critical for large sites)
- First-class i18n support (built into Hugo core)
- Single binary distribution (no runtime dependencies)
- Powerful templating system
- Large theme ecosystem
- Excellent documentation
- Mature and stable

**Cons**:

- Template syntax (Go templates) has a steeper learning curve
- More configuration required than MkDocs
- Primarily a general-purpose SSG (not documentation-specific)
- Content organization may require more manual setup
- Less opinionated than documentation-focused tools

### Option 4: Jekyll

**Description**: GitHub's native SSG with automatic GitHub Pages support and a simple Markdown-based workflow.

**Pros**:

- Native GitHub Pages integration (zero-config deployment)
- Ruby-based (familiar to some developers)
- Mature and stable
- Simple setup for basic sites
- Plugin ecosystem available

**Cons**:

- i18n support is plugin-based and less mature
- Slower build times compared to Hugo
- Less active development than newer alternatives
- Configuration can become complex for larger sites
- Not specifically designed for documentation

### Option 5: Keep Sphinx

**Description**: Maintain the current Sphinx setup, potentially with improvements to workflow or configuration.

**Pros**:

- No migration effort required
- Most powerful for technical documentation
- Excellent cross-referencing capabilities
- Very mature i18n/l10n support
- Widely used in the Python community

**Cons**:

- Retains all current pain points (reST, complexity)
- Higher contributor barrier
- Steeper learning curve
- More maintenance overhead
- Does not address core motivation for change

## Decision

**[DECISION PENDING - DRAFT STATUS]**

This decision should be finalized after:

1. ADR-003 (Repository File Structure) is resolved or decided in parallel
2. Evaluation of migration effort for each candidate (depends on structure decision)
3. Testing localization workflow with top candidates
4. Assessment of feature requirements against course content needs

## Consequences

**[TO BE DETERMINED AFTER DECISION]**

General consequence categories to consider:

### Positive

- Improved contributor experience with Markdown authoring
- Potentially simpler maintenance and configuration
- Better alignment with project needs vs. capabilities

### Negative

- Migration effort (extent depends on chosen option and structure decision)
- Potential feature gaps compared to Sphinx
- Retraining for existing contributors familiar with reST

### Neutral

- Different tooling ecosystems and workflows
- Documentation and learning materials specific to chosen SSG

## Implementation

**[TO BE DEFINED AFTER DECISION]**

Implementation will likely include:

1. Migration strategy for existing content (depends on ADR-003)
2. Localization workflow setup
3. Theme configuration and customization
4. CI/CD pipeline updates for a new build process
5. Documentation updates for contributors
6. Deployment configuration for GitHub Pages

## Related

- ADR-003: Repository File Structure (interdependent - structure decision affects migration effort)
- ADR-001: AI Guidelines Structure and Administration Framework
