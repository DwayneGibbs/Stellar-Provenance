# Security & Best Practices

This document outlines security protocols, best practices, and compliance requirements for the Stellar-Provenance project.

---

## 🛡️ Secrets Management

- Do not commit private keys, API tokens, passwords, or database connection strings.
- Use environment files (.env.local, .env) that stay out of version control; start from .env.example where provided.
- CI runs Gitleaks (see .gitleaks.toml) to block secrets in pull requests; run it locally before pushing:

```bash
gitleaks detect --config .gitleaks.toml --source . --redact
```

- If a secret is exposed, rotate it immediately and remove it from git history (for example, using git filter-repo or GitHub’s scrub tools).

## 🔒 Smart Contract Security

### Pre-Deployment Checklist

#### Code Review (Testnet Phase)
- [ ] **Internal Review**
  - Code review by 2+ developers
  - At least 48 hours for review period
  - All comments addressed before commit
  - Security concerns documented
- [ ] **Automated Analysis**
  - Run Slither (static analysis tool)
  - Run MythX (security analysis)
  - Fix all high/critical issues
  - Document medium/low issues with rationale

#### Testing & Validation (Testnet Phase)
- [ ] **Unit Tests**: 80%+ code coverage
  - Test normal flows
  - Test edge cases
  - Test revert conditions
  - Test access control
- [ ] **Integration Tests**: All contract interactions
  - Multi-contract interactions
  - State changes validation
  - Event emission validation
- [ ] **Fuzzing**: Random input testing
  - Use Hardhat's fuzzing capabilities
  - Test state consistency
- [ ] **Gas Optimization**
  - Document estimated gas per operation
  - Optimize hot paths
  - Add gas limit comments

#### Security Audit (Before Mainnet)
- [ ] **Choose Audit Firm**
  - Recommended: Certik, Trail of Bits, OpenZeppelin
  - Budget: $5K–$30K depending on contract complexity
  - Timeline: 2–4 weeks
- [ ] **Prepare for Audit**
  - Document contract purpose and assumptions
  - Provide test suite
  - List known issues/tradeoffs
  - Prepare for questions
- [ ] **Post-Audit Actions**
  - Fix all high/critical issues
  - Address medium issues (or justify why not)
  - Implement audit recommendations
  - Publish audit report

#### Testnet Public Launch
- [ ] Deploy to Sepolia testnet
- [ ] Request public testing (Discord/GitHub)
- [ ] Monitor for issues
- [ ] Run for minimum 2 weeks before mainnet

### Common Solidity Vulnerabilities to Avoid

#### 1. Reentrancy Attacks
```solidity
// ❌ VULNERABLE
function withdraw() public {
    uint amount = balances[msg.sender];
    (bool success, ) = msg.sender.call{value: amount}("");
    require(success);
    balances[msg.sender] = 0;  // State change AFTER external call
}

// ✅ SAFE
function withdraw() public {
    uint amount = balances[msg.sender];
    balances[msg.sender] = 0;  // State change BEFORE external call
    (bool success, ) = msg.sender.call{value: amount}("");
    require(success);
}

// ✅ SAFE WITH GUARD (Alternative)
import "@openzeppelin/contracts/security/ReentrancyGuard.sol";

contract Safe is ReentrancyGuard {
    function withdraw() public nonReentrant {
        // Protected from reentrancy
    }
}
```

#### 2. Integer Overflow/Underflow
```solidity
// Solidity 0.8.0+ has automatic overflow protection
// But be aware when using assembly or older versions

// ✅ SAFE (Solidity 0.8.0+)
uint256 a = type(uint256).max;
uint256 b = a + 1;  // Reverts: Arithmetic overflow
```

#### 3. Access Control Issues
```solidity
// ❌ VULNERABLE
function transferOwnership(address newOwner) public {
    owner = newOwner;  // Anyone can call!
}

// ✅ SAFE
import "@openzeppelin/contracts/access/Ownable.sol";

contract Safe is Ownable {
    // Automatically enforces onlyOwner
    function transferOwnership(address newOwner) public override onlyOwner {
        super.transferOwnership(newOwner);
    }
}

// ✅ SAFE WITH ROLES (Better for DAO)
import "@openzeppelin/contracts/access/AccessControl.sol";

contract Safe is AccessControl {
    bytes32 public constant ADMIN_ROLE = keccak256("ADMIN_ROLE");
    
    modifier onlyAdmin() {
        require(hasRole(ADMIN_ROLE, msg.sender), "Caller is not admin");
        _;
    }
    
    function criticalFunction() public onlyAdmin {
        // Only admin can call
    }
}
```

#### 4. Front-Running Attacks
```solidity
// ❌ VULNERABLE: Public transaction visibility allows front-running
function setPrice(uint256 newPrice) public {
    price = newPrice;  // Attackers see tx in mempool and front-run
}

// ✅ PARTIAL MITIGATION: Use commit-reveal pattern
bytes32 public priceCommit;

function commitPrice(bytes32 hash) public {
    priceCommit = hash;
}

function revealPrice(uint256 price, bytes32 salt) public {
    require(keccak256(abi.encodePacked(price, salt)) == priceCommit);
    // Update price
}
```

