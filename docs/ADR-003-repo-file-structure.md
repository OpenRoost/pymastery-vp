[//]: # (docs/ADR-003-repo-file-structure.md)

# ADR-003: Repository File Structure

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

| ver. | Date       | Author                                    | Changes description                               |
|------|------------|-------------------------------------------|---------------------------------------------------|
| 1.0  | 2026-01-27 | Serhii Horodilov                          | Accepted                                          |
| 0.11 | 2026-01-27 | Claude Sonnet 4.5 <noreply@anthropic.com> | Revise Implementation to high-level only          |
| 0.10 | 2026-01-27 | Serhii Horodilov                          | Fix typos, final draft                            |
| 0.9  | 2026-01-27 | Claude Sonnet 4.5 <noreply@anthropic.com> | Expand scope to comprehensive repo reorganization |
| 0.8  | 2026-01-26 | Serhii Horodilov                          | Fix typos                                         |
| 0.7  | 2026-01-26 | Claude Sonnet 4.5 <noreply@anthropic.com> | Complete final draft with decision                |
| 0.6  | 2026-01-26 | Serhii Horodilov                          | Replace `locales/**` with `content/**`            |
| 0.5  | 2026-01-26 | Serhii Horodilov                          | Add `upstream` repository link                    |
| 0.4  | 2026-01-25 | Serhii Horodilov                          | Fix typos                                         |
| 0.3  | 2026-01-25 | Claude Sonnet 4.5 <noreply@anthropic.com> | Reframe as ru localization, add pictures/         |
| 0.2  | 2026-01-25 | Serhii Horodilov                          | Fix typos                                         |
| 0.1  | 2026-01-25 | Claude Sonnet 4.5 <noreply@anthropic.com> | Initial draft                                     |

</details>

> [!NOTE]
> **Scope of PR #238**: This ADR is part of a comprehensive documentation effort that introduces three interdependent
> Architecture Decision Records:
> - **ADR-002**: Static Site Generator Replacement (Sphinx → MkDocs)
> - **ADR-003** (this document): Repository File Structure (locale-based content organization)
> - **ADR-004**: Presentation Framework Handling (Remove assets/impress.js)
>
> These ADRs address related aspects of the project's documentation infrastructure and should be reviewed together
> to understand the full scope of changes being proposed.

## Context

The repository currently has an inconsistent structure with content scattered across multiple locations, lacking clear
locale-based organization for a multilingual project. This creates organizational, discoverability, and maintainability
issues.

**Current State:**

- **Active content location**: `/src/` directory contains English course content (`.rst` format)
- **Active content format**: reStructuredText (`.rst`) - no format changes in this ADR
- **Sphinx artifacts**: `/src/_locales/` (gettext Ukrainian translations), `/src/_static/` (Sphinx JS files)
- **Legacy content at root**: All `*.md` files in root (except `AGENTS.md`, `CLAUDE.md`) are legacy Russian lesson
  files
- **Legacy assets scattered**: `/pictures/lesson01/`, `/pictures/lesson02/`, `/pictures/lesson08/` tied to legacy
  lessons
- **Active global assets**: `/assets/` directory contains assets used by current course
- **Infrastructure files**: `.ai/`, `docs/`, `templates/`, `mindmaps/` at root
- **Configuration files**: Various dotfiles; active files `AGENTS.md`, `CLAUDE.md` at root
- **Build artifacts location**: Undefined (Sphinx uses `_build/` but may change with SSG)

**Problems:**

1. **No locale-based organization**: Content lacks clear `en`/`uk`/`ru` structure for a multilingual project
2. **Inconsistent content location**: Active content in `/src/`, legacy at root — no unified approach
3. **Repository clutter**: Root directory contains 45+ legacy lesson files mixed with infrastructure
4. **Contributor confusion**: Where should new content go? Where is active English content?
5. **Scattered legacy content**: Russian files at root, images in `/pictures/lessonXX/` — not colocated
6. **Sphinx artifacts mixed with content**: `_locales/` and `_static/` in content directory
7. **Unclear content root**: `/src/` doesn't signal "this is the content directory"
8. **Historical preservation**: Legacy Russian content lacks a clear "preserved but not maintained" status
9. **Onboarding friction**: New contributors must understand historical context to navigate

**Historical Context:**

The legacy Russian-language files are from the original course (upstream source). The project was translated to English
with Ukrainian support, and content moved to `/src/`. Legacy files remained at root during translation, never organized
as a complete historical artifact.

> [!IMPORTANT]
> This decision is interdependent with ADR-002 (SSG Replacement). This ADR addresses **structure only** (where files
> live, directory organization). ADR-002 addresses **format** (reST → Markdown conversion).
> The two MUST BE coordinated but are distinct concerns.

## Decision Drivers

- **Contributor clarity**: Make it obvious where all content lives (active and legacy)
- **Locale-based organization**: Establish clear structure for multilingual content (`en`, `uk`, `ru`)
- **Historical preservation**: Maintain complete legacy Russian content as historical artifact
- **Content root naming**: Clear directory name signaling "this contains course content"
- **Maintainability**: Reduce a cognitive load when navigating the repository
- **Discoverability**: Clear separation between active and legacy content
- **Content-asset colocation**: Keep related content and assets together within locales
- **Build system compatibility**: Structure should support current and future SSG expectations (MkDocs)
- **Git history preservation**: Maintain git history through move operations
- **Clean separation of concerns**: Sphinx artifacts separate from content
- **Respectful treatment**: Honor original Russian course as project foundation

## Considered Options

### Option 1: Comprehensive Locale-Based Structure with `content/` Root

**Description**: Reorganize the entire repository to establish `content/` as the content root with locale-based
subdirectories (`en/`, `ru/`). Move active content from `/src/` to `content/en/`, organize legacy content into
`content/ru/`, handle Sphinx artifacts appropriately, and keep global assets at repository root.

**Target Structure:**

```
content/                # Content root (renamed from src/)
  _locales/            # Temporary - Sphinx gettext Ukrainian .po files
                       # (Kept until ADR-002 migration replaces with MkDocs i18n)
  en/                  # Active English content
    *.rst              # Content in reST format (unchanged - conversion in ADR-002)
    subdirs/           # Lesson structure preserved
  ru/                  # Legacy Russian content (preserved, not maintained)
    lessons/           # Root lesson*.md, module*.md → here
      lesson*.md
      module*.md
    pictures/          # /pictures/lessonXX/ → here
      lesson01/
      lesson02/
      lesson08/
    README.md          # Explains legacy status

assets/                # Global project assets (stays at root)
  ...                  # Active assets used by course

(Root level remains: .ai/, docs/, templates/, mindmaps/, config files, AGENTS.md, CLAUDE.md)
```

**Note on `_static/`**: The `/src/_static/` directory (containing Sphinx-specific JS files) will be **removed** as it
is unnecessary for MkDocs (per ADR-002). Preserved in git history if needed.

**Note on `uk/`**: No `content/uk/` placeholder created now. Ukrainian content structure will be established during
ADR-002 when setting up MkDocs file-based i18n.

**Pros**:

- **Clear content root**: `content/` clearly signals "this is where course content lives"
- **Locale-based organization**: Establishes proper multilingual structure (`en`, `ru`, future `uk`)
- **Consistent pattern**: All locales use same organizational approach
- **Respectful preservation**: Legacy Russian course honored as a complete historical artifact
- **Clean root directory**: Infrastructure-only (no content files scattered)
- **Content-asset colocation**: Related content and images together within locales
- **Git history maintained**: Uses `git mv` for all operations
- **Future-compatible**: If `ru` support revived, content ready
- **SSG alignment**: MkDocs expects content root (configurable via `docs_dir: content`)
- **Scalable**: Easy to add future locales
- **Sphinx artifacts handled**: `_locales/` kept temporarily, `_static/` removed (not needed)

**Cons**:

- **Largest implementation effort**: Affects both active and legacy content
- **Repository size unchanged**: All content remains (though organized)
- **Temporary Sphinx artifacts**: `_locales/` kept until ADR-002 completes
- **Learning curve**: Contributors familiar with `/src/` must adapt to `content/en/`

### Option 2: Minimal Change, Only Organize Legacy Content

**Description**: Move only legacy Russian content to `content/ru/`, leave active content in `/src/` unchanged.

**Target Structure:**

```
src/                   # Active content (unchanged)
  *.rst
  _locales/
  _static/
content/
  ru/                  # Legacy Russian only
    lessons/
    pictures/
    README.md
```

**Pros**:

- **Minimal disruption**: Active content location unchanged
- **Smaller implementation effort**: Only legacy content moved
- **Partial cleanup**: Root directory cleaner (legacy files removed)

**Cons**:

- **Incomplete solution**: Doesn't address lack of locale structure for active content
- **Inconsistent naming**: Active in `/src/`, legacy in `content/` — confusing pattern
- **No content root**: `/src/` doesn't establish clear locale-based organization
- **Missed opportunity**: Doesn't position a project for proper multilingual structure
- **Sphinx artifacts remain**: `_locales/` and `_static/` still mixed with content

### Option 3: Archive Legacy Content Separately

**Description**: Create `_archive/ru/` for legacy content, keep active content in `/src/`.

**Target Structure:**

```
src/                   # Active content
  *.rst
_archive/
  ru/                  # Archived legacy
    lessons/
    pictures/
```

**Pros**:

- **Explicit archived status**: Underscore prefix signals "not for active use"
- **Active content unchanged**: No disruption to the current location

**Cons**:

- **No locale structure**: Doesn't establish multilingual organization
- **Archive vs. locale confusion**: Less elegant than proper locale structure
- **Incomplete solution**: Doesn't address core organizational issues

### Option 4: Delete Legacy Content

**Description**: Remove all legacy Russian content, document location (upstream repo, git history).

**Pros**:

- **Cleanest repository**: Smallest size, no legacy files
- **Simplest maintenance**: Only active content

**Cons**:

- **Historical context lost**: Harder to understand project evolution
- **Less respectful**: Treats original course as disposable
- **Doesn't address active content structure**: `/src/` still lacks locale organization

### Option 5: Keep As-Is with Documentation

**Description**: Maintain current structure, add README clarification.

**Pros**:

- **Zero implementation effort**: No changes required

**Cons**:

- **Solves nothing**: All current problems persist
- **Missed opportunity**: Doesn't prepare for a proper multilingual structure

## Decision

**Adopt Option 1: Comprehensive Locale-Based Structure with `content/` Root**

**Rationale:**

Option 1 provides a complete, future-proof solution that addresses all organizational issues while establishing
a proper multilingual structure. This decision is driven by:

1. **Proper Multilingual Foundation**: Establishing `content/en/` and `content/ru/` creates the right pattern for a
   multilingual project. When Ukrainian content is adopted from `*.po` files (ADR-002),
   it naturally fits as `content/uk/`.

2. **Clear Content Root**: Renaming `/src/` → `content/` with locale subdirectories makes it immediately obvious where
   all content lives and how it's organized.

3. **Complete Solution**: Addresses both active and legacy content organization in one coherent restructuring rather
   than piecemeal changes.

4. **Respectful Historical Preservation**: Legacy Russian course preserved as a complete, colocated artifact honoring
   project origins.

5. **Clean Repository Root**: All content files moved into `content/` structure, leaving root for infrastructure only.

6. **SSG Alignment**: MkDocs (per ADR-002) works perfectly with `content/` as content root via `docs_dir: content`
   configuration. Locale structure aligns with `mkdocs-static-i18n` plugin expectations.

7. **Git History Preserved**: All moves use `git mv`, maintaining full commit history and attribution.

8. **Proper Sphinx Artifact Handling**:
    - `_locales/` kept temporarily (needed for current Sphinx setup, will be replaced in ADR-002)
    - `_static/` removed immediately (Sphinx-specific JS, not needed for MkDocs, preserved in git history)

9. **Global Assets Properly Located**: `/assets/` stays at root (project-wide resources, not locale-specific).

10. **Scalability**: Structure easily accommodates future locales or content types.

**Why Not Other Options:**

- **Option 2 (Minimal)**: Incomplete, doesn't establish locale structure, misses opportunity
- **Option 3 (Archive)**: Less elegant, doesn't create a multilingual foundation
- **Option 4 (Delete)**: Too aggressive, loses historical context
- **Option 5 (Keep As-Is)**: Solves no problems

**Coordination with ADR-002:**

This ADR establishes the structure; ADR-002 handles format conversion. The sequence:

1. **ADR-003 first**: Reorganize structure (this ADR) — content still in `.rst` format
2. **ADR-002 second**: Convert `.rst` → `.md`, set up MkDocs, create `content/uk/` for Ukrainian

This separation is cleaner than combining structure and format changes.

**Scope Boundaries:**

- **In Scope**: Directory structure, file organization, git moves, Sphinx artifact handling
- **Out of Scope**: File format conversion (reST → Markdown), SSG migration, MkDocs setup
- **Deferred to ADR-002**: Ukrainian `content/uk/` creation, `_locales/` deprecation, MkDocs i18n setup

## Consequences

### Positive

- **Clear Multilingual Organization**: Proper locale-based structure (`content/en/`, `content/ru/`, future
  `content/uk/`)
- **Clean Repository Root**: All content files in `content/`, root contains only infrastructure
- **Improved Contributor Onboarding**: Obvious where content lives and how it's organized
- **Historical Preservation**: Complete Russian course preserved as a self-contained artifact
- **Content-Asset Colocation**: Lesson files and images together within locales
- **Git History Maintained**: Full commit history preserved for all moved files
- **SSG-Ready Structure**: Aligns perfectly with MkDocs expectations (per ADR-002)
- **Respectful Treatment**: Original Russian course honored as project foundation
- **Future-Proof**: Easy to add locales, content types, or reorganize within an established pattern
- **Reduced Cognitive Load**: Clear, intuitive organization reduces mental effort
- **Proper Artifact Handling**: Sphinx-specific files appropriately managed

### Negative

- **Implementation Effort**: Comprehensive reorganization requires careful execution
- **Repository Size Unchanged**: All content preserved (though organized)
- **Temporary Sphinx Artifacts**: `_locales/` remains until ADR-002 completes (technical debt)
- **Learning Curve**: Contributors familiar with `/src/` must adapt to `content/en/`
- **Git Log Shows Moves**: History shows reorganization (though full history via `git log --follow`)
- **Coordination Required**: Must coordinate with ADR-002 to avoid conflicting changes

### Neutral

- **Directory Naming Change**: `/src/` → `content/en/` (neither better nor worse, just different convention)
- **Build System Unaffected**: Current Sphinx build still works (content location configurable)
- **Documentation Updates Needed**: README, contributor guides, `.ai/config.yaml` require updates
- **Active Content Path Changes**: Internal references to `/src/` become `content/en/` (mechanical update)

## Implementation

### Objectives

The comprehensive repository structure reorganization requires achieving the following primary goals:

1. **Establish Content Root**: Create `content/` as the unified content root directory with clear locale-based
   organization
2. **Reorganize Active Content**: Move all active English content from `/src/` to `content/en/` while preserving
   directory structure and file relationships
3. **Organize Legacy Content**: Move all legacy Russian content (root `*.md` files and `/pictures/lessonXX/`
   directories) to `content/ru/` with proper colocation
4. **Handle Sphinx Artifacts**: Move `src/_locales/` to `content/_locales/` temporarily; remove `src/_static/` (
   Sphinx-specific, not needed for MkDocs)
5. **Update Build Configuration**: Update Sphinx configuration (`conf.py`, build scripts) to reference new content
   paths
6. **Update Documentation**: Revise all project documentation (README, contributor guides, `.ai/config.yaml`) to
   reflect new structure
7. **Create Historical Context**: Add `content/ru/README.md` explaining legacy status and providing
   an upstream repository link
8. **Clean Repository Root**: Remove empty `/src/` and `/pictures/` directories, leaving root for infrastructure only

### Target Structure

Post-reorganization repository structure:

```
content/                # Content root (renamed from src/)
  _locales/            # Temporary - Sphinx gettext (Ukrainian .po)
  en/                  # Active English content (.rst format)
    *.rst
    subdirs/
  ru/                  # Legacy Russian content
    lessons/           # lesson*.md, module*.md
    pictures/          # lesson01/, lesson02/, lesson08/
    README.md

assets/                # Global project assets (unchanged at root)

(Root: .ai/, docs/, templates/, mindmaps/, config files, AGENTS.md, CLAUDE.md)
```

### Critical Constraints

The executor must respect these non-negotiable requirements:

1. **Git History Preservation**: ALL file moves MUST use `git mv` to preserve commit history. Verify with
   `git log --follow` after moves
2. **No Format Changes**: Content remains in current format (`.rst` for active, `.md` for legacy). Format conversion is
   ADR-002's scope
3. **Working Branch Protection**: All work on feature branch. The `main` branch remains stable throughout
4. **Sphinx Build Continuity**: Sphinx must continue building successfully with new paths. Update `conf.py` and build
   scripts accordingly
5. **Content Integrity**: No file loss, corruption, or accidental modifications. Only moves and path updates
6. **Asset Preservation**: `/assets/` directory stays at root unchanged (global project assets)
7. **Configuration Updates**: `.ai/config.yaml` and all documentation must reflect new paths
8. **Coordination with ADR-002**: ADR-002 migration will consume structure established here. `content/en/`
   becomes MkDocs source

### Excluded From Scope

The following is explicitly NOT part of this reorganization:

- **Format conversion** (`.rst` → `.md`): Handled by ADR-002
- **Ukrainian content structure** (`content/uk/`): Created during ADR-002 MkDocs migration
- **Removing `content/_locales/`**: Kept temporarily, deprecated during ADR-002
- **MkDocs configuration**: Handled by ADR-002
- **Any content modifications**: Structure only, content unchanged

### Success Criteria

Reorganization is considered successful when all the following are met:

- [ ] All active content moved to `content/en/` (format unchanged)
- [ ] All legacy Russian content organized in `content/ru/`
- [ ] `content/_locales/` preserved temporarily
- [ ] `src/_static/` removed (preserved in git history)
- [ ] `/assets/` unchanged at root
- [ ] The root directory contains only infrastructure files
- [ ] `content/ru/README.md` clearly explains legacy status
- [ ] Root `README.rst` updated with structure information
- [ ] `.ai/config.yaml` updated with new paths
- [ ] Sphinx configuration updated and built successfully
- [ ] Git history preserved for all moved files (verified with `git log --follow`)
- [ ] No broken references in documentation
- [ ] Changes committed with a clear, comprehensive message
- [ ] Project Owner approves structure

### Known Risks

The executor should be aware of and prepare for these risks:

| Risk                             | Impact | Mitigation Strategy                               |
|----------------------------------|--------|---------------------------------------------------|
| Unintended file moves            | High   | Careful inventory and verification at each step   |
| Git history loss                 | High   | Exclusive use of `git mv`; verify with `--follow` |
| Broken Sphinx build              | Medium | Test build after path updates; iterate if needed  |
| Broken documentation references  | Medium | Comprehensive search for hardcoded paths          |
| Conflicting changes with ADR-002 | Medium | ADR-003 executes first; ADR-002 follows           |
| Path reference issues            | Medium | Test all build and deployment processes           |
| Contributor confusion            | Low    | Clear documentation and communication             |

### Rollback Plan

If critical issues emerge:

1. Feature branch protects `main` — no production impact until validated
2. Can abandon the branch and revert to the current structure at any time
3. Git history preserved — original state always recoverable
4. No merge to `main` until full validation complete
5. Post-merge issues can be reverted (history allows reconstruction)

## Related

- [ADR-002][ADR-002]: Static Site Generator Replacement (interdependent, SSG expects content structure)
- [ADR-004][ADR-004]: Presentation Framework Handling (may affect assets organization)
- [ADR-001][ADR-001]: AI Guidelines Structure and Administration Framework
- [Upstream repository][upstream]: Original work by @PonomaryovVladyslav and contributors

[//]: # (@formatter:off)
<!-- ADR references -->
[ADR-001]: ./ADR-001-ai-guidelines-structure.md
[ADR-002]: ./ADR-002-ssg-replacement.md
[ADR-003]: ./ADR-003-repo-file-structure.md
[ADR-004]: ./ADR-004-presentation-framework.md
<!-- upstream repository -->
[upstream]: https://github.com/PonomaryovVladyslav/PythonCourses.git
[//]: # (@formatter:on)
