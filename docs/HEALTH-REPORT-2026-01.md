# Repository Health Report

**Repository:** pymastery-vp
**Date:** 2026-01-24
**Author:** Claude Opus 4.5 (Course Administrator)
**Status:** Decisions Received – Actions Completed

---

## Executive Summary

The repository has a **solid foundation** with well-documented AI guidelines, clear role definitions, and functional
build tooling. However, several administrative gaps need attention:

- **CI/CD is effectively disabled** (manual dispatch only)
- **Git submodules are not initialized** (builds will fail)
- **Legacy content clutters the root directory** (45+ orphan markdown files)
- **Configuration inconsistencies** exist between documentation and reality

**Immediate action required:** Fix submodule configuration before any builds can succeed.

---

## 1. Current State Assessment

### What's Working

| Area                    | Status     | Notes                                                |
|-------------------------|------------|------------------------------------------------------|
| AI Guidelines           | Good       | Well-structured `.ai/` directory with clear rulesets |
| Documentation Structure | Good       | ADR template exists, conventions documented          |
| Build System            | Functional | Makefile, Sphinx, webpack configured correctly       |
| Localization            | Good       | English/Ukrainian support via sphinx-intl            |
| Editor Config           | Good       | Consistent formatting rules defined                  |
| Git Identity            | Good       | Claude agent identities properly configured          |

### What's Not Working

| Area                    | Status       | Impact                                |
|-------------------------|--------------|---------------------------------------|
| CI/CD Workflows         | Disabled     | No automated testing or deployment    |
| Git Submodules          | Empty        | **Build will fail** - missing content |
| Dependency Installation | Not verified | Sphinx not in current environment     |

### Partial/Needs Attention

| Area           | Status       | Issue                                            |
|----------------|--------------|--------------------------------------------------|
| CODEOWNERS     | Incomplete   | Missing coverage for `linux/`, `intro/`, `appx/` |
| Documentation  | Inconsistent | Docs say `README.md`, repo has `README.rst`      |
| GitHub Actions | Outdated     | Using deprecated action versions                 |

---

## 2. Gap Analysis

### 2.1 Critical: Submodule Configuration

**Problem:** All 6 submodules are not initialized (empty directories).

```
Affected submodules:
- problem-sets/          (SSH URL)
- libms-db/              (SSH URL)
- src/spec/blog/         (SSH URL)
- src/spec/giver/        (SSH URL)
- src/spec/libms/        (SSH URL)
- assets/impress.js/     (HTTPS URL)
```

**Risk:** The Sphinx build will fail because `conf.py` references `problem-sets/src`.

**Additional Issue:** 5 of 6 submodules use SSH URLs (`git@github.com:`), which:

- Require SSH key authentication
- Will fail in CI/CD environments without SSH key setup
- Prevent contributors from cloning without SSH configured

**Recommendation:** Convert SSH URLs to HTTPS in `.gitmodules`.

### 2.2 Critical: CI/CD Disabled

**Problem:** Both GitHub Actions workflows have their triggers commented out.

```yaml
# deploy_pages.yml - line 6-8 (commented)
# test_build.yml - line 5-12 (commented)
```

**Current state:** Workflows only run via manual `workflow_dispatch`.

**Additional Issues:**

- `deploy_pages.yml` uploads entire repository (`.`) instead of `_build/`
- `deploy_pages.yml` uses deprecated action versions (v1, v2)
- `test_build.yml` uses `LANGUAGE=en` syntax incompatible with Windows

### 2.3 High: Legacy Content in Root

**Problem:** 45+ markdown files from the upstream Russian course sit in the repository root.

```
lesson01.md through lesson36.md (36 files)
module1.md, module2.md, module3.md (3 files)
tasks_block1.md, tasks_block2.md (2 files)
web_homeworks.md, before_postgres.md, pr_explanation*.md (4+ files)
```

**Evidence these are legacy:**

- Written in Russian (Cyrillic text)
- Not referenced by Sphinx configuration
- Not in `src/` where course content lives

**Risk:** Confuses contributors, inflates repository size, unclear if they should be preserved.

### 2.4 Medium: Configuration Inconsistencies