#### 5. Timestamp Dependency
```solidity
// ⚠️ RISKY: Miners can manipulate block.timestamp slightly
function isExpired() public view returns (bool) {
    return block.timestamp > expiration;  // Avoid relying on exact time
}

// ✅ SAFER: Use block number instead
function isExpired() public view returns (bool) {
    return block.number > expiration;
}
```

### Smart Contract Best Practices

#### 1. Checks-Effects-Interactions (CEI) Pattern
```solidity
function transfer(address to, uint256 amount) public {
    // 1. CHECKS: Validate inputs
    require(to != address(0), "Invalid recipient");
    require(balances[msg.sender] >= amount, "Insufficient balance");
    
    // 2. EFFECTS: Update contract state
    balances[msg.sender] -= amount;
    balances[to] += amount;
    
    // 3. INTERACTIONS: Call external contracts
    emit Transfer(msg.sender, to, amount);
}
```

#### 2. Use OpenZeppelin Libraries
```solidity
import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/security/Pausable.sol";
import "@openzeppelin/contracts/proxy/utils/Initializable.sol";

contract CelestialNFT is ERC721, Ownable, Pausable, Initializable {
    // Battle-tested, audited code
}
```

#### 3. Implement Pause Mechanism
```solidity
import "@openzeppelin/contracts/security/Pausable.sol";

contract CelestialNFT is Pausable {
    function mint(address to) public onlyOwner whenNotPaused {
        // Can be paused in case of emergency
    }
    
    function pause() public onlyOwner {
        _pause();
    }
    
    function unpause() public onlyOwner {
        _unpause();
    }
}
```

#### 4. Emergency Timelock
```solidity
import "@openzeppelin/contracts/governance/TimelockController.sol";

// Critical changes require 2-day delay
TimelockController timelock = new TimelockController(
    minDelay = 2 days,
    proposers = [governanceAddress],
    executors = [governanceAddress],
    admin = multiSigAddress
);
```

#### 5. Document Assumptions & Invariants
```solidity
/**
 * @title CelestialNFT
 * @dev ERC-721 NFT for celestial name ownership
 * 
 * ASSUMPTIONS:
 * - Owner is trusted (multisig or DAO)
 * - Metadata URIs are immutable after minting
 * - Total supply is capped at 10M
 * 
 * INVARIANTS:
 * - Each tokenId has exactly one owner
 * - Total supply never decreases (no burning)
 * - Royalty percentage <= 100%
 */
contract CelestialNFT is ERC721 {
    uint256 public constant MAX_SUPPLY = 10_000_000;
}
```

---

## 🌐 Frontend Security

### Content Security Policy (CSP)
```javascript
// next.config.js
module.exports = {
  async headers() {
    return [
      {
        source: '/:path*',
        headers: [
          {
            key: 'Content-Security-Policy',
            value: "default-src 'self'; script-src 'self' 'unsafe-inline' https://cdn.jsdelivr.net; connect-src 'self' https://*.infura.io https://*.walletconnect.com"
          },
          {
            key: 'X-Content-Type-Options',
            value: 'nosniff'
          },
          {
            key: 'X-Frame-Options',
            value: 'DENY'
          }
        ]
      }
    ];
  }
};
```

### Private Key Management
```typescript
// ❌ NEVER do this
const privateKey = "0x1234567890abcdef...";

// ✅ SAFE: Use environment variables
const privateKey = process.env.PRIVATE_KEY;

// ✅ SAFER: Use hardware wallet
import { ethers } from "ethers";
const signer = new ethers.providers.JsonRpcProvider(rpcUrl).getSigner();
```

### Wallet Connection Best Practices
```typescript
// Use Wagmi hooks (built-in security)
import { useAccount, useConnect, useDisconnect } from 'wagmi';

export function WalletConnector() {
  const { address, isConnected } = useAccount();
  const { connect, connectors, error, isLoading, pendingConnector } = useConnect();
  const { disconnect } = useDisconnect();

  return (
    <div>
      {isConnected ? (
        <>
          <p>Connected: {address}</p>
          <button onClick={() => disconnect()}>Disconnect</button>
        </>
      ) : (
        <div>
          {connectors.map((connector) => (
            <button
              key={connector.id}
              onClick={() => connect({ connector })}
            >
              {connector.name}
            </button>
          ))}
        </div>
      )}
    </div>
  );
}
```

### HTTPS & CORS
```javascript
// frontend/.env.local
NEXT_PUBLIC_API_URL=https://api.stellarProvenance.com
NEXT_PUBLIC_RPC_URL=https://eth-mainnet.alchemyapi.io/v2/YOUR_KEY

// Ensure all API calls use HTTPS
const response = await fetch('https://api.example.com/data');
```

### Dependency Security
```bash
# Regular security audits
npm audit

# Update vulnerable dependencies
npm audit fix

# Use GitHub Dependabot for automated PR reviews
# Add .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: npm
    directory: "/"
    schedule:
      interval: weekly
```

