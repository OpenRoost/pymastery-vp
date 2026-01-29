# ADR-006: AI Agent Configuration Files

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

| ver. | Date       | Author                                    | Changes description           |
|------|------------|-------------------------------------------|-------------------------------|
| 0.2  | 2026-01-29 | Claude Sonnet 4.5 <noreply@anthropic.com> | Remove implementation section |
| 0.1  | 2026-01-29 | Claude Sonnet 4.5 <noreply@anthropic.com> | Initial draft                 |

</details>

## Context

The project currently defines AI roles in `docs/ROLES.md` - a comprehensive human-readable document that establishes role boundaries, responsibilities, and decision authority. While ROLES.md serves well as conceptual documentation for humans, it is not optimized for consumption by AI tools.

**Current State:**
- Role definitions exist only in ROLES.md
- AI tools must parse narrative documentation to understand their scope
- No machine-readable configuration for role-specific behavior
- Limited compatibility with emerging AI tool ecosystems

**Problem Statement:**

As we expand AI-assisted development workflows, we need a format that:
1. AI tools can consume directly without parsing narrative docs
2. Works across multiple AI platforms (Claude Code, GitHub Copilot, VS Code)
3. Provides granular control over tool access and permissions
4. Maintains consistency with human-readable role definitions

**GitHub Copilot Integration Opportunity:**

GitHub has established a standard format for custom agents (`.agent.md` files in `.github/agents/`), which is supported by GitHub Copilot, VS Code, and potentially other tools. This format provides:
- YAML frontmatter for structured configuration
- Markdown body for agent instructions
- Tool access control mechanisms
- Cross-platform compatibility

## Decision Drivers

- **AI Tool Optimization**: Need format optimized for AI consumption, not just humans
- **Cross-Platform Compatibility**: Support Claude Code, GitHub Copilot, VS Code
- **Tool Access Control**: Explicitly control which tools each agent can use
- **Maintainability**: Single source of truth that's easy to update
- **Standards Alignment**: Follow emerging industry standards (GitHub Copilot spec)
- **DRY Principle**: Don't duplicate role definitions across files
- **Progressive Enhancement**: Start simple, add complexity as needed

## Considered Options

### Option 1: Keep ROLES.md Only (Status Quo)

**Description**: Continue with narrative documentation only, no machine-readable format.

**Pros**:
- No additional work required
- Single file to maintain
- Excellent for human readers

**Cons**:
- AI tools must interpret narrative text (error-prone)
- No cross-platform compatibility
- No tool access control mechanism
- Miss opportunity for industry standard adoption
- Limited scalability as AI ecosystem grows

### Option 2: Create Agent Files Following GitHub Copilot Spec

**Description**: Create `.agent.md` files in `.ai/agents/` with symlink to `.github/agents/` for Copilot compatibility. Each role gets dedicated agent configuration file.

**Pros**:
- Follows emerging industry standard (GitHub Copilot)
- Works with Claude Code, GitHub Copilot, VS Code
- Explicit tool access control via YAML frontmatter
- Machine-readable structured format
- Self-contained agent configurations
- Standards-based approach (future-proof)

**Cons**:
- Additional files to maintain alongside ROLES.md
- Learning curve for new format
- Requires discipline to keep aligned with ROLES.md

### Option 3: Replace ROLES.md with Agent Files

**Description**: Eliminate ROLES.md entirely, consolidate everything into agent files.

**Pros**:
- Single source of truth
- No synchronization concerns
- Fully machine-readable

**Cons**:
- Loses human-friendly overview document
- Harder for humans to understand overall role architecture
- Forces AI-first format on human readers
- Not appropriate for conceptual/strategic documentation

### Option 4: Custom JSON/YAML Configuration Format

**Description**: Design our own agent configuration format in JSON or YAML.

**Pros**:
- Full control over schema
- Can optimize for our specific needs
- Potentially simpler than Markdown+YAML hybrid

**Cons**:
- Not compatible with GitHub Copilot or other tools
- Reinventing the wheel
- No ecosystem support
- Requires custom tooling for consumption
- Isolated from industry standards

## Decision

The chosen option is **Option 2: Create Agent Files Following GitHub Copilot Spec** because:

1. **Standards-Based**: GitHub Copilot's `.agent.md` format is becoming an industry standard, supported by multiple tools (VS Code, GitHub Copilot)

2. **Cross-Platform**: Same files work with Claude Code, GitHub Copilot, and potentially other future AI tools

3. **Best of Both Worlds**: 
   - ROLES.md remains as human-friendly conceptual overview
   - Agent files provide machine-readable operational configuration
   - Clear separation of concerns

4. **Tool Control**: YAML frontmatter enables explicit tool access restrictions per role (security and clarity)

5. **Progressive**: Can start minimal and add complexity as needs evolve

6. **Maintainable**: Clear structure with templates reduces inconsistency risk

**Implementation Approach:**

```
.ai/
├── agents/                         # Source of truth for agent configs
│   ├── project-manager.agent.md
│   ├── project-administrator.agent.md
│   ├── content-editor.agent.md
│   └── devops-engineer.agent.md
├── rulesets/                       # (existing) General guidelines
└── config.yaml                     # (existing) Project-wide context

.github/
└── agents -> ../.ai/agents/        # Symlink for GitHub Copilot

docs/
└── ROLES.md                        # (existing) Human-readable overview
```

**Role Distribution:**
- `docs/ROLES.md`: High-level role definitions for humans, conceptual model
- `.ai/agents/*.agent.md`: Operational instructions for AI tools, executable format

## Consequences

### Positive

- **Cross-Tool Compatibility**: Same agent files work with multiple AI platforms
- **Standards Alignment**: Following GitHub's established format ensures long-term viability
- **Explicit Tool Control**: YAML frontmatter provides clear, auditable tool access rules
- **Better AI Performance**: AI tools consume structured format directly (less ambiguity)
- **Scalability**: Easy to add new agents as project needs evolve
- **Clear Separation**: ROLES.md for humans, agent files for AI - each optimized for audience
- **Maintainability**: Template-based approach reduces inconsistencies
- **Git-Based Versioning**: Agent files versioned via Git SHAs (branch/tag support)

### Negative

- **Maintenance Overhead**: Additional files to keep updated
- **Learning Curve**: Team must understand new format and YAML frontmatter
- **Abstraction Complexity**: Introduces another layer in configuration architecture

### Neutral

- **File Count Increase**: 4-5 new agent files added to repository
- **Documentation Split**: Role information now spans ROLES.md and agent files
- **Migration Effort**: Requires extracting operational instructions from ROLES.md
- **Synchronization Consideration**: ROLES.md and agent files need to stay aligned (could be addressed with file partials in future)

## Related

- [ROLES.md](./ROLES.md): Authoritative human-readable role definitions
- [ADR-001](./ADR-001-ai-guidelines-structure.md): AI Guidelines Structure
- [GitHub Copilot Custom Agents Documentation](https://docs.github.com/en/copilot/reference/custom-agents-configuration)
- Template: `templates/AGENT.md` (to be created)
- Pilot: `.ai/agents/project-manager.agent.md` (to be created)

[//]: # (@formatter:off)
<!-- ADR references -->
[ADR-001]: ./ADR-001-ai-guidelines-structure.md
[ADR-006]: ./ADR-006-ai-agent-configuration-files.md
<!-- External references -->
[copilot-docs]: https://docs.github.com/en/copilot/reference/custom-agents-configuration
[//]: # (@formatter:on)
