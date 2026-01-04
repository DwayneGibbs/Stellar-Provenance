# Security Policy

For detailed security guidance, see [docs/SECURITY.md](docs/SECURITY.md).

## Reporting Vulnerabilities
- Open a GitHub Security Advisory (preferred) or a private issue addressed to maintainers.
- Please include reproduction steps, impact, and any mitigations tried. We aim to acknowledge reports within 72 hours.

## Expectations
- Do not publicly disclose a vulnerability before a fix is available.
- Avoid testing on mainnet; use testnets or local environments.
- Never use real secrets—use test credentials only.

## Pre-PR Checklist (must pass locally before opening a PR)
- Run `gitleaks detect --config .gitleaks.toml --source . --redact` and confirm no secrets leak.
- Run tests and linters as applicable; include results in your PR description.
- Update documentation if security-impacting changes are made.

## Versions Supported
- This project is pre-release; all branches are expected to meet the checks above.
