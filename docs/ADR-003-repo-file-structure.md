# ADR-003: Repository File Structure

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

| ver. | Date       | Author                                    | Changes description                          |
|------|------------|-------------------------------------------|----------------------------------------------|
| 0.4  | 2026-01-25 | Serhii Horodilov                          | Fix typos                                    |
| 0.3  | 2026-01-25 | Claude Sonnet 4.5 <noreply@anthropic.com> | Reframe as ru-RU localization, add pictures/ |
| 0.2  | 2026-01-25 | Serhii Horodilov                          | Fix typos                                    |
| 0.1  | 2026-01-25 | Claude Sonnet 4.5 <noreply@anthropic.com> | Initial draft                                |

</details>

## Context

The repository currently contains legacy Russian-language content scattered across multiple locations alongside modern
project infrastructure. This creates organizational and discoverability issues:

**Current State:**

- **Legacy content at root**: All `*.md` files in root directory (except `AGENTS.md` and `CLAUDE.md`) are legacy
  Russian-language lesson files
- **Legacy assets scattered**: `/pictures/lesson01/`, `/pictures/lesson02/`, `/pictures/lesson08/` contain images tied
  to legacy Russian lessons
- **Modern content**: Structured English/Ukrainian content in `/src/` directory
- **Active assets**: All files in `/assets/` directory are actively used by current course content
- **Infrastructure files**: `.ai/`, `docs/`, `templates/`, `mindmaps/` at root
- **Configuration files**: Various dotfiles, and active files `AGENTS.md`, `CLAUDE.md` at root
- **Build artifacts location**: Undefined (Sphinx typically uses `_build/` but this may change with SSG)

**Problems:**

1. **Contributor confusion**: New contributors face unclear directory purpose – where should new content go?
2. **Repository clutter**: Root directory contains many legacy content files mixed with infrastructure
3. **Discoverability issues**: Hard to distinguish current vs. legacy content at a glance (only exceptions are
   `AGENTS.md` and `CLAUDE.md`)
4. **Scattered legacy content**: Russian lesson files at root, related images in `/pictures/lessonXX/` subdirectories –
   content and assets not colocated
5. **Incomplete localization structure**: No clear locale organization (`en-US`, `uk-UA`, `ru-RU`) for multilingual
   project
6. **Maintenance overhead**: Legacy files require consideration during restructuring or tooling changes
7. **Onboarding friction**: New contributors must understand historical context to navigate repository
8. **Historical preservation**: Legacy Russian content lacks a clear "archived but preserved" status

**Historical Context:**

The legacy Russian-language files are from the original course version (the upstream source). The project has since
been translated to English with Ukrainian support, and content has been reorganized into `/src/`. The legacy files were
kept in place during initial translation work but have not been organized as a complete, preserved historical artifact
representing the original `ru-RU` localization.

**Note:** This decision is interdependent with ADR-002 (SSG Replacement). The chosen SSG may have expectations about
content location (e.g., `docs/`, `content/`, `src/`), and conversely, the structure decision affects SSG migration
effort.

## Decision Drivers

- **Contributor clarity**: Make it obvious where current content lives and where new files should be added
- **Historical preservation**: Maintain complete legacy Russian content as historical artifact
- **Localization clarity**: Establish clear locale-based organization (`en-US`, `uk-UA`, `ru-RU`)
- **Maintainability**: Reduce a cognitive load when navigating repository structure
- **Discoverability**: Clear separation between current and historical content
- **Content-asset colocation**: Keep related lesson content and images together
- **Build system compatibility**: Structure should support current and future SSG expectations
- **Git history preservation**: Maintain git history through move operations
- **Respectful treatment**: Honor the original Russian course as the foundation of a project

## Considered Options

### Option 1: Organize as `locales/ru-RU/` (Legacy Localization)

**Description**: Create a `locales/ru-RU/` directory structure that preserves all legacy Russian content (root `*.md`
files and `/pictures/lessonXX/` assets) as complete, self-contained localization. This treats the original Russian
course respectfully as a preserved historical artifact while clearly separating it from active English/Ukrainian
content.

**Pros**:

