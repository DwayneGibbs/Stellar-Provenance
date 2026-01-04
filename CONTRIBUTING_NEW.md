# Contributor Onboarding (Quick Start)

Welcome! This is the fastest way to get set up and ready to contribute.

## 1) Prerequisites
- Node.js 18+
- Git
- VS Code (recommended)
- Optional: MetaMask for local/testnet testing

## 2) Clone and Branch
```
git clone https://github.com/DwayneGibbs/Stellar-Provenance.git
cd Stellar-Provenance
git checkout -b feature/short-description
```

## 3) Install Dependencies
```
npm install
npm install --workspaces
```

## 4) Configure Environment
```
cp .env.example .env.local
# Fill in test values as needed (keep secrets out of git)
```

## 5) Run Local Tooling
- Lint/tests (when available): `npm run lint`, `npm test`
- Secret scan: `gitleaks detect --config .gitleaks.toml --source . --redact`

## 6) Make Your Change
- Follow [CONTRIBUTING.md](CONTRIBUTING.md) for standards and workflows.
- Add/Update docs when behavior or setup changes.

## 7) Prepare Your PR
- Fill out the PR template.
- Include test results and gitleaks output.
- Reference related issues.

## Need Help?
- Ask in GitHub Discussions (enable in repo settings) or open a Question issue.
- For security concerns, follow [SECURITY.md](SECURITY.md).
