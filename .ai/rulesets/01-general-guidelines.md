# General Guidelines

## Core principles

### Content protection

AI agents **MUST NOT** modify course content unless explicitly requested by a human. This includes:

- Lesson text and explanations
- Code examples and snippets
- Exercise instructions
- Quiz questions and answers
- Learning objectives

### Allowed autonomous actions

AI agents **MAY** perform without explicit approval:

- Grammar and spelling corrections
- Formatting fixes (whitespace, line endings, etc.)
- Broken link identification and reporting
- Build configuration updates
- CI/CD pipeline maintenance
- File organization per established conventions

### Decision documentation

All significant decisions **MUST** be documented as ADR (Architecture Decision Record):

- Tool selection (SSG, linters, formatters, etc.)
- Structure changes
- Process modifications
- Integration choices

## Communication standards

### Commit messages

- Each commit **MUST** be a single logical change.
- Each commit **MUST** be accompanied by a meaningful commit message.
- Commit messages **MUST** follow the conventional commit format:
    1. Start with a summary line (max 50 characters)
    2. Leave a blank line after the summary
    3. Add a detailed description with lines wrapped at 72 characters
    4. Use imperative mood ("Add feature" not "Added feature")
    5. Explain what and why, not how
    6. Focus on the changes made in this commit
    7. Use conventional commits

```text
<type>[optional scope]: <summary>

[optional body]

[optional footer(s)]
```

#### Types

| Type       | Usage                                 |
|------------|---------------------------------------|
| `docs`     | Documentation changes                 |
| `fix`      | Bug fixes, corrections                |
| `feat`     | New features, capabilities            |
| `chore`    | Maintenance, housekeeping             |
| `ci`       | CI/CD changes                         |
| `style`    | Formatting, no code change            |
| `refactor` | Restructuring without behavior change |

### Pull request descriptions

- Clear summary of the changes made in the PR
- Link to any relevant issues
- Checklist of verification steps performed
- Impact assessment

## Documentation

- Keep README files current
- Update CHANGELOG for releases
- Maintain accurate setup instructions
- Document all configuration options

## Escalation protocol

When encountering:

1. **Ambiguous requirements** → Ask for clarification
2. **Conflicting guidelines** → Document and request human decision
3. **Security concerns** → Report immediately, do not proceed
4. **Breaking changes** → Create detailed impact analysis
