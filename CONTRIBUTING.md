# Contributing to Stellar-Provenance

Thank you for your interest in contributing to **Stellar-Provenance**! This project is built on a community-first philosophy, and we welcome developers, astronomers, designers, educators, and space enthusiasts of all backgrounds.

This document outlines how you can participate, collaborate, and help shape the future of decentralized astronomy.

---

## 🌌 Before You Start

### Prerequisites
- Familiarity with Git and GitHub
- Node.js 18+ installed
- A text editor (VS Code recommended)
- A Web3 wallet (MetaMask for testing)
- 30 minutes for initial setup

### Required Reading
Please review these documents to understand the project:
1. [`VISION.md`](VISION.md) - Project vision and goals
2. [`TECH_STACK.md`](docs/TECH_STACK.md) - Technology choices
3. [`ROADMAP.md`](docs/ROADMAP.md) - Phase-by-phase plans
4. [`SECURITY.md`](docs/SECURITY.md) - Security requirements

---

## 🛠️ Ways You Can Contribute

### Code Contributions
- Implementing features from the roadmap
- Fixing bugs (see GitHub Issues)
- Improving performance or security
- Writing tests and test coverage
- Refactoring legacy code

### Documentation
- Technical guides and tutorials
- API documentation
- Architecture explanations
- Deployment guides
- Community FAQs

### Research & Astronomy
- Identifying astronomical datasets
- Proposing metadata standards
- Suggesting scientific use cases
- Validating star catalog accuracy

### Community & Content
- Writing blog posts or tutorials
- Creating videos or visual content
- Community management (Discord)
- Event organization
- Design and UI/UX improvements

### Creative Contributions
- Lore and storytelling
- Object descriptions and narratives
- World-building concepts
- Visual mockups and designs

---

## 🚀 Getting Started: Local Development Setup

### Step 1: Clone the Repository

```bash
# Clone the project
git clone https://github.com/DwayneGibbs/Stellar-Provenance.git
cd Stellar-Provenance

# Create your feature branch
git checkout -b feature/your-feature-name
```

### Step 2: Install Dependencies

```bash
# Install root dependencies
npm install

# Install workspace dependencies (monorepo)
npm install --workspaces
```

### Step 3: Setup Environment Variables

```bash
# Copy the environment template
cp .env.example .env.local

# Edit with your values (for testnet development, minimal setup needed)
# Use test values from hardhat node output
```

### Step 4: Start Local Blockchain (for smart contract development)

```bash
cd packages/contracts

# Start local Hardhat network
npx hardhat node

# Output:
# Started HTTP and WebSocket JSON-RPC server at http://127.0.0.1:8545/
# Accounts and private keys will be displayed here

# Keep this terminal running!
```

### Step 5: Deploy Contracts Locally (in new terminal)

```bash
cd packages/contracts

# Deploy to local network
npx hardhat run scripts/deploy.ts --network localhost

# Save the deployed contract addresses
# Add them to packages/frontend/.env.local
```

### Step 6: Start Frontend Development Server

```bash
cd packages/frontend

# Install dependencies
npm install

# Start development server
npm run dev

# Open http://localhost:3000 in your browser
```

### Step 7: Connect MetaMask to Local Network

1. Open MetaMask extension
2. Settings → Networks → Add Network
   - Network Name: `Hardhat Local`
   - RPC URL: `http://localhost:8545`
   - Chain ID: `31337`
   - Currency: `ETH`
3. Add test account (import private key from hardhat output)

---

## 📝 Contributing Code

### Before You Submit

1. **Check for existing issues**
   - Search GitHub Issues first
   - Comment on issues you want to work on
   - Wait for maintainer approval

2. **Create a detailed issue if one doesn't exist**
   - Describe the problem or feature
   - Provide context and examples
   - Suggest a solution if possible

3. **Discuss your approach**
   - For large changes, discuss in Discord first
   - Get feedback before coding
   - Avoid wasted effort on rejected PRs

4. **Run secret scan**
   - Install gitleaks (e.g., `brew install gitleaks` or `choco install gitleaks`)
   - Run `gitleaks detect --config .gitleaks.toml --source . --redact` before opening a PR

