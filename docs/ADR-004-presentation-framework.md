# ADR-004: Presentation Framework Handling

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

**Decision:** **Option 3 (Extract to Separate Repository)** - REVISED from v1.0

**Revision Rationale (v1.1 - 2026-01-31):**

During the planning phase, architectural clarification revealed:

- **Presentations are standalone**: Not embedded in lesson pages, delivered separately from course content
- **Separate build process**: Presentations require webpack/vite build, independent of MkDocs SSG
- **Multiple presentations planned**: One per topic (multiple expected)
- **Independent deployment**: Presentations hosted/deployed separately from the course site

**Option 3 (Separate Repository) better matches this architecture** than the original Option 5 (npm in the main repo)
because:

1. Separates concerns: Course content (MkDocs) vs. presentation tools (webpack/vite)
2. Independent scaling: Presentation repo grows without affecting course repo
3. Build simplicity: Each repo has a single build responsibility
4. Deployment flexibility: Independent release cycles
5. Eliminates webpack from the main course repo (MkDocs only)

**Previous Decision (v1.0):** Option 5 (npm Dependency)  
**Why changed:** New understanding of presentation architecture emerged before implementation

---

**Findings:**

After verification, exactly **one presentation exists** in the repository at `src/rdbms/presentations/normalization/`,
with impress.js imported via `src/conf.js` webpack entry point.

Additional findings during planning:

- Presentations are **not embedded** in MkDocs-generated pages
- Presentations require **separate build** (webpack/vite + impress.js)
- One presentation planned **per course topic** (multiple expected)
- Presentations are **standalone tools**, not integral lesson content

**Rationale:**

1. **Extract presentation to separate repository**: `pymastery-presentations`
2. **Remove impress.js git submodule**: Eliminates submodule complexity from main repo
3. **Add impress.js as npm dependency in presentation repo**: Modern dependency management
4. **Separate build systems**: The main repo uses MkDocs only, presentation repo uses webpack/vite
5. **Clean architectural separation**: Content vs. tools

**Why Option 3 (Separate Repository):**

- **Architectural clarity**: Course content repo vs. presentation tools repo
- **Build separation**: Main repo = MkDocs only; Presentation repo = webpack/vite only
- **Independent scaling**: Adding presentations doesn't bloat course repo
- **Deployment independence**: Deploy course updates without rebuilding presentations
- **Single responsibility**: Each repo has one clear purpose
- **Eliminates webpack from main repo**: Simplifies course contributor workflow
- **Future-proof**: Natural pattern for multiple standalone presentations
- **Flexible presentation hosting**: Can host at subdomain/separate path

**Implementation Approach:**

1. Create new repository: `pymastery-presentations`
2. Extract presentation with git history using `git subtree split`
3. Push to a new repository
4. Set up webpack/vite + impress.js (npm or CDN) in the presentation repo
5. Remove presentation and git submodule from the main course repo
6. Update the course lesson to link to the presentation (external reference)
7. Set up independent CI/CD for presentation repo

**Estimated Implementation Time:** 2–4 hours (repo setup + extraction + CI/CD configuration)

**Dependencies Resolved:**

This decision removes the blocker for ADR-002 (SSG Replacement), as presentation requirements are now clarified.
Main course repo can focus entirely on MkDocs, without webpack complexity.

## Consequences

**Based on Chosen Option 3 (Separate Repository):**

### Positive

- **Architectural clarity**: Clean separation between course content and presentation tools
- **Eliminates submodule complexity**: Main repo has a standard git workflow, no special commands
- **Faster clone times**: The Main repo is significantly smaller without a presentation framework
- **Build simplicity**: The main repo uses MkDocs only; no webpack complexity for course contributors
- **Independent scaling**: Presentation repo grows independently without affecting course repo
- **Deployment independence**: Course updates and presentation updates have separate release cycles
- **Single responsibility**: Each repository has one clear purpose
- **Focused contributor workflow**: Course contributors don't need presentation build knowledge
- **Flexible hosting**: Presentations can be hosted at subdomain or separate infrastructure
- **Future-proof**: Natural pattern for adding multiple standalone presentations

### Negative

- **Repository coordination**: Managing two repositories instead of one
- **Initial extraction effort**: ~2–4 hours to set up a separate repo with proper CI/CD
- **Cross-repository references**: Course lessons link to external presentation URLs
- **Two CI/CD pipelines**: Separate build/deployment workflows to maintain

### Neutral

- **Same total content**: Presentation moved to different location, not removed
- **Git history preserved**: Full history maintained through `git subtree split`
- **Presentation functionality unchanged**: Works identically in the new repository
- **Same team ownership**: Content creators maintain both repositories

## Related

- [ADR-002][ADR-002]: Static Site Generator Replacement (may affect presentation delivery approach)
- [ADR-003][ADR-003]: Repository File Structure (affects overall assets organization)
- [ADR-001][ADR-001]: AI Guidelines Structure and Administration Framework

[//]: # (@formatter:off)
<!-- ADR references -->
[ADR-001]: ./ADR-001-ai-guidelines-structure.md
[ADR-002]: ./ADR-002-ssg-replacement.md
[ADR-003]: ./ADR-003-repo-file-structure.md
[ADR-004]: ./ADR-004-presentation-framework.md
[//]: # (@formatter:on)
