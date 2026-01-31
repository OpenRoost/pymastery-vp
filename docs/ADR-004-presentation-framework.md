# ADR-004: Presentation Framework Handling

[//]: # (@formatter:off)
<!-- document status badges -->
[draft]: https://img.shields.io/badge/document_status-draft-orange.svg
[accepted]: https://img.shields.io/badge/document_status-accepted-green.svg
[rejected]: https://img.shields.io/badge/document_status-rejected-red.svg
[deprecated]: https://img.shields.io/badge/document_status-deprecated-lightgrey.svg
[final]: https://img.shields.io/badge/document_status-final-blue.svg
[//]: # (@formatter:on)
![status][final]

<details>
<summary>Document Changelog</summary>

[//]: # (order by version number descending)

| ver. | Date       | Author                                    | Changes description                               |
|------|------------|-------------------------------------------|---------------------------------------------------|
| 1.2  | 2026-01-31 | Serhii Horodilov, Claude Sonnet 4.5       | Simplify scope: remove submodule; final           |
| 1.1  | 2026-01-31 | Serhii Horodilov, Claude Sonnet 4.5       | Revise decision: Option 5→3 (separate repo)       |
| 1.0  | 2026-01-27 | Serhii Horodilov                          | Accepted                                          |
| 0.6  | 2026-01-27 | Claude Sonnet 4.5 <noreply@anthropic.com> | Final review improvements and status finalization |
| 0.5  | 2026-01-27 | Serhii Horodilov                          | Fix typos and formatting                          |
| 0.4  | 2026-01-27 | Claude Sonnet 4.5 <noreply@anthropic.com> | Technical corrections per implementation review   |
| 0.3  | 2026-01-26 | Claude Sonnet 4.5 <noreply@anthropic.com> | Complete final draft                              |
| 0.2  | 2026-01-25 | Serhii Horodilov                          | Fix links and typos                               |
| 0.1  | 2026-01-25 | Claude Sonnet 4.5 <noreply@anthropic.com> | Initial draft                                     |

</details>

## Context

The repository currently includes impress.js as a **git submodule** at `/assets/impress.js/`, which references the
complete presentation framework library. While git submodules don't directly bloat the main repository, they create
development friction and maintenance challenges.

**Current State:**

- **Type**: Git submodule (not vendored code)
- **Location**: `/assets/impress.js/` directory at repository root
- **Size**: Extremely large (~33,000+ lines when cloned with `--recurse-submodules`)
- **Contents**: Complete impress.js library including:
    - Source code and build system
    - Package management files (package.json, package-lock.json)
    - Multiple example presentations
    - Extensive plugin ecosystem
    - Test suites
    - Localization files for multiple languages
    - Documentation
    - Full dependency tree
- **Active assets**: All other files in `/assets/` directory (icons, images, mermaid diagrams) are actively used
- **Impact**:
    - Contributors cloning with `--recurse-submodules` download the entire library
    - Submodule updates require manual git commands
    - Unclear whether local copy is necessary vs. alternative approaches
    - Development workflow complexity for presentation features

**Current Usage (Verified):**

- **Confirmed**: One presentation exists at `src/rdbms/presentations/normalization/`
- **Confirmed**: impress.js imported in `src/conf.js` webpack entry point
- **Confirmed**: A build process uses webpack to bundle impress.js with presentation
- **Unknown**: Whether additional course materials depend on presentations
- **Unknown**: Whether a presentation feature is actively used in the course delivery workflow

**Problems:**

1. **Submodule complexity**: Requires `--recurse-submodules` flag and manual update commands
2. **Clone time**: Slower repository cloning for contributors who use `--recurse-submodules`
3. **Maintenance overhead**: Requires keeping a library updated if actively used
4. **Unclear necessity**: No clear documentation of why git submodule is required vs. npm package
5. **Development friction**: Submodule workflow adds complexity for presentation features
6. **Backup inefficiency**: Submodule references require special handling in backups

**Historical Context:**

The impress.js framework was likely added for creating presentation-style course materials or demos. However, it's
unclear whether this feature is actively used in the current course structure or if presentations were part of the
legacy Russian version that has since been deprecated.

> [!IMPORTANT]
> This decision may impact ADR-002 (SSG Replacement) if presentations are part of course delivery requirements.

## Decision Drivers

- **Repository complexity**: Impact on clone process, submodule management, and contributor experience
- **Active usage**: Whether presentations are part of the current course delivery
- **Maintenance burden**: Effort required to keep a library updated vs. benefits
- **Dependency management**: Best practices for managing third-party libraries
- **Build requirements**: Whether a local copy is needed for a build process vs. runtime
- **Alternative approaches**: CDN usage, npm package management, separate repositories
- **Course delivery needs**: Requirements for presentation-style content

## Considered Options

### Option 1: Keep As-Is (Git Submodule)

**Description**: Maintain the current `/assets/impress.js/` git submodule in the repository.

**Pros**:

- **No migration needed**: Zero implementation effort
- **Self-contained**: Works offline, no external dependencies
- **Version locked**: Guaranteed compatibility with a frozen version
- **No breaking changes**: Existing presentations (if any) work unchanged

**Cons**:

- **Submodule complexity**: Requires `--recurse-submodules` flag and manual updates
- **Slow clones**: Contributors who recurse submodules download an entire presentation framework
- **Maintenance burden**: Must manually update submodule for security/features
- **No clear justification**: Benefits unclear if presentations are rarely/never used
- **Poor dependency management**: Violates modern best practices (use package managers)

### Option 2: Remove and Use CDN

**Description**: Delete `/assets/impress.js/` submodule entirely. If presentations are needed, reference impress.js
from a CDN (e.g., cdnjs, jsDelivr) in HTML files.

**Pros**:

- **Eliminates submodule complexity**: Standard git workflow, no special commands
- **Faster clones**: No submodule to recurse
- **Automatic updates**: CDN provides latest stable version
- **Best practice**: Standard approach for client-side libraries
- **No maintenance**: No need to update the library manually
- **Better caching**: Users benefit from shared CDN cache across sites

**Cons**:

- **Requires internet**: Won't work offline (though the course is web-delivered anyway)
- **External dependency**: Relies on CDN availability
- **Version changes**: CDN updates might break presentations (mitigated by version pinning)
- **Migration effort**: Update existing presentation HTML to reference CDN

> [!NOTE]
> Use version-pinned CDN URLs like:
> ```html
> <script src="https://cdn.jsdelivr.net/gh/impress/impress.js@2.0.0/js/impress.js"></script>
> ```

### Option 3: Extract to Separate Repository

**Description**: Move `/assets/impress.js/` to a separate repository (e.g., `pymastery-presentations`). Main course
repo references it as a git submodule or documents how to clone separately if needed.

**Pros**:

- **Clean separation**: The main repo focuses on course content
- **Optional dependency**: Only those working on presentations clone it
- **Preserves history**: Git history for presentation development maintained
- **Flexible**: Can still use presentations without CDN
- **Modular**: Clear boundary between course and presentation tooling

**Cons**:

- **Complexity**: Adds git submodule or separate clone step
- **Maintenance**: Now managing two repositories
- **Coordination**: Changes across both repos require more planning
- **Overkill if unused**: Extra complexity for a potentially unused feature

### Option 4: Archive to `_archive/impress.js/`

**Description**: Move `/assets/impress.js/` submodule to `_archive/impress.js/` to signal it's historically/not
actively used, but preserve it in repository.

**Pros**:

- **Preserves history**: Library remains in repository for reference
- **Clearer status**: Archive location signals "not for active use"
- **Git history intact**: Move operation preserves full history
- **Easy recovery**: Can restore if needed

**Cons**:

- **Doesn't solve complexity**: Submodule management unchanged
- **Unclear benefit**: If not used, why keep it?
- **Maintenance still unclear**: Archive status doesn't eliminate update question

### Option 5: Convert to npm Dependency

**Description**: Remove `/assets/impress.js/` submodule. If presentations are actively used, add impress.js as a npm
dependency in `package.json` and use build tools to bundle it.

**Pros**:

- **Modern dependency management**: Uses npm ecosystem properly
- **Version control**: Exact version pinning in package.json
- **Automated updates**: npm tools for dependency management
- **Build-time bundling**: Include only what's needed
- **Developer-friendly**: Standard workflow for JavaScript projects

**Cons**:

- **Requires build tooling**: Need webpack/rollup/etc. if not already present
- **Complexity increase**: Build pipeline for asset management
- **Overkill if simple**: May be unnecessary if just serving static HTML
- **Learning curve**: Contributors need to understand a build process

## Decision

**Decision:** **Remove impress.js Git Submodule** – SIMPLIFIED SCOPE from v1.1

**Scope Clarification (v1.2 - 2026-01-31):**

This ADR addresses **only the impress.js framework/submodule question**: "What do we do with `/assets/impress.js/`
submodule?"

**Decision:** Remove the git submodule entirely from the main repository.

**Rationale:**

- Git submodule adds development friction (requires `--recurse-submodules`, manual updates)
- Presentation content will be extracted to a separate repository (see ADR-006)
- The separate presentation repository will handle impress.js dependency (npm or CDN)
- The main course repository focuses on content delivery (MkDocs), no presentation framework needed

**Presentation Content:** Addressed separately in [ADR-006][ADR-006] (Presentation Content Repository Separation)

---

**Previous Decision (v1.0):** Option 5 (npm Dependency)  
**Previous Revision (v1.1):** Option 3 (Extract to Separate Repository) – scope was too broad  
**Current Decision (v1.2):** Remove submodule - presentation extraction covered in ADR-006

---

**Dependencies:**

- This decision complements [ADR-006][ADR-006] (Presentation Content Repository Separation)
- Unblocks [ADR-002][ADR-002] (SSG Replacement) – the main repo can focus on MkDocs

## Consequences

**Based on Decision: Remove impress.js Git Submodule**

### Positive

- **Eliminates submodule complexity**: Standard git workflow, no `--recurse-submodules` needed
- **Faster clones**: No submodule to download
- **Simpler contributor workflow**: One less thing to understand
- **Cleaner repository**: Presentation framework managed elsewhere (ADR-006)
- **Focuses main repo**: Course content only, no presentation tooling

### Negative

- **Potential build and presentation breakage if sequencing is wrong**: The current repo still imports/references `assets/impress.js` (for example from `src/conf.js` and `src/rdbms/presentations/normalization.html`). Removing the `impress.js` git submodule *before* updating or removing these references (or providing an equivalent asset path) will break the webpack build and/or existing presentations.
- **Additional migration work required**: All references to `assets/impress.js` must be updated, removed, or redirected as part of the implementation of this ADR (and related work in [ADR-006][ADR-006]) before the submodule can be safely removed without disrupting contributors or learners.

### Neutral

- **Presentation content handling**: Covered separately in [ADR-006][ADR-006]
- **Git history**: Submodule reference removed, but history preserved in the main repo

## Related

- [ADR-006][ADR-006]: Presentation Content Repository Separation (where presentation content will live)
- [ADR-002][ADR-002]: Static Site Generator Replacement (unblocked by removing a presentation framework)
- [ADR-003][ADR-003]: Repository File Structure (affects overall assets organization)
- [ADR-001][ADR-001]: AI Guidelines Structure and Administration Framework

[//]: # (@formatter:off)
<!-- ADR references -->
[ADR-001]: ./ADR-001-ai-guidelines-structure.md
[ADR-002]: ./ADR-002-ssg-replacement.md
[ADR-003]: ./ADR-003-repo-file-structure.md
[ADR-004]: ./ADR-004-presentation-framework.md
[ADR-006]: ./ADR-006-presentation-content.md
[//]: # (@formatter:on)
