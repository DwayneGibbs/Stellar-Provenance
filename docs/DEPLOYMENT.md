# Deployment Guide

This document provides comprehensive instructions for deploying Stellar-Provenance across different environments and networks.

---

## 📋 Prerequisites

Before deploying, ensure you have:

### Required Tools
```bash
# Node.js 18+ and npm/yarn
node --version
npm --version

# Hardhat (for smart contracts)
npm install -g hardhat

# Git
git --version

# AWS CLI or similar (if using cloud deployment)
aws --version
```

### Required Accounts & Keys
- [ ] Infura or Alchemy account (for RPC endpoints)
- [ ] Etherscan account (for contract verification)
- [ ] GitHub account (for CI/CD)
- [ ] Vercel or AWS account (for frontend hosting)
- [ ] Pinata or NFT.storage account (for IPFS metadata)

### Environment Setup
```bash
# Clone repository
git clone https://github.com/DwayneGibbs/Stellar-Provenance.git
cd Stellar-Provenance

# Install dependencies
npm install

# Create environment files
cp .env.example .env.local
# Fill in your actual keys in .env.local
```

---

## 🧪 Local Development Deployment

### 1. Start Local Hardhat Network
```bash
cd packages/contracts

# Start local blockchain
npx hardhat node

# Output:
# Started HTTP and WebSocket JSON-RPC server at http://127.0.0.1:8545/
```

### 2. Deploy Contracts Locally
```bash
# In another terminal window
cd packages/contracts

# Deploy to local network
npx hardhat run scripts/deploy.ts --network localhost

# Output:
# Deploying NamingRegistry...
# NamingRegistry deployed to: 0x5FbDB2315678afccb333f8a9c45b65d30061dE7a
# Deploying CelestialNFT...
# CelestialNFT deployed to: 0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512
```

### 3. Verify Contracts Locally
```bash
# Test contract functionality
npx hardhat test

# Output:
# NamingRegistry
#   ✓ should register a celestial name
#   ✓ should transfer ownership
#   ✓ should update metadata
# 3 passing
```

### 4. Start Frontend Locally
```bash
cd packages/frontend

# Install dependencies
npm install

# Set environment for local network
# .env.local should have:
NEXT_PUBLIC_CONTRACT_ADDRESS=0x5FbDB2315678afccb333f8a9c45b65d30061dE7a
NEXT_PUBLIC_RPC_URL=http://localhost:8545

# Start dev server
npm run dev

# Visit http://localhost:3000
```

### 5. Connect MetaMask to Local Network
1. Open MetaMask
2. Settings → Networks → Add Network
3. **Network Name:** Hardhat Local
4. **RPC URL:** http://localhost:8545
5. **Chain ID:** 31337
6. **Currency Symbol:** ETH
7. Click "Save"

### 6. Import Test Accounts
Hardhat provides 20 test accounts with 10,000 ETH each:

```bash
# First account private key (from hardhat node output)
# 0xac0974bec39a17e36ba4a6b4d238ff944bacb476caded732d6fcd1330be270
```

Add to MetaMask:
1. Click account menu
2. "Import Account"
3. Paste private key
4. Confirm

---

## 🧪 Testnet Deployment (Sepolia)

### 1. Get Sepolia Testnet ETH