### Code Standards

#### Smart Contracts (Solidity)
```solidity
// Follow Solidity style guide
// Use OpenZeppelin contracts for standards
// Write comprehensive comments for complex logic

/// @notice Register a celestial name
/// @param celestialId The ID of the celestial object
/// @param name The desired name for the object
function registerName(string calldata celestialId, string calldata name) public {
    // Implementation
}
```

#### Frontend (React/TypeScript)
```typescript
// Use TypeScript for type safety
// Write functional components with hooks
// Keep components small and focused

interface CelestialNameProps {
  celestialId: string;
  onSuccess?: () => void;
}

export const CelestialNameRegistration: React.FC<CelestialNameProps> = ({
  celestialId,
  onSuccess,
}) => {
  // Implementation
};
```

#### Backend (if contributing)
```typescript
// Use TypeScript
// Write clear, documented functions
// Implement proper error handling

async function registerCelestialName(
  contractAddress: string,
  celestialId: string,
  name: string
): Promise<TransactionReceipt> {
  // Implementation
}
```

### Testing Requirements

#### Smart Contracts
```bash
# Write tests for all new functions
npm run test:contracts

# Aim for 80%+ code coverage
npm run coverage
```

#### Frontend
```bash
# Write tests for new components
npm run test:frontend

# Components should be tested:
npm run test:frontend -- CelestialForm.test.tsx
```

### Git Workflow

```bash
# 1. Create descriptive branch name
git checkout -b feature/add-marketplace-ui

# 2. Make changes in logical commits
git add .
git commit -m "feat: add marketplace listing component"
git commit -m "fix: handle wallet connection errors"

# 3. Keep commits focused (one feature per commit when possible)

# 4. Push to your fork
git push origin feature/add-marketplace-ui

# 5. Open PR on GitHub
```

### Commit Message Format

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <subject>

<body>

<footer>

# Types: feat, fix, docs, style, refactor, test, chore
# Examples:
# feat(contracts): add name transfer functionality
# fix(frontend): resolve wallet connection bug
# docs(security): add audit checklist
```

---

## 📤 Submitting a Pull Request

### PR Checklist
- [ ] Branch created from latest `main`
- [ ] Code changes made
- [ ] Tests written and passing
- [ ] Documentation updated
- [ ] Secret scan (gitleaks) passes
- [ ] No console errors or warnings
- [ ] Commit messages follow format
- [ ] Ready for review

### PR Template
```markdown
## Description
Brief explanation of changes

## Related Issues
Closes #123

## Changes Made
- Change 1
- Change 2

## Testing
- [ ] Tested locally
- [ ] Tests added/updated
- [ ] Manual testing completed

## Screenshots (if applicable)
[Add screenshots here]

## Breaking Changes
None / List any breaking changes

