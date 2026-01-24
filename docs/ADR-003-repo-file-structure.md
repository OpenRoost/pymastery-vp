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

| ver. | Date       | Author                                    | Changes description |
|------|------------|-------------------------------------------|---------------------|
| 0.2  | 2026-01-25 | Serhii Horodilov                          | Fix typos           |
| 0.1  | 2026-01-25 | Claude Sonnet 4.5 <noreply@anthropic.com> | Initial draft       |

</details>

## Context

The repository currently contains legacy content files at the repository root alongside modern project infrastructure.
This creates several issues:

**Current State:**

- **Legacy content at root**: All `*.md` files in root directory (except `AGENTS.md` and `CLAUDE.md`) are legacy
  Russian-language content
- **Modern content**: Structured English/Ukrainian content in `/src/` directory
- **Infrastructure files**: `.ai/`, `docs/`, `templates/`, `mindmaps/` at root
- **Configuration files**: Various dotfiles, and active files `AGENTS.md`, `CLAUDE.md` at root
- **Build artifacts location**: Undefined (Sphinx typically uses `_build/` but this may change with SSG)

**Problems:**

1. **Contributor confusion**: New contributors face unclear directory purpose – where should new content go?
2. **Repository clutter**: Root directory contains many legacy content files mixed with infrastructure
3. **Discoverability issues**: Hard to distinguish current vs. legacy content at a glance (only exceptions are
   `AGENTS.md` and `CLAUDE.md`)
4. **Maintenance overhead**: Legacy files require consideration during restructuring or tooling changes
5. **Onboarding friction**: New contributors must understand historical context to navigate repository

**Historical Context:**
The legacy Russian-language Markdown files are from the original course version. The project has since been translated
to English with Ukrainian support, and content has been reorganized into `/src/`. The legacy files were kept in place
during initial translation work but have not been removed or archived.

**Note:** This decision is interdependent with ADR-002 (SSG Replacement). The chosen SSG may have expectations about
content location (e.g., `docs/`, `content/`, `src/`), and conversely, the structure decision affects SSG migration
effort.

## Decision Drivers

- **Contributor clarity**: Make it obvious where current content lives and where new files should be added
- **Maintainability**: Reduce a cognitive load when navigating repository structure
- **Discoverability**: Clear separation between current and historical content
- **Build system compatibility**: Structure should support current and future SSG expectations
- **Preservation vs. cleanup**: Balance between historical preservation and practical usability
- **Migration impact**: Minimize disruption to ongoing work and SSG transition
- **Git history**: Maintain or lose git history for moved/removed files
- **Upstream reference**: Ability to reference original Russian content if needed

## Considered Options

### Option 1: Archive Legacy Content to `_archive/`

**Description**: Move all legacy `*.md` files (except `AGENTS.md` and `CLAUDE.md`) to a new `_archive/legacy-russian/`
directory at repository root. This preserves the content in the repository but clearly separates it from active
content.

**Pros**:

- Preserves legacy content within the repository for reference
- Maintains git history through `git mv` operations
- Clear visual separation (underscore prefix convention signals "not for production")
- Allows future contributors to reference original Russian content if needed
- Simple implementation (move files)
- No external dependencies

**Cons**:

- Repository still contains legacy content (though better organized)
- Archive directory adds to repository size (minor concern)
- May create the expectation that archived content is maintained
- Still requires explanation to new contributors about the archive purpose

### Option 2: Delete Legacy Content Entirely

**Description**: Remove all legacy `*.md` files (except `AGENTS.md` and `CLAUDE.md`) from the repository. The original
Russian content remains available in the upstream repository or through git history if needed.

**Pros**:

- Cleanest repository structure
- No confusion about active vs. inactive content
- Reduced repository size
- Forces complete focus on current English/Ukrainian content
- Simplest long-term maintenance

**Cons**:

- **Loss of easy reference**: Content only accessible via git history or upstream
- **Requires documentation**: Must document where to find original Russian content
- **Irreversible without git revert**: More effort to recover if needed later
- **Historical context lost**: Git history shows deletion but not original content organization
- **Potential contributor concern**: Some may prefer preserved history

### Option 3: Keep As-Is with README Clarification

**Description**: Maintain current structure but add clear documentation in README.md explaining legacy files and
directing contributors to `/src/` for active content.

**Pros**:

- Zero implementation effort
- No risk of accidental issues from file moves
- Git history completely intact
- Preserves all contents in place
- No decisions needed about archival vs. deletion

**Cons**:

