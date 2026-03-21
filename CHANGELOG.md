# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Planned

- Smart Contract (`Tracking.sol`): Product registration, ownership transfer, history tracking
- Solidity unit tests (`*.t.sol`) and TypeScript integration tests
- Hardhat Ignition deployment module
- Frontend UI: Wallet connection, product registration, ownership transfer, history view
- Deploy to Sepolia testnet

---

## [0.1.0] - 2026-03-17

### Added

- **Initial Project Setup**
  - Monorepo structure (Bun Workspaces) with `packages/blockchain` and `packages/frontend`
  - Hardhat 3 + TypeScript setup for smart contracts
  - Next.js 16 + React 19 + TailwindCSS 4 for frontend
  - ethers.js v6 for blockchain interaction
  - Project documentation: README.md, CLAUDE.md

- **Documentation**
  - PLAN.md: Implementation plan with architecture design
  - TASKS.md: Initial task breakdown
  - TODO.md: Detailed implementation backlog with all work items

### Status

- Project structure initialized
- Tech stack configured
- Development environment ready
- Implementation backlog documented

---

## Project Milestones (Planned)

| Milestone | Status | Description |
|-----------|--------|-------------|
| **Smart Contract Core** | 🔄 Pending | Implement `Tracking.sol` with product registration, ownership transfer, and history |
| **Smart Contract Tests** | 🔄 Pending | Unit tests (Solidity) + Integration tests (TypeScript) |
| **Deployment Module** | 🔄 Pending | Hardhat Ignition module + Sepolia testnet deployment |
| **Frontend MVP** | 🔄 Pending | Wallet connection, UI for register/transfer/history views |
| **Security Audit** | 🔄 Pending | Pre-mainnet review (if applicable) |

---

## Notes

- Use `bun` instead of `npm` throughout the project
- All development happens on local network (`hardhatMainnet`) before testnet deployment
- Sepolia deployment requires `SEPOLIA_RPC_URL` and `SEPOLIA_PRIVATE_KEY` (set via `bunx hardhat keystore set`)
- No private keys or RPC URLs should be committed to the repository