| Location                             | Issue                                                                                                 |
|--------------------------------------|-------------------------------------------------------------------------------------------------------|
| `pyproject.toml:15`                  | `repository` URL points to `edu-python-course` not `OpenRoost`                                        |
| `.ai/rulesets/03-documentation.md:8` | Says "Project README" is `/README.md` but file is `README.rst`                                        |
| `.github/`                           | Duplicate files: `CONTRIBUTING.md` + `CONTRIBUTING.rst`, `CODE_OF_CONDUCT.md` + `CODE_OF_CONDUCT.rst` |
| `README.rst:103`                     | Links to `./.github/CONTRIBUTING.rst` - path may not resolve on GitHub                                |

### 2.5 Medium: Incomplete CODEOWNERS

**Current coverage:**

```
/src/basics/  - @shorodilov @Bandydan @Jules57
/src/vcs/     - @shorodilov @Bandydan
/src/rdbms/   - @shorodilov @Bandydan
/src/oop/     - @shorodilov
/src/django/  - @PonomaryovVladyslav @shorodilov
/src/deploy/  - @PonomaryovVladyslav @shorodilov
```

**Missing coverage:**

- `/src/linux/`
- `/src/intro/`
- `/src/appx/`
- `/.ai/`
- `/docs/`

### 2.6 Low: Documentation Gaps

- No ADR index/catalog (only ADR-0001 exists)
- No documented process for handling upstream sync
- No release/versioning strategy documented
- Review cycle mentioned in guidelines but no tracking mechanism

---

## 3. Recommended Backlog

### Priority 1: Build-Breaking (Immediate)

| Task                                     | Effort | Requires PM Decision                       |
|------------------------------------------|--------|--------------------------------------------|
| Initialize git submodules                | 5 min  | No                                         |
| Convert submodule URLs from SSH to HTTPS | 15 min | **Yes** - may affect contributor workflows |

### Priority 2: CI/CD Restoration (This Week)

| Task                                        | Effort | Requires PM Decision                     |
|---------------------------------------------|--------|------------------------------------------|
| Re-enable PR test workflow triggers         | 10 min | **Yes** - confirm desired trigger events |
| Fix Windows compatibility in test_build.yml | 30 min | No                                       |
| Update deprecated GitHub Action versions    | 20 min | No                                       |
| Fix deploy artifact path (`.` → `_build/`)  | 10 min | No                                       |

### Priority 3: Cleanup (Near-Term)

| Task                                          | Effort | Requires PM Decision                    |
|-----------------------------------------------|--------|-----------------------------------------|
| Archive or remove legacy Russian lesson files | 1 hr   | **Yes** - preserve, archive, or delete? |
| Remove duplicate .md/.rst community files     | 20 min | **Yes** - which format to keep?         |
| Update pyproject.toml repository URL          | 5 min  | No                                      |
| Update CODEOWNERS for missing sections        | 15 min | **Yes** - who owns linux/intro/appx?    |

### Priority 4: Documentation (Ongoing)

| Task                                               | Effort | Requires PM Decision               |
|----------------------------------------------------|--------|------------------------------------|
| Create ADR for SSG replacement evaluation          | 1 hr   | **Yes** - if evaluation is ongoing |
| Document upstream sync process                     | 30 min | No                                 |
| Align README documentation with actual file format | 15 min | No                                 |

### Priority 5: Automation Opportunities

| Task                             | Effort | Requires PM Decision |
|----------------------------------|--------|----------------------|
| Add link checker to CI           | 1 hr   | No                   |
| Add spelling/grammar check to CI | 2 hr   | No                   |
| Create dependabot configuration  | 30 min | No                   |
| Add PR template                  | 30 min | No                   |

---

## 4. Blockers & Decisions Needed

### Decisions Required from PM

1. **Submodule URLs:** Convert SSH to HTTPS?
    - SSH requires key setup for all contributors
    - HTTPS works universally but may affect existing workflows