- **Does not solve the core problem**: Repository remains cluttered
- **Requires reading documentation**: Contributors must read and follow README guidance
- **Ongoing confusion**: Visual clutter persists regardless of documentation
- **Maintenance burden continues**: Legacy files remain a consideration during restructuring
- **Does not address motivation**: Fails to improve repository organization

### Option 4: Hybrid - Archive + Upstream Reference

**Description**: Delete legacy content from repository but create `docs/LEGACY.md` documenting exactly where the
original Russian content can be found (upstream repo URL, specific commit, directory structure).

**Pros**:

- Clean the repository structure (like Option 2)
- Documented path to legacy content (unlike simple deletion)
- Best of both worlds for cleanliness and reference
- Educates contributors about project history
- Minimal overhead (single documentation file)

**Cons**:

- Requires maintaining a link to the upstream repository
- Content not immediately accessible (requires external navigation)
- Documentation must be kept current if upstream changes
- Extra step for anyone needing to reference legacy content

### Option 5: Content Reorganization – Consolidate Active Content

**Description**: Beyond handling legacy files, reorganize active content location. For example, move `/src/` to
`/docs/` or `/content/` based on common SSG conventions or rename `src/` to something more descriptive like `/course/`.

**Pros**:

- Opportunity for clearer naming (e.g., `/course/` is more descriptive than `/src/`)
- Can align with SSG expectations (some prefer `/docs/` or `/content/`)
- Comprehensive structural cleanup in a single effort
- Sets foundation for long-term organization

**Cons**:

- **Larger scope**: Affects active content, not just legacy
- **Breaking changes**: Updates required in build configuration, documentation, links
- **SSG dependency**: Should be coordinated with ADR-002 decision
- **Higher risk**: More complex than just handling legacy files
- **Timing concern**: May be premature before SSG decision

## Decision

**[DECISION PENDING - DRAFT STATUS]**

Recommended decision approach:

1. **Resolve legacy content handling** (Options 1–4) independently or in parallel with the SSG decision
2. **Defer active content reorganization** (Option 5) until after ADR-002 is decided, as SSG choice may inform the
   ideal structure

Recommended preference (for legacy content):

- **Primary recommendation**: Option 2 (Delete) or Option 4 (Delete and Documentation)
    - Cleanest long-term solution
    - Legacy content available via git history and upstream
    - Reduces maintenance burden
    - Option 4 adds helpful documentation at minimal cost

Decision should consider:

- Team's preference for preservation vs. cleanliness
- Likelihood of needing to reference legacy content
- Complexity tolerance for repository structure

## Consequences

**[TO BE DETERMINED AFTER DECISION]**

General consequence categories based on likely options:

### Positive (if Option 1, 2, or 4 is chosen)

- Clearer repository structure for new contributors
- Reduced cognitive load when navigating a project
- Better separation of concerns (active vs. historical)
- Easier to explain repository organization

### Negative (if Option 1 is chosen)

- Archive directory adds conceptual overhead
- May create questions about archive maintenance

### Negative (if Option 2 or 4 is chosen)

- Legacy content requires git history access or external reference
- One-time effort to document a recovery path (if Option 4)

### Neutral

- Git history shows either deletion or move operations
- Repository size impact (minimal regardless of option)

## Implementation

**[TO BE DEFINED AFTER DECISION]**

Potential implementation for each option:

**Option 1 (Archive):**

1. Create `_archive/legacy-russian/` directory
2. Use `git mv` to move all legacy `*.md` files (excluding `AGENTS.md` and `CLAUDE.md`)
3. Add `_archive/README.md` explaining purpose and content
4. Update root `README.md` to clarify content locations
5. Update `.gitignore` if needed (ensure archive is tracked)

**Option 2 (Delete):**

1. Remove all legacy `*.md` files (excluding `AGENTS.md` and `CLAUDE.md`) using `git rm`
2. Update `README.md` noting that original content is in git history
3. Consider adding a note about upstream repository location

**Option 4 (Delete + Documentation):**

1. Remove all legacy `*.md` files (excluding `AGENTS.md` and `CLAUDE.md`) using `git rm`
2. Create `docs/LEGACY.md` documenting:
    - What was removed and why
    - Upstream repository URL and structure
    - How to access via git history if needed
    - Historical context of Russian → English transition
3. Update root `README.md` to reference `docs/LEGACY.md`

**Option 5 (Full Reorganization):**

1. Coordinate with ADR-002 SSG decision
2. Plan based on SSG requirements
3. Create a migration work package for executor

## Related

- ADR-002: Static Site Generator Replacement (interdependent - SSG may have structure expectations)
- ADR-001: AI Guidelines Structure and Administration Framework
- Upstream repository: [URL to be documented if Option 4 is chosen]
