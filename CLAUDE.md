# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Supply Chain DApp for product traceability on blockchain. System allows manufacturers to register products and owners to transfer ownership with immutable custody history.

## Tech Stack

- **Smart Contracts**: Solidity ^0.8.28, Hardhat 3
- **Frontend**: Next.js 16, React 19, TailwindCSS 4
- **Blockchain Interaction**: ethers.js v6
- **Runtime**: Bun Workspaces (monorepo)

## Commands

```bash
bun install                          # Install all workspace deps from root
```

### Blockchain (packages/blockchain)
```bash
bun run --filter blockchain test     # Run all tests
bun run --filter blockchain test:sol # Run Solidity tests only
bun run --filter blockchain test:ts  # Run TypeScript tests only
bun run --filter blockchain compile  # Compile contracts
bun run --filter blockchain deploy   # Deploy locally
bun run --filter blockchain deploy:sepolia  # Deploy to Sepolia
```

### Frontend (packages/frontend)
```bash
bun run --filter frontend dev        # Start dev server
bun run --filter frontend build      # Production build
bun run --filter frontend lint       # ESLint
```

## Project Structure

```
packages/
  blockchain/
    contracts/         # Solidity contracts + Foundry tests (*.t.sol)
    test/              # TypeScript integration tests (Mocha + ethers)
    ignition/modules/  # Hardhat Ignition deployment modules
    scripts/           # Utility scripts
  frontend/
    app/               # Next.js App Router
```

## Architecture Notes

- Bun Workspaces monorepo with shared dependencies at root
- Hardhat 3 with `hardhat-toolbox-mocha-ethers` plugin
- Two test types: Foundry-compatible Solidity tests (`*.t.sol`) and TypeScript Mocha tests
- Networks: local simulated (hardhatMainnet, hardhatOp), Sepolia testnet
- Sepolia requires `SEPOLIA_RPC_URL` and `SEPOLIA_PRIVATE_KEY` config vars (set via `bunx hardhat keystore set`)

## Bun Preferences

- Use `bun` / `bunx` instead of `npm` / `npx`
- Bun auto-loads `.env` files
- See `node_modules/bun-types/docs/**.mdx` for Bun API docs