---

## 🔐 Backend Security

### Environment Variables
```bash
# .env.example (NEVER commit actual secrets)
PRIVATE_KEY=YOUR_TEST_KEY_HERE
INFURA_API_KEY=YOUR_KEY_HERE
DATABASE_URL=postgresql://user:pass@localhost:5432/db
REDIS_URL=redis://localhost:6379
JWT_SECRET=your_jwt_secret_here
```

### API Rate Limiting
```typescript
// backend/src/middleware/rateLimiter.ts
import rateLimit from 'express-rate-limit';

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // Limit each IP to 100 requests per windowMs
  message: 'Too many requests from this IP, please try again later.'
});

app.use('/api/', limiter);
```

### Input Validation
```typescript
// Use libraries like Joi or Zod for validation
import { z } from 'zod';

const RegisterNameSchema = z.object({
  celestialObjectId: z.string().min(1),
  name: z.string().min(3).max(50).regex(/^[a-zA-Z0-9_-]+$/),
  description: z.string().max(500).optional(),
});

app.post('/api/register', (req, res) => {
  const validated = RegisterNameSchema.parse(req.body);
  // Process validated data
});
```

### SQL Injection Prevention
```typescript
// ❌ VULNERABLE
const query = `SELECT * FROM names WHERE id = ${req.query.id}`;

// ✅ SAFE (with Prisma)
const name = await prisma.name.findUnique({
  where: { id: req.query.id }
});

// ✅ SAFE (with raw SQL)
const name = await db.raw(
  'SELECT * FROM names WHERE id = ?',
  [req.query.id]
);
```

---

## 🔍 Monitoring & Incident Response

### Contract Monitoring
```typescript
// Monitor for unusual activity
const provider = ethers.getDefaultProvider('mainnet');

// Listen for Transfer events
contract.on('Transfer', (from, to, value, event) => {
  console.log(`Transfer detected: ${from} -> ${to}`);
  // Alert if suspicious
  if (value > LARGE_THRESHOLD) {
    alertSecurityTeam();
  }
});
```

### Error Logging
```typescript
// Use Sentry for error tracking
import * as Sentry from "@sentry/node";

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.NODE_ENV,
  tracesSampleRate: 1.0,
});

app.use((err, req, res, next) => {
  Sentry.captureException(err);
  res.status(500).json({ error: 'Internal server error' });
});
```

### Incident Response Procedure
1. **Detect** - Alert team immediately
2. **Assess** - Understand scope and impact
3. **Respond** - Pause contract, communicate with users
4. **Investigate** - Root cause analysis
5. **Remedy** - Deploy fix or mitigation
6. **Document** - Post-mortem analysis

---

## ✅ Pre-Launch Checklist

### Smart Contracts
- [ ] All tests pass (80%+ coverage)
- [ ] Security audit completed
- [ ] Slither analysis shows no critical issues
- [ ] Code review by 2+ senior devs
- [ ] Pausable mechanism implemented
- [ ] Events logged for all state changes
- [ ] Admin functions protected with role-based access

### Frontend
- [ ] HTTPS enabled
- [ ] CSP headers configured
- [ ] No private keys in code
- [ ] Dependency audit passed
- [ ] Performance tested (Lighthouse >80)
- [ ] Cross-browser tested

### Backend
- [ ] Rate limiting enabled
- [ ] Input validation on all endpoints
- [ ] Error handling comprehensive
- [ ] Logging configured (Sentry/DataDog)
- [ ] Database backups automated
- [ ] API documentation complete

### Deployment
- [ ] Staged rollout plan (testnet → mainnet)
- [ ] Rollback procedure documented
- [ ] Monitoring alerts configured
- [ ] On-call rotation established
- [ ] Legal review of T&Cs
- [ ] Insurance/coverage for smart contracts

---

## 📋 Security Review Schedule

| Review Type | Frequency | Owner |
|-------------|-----------|-------|
| Code review | Every PR | Dev lead |
| Dependency audit | Weekly | DevOps |
| Smart contract audit | Before mainnet | External firm |
| Security training | Quarterly | Team |
| Penetration test | Annually | External firm |

---

## 🚨 Security Contacts

| Role | Email | Availability |
|------|-------|--------------|
| Security Lead | security@stellarprovenance.com | 24/7 |
| Smart Contract Dev | dev@stellarprovenance.com | Business hours |
| Ops Lead | ops@stellarprovenance.com | 24/7 |

**To report vulnerabilities:** Email security@stellarprovenance.com with details.

---

## 📚 Additional Resources

- [Solidity Security Patterns](https://blog.openzeppelin.com/smart-contract-security/)
- [OWASP Top 10 for Smart Contracts](https://owasp.org/www-project-smart-contract-top-10/)
- [Ethereum Security Best Practices](https://consensys.github.io/smart-contract-best-practices/)
- [Web3 Security Best Practices](https://www.a16z.com/web3-security/)

---

**Last Updated:** December 23, 2025  
**Maintained By:** Stellar-Provenance Security Team