#### Option A: Faucets
- [Sepolia Faucet](https://sepoliafaucet.com/) - Requires Alchemy account
- [Alchemy Faucet](https://www.alchemy.com/faucets/ethereum-sepolia)
- [Infura Faucet](https://www.infura.io/faucet)

#### Option B: Request from Project Team
Contact: dev@stellarprovenance.com with your wallet address

### 2. Setup Environment Variables
```bash
# .env.local (add to .env.example)
SEPOLIA_RPC_URL=https://eth-sepolia.alchemyapi.io/v2/YOUR_ALCHEMY_KEY
PRIVATE_KEY=your_wallet_private_key_here
ETHERSCAN_API_KEY=your_etherscan_key_here

# For Infura (alternative)
SEPOLIA_RPC_URL=https://sepolia.infura.io/v3/YOUR_INFURA_PROJECT_ID
```

### 3. Deploy Smart Contracts to Sepolia
```bash
cd packages/contracts

# Deploy
npx hardhat run scripts/deploy.ts --network sepolia

# Output:
# Deploying to Sepolia...
# NamingRegistry deployed to: 0x1234567890123456789012345678901234567890
# CelestialNFT deployed to: 0x0987654321098765432109876543210987654321
```

### 4. Verify Contracts on Etherscan
```bash
# Get constructor arguments in hex format
# Then verify
npx hardhat verify \
  --network sepolia \
  0x1234567890123456789012345678901234567890 \
  "arg1" "arg2"

# Once verified, contract will show source code on Etherscan
```

### 5. Update Frontend Environment
```bash
# packages/frontend/.env.local
NEXT_PUBLIC_NETWORK=sepolia
NEXT_PUBLIC_CONTRACT_ADDRESS=0x1234567890123456789012345678901234567890
NEXT_PUBLIC_RPC_URL=https://eth-sepolia.alchemyapi.io/v2/YOUR_KEY
NEXT_PUBLIC_ETHERSCAN_URL=https://sepolia.etherscan.io
```

### 6. Deploy Frontend to Vercel (Staging)
```bash
# Option A: Via CLI
npm i -g vercel
cd packages/frontend
vercel --prod

# Option B: Via GitHub integration
# Push to GitHub, Vercel auto-deploys on push

# Staging URL: https://stellar-provenance-staging.vercel.app
```

### 7. Test on Sepolia Testnet
- [ ] Connect MetaMask to Sepolia
- [ ] Test registering a name
- [ ] Test transferring ownership
- [ ] Test on Etherscan
- [ ] Run user acceptance tests
- [ ] Document any issues

---

## 🚀 Polygon Mainnet Deployment (Production)

### 1. Security Audit Completion
Before deploying to Polygon mainnet:
- [ ] Smart contract audit completed
- [ ] All audit findings addressed
- [ ] Code review by 2+ senior developers
- [ ] Security checklist passed (see SECURITY.md)

### 2. Get Polygon Native Currency (MATIC)
```bash
# Exchange ETH → MATIC on exchange (e.g., Uniswap, Binance)
# Or bridge ETH from Ethereum mainnet to Polygon using:
# - Polygon Bridge: https://wallet.polygon.technology/
# - Across: https://across.to/
# - Stargate: https://stargate.finance/

# Send MATIC to your deployer wallet
```

### 3. Setup Environment Variables
```bash
# .env.local
POLYGON_RPC_URL=https://polygon-rpc.com
# Or: https://polygon-mainnet.g.alchemy.com/v2/YOUR_KEY
PRIVATE_KEY=your_wallet_private_key_here
POLYGONSCAN_API_KEY=your_polygonscan_key_here
```

### 4. Deploy Contracts to Polygon
```bash
cd packages/contracts

# Deploy
npx hardhat run scripts/deploy.ts --network polygon

# Output:
# Deploying to Polygon...
# NamingRegistry deployed to: 0xABC...
# CelestialNFT deployed to: 0xDEF...

# Verify on PolygonScan
npx hardhat verify --network polygon 0xABC... "arg1" "arg2"
```

### 5. Initialize Liquidity & Markets
```bash
# If using DEX (e.g., Uniswap):
# 1. Deploy governance token (if applicable)
# 2. Create liquidity pool
# 3. Add initial liquidity
# 4. Lock liquidity for credibility

# Commands depend on chosen DEX
```

### 6. Deploy Frontend to Production
```bash
# Set production environment variables
# packages/frontend/.env.production

NEXT_PUBLIC_NETWORK=polygon
NEXT_PUBLIC_CONTRACT_ADDRESS=0xABC...
NEXT_PUBLIC_RPC_URL=https://polygon-rpc.com
NEXT_PUBLIC_ETHERSCAN_URL=https://polygonscan.com

# Deploy
cd packages/frontend
npm run build
npm run start

# Or via Vercel
vercel --prod
```

### 7. Setup The Graph Subgraph (Optional)
```bash
# Create subgraph for indexing
cd packages/subgraph

# Authenticate
graph auth https://api.thegraph.com/deploy/ YOUR_DEPLOY_KEY

# Deploy subgraph
graph deploy --node https://api.thegraph.com/deploy/ \
  stellar-provenance/celestial-names
```

### 8. Post-Deployment Verification
- [ ] Verify contract on PolygonScan
- [ ] Test core functionality (register, transfer, burn)
- [ ] Monitor gas prices
- [ ] Check frontend loads and functions
- [ ] Verify wallet connections
- [ ] Test transaction submissions
- [ ] Monitor contract events

---

## 📊 Ethereum Mainnet Deployment (Optional - Premium Feature)

Deploy to Ethereum mainnet for users who prefer the primary network:

### 1. Production Readiness
- [ ] Battle-tested on Polygon for 1M+ transactions
- [ ] $10M+ TVL (if applicable)
- [ ] $250K+ deployment budget (gas costs)
- [ ] Professional audit with insurance

### 2. Setup
```bash
# .env.local
MAINNET_RPC_URL=https://eth-mainnet.alchemyapi.io/v2/YOUR_KEY
PRIVATE_KEY=your_deployer_key
ETHERSCAN_API_KEY=your_key
```

### 3. Deploy
```bash
cd packages/contracts

# Deploy (takes 30+ minutes depending on network congestion)
npx hardhat run scripts/deploy.ts --network mainnet

# Verify
npx hardhat verify --network mainnet 0xABC...
```

---

## 🔄 Continuous Deployment (GitHub Actions)

### 1. Setup CI/CD Pipeline
Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy

on:
  push:
    branches: [main]

env:
  SEPOLIA_RPC_URL: ${{ secrets.SEPOLIA_RPC_URL }}
  POLYGON_RPC_URL: ${{ secrets.POLYGON_RPC_URL }}
  PRIVATE_KEY: ${{ secrets.PRIVATE_KEY }}

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18
      - run: npm install
      - run: npm run test

  deploy-staging:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18
      - run: npm install
      
      # Deploy smart contracts to Sepolia
      - run: npm run deploy:sepolia
      
      # Deploy frontend to Vercel staging
      - run: npm run deploy:frontend:staging
        env:
          VERCEL_TOKEN: ${{ secrets.VERCEL_TOKEN }}
```

### 2. Add Secrets to GitHub
Settings → Secrets and variables → Actions → New repository secret

```
SEPOLIA_RPC_URL
POLYGON_RPC_URL
PRIVATE_KEY (use deployer account only)
ETHERSCAN_API_KEY
VERCEL_TOKEN
```

---

## 🔍 Contract Addresses

### Sepolia Testnet
```
NamingRegistry:  0x[ADDRESS]
CelestialNFT:    0x[ADDRESS]
```

### Polygon Mainnet
```
NamingRegistry:  0x[ADDRESS]
CelestialNFT:    0x[ADDRESS]
GovernanceToken: 0x[ADDRESS] (future)
```

### Ethereum Mainnet
```
NamingRegistry:  0x[ADDRESS] (optional)
CelestialNFT:    0x[ADDRESS] (optional)
```

---

## 📈 Monitoring & Observability

### Smart Contract Monitoring
```bash
# Monitor contract events
# Use tools like:
# - Tenderly: https://tenderly.co/ (real-time monitoring)
# - DefiLlama: For TVL tracking
# - Etherscan/PolygonScan: Block explorer monitoring

# Set up alerts:
# - Large transactions
# - Failed transactions
# - Pause/unpause events
# - Access control events
```

### Frontend Monitoring
```bash
# Monitor with Vercel Analytics
# Automatic with Vercel deployment

# Or use external tools:
# - Sentry: Error tracking
# - Datadog: Performance monitoring
# - Google Analytics: Traffic analytics
```

### Backend Monitoring
```bash
# If backend services deployed:
# - Health check: GET /health
# - Prometheus metrics: GET /metrics
# - Log aggregation: CloudWatch, DataDog, or LogRocket
```

---

## 🔄 Upgrade & Rollback Procedures

### Upgrading Smart Contracts
If using Proxy pattern:

```bash
# Deploy new implementation
cd packages/contracts

npx hardhat run scripts/upgradeProxy.ts --network polygon

# Verify new implementation
npx hardhat verify --network polygon 0xNEW_IMPL_ADDRESS
```

### Rollback Procedure
1. Identify issue
2. Stop new version
3. Revert to previous implementation
4. Deploy fix
5. Test thoroughly

```bash
# Rollback (if using proxy)
npx hardhat run scripts/rollbackProxy.ts --network polygon
```

---

## 🚨 Incident Response

### If Something Goes Wrong

1. **Pause Contract** (if possible)
   ```bash
   npx hardhat run scripts/pause.ts --network polygon
   ```

2. **Communicate with Users**
   - Post in Discord
   - Tweet status update
   - Email users (if applicable)

3. **Investigate**
   - Check Etherscan/PolygonScan
   - Review logs
   - Run tests

4. **Fix & Deploy**
   - Fix bug
   - Test locally
   - Deploy to testnet first
   - Deploy to mainnet

5. **Unpause**
   ```bash
   npx hardhat run scripts/unpause.ts --network polygon
   ```

---

## ✅ Deployment Checklist

### Pre-Deployment
- [ ] All tests pass
- [ ] Code reviewed
- [ ] Security audit completed
- [ ] Environment variables configured
- [ ] Deployment scripts tested locally
- [ ] Team trained on rollback procedures
- [ ] Monitoring alerts configured

### Deployment
- [ ] Database backed up
- [ ] Testnet deployment successful
- [ ] Smart contracts verified
- [ ] Frontend loads and functions
- [ ] Critical user paths tested
- [ ] Monitoring active

### Post-Deployment
- [ ] Monitor for 24+ hours
- [ ] Document any issues
- [ ] Update team on status
- [ ] Collect user feedback
- [ ] Plan next release

---

## 📞 Support & Escalation

| Issue | Contact | Escalation |
|-------|---------|-----------|
| Deployment failure | @devlead | CTO |
| Smart contract bug | @security | Legal team |
| Frontend issue | @frontend | Product manager |
| Infrastructure | @devops | Director of Eng |

---

## 📚 Additional Resources

- [Hardhat Deployment Guide](https://hardhat.org/hardhat-runner/docs/guides/deploying)
- [Vercel Deployment Guide](https://vercel.com/docs/deployments/overview)
- [Polygon Documentation](https://docs.polygon.technology/)
- [Etherscan API Guide](https://docs.etherscan.io/)

---

**Last Updated:** December 23, 2025  
**Maintained By:** Stellar-Provenance DevOps Team
