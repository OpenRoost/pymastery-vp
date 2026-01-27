[//]: # (docs/ADR-004-presentation-framework.md)

# ADR-004: Presentation Framework Handling

[//]: # (@formatter:off)
<!-- document status badges -->
[draft]: https://img.shields.io/badge/document_status-draft-orange.svg
[final]: https://img.shields.io/badge/document_status-final-blue.svg
[accepted]: https://img.shields.io/badge/document_status-accepted-green.svg
[rejected]: https://img.shields.io/badge/document_status-rejected-red.svg
[deprecated]: https://img.shields.io/badge/document_status-deprecated-lightgrey.svg
[//]: # (@formatter:on)
![status][accepted]

<details>
<summary>Document Changelog</summary>

[//]: # (order by version number descending)

| ver. | Date       | Author                                    | Changes description                               |
|------|------------|-------------------------------------------|---------------------------------------------------|
| 1.0  | 2026-01-27 | Serhii Horodilov                          | Accepted                                          |
| 0.6  | 2026-01-27 | Claude Sonnet 4.5 <noreply@anthropic.com> | Final review improvements and status finalization |
| 0.5  | 2026-01-27 | Serhii Horodilov                          | Fix typos and formatting                          |
| 0.4  | 2026-01-27 | Claude Sonnet 4.5 <noreply@anthropic.com> | Technical corrections per implementation review   |
| 0.3  | 2026-01-26 | Claude Sonnet 4.5 <noreply@anthropic.com> | Complete final draft                              |
| 0.2  | 2026-01-25 | Serhii Horodilov                          | Fix links and typos                               |
| 0.1  | 2026-01-25 | Claude Sonnet 4.5 <noreply@anthropic.com> | Initial draft                                     |

</details>

> [!NOTE]
> **Scope of PR #238**: This ADR is part of a comprehensive documentation effort that introduces three interdependent
> Architecture Decision Records:
> - **ADR-002**: Static Site Generator Replacement (Sphinx → MkDocs)
> - **ADR-003**: Repository File Structure (locale-based content organization)
> - **ADR-004** (this document): Presentation Framework Handling (impress.js submodule → npm dependency)
>
> These ADRs address related aspects of the project's documentation infrastructure and should be reviewed together
> to understand the full scope of changes being proposed.

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

**Decision:** **Option 5 (npm Dependency)** - Lowest-effort implementation

**Findings:**

After verification, exactly **one presentation exists** in the repository at `src/rdbms/presentations/normalization/`,
with impress.js imported via `src/conf.js` webpack entry point.

**Rationale:**

1. **Keep presentation in the main repository**: No need for a separate submodule or repository
2. **Remove impress.js git submodule**: Eliminates submodule management complexity
3. **Add impress.js as npm build dependency**: Modern dependency management using existing package.json
4. **Leverage existing webpack setup**: Zero new tooling required; webpack 5 already configured

**Why Option 5 (npm Dependency):**

- **Matches existing pattern**: Project already uses npm for dependencies (mermaid@10.8.0, webpack tooling)
- **Leverages existing infrastructure**: Webpack 5 + html-webpack-plugin already in place
- **Self-contained development**: Works offline (unlike the CDN approach in Option 2)
- **Version locked**: package-lock.json ensures consistency across environments
- **Minimal implementation effort**: Single line addition to package.json + import path update
- **Future-proof**: If more presentations are added, infrastructure is ready
- **Eliminates submodule complexity**: Standard git workflow, no `--recurse-submodules` needed
- **Standard workflow**: Contributors already familiar with npm (mermaid pattern)

**Implementation Approach, Lowest Effort:**

1. Add `impress.js` as dependency to package.json (one line: `npm install impress.js --save`)
2. Update webpack entry point imports (change `import '../assets/impress.js/...'` to `import 'impress.js'` in
   `src/conf.js`)
3. Remove git submodule at `/assets/impress.js/` (`git submodule deinit` and `git rm`)
4. Verify webpack bundles impress.js correctly (npm run build)
5. Test presentation functionality with bundled output

**Dependencies Resolved:**

This decision removes the blocker for ADR-002 (SSG Replacement), as presentation requirements are now clarified and
asset organization is simplified.

## Consequences

**Based on Chosen Option 5 (npm Dependency):**

### Positive

- **Eliminates submodule complexity**: Standard git workflow without special commands
- **Faster clone times**: No submodule to recurse, improved contributor onboarding experience
- **Modern dependency management**: Uses npm ecosystem properly, matches an existing mermaid pattern
- **Clearer project structure**: Assets directory contains only actual course assets
- **Reduced maintenance burden**: npm handles library updates via standard tooling
- **Version control**: Exact version pinning in package.json and package-lock.json
- **Self-contained development**: Works offline during development (unlike the CDN approach)
- **Minimal migration effort**: Leverages existing webpack configuration, zero new tooling
- **No learning curve**: Contributors already familiar with npm workflow from mermaid

### Negative

- **One-time migration effort**: update presentation imports and remove submodule
- **Dependency added**: Project now has one additional npm dependency (minimal impact)
- **Build process required**: Presentation now requires `npm install` and webpack build (already required for mermaid
  and other assets)

### Neutral

- **No impact on course content**: Single presentation continues to work identically after migration
- **Future presentations enabled**: Infrastructure ready if more presentations are added
- **Webpack requirement unchanged**: Build tooling already in place for existing dependencies

## Implementation

**Chosen Implementation: Option 5 (npm Dependency), Lowest Effort Approach**

### Step-by-Step Implementation:

**1. Add impress.js as `npm` dependency:**

```bash
npm install impress.js --save
```

This updates `package.json`:

```json
{
    "dependencies": {
        "@mermaid-js/mermaid-cli": "^10.8.0",
        "mermaid": "^10.8.0",
        "impress.js": "^2.0.0"
    }
}
```

> [!NOTE]
> The caret range (`^2.0.0`) allows patch and minor updates (2.0.x, 2.x.x) while `package-lock.json` locks the exact
> installed version. For stricter control, use an exact version without a caret (e.g., `"2.0.0"`).

**2. Update webpack entry point imports** (in `src/conf.js` or presentation entry point):

**Before** (git submodule reference):

```javascript
// In src/conf.js or presentation entry point
import '../assets/impress.js/js/impress.js';
```

**After** (npm package import):

```javascript
// In src/conf.js or presentation entry point
import 'impress.js';
```

> [!IMPORTANT]
> **Technical Detail:** Presentation HTML files that are part of the webpack build reference webpack's **bundled
> output** (currently `_build/webpack/js/main.bundle.js`, per `webpack.config.js`), not npm package names directly.
> Changes are made to JavaScript import statements in webpack entry points, which webpack then bundles for browser
> consumption. Legacy standalone presentations (for example
> `src/rdbms/presentations/normalization.html`) may still reference `assets/impress.js/...` directly; they are not
> wired through the webpack bundle and should be updated or deprecated separately if needed.

> [!NOTE]
> **CSS Dependencies:** If the presentation uses impress.js CSS files, update those import paths as well following the
> same pattern. For example:
> - Before: `import '../assets/impress.js/css/impress-common.css';`
> - After: `import 'impress.js/css/impress-common.css';`

**3. Remove git submodule:**

```bash
git submodule deinit assets/impress.js
git rm assets/impress.js
rm -rf .git/modules/assets/impress.js
```

**4. Verify build:**

```bash
npm install
npm run build
```

Existing webpack configuration will automatically bundle impress.js with the presentation.

**5. Test presentation:**

```bash
npm start  # Development server
# OR
npm run build  # Production build
```

**Verification Checklist:**

- ✅ Presentation loads without console errors
- ✅ Slide navigation works (arrow keys, click targets)
- ✅ Visual styles render correctly (no missing CSS)
- ✅ Transitions/animations function as expected
- ✅ No webpack bundling errors in build output

---

### Technical Details:

- **Webpack configuration**: No changes required, existing html-webpack-plugin handles bundling
- **Version pinning**: Locked to a specific version in package-lock.json (e.g., `impress.js@2.0.0`)
- **Build output**: Presentation HTML with bundled impress.js in the output directory
- **Development workflow**: `npm start` for dev server, `npm run build` for production

---

### Rollback Plan:

If issues arise during or after implementation, use one of the following rollback strategies:

**Option 1: Temporary CDN Reference** (Quick fix for broken presentation):

```html

<script src="https://cdn.jsdelivr.net/gh/impress/impress.js@2.0.0/js/impress.js"></script>
```

This provides immediate functionality while investigating npm/webpack issues.

**Option 2: Restore Git Submodule** (If npm approach is fundamentally incompatible):

```bash
git revert [commit-hash]  # Revert the submodule removal commit
git submodule update --init --recursive
```

This restores the original submodule-based setup completely.

---

### Success Criteria:

- ✅ Git submodule removed, standard git workflow restored
- ✅ Presentation loads and functions correctly
- ✅ Build process completes without errors
- ✅ No git submodule references remain
- ✅ package.json and package-lock.json updated
- ✅ Contributors can clone and build without additional steps beyond standard `npm install`

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
