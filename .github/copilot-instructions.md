# Copilot Instructions for Stellar-Provenance

Welcome to the Stellar-Provenance project! This document provides guidance for using GitHub Copilot effectively in this codebase.

## Project Overview

Stellar-Provenance is a project focused on [DevOps/Distributed Systems/Blockchain] related to provenance tracking and stellar blockchain integration. Refer to `VISION.md` and the whitepaper for detailed context.

## Key Guidelines

### 1. Code Style and Standards

- Follow the coding standards outlined in `CONTRIBUTING.md`
- Maintain consistency with existing code patterns in the repository
- Ensure all code is well-documented with comments for complex logic

### 2. Documentation

- Update relevant documentation in the `docs/` directory when making changes
- Maintain the `research_notes.md` and `whitepaper.md` for architectural decisions
- Add comments to explain non-obvious code sections

### 3. Testing

- Write tests for new features and bug fixes
- Ensure existing tests continue to pass
- Follow the project's testing conventions

### 4. Commit Messages

- Write clear, descriptive commit messages
- Reference related issues when applicable
- Follow conventional commits format if used in the project

### 5. Code Review

- Refer to `tools/copilot/review_checklist.md` for code review criteria
- Use the prompt templates in `tools/copilot/prompt_templates/` for structured AI assistance
- Follow the review guidelines in `tools/copilot/prompts.md`

## Using Copilot Effectively

### Best Practices

1. **Provide Context**: Include relevant file names, function signatures, and existing code patterns
2. **Be Specific**: Rather than vague requests, describe exactly what you want to achieve
3. **Review Generated Code**: Always review Copilot's suggestions before accepting them
4. **Iterative Refinement**: Use follow-up prompts to refine and improve suggestions
5. **Security First**: Never commit credentials, API keys, or sensitive data

### Common Tasks

#### Implementing a New Feature
- Describe the feature requirements clearly
- Reference related modules and dependencies
- Ask Copilot to follow existing architectural patterns

#### Fixing a Bug
- Provide error messages and stack traces
- Share the problematic code section
- Ask for explanations before accepting fixes

#### Writing Tests
- Specify the testing framework being used
- Reference existing tests as examples
- Ask for edge cases and error scenarios

#### Code Refactoring
- Explain the refactoring goal
- Share the current implementation
- Request improvements maintaining backward compatibility

## Project Structure

```
.
├── .github/                    # GitHub specific files
├── docs/                       # Documentation and research
│   ├── research_notes.md
│   ├── whitepaper.md
│   ├── conversation-history/
│   └── requirements/
├── tools/                      # Development tools and scripts
│   └── copilot/               # Copilot-specific configurations
│       ├── prompts.md
│       ├── review_checklist.md
│       └── prompt_templates/
├── CONTRIBUTING.md            # Contribution guidelines
├── LICENSE                    # Project license
├── README.md                  # Project overview
└── VISION.md                  # Project vision and goals
```

## Important Files to Reference

- **VISION.md**: Project vision, goals, and long-term objectives
- **CONTRIBUTING.md**: Contribution guidelines and standards
- **docs/whitepaper.md**: Technical architecture and specifications
- **docs/research_notes.md**: Research findings and design decisions
- **tools/copilot/review_checklist.md**: Code review criteria

## Getting Help

When asking Copilot for help:

1. Reference this file: "As per `.github/copilot-instructions.md`..."
2. Link to relevant documentation in the `docs/` folder
3. Share code snippets or error messages when applicable
4. Ask for explanations of suggestions to ensure understanding

## Additional Resources

- See `tools/copilot/prompts.md` for example prompts and workflows
- Use templates in `tools/copilot/prompt_templates/` for structured requests
- Refer to `tools/copilot/review_checklist.md` for quality standards

---

Last updated: December 23, 2025
