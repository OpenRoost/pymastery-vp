[//]: # (docs/ADR-002-ssg-replacement.md)

# ADR-002: Static Site Generator Replacement

[//]: # (@formatter:off)
<!-- document status badges -->
[draft]: https://img.shields.io/badge/document_status-draft-orange.svg
[accepted]: https://img.shields.io/badge/document_status-accepted-green.svg
[rejected]: https://img.shields.io/badge/document_status-rejected-red.svg
[deprecated]: https://img.shields.io/badge/document_status-deprecated-lightgrey.svg
[final]: https://img.shields.io/badge/document_status-final-blue.svg
[//]: # (@formatter:on)
![status][accepted]

<details>
<summary>Document Changelog</summary>

[//]: # (order by version number descending)

| ver. | Date       | Author            | Changes description                      |
|------|------------|-------------------|------------------------------------------|
| 1.0  | 2026-01-27 | Serhii Horodilov  | Accepted                                 |
| 0.7  | 2026-01-27 | Claude Sonnet 4.5 | Revise Implementation to high-level only |
| 0.6  | 2026-01-26 | Serhii Horodilov  | Fix typos                                |
| 0.5  | 2026-01-26 | Claude Sonnet 4.5 | Complete final draft with decision       |
| 0.4  | 2026-01-25 | Serhii Horodilov  | Fix typos                                |
| 0.3  | 2026-01-25 | Claude Sonnet 4.5 | Add ADR-004 cross-reference              |
| 0.2  | 2026-01-25 | Serhii Horodilov  | Fix typos                                |
| 0.1  | 2026-01-25 | Claude Sonnet 4.5 | Initial draft                            |

</details>

> [!NOTE]
> **Scope of PR #238**: This ADR is part of a comprehensive documentation effort that introduces interdependent
> Architecture Decision Records:
> - **ADR-002** (this document): Static Site Generator Replacement (Sphinx → MkDocs)
> - **ADR-003**: Repository File Structure (locale-based content organization)
> - **ADR-004**: Presentation Framework Handling (removing impress.js submodule)
> - **ADR-006**: Presentation Content Repository Separation
>
> These ADRs address related aspects of the project's documentation infrastructure and should be reviewed together
> to understand the full scope of changes being proposed.

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

> [!IMPORTANT]
> This decision is interdependent with ADR-003 (Repository File Structure). The chosen SSG may have specific
> expectations about directory structure, and the migration effort will vary based on how legacy content is handled.

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
- Lowers the learning curve for contributors

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

**Adopt MkDocs with Material Theme (Option 1)**

**Rationale:**

`MkDocs` with `Material` theme provides the optimal balance of simplicity, contributor accessibility, and feature
completeness for this educational project. This decision is driven by the following factors:

1. **Primary Goal Achievement**: The main motivation for SSG replacement is reducing contributor friction. `MkDocs`
   delivers native Markdown support, eliminating the reST learning curve that motivated this change.

2. **Documentation-Specific Design**: Unlike general-purpose SSGs (Hugo, Jekyll), `MkDocs` is purpose-built for
   documentation projects, providing sensible defaults and conventions that align with course content structure.

3. **Maintainability**: Single `mkdocs.yml` configuration file and straightforward plugin system minimize long-term
   maintenance burden compared to Sphinx's complexity or Docusaurus's React ecosystem.

4. **Localization Support**: While i18n is plugin-based rather than first-class, the `mkdocs-static-i18n` plugin
   provides mature multilingual support sufficient for English/Ukrainian workflow. The plugin's approach is
   well-documented and actively maintained.

5. **GitHub Pages Integration**: Simple deployment workflow via `mkdocs gh-deploy` command or GitHub Actions, with no
   complex build pipeline required.

6. **Material Theme Quality**: Provides a professional, modern UI out-of-box with excellent mobile responsiveness and
   built-in search, dark mode, and navigation features.

7. **Community and Ecosystem**: Large, active community with extensive plugin ecosystem and excellent documentation.
   Widely adopted for educational and technical documentation projects.

8. **Migration Feasibility**: Pandoc-based conversion from reST to Markdown is well-established. Migration complexity
   is manageable and can be automated with scripting.

9. **Feature Coverage**: Supports all required documentation features (admonitions, code blocks with syntax
   highlighting, tabbed content, content tabs for code examples, custom CSS/JS when needed).

10. **Build Performance**: Fast build times suitable for iterative content development.

**Why Not Other Options:**

- **Docusaurus**: Excellent but overkill for linear course content. React dependency and MDX complexity exceed project
  needs. First-class i18n doesn't justify the heavier ecosystem.
- **Hugo**: Extremely fast but requires Go template knowledge for customization. General-purpose nature means more
  manual setup. Speed advantage is not critical for this project size.
- **Jekyll**: Native GitHub Pages support appealing but i18n support less mature. Slower builds and declining community
  momentum compared to alternatives.
- **Keep Sphinx**: Does not address the core motivation (reST complexity and contributor friction). Keeping Sphinx
  means accepting current pain points indefinitely.

**Coordination with ADR-003:**

MkDocs has flexible content organization and doesn't impose strict directory requirements. The decision in ADR-003 to
organize legacy content as `content/ru/` is fully compatible. MkDocs typically uses `docs/` as content root, but this
is configurable via `docs_dir` setting in `mkdocs.yml`.

## Consequences

### Positive

- **Improved Contributor Experience**: Markdown authoring significantly lowers a barrier to entry. Contributors
  familiar with GitHub wikis, README files, or other Markdown contexts can immediately write content.
- **Reduced Maintenance Burden**: Simpler configuration and fewer moving parts compared to Sphinx. Single `mkdocs.yml`
  file vs. Sphinx's `conf.py` and multiple configuration contexts.
- **Modern UI Out-of-Box**: Material theme provides a professional appearance with no customization required. Built-in
  features (search, navigation, dark mode) work immediately.
- **Faster Iteration**: Simpler build process and live-reload development server enable rapid content development and
  preview.
- **Better Alignment with Project Needs**: Documentation-focused tool matches educational content structure better than
  general-purpose SSGs or complex frameworks.
- **Strong Localization Support**: `mkdocs-static-i18n` plugin provides mature multilingual workflow adequate for
  English/Ukrainian content.
- **GitHub Pages Compatibility**: Simple deployment with `mkdocs gh-deploy` or GitHub Actions integration.
- **Active Community**: Large ecosystem of plugins and themes. Regular updates and strong documentation.
- **Markdown Portability**: Content in Markdown is more portable and reusable than proprietary reST syntax.

### Negative

- **Migration Effort**: One-time cost to convert existing Sphinx/reST content to MkDocs/Markdown format.
- **Feature Gaps**: Some advanced Sphinx features (sphinx-autodoc for Python API documentation, complex
  cross-referencing) have no direct equivalent. However, course content does not currently use these features.
- **Plugin Dependency for i18n**: Localization is plugin-based rather than a core feature.
  This adds one dependency, but the plugin is mature and actively maintained.
- **Learning Curve for Customization**: Team members familiar with Sphinx/Python will need to learn MkDocs conventions
  and YAML configuration.
- **Retraining Contributors**: Existing contributors familiar with reST will need to adapt to Markdown (though this is
  generally easier than the reverse).
- **Theme Lock-In**: Material theme is excellent, but switching themes in the future would require adjustment.
  However, the theme ecosystem is large and theme switching is possible if needed.

### Neutral

- **Different Plugin Ecosystem**: MkDocs plugins differ from Sphinx extensions. Some Sphinx extensions have MkDocs
  equivalents, others don't. Need to evaluate plugins case-by-case.
- **Build System Change**: CI/CD pipeline will need updates for new build command and Python dependencies (MkDocs,
  plugins).
- **Configuration Format Change**: YAML-based configuration vs. Python-based Sphinx conf.py. Neither is inherently
  better; just different.
- **Documentation Rewrite**: Contributor documentation will need updates to reflect Markdown authoring and MkDocs
  workflows.

## Implementation

### Objectives

The migration from Sphinx to MkDocs with a Material theme requires achieving the following primary goals:

1. **Content Format Conversion**: Convert all reStructuredText (`.rst`) files to Markdown (`.md`) format while
   preserving content structure, code blocks, admonitions, internal links, and asset references
2. **SSG Replacement**: Replace Sphinx with MkDocs and Material theme, including all build configuration and tooling
3. **Multilingual Support**: Establish English/Ukrainian localization using `mkdocs-static-i18n` plugin with file-based
   i18n approach
4. **Theme Configuration**: Configure Material theme for professional appearance, search functionality, dark mode, and
   mobile responsiveness
5. **Deployment Automation**: Set up GitHub Actions for automated builds and GitHub Pages deployment
6. **Documentation Updates**: Update all project documentation (README, contributor guides) to reflect Markdown
   authoring and MkDocs workflows
7. **Cleanup**: Remove Sphinx-specific artifacts (`conf.py`, `_build/`, `_static/`, `_templates/`, etc.) while
   preserving in git history

### Critical Constraints

The executor must respect these non-negotiable requirements:

1. **Git History Preservation**: All file operations must preserve git history. Content files should be moved/renamed
   in ways that maintain commit history (e.g., avoid delete and create patterns)
2. **Content Integrity**: Content meaning and structure must remain unchanged during format conversion. No content loss
   or corruption is acceptable
3. **Working Branch Protection**: All work must be done on a feature branch (`feature/mkdocs-migration` or similar).
   The `main` branch remains stable and functional throughout migration
4. **Coordination with ADR-003**: Repository structure established by ADR-003 must be respected. Content location is
   `content/en/` (post-ADR-003) or `/src/` (pre-ADR-003), configurable via MkDocs `docs_dir`
5. **Ukrainian Translations**: Current gettext-based Ukrainian translations in `content/_locales/` (or
   `/src/_locales/`) must be migrated to MkDocs file-based i18n approach with `content/uk/` locale structure
6. **Build Success**: Site must build successfully with MkDocs and deploy to GitHub Pages without errors
7. **Feature Parity**: All current documentation features (code highlighting, admonitions, cross-references, search)
   must work in MkDocs
8. **No Downtime**: Existing Sphinx site remains functional until MkDocs migration is complete and approved

### Success Criteria

Migration is considered successful when all the following are met:

- [ ] All course content converted to Markdown and built without errors
- [ ] English and Ukrainian locales are both functional with working language switcher
- [ ] GitHub Pages deployment working automatically via GitHub Actions
- [ ] All images and assets display correctly
- [ ] Navigation and search work in both languages
- [ ] Site is responsive and works on mobile devices
- [ ] Contributor documentation updated with a Markdown authoring guide
- [ ] Sphinx artifacts removed from the repository (preserved in git history)
- [ ] Build time is acceptable (< 1 minute for a full build)
- [ ] Project Owner approves final site quality

### Known Risks

The executor should be aware of and prepare for these risks:

| Risk                                    | Impact | Mitigation Strategy                                   |
|-----------------------------------------|--------|-------------------------------------------------------|
| Pandoc conversion artifacts             | Medium | Test conversions early; manual review required        |
| i18n plugin limitations                 | Medium | Test plugin thoroughly; verify fallback plans         |
| Complex Sphinx features not convertible | Low    | Current content audit shows minimal advanced features |
| Timeline overrun                        | Medium | Iterative approach; prioritize core functionality     |
| Deployment pipeline issues              | Medium | Test early; maintain manual deployment option         |

### Rollback Plan

If critical issues emerge during migration:

1. Migration branch protects `main` — no changes to production until validated
2. Sphinx environment preserved in git history — reversion possible at any time
3. Migration can be paused while maintaining Sphinx in the short term
4. No merge to `main` until full validation complete

## Related

- [ADR-003][ADR-003]: Repository File Structure (interdependent - structure decision affects migration effort)
- [ADR-004][ADR-004]: Presentation Framework Handling (may affect asset requirements and SSG choice)
- [ADR-001][ADR-001]: AI Guidelines Structure and Administration Framework

[//]: # (@formatter:off)
<!-- ADR references -->
[ADR-001]: ./ADR-001-ai-guidelines-structure.md
[ADR-002]: ./ADR-002-ssg-replacement.md
[ADR-003]: ./ADR-003-repo-file-structure.md
[ADR-004]: ./ADR-004-presentation-framework.md
[//]: # (@formatter:on)