- **Respectful preservation**: Honors original Russian course as project foundation
- **Complete artifact**: Colocates lesson files with their images in a single locale directory
- **Clear localization model**: Establishes a pattern for future locale organization
- **Git history maintained**: Uses `git mv` to preserve full history
- **Historical reference**: Easy-to-browse complete original course structure
- **No data loss**: Everything preserved in an organized, discoverable location
- **Future-compatible**: If `ru-RU` support is ever revived, content is ready
- **Clean separation**: Root becomes infrastructure-only, locales contain all content variations

**Cons**:

- **Repository size unchanged**: All legacy content remains (though organized)
- **Implementation effort**: Requires careful restructuring of both files and assets
- **Migration complexity**: Need to ensure paths and references are updated if any cross-references exist

**Structure Example**:

```
locales/
  ru-RU/           # Legacy Russian (preserved, not maintained)
    lessons/       # All lesson*.md, module*.md from root
    pictures/      # All pictures/lessonXX/ directories
    README.md      # Explains this is legacy/historical
  (Future: en-US/, uk-UA/ if locale-based organization adopted)
```

### Option 2: Organize as `_archive/ru-RU/` (Archived Localization)

**Description**: Similar to Option 1, but uses `_archive/` prefix to more explicitly signal "not for active use."
Creates `_archive/ru-RU/` containing all legacy Russian content.

**Pros**:

- **Explicit "archived" status**: Underscore prefix convention clearly signals historical content
- **Complete preservation**: All benefits of Option 1 regarding colocation and history
- **Prevents accidental modification**: Clear visual signal this content is frozen
- **Git history maintained**: Uses `git mv` operations

**Cons**:

- **Less elegant than `locales/`**: Archive structure doesn't map to standard i18n patterns
- **Harder to revive**: If `ru-RU` support returns, content is in "archive" not "locales"
- **Repository size unchanged**: All legacy content remains

**Structure Example**:

```
_archive/
  ru-RU/
    lessons/
    pictures/
    README.md
```

### Option 3: Delete Legacy Content with Documentation

**Description**: Remove all legacy Russian content (root `*.md` files and `/pictures/lessonXX/` directories) from the
repository. Create `docs/LEGACY.md` documenting where original content can be found (upstream repo, git history).

**Pros**:

- **Cleanest repository**: Smallest repo size, zero legacy content
- **Reduced complexity**: No need to navigate historical content
- **Simplest long-term maintenance**: Only active content in the repository

**Cons**:

- **Loss of easy reference**: Content only accessible via git history or upstream
- **Historical context lost**: Harder to understand project evolution
- **Requires external navigation**: Must go to upstream or git history to see original
- **Less respectful**: Treats original course as disposable rather than foundational
- **Irreversible without effort**: Recovery requires git revert or upstream reference

### Option 4: Keep As-Is with README Clarification

**Description**: Maintain current structure but add clear documentation in README.md explaining legacy files and
directing contributors to `/src/` for active content.

**Pros**:

- **Zero implementation effort**: No file moves or restructuring
- **Git history intact**: No risk from file operations
- **Preserves everything in place**: All content immediately accessible

**Cons**:

- **Does not solve the core problem**: Repository remains cluttered
- **Ongoing confusion**: Visual clutter persists regardless of documentation
- **Scattered content**: Lesson files at root, images in subdirectories
- **Maintenance burden continues**: Legacy files remain consideration during changes
- **No localization structure**: Misses an opportunity to establish i18n patterns

### Option 5: Comprehensive Locale Reorganization

**Description**: Beyond handling legacy content, reorganize ALL content (including active `/src/`) into locale-based
structure: `locales/en-US/`, `locales/uk-UA/`, `locales/ru-RU/`. This is the most comprehensive approach.

**Pros**:

- **Complete i18n structure**: Establishes proper multilingual organization
- **Consistent pattern**: All localizations use same structure
- **Scalable**: Easy to add future locales
- **Best long-term structure**: Industry-standard i18n organization

**Cons**:

- **Largest scope**: Affects ALL content, not just legacy
- **Breaking changes**: Major updates to a build system, references, documentation
- **SSG dependency**: Should coordinate with ADR-002 decision
- **High implementation risk**: Complex migration affecting active content
- **Timing concern**: Premature before SSG decision
- **May conflict with SSG expectations**: Some SSGs have their own locale conventions

