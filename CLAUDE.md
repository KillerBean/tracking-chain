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

## Testing

| Nível | O que testa | Ferramentas |
|-------|-------------|-------------|
| **Solidity unit** | Contrato em isolamento, invariantes, reverts | Hardhat + Foundry (`*.t.sol`) |
| **TypeScript integration** | Fluxo completo (deploy → interact) em rede simulada | Mocha + ethers.js v6 |

Sempre testar em rede local (`hardhatMainnet`) antes de fazer deploy em Sepolia.
Cenários obrigatórios: happy path, acesso negado (non-owner), transfer de propriedade, edge cases de inputs.

## Smart Contract Security

- **Reentrancy**: use Checks-Effects-Interactions ou `ReentrancyGuard` (OpenZeppelin) em funções que fazem calls externas
- **Access Control**: use `onlyOwner` / `onlyManufacturer` em todas as funções de escrita críticas
- **Integer overflow**: Solidity 0.8+ reverte automaticamente — não use `unchecked` sem justificativa
- **Validação de input**: sempre validar `address != address(0)` e bounds de arrays
- **Eventos**: emitir eventos em todas as mudanças de estado para rastreabilidade off-chain
- **Imutabilidade**: contratos deployados não podem ser atualizados — use Proxy Pattern se upgrades forem necessários

## Deploy & Secrets

Nunca commitar chaves privadas ou RPC URLs. Use o keystore do Hardhat:
```bash
bunx hardhat keystore set SEPOLIA_RPC_URL
bunx hardhat keystore set SEPOLIA_PRIVATE_KEY
```

Deploy em Sepolia é irreversível — testar exaustivamente em rede local primeiro.
Guardar o endereço do contrato deployado e o artifact em `ignition/deployments/`.

## Bun Preferences

- Use `bun` / `bunx` instead of `npm` / `npx`
- Bun auto-loads `.env` files
- See `node_modules/bun-types/docs/**.mdx` for Bun API docs
