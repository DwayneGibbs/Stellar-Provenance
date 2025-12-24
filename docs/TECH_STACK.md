# Technology Stack

This document outlines the technologies and frameworks that power Stellar-Provenance.

## Overview

Stellar-Provenance is built as a **Web3 game** that integrates with the Ethereum blockchain to create a community-driven experience around naming and tracking astronomical objects. The architecture is designed to be modular, scalable, and developer-friendly.

---

## 📦 Smart Contracts Layer

### Framework & Language
- **Framework:** [Hardhat](https://hardhat.org/) (Ethereum development environment)
  - Why: Best-in-class testing, great documentation, excellent TypeScript support
  - Alternative: Foundry (for performance-focused teams)
- **Language:** Solidity 0.8.20+
  - Why: Most mature, extensive ecosystem, security improvements in 0.8.x

### Key Libraries
- **OpenZeppelin Contracts:** ERC-721 (NFTs), Access Control, Security patterns
- **OpenZeppelin Upgrades:** Proxy pattern for upgradeable contracts

### Target Networks
- **Development:** Hardhat Local Network (localhost)
- **Testing:** Sepolia Testnet
- **Production:** 
  - Ethereum Mainnet (primary)
  - Polygon (L2 scaling, lower gas fees)
  - Arbitrum (optional, for additional liquidity)

### Smart Contract Architecture
```
contracts/
├── interfaces/
│   ├── INamingRegistry.sol
│   └── ICelestialNFT.sol
├── NamingRegistry.sol      # Main registry for celestial names
├── CelestialNFT.sol        # ERC-721 NFT representing ownership
├── GovernanceToken.sol     # DAO governance token (future)
└── AstronomicalOracle.sol  # Integration with external data
```

### Key Contracts
| Contract | Purpose | Status |
|----------|---------|--------|
| NamingRegistry | Register/transfer celestial names | TBD |
| CelestialNFT | ERC-721 NFTs for name ownership | TBD |
| GovernanceToken | Community governance (future phase) | Planned |

---

## 🎨 Frontend Layer

### Framework & Language
- **Framework:** [Next.js 14+](https://nextjs.org/)
  - Why: Server-side rendering, static generation, API routes, great for Web3 UX
- **Language:** TypeScript
  - Why: Type safety, better DX, catches errors early
- **UI Library:** React 18+

### Web3 Integration
- **Web3 Library:** [Ethers.js v6](https://docs.ethers.org/v6/)
  - Why: Modern, minimal dependencies, excellent documentation
  - Alternative: Web3.js (if you prefer)
- **Wallet Connection:** [Wagmi](https://wagmi.sh/)
  - Why: React hooks for wallet interaction, great UX
  - Supports: MetaMask, WalletConnect, Coinbase Wallet, etc.

### State Management
- **Server State:** [TanStack Query (React Query)](https://tanstack.com/query/latest)
  - Why: Handles API data fetching, caching, synchronization
- **Client State:** [Zustand](https://github.com/pmndrs/zustand)
  - Why: Lightweight, easy to use, great for wallet/UI state

### Styling & UI
- **CSS Framework:** [Tailwind CSS](https://tailwindcss.com/)
  - Why: Utility-first, fast development, responsive design
- **Component Library:** [Shadcn/ui](https://ui.shadcn.com/) or [Radix UI](https://www.radix-ui.com/)
  - Why: Accessible, unstyled, can customize with Tailwind

### Key Libraries
```json
{
  "dependencies": {
    "next": "^14.0.0",
    "react": "^18.0.0",
    "ethers": "^6.0.0",
    "wagmi": "^1.0.0",
    "@tanstack/react-query": "^5.0.0",
    "zustand": "^4.0.0",
    "tailwindcss": "^3.0.0"
  },
  "devDependencies": {
    "typescript": "^5.0.0",
    "@types/react": "^18.0.0",
    "eslint": "^8.0.0",
    "prettier": "^3.0.0"
  }
}
```

### Frontend Architecture
```
packages/frontend/
├── src/
│   ├── app/                    # Next.js 14 app router
│   │   ├── layout.tsx
│   │   ├── page.tsx
│   │   ├── game/
│   │   ├── marketplace/
│   │   └── dashboard/
│   ├── components/             # Reusable React components
│   │   ├── Wallet/
│   │   ├── Forms/
│   │   ├── Cards/
│   │   └── Navigation/
│   ├── hooks/                  # Custom React hooks
│   │   ├── useContract.ts      # Interact with smart contracts
│   │   ├── useWallet.ts        # Wallet integration
│   │   └── useAstronomicalData.ts
│   ├── lib/                    # Utilities
│   │   ├── api.ts             # API client
│   │   ├── web3.ts            # Web3 helpers
│   │   └── constants.ts       # Network/contract addresses
│   ├── store/                  # Zustand stores
│   │   ├── walletStore.ts
│   │   └── gameStore.ts
│   ├── styles/
│   │   └── globals.css
│   └── types/
│       └── index.ts
├── public/
│   ├── images/
│   └── fonts/
├── next.config.js
├── tsconfig.json
└── tailwind.config.js
```

---

## 🔧 Backend Layer (Optional)

### Framework & Language
- **Runtime:** [Node.js](https://nodejs.org/) 18+ LTS
- **Framework:** [Express.js](https://expressjs.com/) or [Nest.js](https://nestjs.com/)
  - Express: Lightweight, minimal structure
  - Nest.js: Opinionated, great for larger projects, TypeScript-first
- **Language:** TypeScript

### Key Libraries
```json
{
  "dependencies": {
    "express": "^4.18.0",
    "cors": "^2.8.0",
    "dotenv": "^16.0.0",
    "axios": "^1.4.0",
    "ethers": "^6.0.0"
  },
  "devDependencies": {
    "typescript": "^5.0.0",
    "ts-node": "^10.0.0",
    "nodemon": "^3.0.0",
    "eslint": "^8.0.0"
  }
}
```

### Backend Architecture
```
packages/backend/
├── src/
│   ├── routes/
│   │   ├── celestial.ts        # Celestial data endpoints
│   │   ├── nft.ts              # NFT metadata endpoints
│   │   └── user.ts             # User profile endpoints
│   ├── services/
│   │   ├── AstronomicalService.ts
│   │   ├── BlockchainService.ts
│   │   └── CacheService.ts
│   ├── middleware/
│   │   ├── errorHandler.ts
│   │   └── rateLimiter.ts
│   ├── models/
│   │   └── index.ts
│   ├── config/
│   │   └── index.ts
│   └── index.ts               # Entry point
├── tests/
├── .env.example
└── package.json
```

### Responsibilities
- Serve astronomical data APIs (SkyMapper, Gaia, etc.)
- Provide metadata for NFTs (IPFS gateway)
- Cache blockchain data
- Analytics and monitoring
- Rate limiting for APIs

---

## 📊 Data & Indexing Layer

### Blockchain Indexing
- **The Graph:** [TheGraph Protocol](https://thegraph.com/)
  - What: Query smart contract data via GraphQL
  - Why: Fast, decentralized, real-time blockchain data
  - Subgraph: Custom indexing for Naming Registry contract
  
Example query:
```graphql
query GetCelestialNames {
  celestialNames(first: 10) {
    id
    owner
    name
    registeredAt
    metadata
  }
}
```

### Database (Optional Backend)
- **Database:** PostgreSQL 14+
  - Why: Reliable, ACID compliance, JSON support
- **ORM:** [Prisma](https://www.prisma.io/)
  - Why: Type-safe, great migrations, intuitive API
- **Caching:** Redis
  - Why: High-performance caching layer

### Astronomical Data Sources
- **SkyMapper Database:** Stellar catalog, astronomical object coordinates
  - Endpoint: [SkyMapper DR2](https://skymapper.anu.edu.au/)
  - Authentication: API Key (free tier available)
- **ESA Gaia Mission:** Precise stellar positions and distances
  - Endpoint: [Gaia EDR3](https://gea.esac.esa.int/archive/)
  - Authentication: Basic auth or API key
- **IPFS:** Decentralized file storage for metadata
  - Node: Pinata or NFT.storage (managed IPFS)
  - Why: Censorship-resistant metadata storage

---

## 🔐 Security & Deployment

### Development Tools
- **Linting:** ESLint + Prettier
  - For consistent code style
- **Testing Framework:** 
  - **Smart Contracts:** Hardhat + Chai
  - **Frontend:** Vitest + React Testing Library
  - **Backend:** Jest
- **Git Hooks:** Husky + lint-staged
  - Enforce code quality on commit

### CI/CD Pipeline
- **GitHub Actions** for:
  - Linting & formatting checks
  - Unit tests
  - Contract compilation and deployment tests
  - Frontend build verification

### Security Best Practices
- **Smart Contracts:**
  - OpenZeppelin security libraries
  - Formal verification (optional, for critical contracts)
  - External audits before mainnet deployment
- **Frontend:**
  - Content Security Policy (CSP)
  - HTTPS only
  - Dependencies scanning (Dependabot)
- **Backend:**
  - Environment variable management (.env)
  - API rate limiting
  - CORS configuration

### Deployment Targets
- **Smart Contracts:** 
  - Sepolia (testnet) → Polygon/Ethereum (production)
- **Frontend:**
  - Vercel (recommended for Next.js)
  - Alternative: AWS S3 + CloudFront
- **Backend:**
  - AWS EC2 or ECS
  - Alternative: Railway, Render, or DigitalOcean

---

## 📋 Summary Table

| Layer | Technology | Why | Status |
|-------|-----------|-----|--------|
| Smart Contracts | Solidity + Hardhat | Industry standard, excellent tooling | Planned |
| Frontend | Next.js + React + Wagmi | Best UX, Web3 integration | Planned |
| Backend | Express.js/Nest.js + TypeScript | Lightweight, flexible | Optional |
| Indexing | The Graph | Decentralized data queries | Planned |
| Data | PostgreSQL + Redis | Reliable, fast caching | Optional |
| Styling | Tailwind CSS | Rapid UI development | Planned |
| Testing | Hardhat + Vitest + Jest | Comprehensive coverage | Planned |
| Deployment | Vercel + Ethereum networks | Scalable, reliable | Planned |

---

## 🚀 Getting Started

See [`CONTRIBUTING.md`](../CONTRIBUTING.md) for local development setup.

## 📖 Additional Resources

- [Hardhat Documentation](https://hardhat.org/docs)
- [Next.js Documentation](https://nextjs.org/docs)
- [Wagmi Documentation](https://wagmi.sh/)
- [OpenZeppelin Contracts](https://docs.openzeppelin.com/contracts/)
- [The Graph Documentation](https://thegraph.com/docs/)

---

**Last Updated:** December 23, 2025  
**Maintained By:** Stellar-Provenance Team