## Decision

**[DECISION PENDING - DRAFT STATUS]**

Recommended decision approach:

1. **Resolve legacy content organization** (Options 1–3) independently or in parallel with the SSG decision
2. **Defer comprehensive locale reorganization** (Option 5) until after ADR-002 is decided, as SSG choice strongly
   influences ideal i18n structure

**Recommended preference (for legacy content):**

- **Primary recommendation**: **Option 1** (`locales/ru-RU/`)
    - Respectfully preserves Russian content as complete historical artifact
    - Establishes foundation for future locale organization
    - Colocates related content and assets
    - Maintains git history
    - Cleaner conceptual model than "archive"

- **Alternative**: **Option 2** (`_archive/ru-RU/`) if stronger "frozen" signal is desired

Decision should consider:

- Value of preserving a complete historical artifact vs. repository size
- Desire to establish localization patterns for the future
- Likelihood of SSG choice affecting preferred locale organization

## Consequences

**[TO BE DETERMINED AFTER DECISION]**

General consequence categories based on likely options:

### Positive (if Option 1 or 2 is chosen)

- Complete preservation of historical Russian course content
- Clearer repository structure for new contributors
- Reduced cognitive load when navigating repository
- Better separation of concerns (active vs. historical)
- Colocated lesson content and assets
- Foundation for future localization structure
- Respectful treatment of project origins
- Easy reference to the original course if needed

### Negative (if Option 1 or 2 is chosen)

- Repository size unchanged (all content preserved)
- One-time migration effort required
- Need to document new structure for contributors
- Potential references may need updating

### Negative (if Option 3 is chosen)

- Loss of easy reference to the original course
- Historical context is harder to access
- Less respectful to project origins

### Neutral

- Git history shows move operations (Option 1/2) or deletions (Option 3)
- SSG may or may not influence further locale organization

## Implementation

**[TO BE DEFINED AFTER DECISION]**

Potential implementation for each option:

**Option 1 (locales/ru-RU/):**

1. Create directory structure:
   ```
   locales/
     ru-RU/
       lessons/
       pictures/
   ```
2. Move legacy content:
    - `git mv` all root `*.md` files (except `AGENTS.md`, `CLAUDE.md`) → `locales/ru-RU/lessons/`
    - `git mv pictures/lesson01/ pictures/lesson02/ pictures/lesson08/` → `locales/ru-RU/pictures/`
3. Create `locales/ru-RU/README.md` explaining:
    - This is the original Russian course (legacy, preserved for history)
    - Not actively maintained
    - Represents project foundation and historical artifact
    - Active course is in `/src/` (English/Ukrainian)
4. Update root `README.md`:
    - Clarify repository structure
    - Note legacy Russian content in `locales/ru-RU/`
    - Direct contributors to `/src/` for active content
5. Update `.gitignore` if needed (ensure locale directories are tracked)

**Option 2 (_archive/ru-RU/):**

Similar to Option 1, but use `_archive/ru-RU/` path instead of `locales/ru-RU/`.

**Option 3 (Delete):**

1. Remove legacy content:
    - `git rm` all root `*.md` files (except `AGENTS.md`, `CLAUDE.md`)
    - `git rm -r pictures/lesson01/ pictures/lesson02/ pictures/lesson08/`
2. Create `docs/LEGACY.md` documenting:
    - What was removed and why
    - Upstream repository URL
    - How to access via git history
    - Historical context of Russian → English transition
3. Update root `README.md` to reference `docs/LEGACY.md`

**Option 5 (Comprehensive Locale Reorganization):**

1. Coordinate with ADR-002 SSG decision
2. Research SSG locale/i18n conventions
3. Create a comprehensive migration plan
4. Create a work package for executor

## Related

- ADR-002: Static Site Generator Replacement (interdependent – SSG may have i18n/locale expectations)
- ADR-004: Presentation Framework Handling (may affect assets directory organization)
- ADR-001: AI Guidelines Structure and Administration Framework
- Upstream repository: [URL to be documented]
