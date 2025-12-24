# AI Assistant Guidelines for Stellar-Provenance

## Overview

The Stellar-Provenance project embraces artificial intelligence tools as collaborative partners in development. These guidelines establish clear expectations for responsible, transparent use of AI assistants like GitHub Copilot, ChatGPT, Claude, and similar tools within our project.

**Core Principle:** AI-generated code and content are permitted when reviewed, tested, and properly attributed. We trust developer judgment while maintaining quality and transparency standards.

---

## 1. Acceptable Uses of AI Tools

### ✅ Permitted Activities

- **Scaffolding & Boilerplate:** Use AI to generate initial project structure, configuration files, and common patterns
- **Drafts & Prototypes:** Create experimental code, proof-of-concept implementations, and draft documentation
- **Testing & Validation:** Generate unit tests, integration tests, and test fixtures
- **Documentation:** Draft API documentation, README sections, and inline code comments
- **Refactoring Suggestions:** Ask AI to suggest improvements to existing code patterns
- **Research & Learning:** Use AI to understand new technologies, libraries, and frameworks
- **Code Review:** Use AI as a second reviewer to catch potential issues, security concerns, or improvements
- **Accessibility Improvements:** Generate alt text, ARIA labels, and accessibility enhancements
- **Localization Support:** Assist with translation and internationalization (i18n) content

### ❌ Prohibited Activities

- **Copying Protected Content:** Do not copy substantial portions of code or text from external sources without attribution
- **Credentials & Secrets:** Never use AI to generate, test, or process API keys, passwords, or sensitive data
- **Licensing Violations:** Do not use AI to circumvent intellectual property rights or licensing terms
- **Security Shortcuts:** Do not skip security reviews or testing by relying solely on AI-generated security code
- **Unattributed External Code:** Never pass off AI output as original when it's derived from specific open-source projects
- **Bypassing Human Review:** Do not commit AI-generated code without human review and testing

---

## 2. AI Usage in Pull Requests

### Disclosure Requirements

All contributions using AI assistance must include transparent disclosure in the PR description:

**Format:**
```markdown
## AI Assistance Disclosure

**Tools Used:** [e.g., GitHub Copilot, ChatGPT v4, Claude 3]

**AI Usage Summary:**
- Component/Feature: [e.g., User authentication module]
- Prompt Used: [Include the specific prompt or query]
- Extent of Use: [e.g., "Generated initial scaffold, manually refactored for our architecture"]
- Testing Added: [Describe tests written to validate AI-generated code]
```

### Example

```markdown
## AI Assistance Disclosure

**Tools Used:** GitHub Copilot

**AI Usage Summary:**
- Component: Rate limiting middleware
- Prompt: "Create an Express.js middleware for rate limiting with configurable thresholds and Redis support"
- Extent: Generated initial implementation; refactored to use our custom caching layer and added error handling
- Testing: Added 12 unit tests covering normal operation, edge cases, and failure scenarios
```

### Review Expectations

- Reviewers should scrutinize AI-generated code with the same rigor as human-written code
- PR authors are responsible for understanding and validating all AI-generated content
- AI disclosure does not exempt code from code review requirements

---

## 3. Code Quality Standards for AI-Generated Code

### Testing Requirements

- **Unit Tests:** Minimum 80% code coverage for AI-generated functionality
- **Integration Tests:** Test interactions with other system components
- **Edge Cases:** Include tests for boundary conditions and error scenarios
- **Real-World Scenarios:** Validate against actual use cases, not just happy paths

### Documentation Requirements

- **Function/Method Comments:** Every function must have clear docstrings explaining purpose, parameters, and return values
- **Complex Logic:** Add inline comments explaining non-obvious algorithms or decisions
- **Setup Instructions:** Document any dependencies, configuration, or initialization steps
- **Known Limitations:** Explicitly document limitations or known issues in generated code

### Code Standards

- Follow project style guides and linting rules (ESLint, Prettier, Black, etc.)
- Maintain consistency with existing code patterns in the repository
- Ensure generated code meets accessibility and security standards
- Performance should be validated against project benchmarks

---

## 4. Intellectual Property & Attribution

### External Content

If AI-generated code is substantially derived from a specific open-source project:

1. **Cite the Source:** Include a comment referencing the original project and license
2. **Verify License Compatibility:** Ensure the original license is compatible with our MIT license
3. **Include Attribution:** Add source attribution in code comments or CHANGELOG

**Example:**
```javascript
// Adapted from: https://github.com/example/project (MIT License)
// Original implementation modified for Stellar-Provenance architecture
function customLogic() {
  // Implementation here
}
```

### Originality Verification

- For significant AI-generated features, run plagiarism detection tools if available
- Compare output against popular libraries in the same domain
- Be especially cautious with domain-specific implementations

---

## 5. Security Considerations

### Security-Critical Code

- **Never rely solely on AI for security-critical functionality** (authentication, encryption, authorization)
- Have security experts review AI-generated security code
- Run SAST (Static Application Security Testing) tools on generated code
- Conduct penetration testing for authentication/authorization systems

### Sensitive Data

- Do not use AI tools to process, analyze, or generate code for personal or sensitive user data
- Do not train project-specific models on proprietary code without explicit permission
- Verify that AI tool providers' data retention policies meet compliance requirements

### Dependencies

- Validate that AI-suggested packages are from trusted sources
- Check package security scores and vulnerability databases
- Verify license compatibility before including dependencies

---

## 6. Best Practices for AI Collaboration

