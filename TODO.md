# TODO — tracking-chain

> Supply Chain DApp: product traceability on blockchain.
> Check items off as they're completed.

---

## Smart Contract (`packages/blockchain/contracts/`)

- [ ] Create `Tracking.sol` — main supply chain contract
  - [ ] Struct `Product` (id, name, manufacturer, currentOwner, timestamp)
  - [ ] `registerProduct(name)` — only manufacturer role
  - [ ] `transferOwnership(productId, newOwner)` — only current owner
  - [ ] `getProductHistory(productId)` — returns full custody chain
  - [ ] Access control: `onlyOwner` / `onlyManufacturer` modifiers
  - [ ] Input validation: `address != address(0)`, bounds checks
  - [ ] Events on every state change (`ProductRegistered`, `OwnershipTransferred`)
  - [ ] Reentrancy protection (Checks-Effects-Interactions or OZ `ReentrancyGuard`)
- [ ] Remove / replace `Counter.sol` (template artifact)

---

## Tests (`packages/blockchain/contracts/*.t.sol` + `test/*.ts`)

- [ ] `Tracking.t.sol` — Solidity unit tests
  - [ ] Happy path: register product, transfer ownership
  - [ ] Revert: non-manufacturer tries to register
  - [ ] Revert: non-owner tries to transfer
  - [ ] Revert: transfer to `address(0)`
  - [ ] Edge case: empty product name
  - [ ] Fuzz: multiple transfers, history length
- [ ] `Tracking.ts` — TypeScript integration tests (Mocha + ethers.js v6)
  - [ ] Deploy → register → transfer full flow on `hardhatMainnet`
  - [ ] Verify events emitted correctly
  - [ ] Verify `getProductHistory` returns correct chain
  - [ ] Access denied scenarios (non-owner, non-manufacturer)
- [ ] Remove / replace `Counter.ts` and `Counter.t.sol` (template artifacts)

---

## Deployment (`packages/blockchain/ignition/modules/`)

- [ ] Create `Tracking.ts` Hardhat Ignition module
  - [ ] Deploy `Tracking.sol`
  - [ ] Post-deploy: register a sample product (optional smoke test)
- [ ] Test deploy on `hardhatMainnet` locally before Sepolia
- [ ] Deploy to Sepolia — save address + artifact to `ignition/deployments/`
- [ ] Set secrets via Hardhat keystore:
  - [ ] `bunx hardhat keystore set SEPOLIA_RPC_URL`
  - [ ] `bunx hardhat keystore set SEPOLIA_PRIVATE_KEY`
- [ ] Remove / replace `Counter.ts` ignition module

---

## Frontend (`packages/frontend/`)

- [ ] Add `ethers` v6 to frontend dependencies
- [ ] Web3 provider setup
  - [ ] MetaMask / wallet connection button
  - [ ] Provider context (React Context or similar)
  - [ ] Network check (warn if not on correct network)
- [ ] Pages / UI
  - [ ] Replace boilerplate `page.tsx` with app home/dashboard
  - [ ] Register Product page — form: name → call `registerProduct`
  - [ ] Transfer Ownership page — form: productId + newOwner address → call `transferOwnership`
  - [ ] Product History page — input productId → display custody chain timeline
- [ ] Import ABI from compiled artifacts (`ignition/deployments/`)
- [ ] Handle tx states: pending / confirmed / error feedback

---

## Security Checklist (pre-deploy to Sepolia)

- [ ] No private keys or RPC URLs committed to repo
- [ ] All write functions have proper access control
- [ ] All inputs validated (zero address, empty strings, array bounds)
- [ ] Events emitted for every state change
- [ ] Reentrancy considered and mitigated
- [ ] Tests pass on local network (`hardhatMainnet`)
- [ ] Contract audited / reviewed before mainnet (if applicable)
