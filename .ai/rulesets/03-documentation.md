# Documentation

## Documentation types

| Type           | Location     | Purpose                                  |
|----------------|--------------|------------------------------------------|
| Project README | `/README.md` | Overview, quick start                    |
| AI Rules       | `/.ai`       | Agent guidelines                         |
| ADR            | `/docs/`     | Architecture Decision Records            |
| Developer docs | `/docs/`     | Technical and community health documents |

## README file requirements

### Root README.md

**Must** contain:

- Project title and overview
- Quick start guide
- Prerequisites
- Local development setup
- Contributing guidelines link
- License information

## Architecture Decision Records (ADR)

### When to Create

- Tool/technology selection
- Significant structural changes
- Process modifications
- Integration decisions

### ADR Lifecycle

1. **Proposed** → Under discussion
2. **Accepted** → Decision made
3. **Deprecated** → Superseded by another
4. **Rejected** → Not implemented

### Template

Located at: `templates/ADR.md`

## AI Rules Documentation

### Update Triggers
- New tool integration
- Process change
- Role modification
- Scope adjustment

### Review Cycle
- Monthly review for accuracy
- Update after significant changes
- Version in CHANGELOG

## Maintenance Responsibilities

### AI Agent Tasks
- Keep technical docs accurate
- Update setup instructions
- Maintain ADR index
- Fix broken internal links

### Human Tasks
- Approve content-related docs
- Review ADR decisions
- Validate accuracy of guides

## Quality Checklist

- [ ] All code examples are tested
- [ ] Links are valid
- [ ] No outdated information
- [ ] Consistent terminology
- [ ] Clear for the target audience
- [ ] Properly formatted