### Before Using AI

1. **Clarify Requirements:** Write clear specifications before asking AI to generate code
2. **Provide Context:** Share relevant code snippets and architectural patterns
3. **Set Constraints:** Specify language, framework, and style requirements
4. **Define Success:** Describe what "done" looks like

### During AI Generation

1. **Iterate & Refine:** Ask follow-up questions to improve output
2. **Request Explanations:** Ask the AI to explain what the code does
3. **Verify Logic:** Ensure the approach aligns with project architecture
4. **Ask for Edge Cases:** Request test scenarios and error handling

### After Generation

1. **Review Thoroughly:** Read and understand every line of generated code
2. **Test Comprehensively:** Write tests before considering it production-ready
3. **Refactor as Needed:** Improve clarity, performance, or maintainability
4. **Document Decisions:** Explain why you accepted or modified AI suggestions

### Example: Good AI Collaboration

```
❌ Bad: "Write a login function"

✅ Good: "Write an async Express.js route handler for user login that:
- Validates email format using our EmailValidator utility
- Uses bcrypt for password comparison (config from env vars)
- Returns JWT tokens matching our auth schema
- Includes rate limiting for failed attempts
- Logs security events to our monitoring service"
```

---

## 7. When NOT to Use AI

- **Complex Business Logic:** Algorithms that encode domain-specific rules should be human-written
- **Architectural Decisions:** System design requires human expertise and judgment
- **Security Protocols:** Authentication and authorization should be carefully designed by humans
- **Critical Path Code:** Core functionality affecting data integrity or user safety
- **Compliance Code:** Features subject to regulatory requirements (GDPR, SOC 2, etc.)

---

## 8. Transparency & Team Communication

### Team Discussions

- Share how you used AI in team standups or communication channels
- Discuss lessons learned when AI-assisted code needs rework
- Help team members learn effective AI collaboration techniques
- Share useful prompts and approaches with the team

### Documentation Updates

- Update project documentation if AI tools become standard practice
- Document approved AI workflows in your contribution guide
- Create templates for common AI-assisted tasks (e.g., test generation)

---

## 9. Compliance & Legal

### License Compliance

The Stellar-Provenance project is licensed under **MIT**. Ensure all AI-generated code complies with:

- MIT License requirements
- Compatibility with dependencies' licenses
- No incorporation of GPL-licensed code without appropriate licensing changes

### Data Privacy

- Do not submit proprietary code to public AI services without approval
- Assume all queries to cloud-based AI tools may be logged
- For sensitive features, use locally-run AI tools or approved enterprise versions

### Attribution

Maintain a `CONTRIBUTORS.md` and changelog that documents:
- Human contributors and their work
- Use of AI tools in significant features
- External sources or inspirations for implementations

---

## 10. Questions & Escalation

### When to Ask

If uncertain about whether a specific AI usage aligns with these guidelines:

1. **Ask in a Discussion:** Open a GitHub Discussion with the `ai-usage` label
2. **Consult Maintainers:** Reach out to project maintainers before committing
3. **Get Feedback:** Request code review input from experienced contributors

### Common Questions

**Q: Can I use AI to write tests?**  
A: Yes, but verify test coverage and add edge cases manually. Run tests to ensure they fail when code changes.

**Q: Is AI-generated documentation acceptable?**  
A: Yes, as draft. Have humans review and ensure accuracy before merging.

**Q: What if I use code from a Stack Overflow post generated by AI?**  
A: Still cite Stack Overflow. If it's substantially derived from AI, note that and verify the approach.

**Q: Can I use AI for database migrations?**  
A: Generate the migration scaffold, but carefully review for correctness and reversibility.

---

## 11. Resources & Tools

### Recommended AI Tools

- **GitHub Copilot:** For in-editor code suggestions
- **ChatGPT / Claude:** For architecture discussion and documentation
- **Tabnine:** For code completion
- **Codeium:** For open-source AI assistance

### Related Documentation

- See `CONTRIBUTING.md` for general contribution guidelines
- Refer to `tools/copilot/prompts.md` for project-specific prompt templates
- Check `tools/copilot/review_checklist.md` for code review criteria

### Further Reading

- [GitHub's AI-Assisted Coding Guidelines](https://docs.github.com/en/github-ae/get-started/using-github-ae-with-ai)
- [Open Source AI Governance Best Practices](https://www.linuxfoundation.org/)
- [Software Engineering Institute on AI in Development](https://www.sei.cmu.edu/)

---

## 12. Changelog & Updates

| Date | Change |
|------|--------|
| 2025-12-23 | Initial publication of comprehensive AI guidelines |

---

## Contact & Feedback

Have suggestions for improving these guidelines? Found an edge case we haven't covered?

- **Discussion:** Open a GitHub Discussion tagged `ai-usage`
- **Issues:** File an issue if a guideline needs clarification
- **Direct Contact:** Reach out to the maintainers

---

**Last Updated:** December 23, 2025  
**Status:** Active & Maintained  
**License:** MIT (Same as Stellar-Provenance)

---

*These guidelines are intended to promote responsible AI usage while enabling innovation and productivity. We appreciate your commitment to transparency and quality.*SISTANT_GUIDELINES.md (short excerpt)
- State that AI tools are allowed for scaffolding and drafts but all AI output must be reviewed.
- Require contributors to add a short “AI usage” note in PR descriptions with the prompt used.
- Prohibit copying external copyrighted text verbatim without attribution.
- Require tests and documentation for any AI‑generated code.