## Deployment Notes
Any special deployment instructions
```

### Review Process
1. **Automated Checks**
   - Linting
   - Tests
   - Security scans

2. **Code Review**
   - At least 1 maintainer review
   - Address feedback
   - Request re-review if needed

3. **Merge**
   - Squash commits (for cleanliness)
   - Merge to `main`
   - Delete branch

---

## 🔍 Code Review Guidelines

### As a Reviewer
- [ ] Code works and solves the problem
- [ ] Follows project standards
- [ ] Tests are comprehensive
- [ ] Documentation is clear
- [ ] No security concerns
- [ ] Performance is acceptable

### As an Author
- [ ] Respond to all feedback
- [ ] Explain reasoning if disagreeing
- [ ] Make requested changes
- [ ] Request re-review when complete

---

## 🌍 Community Standards

### Code of Conduct
We follow these principles:
- **Be respectful** - Treat all members with courtesy
- **Be constructive** - Provide helpful feedback
- **Be transparent** - Communicate openly
- **Assume good intent** - Assume people mean well
- **Be collaborative** - Work together toward solutions

### Reporting Issues
If you experience harassment or misconduct:
- Report to community managers in Discord DMs
- Email: security@stellarprovenance.com (for sensitive issues)
- All reports will be taken seriously and confidentially

---

## 🎯 Contribution Opportunities by Skill Level

### Beginner
- [ ] Fix typos in documentation
- [ ] Improve README clarity
- [ ] Add comments to complex code
- [ ] Create astronomy fact posts
- [ ] Share project on social media

### Intermediate
- [ ] Implement small features from roadmap
- [ ] Fix bugs with clear reproduction steps
- [ ] Write unit tests
- [ ] Create blog post tutorials
- [ ] Improve UI components

### Advanced
- [ ] Design new smart contracts
- [ ] Optimize gas usage
- [ ] Build complex features
- [ ] Lead working groups
- [ ] Conduct security audits

---

## 💰 Contributor Incentives

### Phase 1 (Months 1–2)
- Early contributor Discord role
- 100+ testnet NFTs (for active contributors)
- Spotlight on GitHub/website

### Phase 2 (Months 3–4)
- Developer grants ($500–$5,000)
- Testnet token rewards
- Featured contributor spotlight

### Phase 3 (Months 5+)
- Governance token allocation
- Treasury share from fees
- Permanent DAO roles

---

## 📚 Helpful Resources

### Documentation
- [`TECH_STACK.md`](docs/TECH_STACK.md) - Technology overview
- [`SECURITY.md`](docs/SECURITY.md) - Security best practices
- [`DEPLOYMENT.md`](docs/DEPLOYMENT.md) - Deployment guide
- [`ROADMAP.md`](docs/ROADMAP.md) - Project roadmap

### Learning Resources
- [Hardhat Documentation](https://hardhat.org/)
- [React Documentation](https://react.dev/)
- [Solidity Documentation](https://docs.soliditylang.org/)
- [Ethers.js Guide](https://docs.ethers.org/)
- [Web3 Security Best Practices](https://consensys.github.io/smart-contract-best-practices/)

### Community
- **Discord:** [Coming Soon]
- **GitHub Issues:** For bug reports and feature requests
- **GitHub Discussions:** For ideas and Q&A

---

## 🎓 Development Checklists

### Smart Contract Development
- [ ] Feature specified in roadmap
- [ ] Function signature documented with NatSpec
- [ ] Unit tests written (80%+ coverage)
- [ ] Edge cases tested
- [ ] Gas optimization considered
- [ ] Security audit checklist reviewed
- [ ] Deployed to testnet
- [ ] Verified on Etherscan/PolygonScan

### Frontend Development
- [ ] Component designed before implementation
- [ ] Responsive design (mobile, tablet, desktop)
- [ ] Accessibility tested (a11y)
- [ ] Error handling comprehensive
- [ ] Loading states implemented
- [ ] Unit and integration tests
- [ ] TypeScript types complete
- [ ] Performance optimized

### Documentation Updates
- [ ] Spelling and grammar checked
- [ ] Code examples tested
- [ ] Links verified
- [ ] Formatting consistent
- [ ] Images/diagrams included (if helpful)

---

## 🤝 Getting Help

### Stuck or Have Questions?
1. Check existing documentation and guides
2. Search GitHub Issues for similar problems
3. Ask in Discord #developers channel
4. Create a GitHub Discussion for complex questions

### Report a Bug
1. Create a GitHub Issue with detailed reproduction steps
2. Include: OS, browser, wallet, network
3. Attach error messages and screenshots
4. Label as `bug`

### Suggest a Feature
1. Check ROADMAP.md for planned features
2. Create a GitHub Discussion to gauge community interest
3. If accepted, issues can be created for implementation

---

## 🚀 Next Steps

1. **Read the docs** - Understand the project vision and tech stack
2. **Setup locally** - Follow the setup steps above
3. **Find an issue** - Check GitHub Issues or ROADMAP.md
4. **Ask questions** - Join Discord and ask in #developers
5. **Submit a PR** - Follow the guidelines above
6. **Get feedback** - Iterate based on reviews
7. **Celebrate** - Your contribution helps build something amazing!

---

## ⭐ Thank You

Your contributions help build a shared cosmic commons that blends science, exploration, and Web3. Whether you're writing code, improving documentation, or building community, you're part of something meaningful.

**Together, we map the stars.**

---

**Last Updated:** December 23, 2025  
**Questions?** Reach out in Discord or open a GitHub Discussion.
