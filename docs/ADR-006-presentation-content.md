# ADR-006: Presentation Content Repository Separation

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

| ver. | Date       | Author                          | Changes description |
|------|------------|---------------------------------|---------------------|
| 1.0  | 2026-01-31 | Serhii Horodilov, Claude Sonnet 4.5 | Accepted            |
| 0.1  | 2026-01-31 | Claude Sonnet 4.5               | Initial draft       |

</details>

## Context

The repository currently contains presentation content at `src/rdbms/presentations/normalization/` as part of the main course repository. This presentation is built separately using webpack and is delivered as a standalone tool, not embedded in MkDocs-generated lesson pages.

**Current State:**

- **Location**: `src/rdbms/presentations/normalization/`
- **Build system**: Webpack (separate from MkDocs course build)
- **Delivery**: Standalone presentation, not embedded in lessons
- **Usage**: Interactive visualization tool for database normalization concepts
- **Framework**: Uses impress.js (currently via git submodule in `/assets/impress.js/`)

**Course Architecture:**

- **Course content**: Built with MkDocs → static HTML lessons
- **Presentations**: Built with webpack → standalone interactive presentations
- **Delivery**: Presentations linked from lessons, but served separately

**Problems:**

1. **Mixed build systems**: Main repo requires BOTH MkDocs (for content) AND webpack (for presentations)
2. **Unclear scope**: Course content mixed with presentation tools
3. **Scaling concerns**: Each new presentation adds webpack complexity to course repo
4. **Deployment coupling**: Presentation updates require full course repo build
5. **Contributor confusion**: Course contributors need presentation build knowledge
6. **Repository bloat**: As presentations grow, course repo grows unnecessarily

**Planning Findings:**

During ADR-004 planning, architectural clarification revealed:
- **Presentations are standalone**: Not embedded in lesson pages
- **Separate build process**: Webpack/vite build, independent from MkDocs
- **Multiple presentations planned**: One per topic (multiple anticipated)
- **Independent deployment**: Presentations hosted/deployed separately from course

> [!IMPORTANT]
> This decision is related to [ADR-004][ADR-004] (Presentation Framework Handling), which addresses removing the impress.js submodule.

## Decision Drivers

- **Architectural clarity**: Clear separation between course content and presentation tools
- **Build simplicity**: Single build system per repository
- **Independent scaling**: Presentations grow without affecting course repo
- **Deployment flexibility**: Independent release cycles for content vs. presentations
- **Contributor workflow**: Course contributors don't need presentation build knowledge
- **Future growth**: Multiple presentations planned (one per topic)
- **Maintenance overhead**: Managing mixed concerns in single repository

## Considered Options

### Option 1: Keep Presentations in Main Repository

**Description**: Maintain presentations in `src/rdbms/presentations/` within the main course repository.

**Pros**:

- **No migration needed**: Zero implementation effort
- **Single repository**: Everything in one place
- **Git history intact**: All changes in same repository
- **Simpler for small scale**: Works fine for one or two presentations

**Cons**:

- **Mixed build systems**: Both MkDocs AND webpack in same repo
- **Unclear scope**: Course content mixed with presentation tools
- **Scaling problems**: Each presentation adds complexity
- **Coupled deployment**: Presentation changes trigger full course builds
- **Contributor confusion**: Must understand both build systems

### Option 2: Extract to Separate Repository

**Description**: Move presentation content to separate repository (`pymastery-presentations`), maintaining git history.

**Pros**:

- **Architectural clarity**: Clean separation between content and tools
- **Build simplicity**: Main repo = MkDocs only; Presentation repo = webpack only
- **Independent scaling**: Presentations grow independently
- **Deployment independence**: Separate release cycles
- **Focused workflows**: Course contributors don't need webpack knowledge
- **Future-proof**: Natural pattern for multiple presentations
- **Flexible hosting**: Can host at subdomain or separate infrastructure

**Cons**:

- **Repository coordination**: Managing two repositories
- **Initial extraction effort**: ~2-4 hours setup
- **Cross-repository references**: Lessons link to external URLs
- **Two CI/CD pipelines**: Separate workflows to maintain

### Option 3: Submodule Approach

**Description**: Keep presentations in separate repository, reference as git submodule in main repo.

**Pros**:

- **Technically linked**: Submodule reference in main repo
- **Git history separated**: Each repo maintains own history

**Cons**:

- **Submodule complexity**: Same issues as ADR-004 identified
- **Worst of both worlds**: Coordination overhead + submodule friction
- **Already rejecting submodules**: ADR-004 removes impress.js submodule

## Decision

**Decision:** **Option 2 (Extract to Separate Repository)**

**Rationale:**

Presentations should be in a separate repository (`pymastery-presentations`) because:

1. **Different build systems**: Course (MkDocs) vs. Presentations (webpack/vite)
2. **Different delivery model**: Embedded lessons vs. standalone tools
3. **Independent scaling**: Multiple presentations planned (one per topic)
4. **Clear separation of concerns**: Content creation vs. tool development
5. **Deployment independence**: Presentation updates don't trigger course rebuilds

**Why This Architecture:**

The current situation has two distinct responsibilities:
- **Course repository**: Educational content delivery via MkDocs
- **Presentation repository**: Interactive visualization tools via webpack

These should be separate repositories with clear boundaries.

**Framework Handling:**

The separate presentation repository will handle impress.js dependency independently (npm or CDN), as covered in [ADR-004][ADR-004].

## Consequences

### Positive

- **Architectural clarity**: Course content vs. presentation tools cleanly separated
- **Build simplicity**: Main repo uses MkDocs only (no webpack)
- **Faster course builds**: No presentation webpack compilation
- **Independent scaling**: Add presentations without bloating course repo
- **Deployment independence**: Separate release cycles
- **Focused contributor workflow**: Course contributors don't need webpack knowledge
- **Flexible presentation hosting**: Can use subdomain or separate infrastructure
- **Future-proof**: Natural pattern for multiple standalone presentations

### Negative

- **Repository coordination**: Managing two repositories instead of one
- **Initial extraction effort**: ~2-4 hours to set up with CI/CD
- **Cross-repository references**: Course lessons link to external presentation URLs
- **Two CI/CD pipelines**: Separate build/deployment workflows

### Neutral

- **Same total content**: Moved, not removed
- **Git history preserved**: Full history via `git subtree split`
- **Presentation functionality unchanged**: Works identically in new repository
- **Same team ownership**: Content creators maintain both repositories

## Related

- [ADR-004][ADR-004]: Presentation Framework Handling (removes impress.js submodule)
- [ADR-002][ADR-002]: Static Site Generator Replacement (main repo focuses on MkDocs)
- [ADR-003][ADR-003]: Repository File Structure (affects content organization)
- [ADR-001][ADR-001]: AI Guidelines Structure and Administration Framework

[//]: # (@formatter:off)
<!-- ADR references -->
[ADR-001]: ./ADR-001-ai-guidelines-structure.md
[ADR-002]: ./ADR-002-ssg-replacement.md
[ADR-003]: ./ADR-003-repo-file-structure.md
[ADR-004]: ./ADR-004-presentation-framework.md
[ADR-006]: ./ADR-006-presentation-content.md
[//]: # (@formatter:on)
