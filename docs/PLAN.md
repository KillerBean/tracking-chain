# Implementation Plan - Supply Chain DApp

## Goal Description
Create a Decentralized Application (DApp) for product traceability. The system will allow manufacturers to register products and current owners to transfer ownership, maintaining an immutable history of custody on the blockchain.

## User Review Required
> [!IMPORTANT]
> This plan assumes a local development environment (Hardhat Network). Deployment to testnets (Sepolia, etc.) is out of scope for the initial setup but can be added later.

## Proposed Architecture

### Tech Stack
- **Smart Contracts**: Solidity ^0.8.0
- **Dev Environment**: Hardhat (Compilation, Testing, Local Node)
- **Frontend**: Next.js (React Framework), ethers.js (Blockchain Interaction), TailwindCSS (Styling)

### Smart Contract Design (`Tracking.sol`)

#### Data Structures
```solidity
struct Transfer {
    address owner;
    uint256 timestamp;
}

struct Product {
    uint256 id;
    string name;
    string manufacturer;
    address currentOwner;
    bool exists;
    // We might store history in a separate mapping to save gas on the struct, 
    // or keep it simple for this MVP.
}
```

#### Key Functions
1.  **`registerProduct(string _name, string _manufacturer)`**
    -   **inputs**: Product Name, Manufacturer Name.
    -   **logic**: Creates a new product with a unique ID. Assigns `msg.sender` as the creator and current owner. Records the initial "creation" event in history.
    -   **events**: `ProductRegistered(uint256 indexed id, address owner)`

2.  **`transferOwnership(uint256 _productId, address _newOwner)`**
    -   **inputs**: Product ID, New Owner Address.
    -   **checks**: Only the `currentOwner` can transfer.
    -   **logic**: Updates `currentOwner` to `_newOwner`. Adds a new entry to the product's history with the timestamp.
    -   **events**: `OwnershipTransferred(uint256 indexed id, address previousOwner, address newOwner)`

3.  **`getProductHistory(uint256 _productId)`**
    -   **inputs**: Product ID.
    -   **returns**: Array of `Transfer` structs (previous owners + timestamps).

### Frontend Design (Next.js)
1.  **Wallet Connection**: Button to connect MetaMask/Browser Wallet.
2.  **Register View**: unique view for Minting/Registering items.
3.  **Dashboard/Inventory**: List of items currently owned by the connected wallet.
4.  **Product Detail**: View showing full history of a specific product ID.

## Verification Plan

### Automated Tests (Hardhat)
-   **Register**: Verify product is created with correct data.
-   **Transfer**: Verify ownership changes and history length increases.
-   **Security**: Verify *only* the owner can transfer a product.

### Manual Verification
-   Start local node (`npx hardhat node`).
-   Deploy contract (`npx hardhat run scripts/deploy.js`).
-   Start frontend (`npm run dev`).
-   Connect Wallet 1 (Manufacturer).
-   Register "Coffee Batch #101".
-   Verify it appears in inventory.
-   Transfer to Wallet 2 (Distributor).
-   Switch to Wallet 2 in MetaMask.
-   Verify "Coffee Batch #101" is now in Wallet 2's inventory.
-   Check History log for the product.