2. **Legacy Russian content:** What to do with 45+ lesson files in root?
    - Option A: Delete (they're in upstream anyway)
    - Option B: Move to `_legacy/` directory
    - Option C: Keep as-is (not recommended)

3. **CI/CD triggers:** Which events should trigger workflows?
    - `test_build.yml`: PRs to which branches? All paths or just src/?
    - `deploy_pages.yml`: Push to main/devel? Only manual?

4. **CODEOWNERS expansion:** Who owns uncovered sections?
    - `linux/`, `intro/`, `appx/` need owners assigned

5. **Documentation format:** Keep `.md` or `.rst` for community files?
    - Currently have both; should consolidate

### Information Needed

- Is there an active SSG replacement evaluation? (mentioned in config.yaml)
- What's the upstream sync cadence/process?
- Are there any pending decisions that need ADRs?

---

## 5. Report Location Justification

This report is placed at `/docs/HEALTH-REPORT-2026-01.md` because:

1. **Follows documentation conventions** - `/docs/` is designated for developer/technical documents
2. **Date-stamped** - Indicates when assessment was performed; can be archived later
3. **Flat structure** - Aligns with documented preference (no subdirectories)
4. **Discoverable** - Lives alongside ADRs for easy reference
5. **Not cluttering root** - Avoids adding more files to already-cluttered root directory

---

## Appendix A: File Structure Snapshot

```
pymastery-vp/
├── .ai/                    # AI guidelines (well-organized)
├── .github/                # GitHub config (needs cleanup)
├── assets/                 # Static assets (submodule empty)
├── docs/                   # Documentation (sparse - 1 ADR)
├── libms-db/               # Submodule (empty)
├── mindmaps/               # Mermaid diagrams (7 files)
├── pictures/               # Course images
├── problem-sets/           # Submodule (empty)
├── src/                    # Course content (protected)
├── templates/              # Document templates
├── lesson*.md (36)         # Legacy Russian content
├── module*.md (3)          # Legacy Russian content
├── *.md (6+)               # Misc legacy files
├── Makefile                # Build automation
├── pyproject.toml          # Python config
├── package.json            # Node.js config
├── webpack.config.js       # Webpack config
├── README.rst              # Project README
├── AGENTS.md / CLAUDE.md   # AI entry points
└── index.html              # Root landing page
```

---

## Appendix B: CI/CD Status Detail

### deploy_pages.yml Issues

```yaml
# Line 64: Uploads entire repo instead of build output
path: "."  # Should be: "_build/"

# Line 57: Deprecated version
uses: actions/configure-pages@v2  # Current: v4

# Line 60: Deprecated version
uses: actions/upload-pages-artifact@v1  # Current: v3

# Line 67: Deprecated version
uses: actions/deploy-pages@v1  # Current: v4
```

### test_build.yml Issues

```yaml
# Line 39-40: Windows incompatible
run: |
    LANGUAGE=en make html  # Won't work on Windows
    LANGUAGE=uk make html

# Should use cross-platform approach:
# - shell: bash (force bash on all platforms)
# - or use matrix.include for OS-specific commands
```

---

## 6. PM Decisions & Actions Taken (2026-01-24)

### Decisions Received

| Item                | Decision                              |
|---------------------|---------------------------------------|
| Submodule URLs      | Convert SSH to HTTPS                  |
| Legacy lesson files | Keep as-is                            |
| CI/CD triggers      | No changes for now (address later)    |
| CODEOWNERS gaps     | Assign `@shorodilov` as default owner |
| Duplicate docs      | Keep `.md` only                       |

### Actions Completed

1. **Converted submodule URLs to HTTPS** (`.gitmodules`)
    - `problem-sets`, `blog-spec`, `libms-db`, `libms-spec`, `giver-spec` converted from SSH to HTTPS
    - Added `.git` suffix for consistency

2. **Updated CODEOWNERS** (`.github/CODEOWNERS`)
    - Added `* @shorodilov` as default rule for all contents
    - Removed previous section-specific ownership rules so that the wildcard rule now applies to all paths

3. **Removed duplicate .rst files** (`.github/`)
    - Deleted `CODE_OF_CONDUCT.rst` (keeping `.md`)
    - Deleted `CONTRIBUTING.rst` (keeping `.md`)

4. **Updated README.rst**
    - Fixed contributing guide link to point to `CONTRIBUTING.md`

### Remaining Items

| Item                     | Status    | Notes                                                     |
|--------------------------|-----------|-----------------------------------------------------------|
| CI/CD restoration        | Deferred  | PM decision: address later                                |
| Legacy lesson files      | No action | PM decision: keep as-is                                   |
| Submodule initialization | Pending   | Run `git submodule update --init --recursive` to populate |

---

*Report generated by Claude Opus 4.5 as part of initial repository assessment.*
*Updated 2026-01-24 with PM decisions and completed actions.*
