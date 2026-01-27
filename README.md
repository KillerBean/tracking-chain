# Supply Chain DApp

Supply Chain DApp for product traceability on blockchain. System allows manufacturers to register products and owners to transfer ownership with immutable custody history.

## Tech Stack

- **Smart Contracts**: Solidity ^0.8.28, Hardhat 3
- **Frontend**: Next.js 16, React 19, TailwindCSS 4
- **Blockchain Interaction**: ethers.js v6
- **Runtime**: Bun Workspaces (monorepo)

## Project Structure

```
packages/
  blockchain/
    contracts/         # Solidity contracts + Foundry tests (*.t.sol)
    test/              # TypeScript integration tests (Mocha + ethers)
    ignition/modules/  # Hardhat Ignition deployment modules
  frontend/
    app/               # Next.js App Router
```

## Quick Start

```bash
# Install dependencies
bun install

# Run blockchain tests
bun run --filter blockchain test

# Start frontend dev server
bun run --filter frontend dev
```

## Development

### Blockchain

```bash
bun run --filter blockchain compile  # Compile contracts
bun run --filter blockchain test     # Run all tests
bun run --filter blockchain test:sol # Run Solidity tests only
bun run --filter blockchain test:ts  # Run TypeScript tests only
```

### Frontend

```bash
bun run --filter frontend dev        # Start dev server
bun run --filter frontend build      # Production build
bun run --filter frontend lint       # ESLint
```

## Deploy

### Local

```bash
bun run --filter blockchain deploy
```

### Sepolia Testnet

Requires `SEPOLIA_RPC_URL` and `SEPOLIA_PRIVATE_KEY` config vars:

```bash
bunx hardhat keystore set SEPOLIA_RPC_URL
bunx hardhat keystore set SEPOLIA_PRIVATE_KEY
bun run --filter blockchain deploy:sepolia
```
