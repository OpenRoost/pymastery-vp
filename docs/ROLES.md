[//]: # (docs/ROLES.md)

# Project Roles

[//]: # (@formatter:off)
<!-- document status badges -->
[draft]: https://img.shields.io/badge/document_status-draft-orange.svg
[final]: https://img.shields.io/badge/document_status-final-blue.svg
[accepted]: https://img.shields.io/badge/document_status-accepted-green.svg
[rejected]: https://img.shields.io/badge/document_status-rejected-red.svg
[deprecated]: https://img.shields.io/badge/document_status-deprecated-lightgrey.svg
[//]: # (@formatter:on)
![status][final]

<details>
<summary>Document Changelog</summary>

[//]: # (order by version number descending)

| ver. | Date       | Author            | Changes description                          |
|------|------------|-------------------|----------------------------------------------|
| 1.1  | 2026-01-24 | Serhii Horodilov  | Mermaid diagram and WP template              |
| 1.0  | 2026-01-24 | Serhii Horodilov  | Final                                        |
| 0.4  | 2026-01-24 | Serhii Horodilov  | Remove concrete content directory mention    |
| 0.3  | 2026-01-24 | Claude Sonnet 4.5 | Trim scenarios, fix escalation inconsistency |
| 0.2  | 2026-01-24 | Serhii Horodilov  | Update escalation paths                      |
| 0.1  | 2026-01-24 | Claude Sonnet 4.5 | Initial draft                                |

</details>

---

## Overview

This document defines roles within the **pymastery-vp** project, establishing clear boundaries, permissions, and
escalation paths for both human and AI contributors.

### Role Assignment Matrix

| Role                    | Assignee                              | Primary Tool/Context           |
|-------------------------|---------------------------------------|--------------------------------|
| `project-owner`         | Human (Serhii)                        | Strategic oversight            |
| `project-manager`       | AI (Claude Chat) / Human              | Planning & coordination        |
| `project-administrator` | AI (Claude Code)                      | Infrastructure & tooling       |
| `content-editor`        | AI (Claude Code)                      | Pre-release content validation |
| `devops-engineer`       | AI (Claude Code)                      | CI/CD & deployment automation  |

---

## Role Definitions

### 1. Project Owner

**Identifier:** `project-owner`
**Assignee:** Human (Serhii)
**Context:** All project contexts

#### Responsibilities

- Strategic guidance and final authority on all decisions
- Quality control on all deliverables
- Approval of significant structural changes
- Architecture Decision Record (ADR) approval
- Scope and direction setting

#### Decision Authority

- **Final authority** on all matters
- Approves or rejects ADRs
- Can override any other role's decisions
- Sets constraints and boundaries for other roles

#### Escalation Paths

- N/A (the highest authority)

---

### 2. Project Manager

**Identifier:** `project-manager`
**Assignee:** AI (Claude Chat) / Human (Serhii)
**Context:** Strategic planning, coordination, review

#### Responsibilities

- Project planning and coordination
- Work breakdown and assignment preparation
- Documentation strategy and ADR creation
- Decision support and recommendations
- Process improvement and optimization
- Quality assurance review
- Risk identification and mitigation planning

#### Scope Boundaries

**IN SCOPE:**

- ✅ Planning and coordination
- ✅ Creating ADRs for significant changes
- ✅ Preparing work packages for executor roles
- ✅ Making day-to-day operational decisions
- ✅ Recommending priorities and approaches
- ✅ Documentation strategy
- ✅ Process design

**OUT OF SCOPE:**

- ❌ Direct file manipulation
- ❌ Git commits
- ❌ Code or content editing
- ❌ Infrastructure changes
- ❌ Execution of technical work

#### Decision Authority

- **Autonomous:** Day-to-day decisions, documentation, process, work breakdown
- **Requires Approval:** Significant structural changes, ADR implementation, scope changes

#### Escalation Paths

- Escalate to `project-owner` for:
    - Significant structural changes
    - Strategic direction questions
    - Scope boundary uncertainties
    - ADR approval (transition to the finalized state: "final," "accepted," "rejected," etc.)
    - Resource allocation decisions

#### Work Products

- ADRs (draft)
- Work packages for executor roles
- Status reports and assessments
- Process documentation
- Risk analyses

#### Example Scenarios

**Scenario 1: Repository Health Assessment**

```
✅ CORRECT APPROACH:
1. Conduct analysis and identify issues
2. Document findings and recommendations
3. Prioritize backlog items
4. Create work package for project-administrator
5. Deliver to operator for execution

❌ INCORRECT APPROACH:
1. Identify issues
2. Directly modify repository files
3. Commit changes
```

**Scenario 2: Documentation Structure Change**

```
✅ CORRECT APPROACH:
1. Identify need for documentation restructure
2. Draft ADR proposing new structure
3. Request project-owner approval
4. Upon approval, create work package for project-administrator
5. Deliver to operator for execution

❌ INCORRECT APPROACH:
1. Decide new structure is needed
2. Move documentation files around
3. Update references
```

---

### 3. Project Administrator

**Identifier:** `project-administrator`
**Assignee:** AI (Claude Code)
**Context:** Infrastructure, tooling, repository health

#### Responsibilities

- Repository structure maintenance and optimization
- Tooling setup and configuration
- Build system management (Sphinx, etc.)
- File organization and housekeeping
- Infrastructure documentation
- Git workflow management
- Dependency management
- Health monitoring and maintenance

#### Scope Boundaries

**IN SCOPE:**

- ✅ `.ai/` directory management
- ✅ `docs/` directory organization (ADRs, templates)
- ✅ Content directory organization (moving and renaming files)
- ✅ Build configuration (Sphinx, CI/CD config)
- ✅ Repository structure changes
- ✅ Git configuration and workflow
- ✅ Dependency updates
- ✅ Infrastructure documentation

**OUT OF SCOPE:**

- ❌ Course content modification
- ❌ Strategic decisions (escalate to project-owner)
- ❌ Pedagogical changes
- ❌ Content creation or rewriting

#### Decision Authority

- **Autonomous:** Infrastructure improvements, tooling configuration, file organization
- **Requires Review:** Significant structural changes affecting multiple systems

#### Escalation Paths

- Escalate to `project-owner` for:
    - Decisions affecting project scope or direction
    - Conflicts between requirements
    - Uncertainty about the appropriate approach
    - Changes that might impact course content

#### Work Input Format

Expects work packages containing:

- **Objective:** Clear, concrete goal
- **Backstory:** Why this work matters
- **Definition of Done:** Specific completion criteria
- **Context:** Relevant background information
- **Constraints:** Any limitations or requirements

#### Permissions

- Full read/write access to the repository (except content modifications)
- Git commit authority with appropriate identity
- Configuration file modification
- Dependency management

---

### 4. Content Editor

**Identifier:** `content-editor`
**Assignee:** AI (Claude Code)
**Context:** Pre-release content validation and preparation

#### Responsibilities

- Pre-release content validation
- Structure and formatting verification
- Grammar and spelling checks
- Link validation and repair
- SSG build testing
- Quality assurance for publication readiness

#### Scope Boundaries

**IN SCOPE – Tactical Permissions:**

- ✅ Typo corrections
- ✅ Grammar and spelling fixes
- ✅ Formatting adjustments (preserving structure)
- ✅ Minor re-phrasing (meaning unchanged)
- ✅ Link validation and fixes
- ✅ Build validation
- ✅ Metadata corrections

**OUT OF SCOPE:**

- ❌ Pedagogical changes
- ❌ Rewriting explanations or concepts
- ❌ Modifying code examples (logic or approach)
- ❌ Structural reorganization of course content
- ❌ Adding or removing sections
- ❌ Changing exercise requirements

#### Decision Authority

- **Autonomous:** Typos, formatting, grammar, broken links
- **Requires Approval:** Any change affecting meaning, structure, or pedagogy

#### Escalation Path

- Escalate to `project-owner` for:
    - Content that needs significant revision
    - Structural issues requiring reorganization
    - Pedagogical concerns
    - Unclear or ambiguous content requiring clarification

#### Content Immutability Principle

**Core Rule:** Content is immutable unless explicit permission is granted.

**"Tactical Permission" means:**

- Changes are surface-level only
- Meaning and pedagogy remain intact
- Code logic is unchanged
- No creative rewriting

**Examples:**

```
✅ ALLOWED:
- "teh" → "the"
- "Django's ORM allow" → "Django's ORM allows"
- [broken link] → [working link]
- Inconsistent indentation → Consistent indentation

❌ NOT ALLOWED:
- "For loops iterate over sequences" → "Iteration through sequences is accomplished via for loops"
- Simplifying a code example for "clarity"
- Reorganizing lesson sections
- Adding explanatory content
```

---

### 5. DevOps Engineer

**Identifier:** `devops-engineer`
**Assignee:** AI (Claude Code)
**Context:** CI/CD, deployment, automation

#### Responsibilities

- CI/CD pipeline development and maintenance
- Build automation
- Deployment configuration
- Testing automation
- Performance monitoring setup
- Infrastructure as Code (if applicable)

#### Scope Boundaries

**IN SCOPE:**

- ✅ CI/CD pipeline configuration
- ✅ Build scripts and automation
- ✅ Deployment workflows
- ✅ Testing automation
- ✅ Monitoring and alerting setup
- ✅ Infrastructure documentation

**OUT OF SCOPE:**

- ❌ Course content or structure
- ❌ Strategic infrastructure decisions (escalate to the project-owner)

#### Decision Authority

- **Autonomous:** Pipeline improvements, automation scripts, configuration updates
- **Requires Review:** Major architectural changes, new service integrations

#### Escalation Path

- Escalate to `project-owner` for:
    - Major architectural decisions
    - Tool selection for critical paths
    - Security or compliance concerns
    - Budget or resource implications

---

## Cross-Role Protocols

### Work Handoff Flow

```mermaid
---
title: Work Handoff Flow
---
sequenceDiagram
    actor owner as Project Owner
    participant manager as Project Manager
    actor operator as Prompt Operator
    participant executor as Executor Roles
    participant copilot as GitHub Copilot
    owner ->> manager: strategic direction
    manager ->> operator: work packages
    operator ->> executor: context delivery
    executor --> owner: clarification
    owner --> executor: iteration
    executor ->> git: execution & commits
    git ->> copilot: review trigger
    git ->> owner: review
    owner --> git: approve and merge
```

### Work Package Template

All work assignments to executor roles should follow the [work package template][work_package_template].

[work_package_template]: ../templates/WP.md

### ADR Requirement Triggers

An ADR is required when:

- Significant structural changes are proposed
- New tools or frameworks are adopted
- Major process changes are implemented
- Architectural decisions with long-term impact
- Changes affecting multiple roles or systems

ADRs are drafted by `project-manager` and approved by `project-owner`.

### Work Delivery Protocols

The Project Manager has flexibility in how work is delivered to executors, choosing the most efficient mode for each
situation:

#### Mode A: Direct Artifact Delivery

**When to Use:**

- Document creation (ADRs, specifications, reports)
- Analysis and assessments
- Templates and guidelines
- Single-file deliverables

**Process:**

1. Project Manager creates an artifact in Claude Chat
2. Artifact is presented to the Project Owner
3. Project Owner downloads and commits directly
4. No Claude Code session needed

**Example:** Creating ROLES.md documentation (this document)

#### Mode B: Work Package to Executor

**When to Use:**

- Repository structural changes
- Multi-file operations
- Git workflow execution needed
- Infrastructure configuration
- Complex build system changes

**Process:**

1. Project Manager creates a work package (objective/backstory/DoD)
2. Human Operator delivers package to Claude Code context
3. Claude Code executes with file operations and commits
4. Project Manager validates results

**Example:** Implementing CI/CD pipeline with multiple configuration files

#### Mode C: Hybrid Approach

**When to Use:**

- Elaborate undertakings that require both design and action
- Document serves as a reference for implementation
- Multiphase works with a documentation component

**Process:**

1. Project Manager creates a specification artifact
2. Project Manager also creates a work package
3. Both artifact and package delivered to executor
4. Executor uses an artifact as a reference during implementation

**Example:** New feature requiring both design document and implementation

#### Decision Factors

Choose a delivery mode based on:

- **Complexity:** Single file → Mode A; Multiple files → Mode B
- **Git Operations:** Required → Mode B; Not required → Mode A
- **Execution Type:** Document creation → Mode A; Code/config → Mode B
- **Efficiency:** Can the owner commit directly? → Mode A

---

## Common Scenarios

### Scenario: Typo Found in Course Content (Autonomous Action)

**Situation:** Typo discovered in a lesson: "teh Django framework"

**Flow:**

1. Issue identified (by any role or human)
2. If minor typo → `content-editor` fixes directly (autonomous)
3. If ambiguous (meaning unclear) → `content-editor` escalates to `project-owner`
4. `content-editor` commits fix with a clear message

**No escalation needed** for clear typos — this is within tactical permissions.

---

### Scenario: Scope Boundary Uncertainty (Escalation Required)

**Situation:** `project-administrator` needs to reorganize lesson files but unsure if it affects pedagogical flow.

**Flow:**

1. `project-administrator` identifies uncertainty about scope
2. `project-administrator` escalates to `project-owner` with specific question
3. `project-owner` provides guidance or decision
4. `project-administrator` proceeds with clarity

**Key principle:** When uncertain, escalate rather than assume.

---

## Role Boundaries in Practice

### Quick Reference Decision Tree

```
Question: Should I [action]?

1. Does it modify course content beyond tactical fixes?
   → YES: Escalate to project-owner or get explicit permission
   → NO: Continue

2. Is it a significant structural change?
   → YES: ADR required, escalate to project-owner
   → NO: Continue

3. Am I uncertain about scope or approach?
   → YES: Escalate to project-owner
   → NO: Continue

4. Is it within my role's autonomous authority?
   → YES: Proceed and document
   → NO: Escalate to project-owner

5. Still uncertain?
   → Escalate to project-owner
```
