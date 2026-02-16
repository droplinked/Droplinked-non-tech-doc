# PRD-010: Onchain Inventory (Record Products on Blockchain)

---

## Feature Specification

- **Feature Title:** Onchain Inventory - Record Products on Blockchain
- **Feature ID:** IAA-WEB3-010
- **Category:** Shop Builder | Web3 Features
- **Actors:** Merchant, Customer, Blockchain
- **Channel:** Web Dashboard, Shopfront, Blockchain
- **Status:** Defined
- **Owner:** Product Management
- **Linked Ticket:** Ticket 4 - Onchain Inventory – Record Products on Blockchain

---

## Part 1: Human-Readable Spec

### Problem Statement

Merchants need the ability to record their products on the blockchain to provide transparency, provenance, and authenticity verification. This creates an immutable record of product creation, ownership, and transaction history. After recording, products can be tracked through the entire lifecycle - who created it, who bought it, who sold it, and how payments were distributed - creating trust and transparency in the commerce ecosystem.

### User Stories

- As a merchant, I want to record my products on the blockchain so that I can prove authenticity and ownership.
- As a customer, I want to verify a product's blockchain record so that I can trust its provenance.
- As a merchant, I want to track my product's entire lifecycle on-chain so that I have transparent records.
- As a platform, I want products recorded on blockchain to have verifiable NFT certificates so that we differentiate from competitors.

### Key User Journeys

**Journey 1: Recording a Product on Blockchain**
```
Merchant Opens Product Detail
    ↓
Clicks "Record on Blockchain"
    ↓
Selects Network (Polygon, Ethereum, etc.)
    ↓
Reviews Product Data to be Recorded
    ↓
Confirms and Pays Gas Fee
    ↓
Smart Contract Creates NFT
    ↓
Product Becomes Read-Only (Immutable)
    ↓
NFT Verification Link Available on Storefront
```

**Journey 2: Customer Verification**
```
Customer Views Product on Storefront
    ↓
Sees "Verified on Blockchain" Badge
    ↓
Clicks Verification Link
    ↓
Views Blockchain Record:
    - Creator wallet address
    - Creation timestamp
    - Transaction history
    - Ownership transfers
    - Payment distributions
    ↓
Verifies Authenticity via Etherscan
```

**Journey 3: Post-Sale On-Chain Transfer**
```
Customer Purchases Recorded Product
    ↓
Payment Confirmed on Blockchain
    ↓
Smart Contract:
    - Transfers payment to merchant
    - Records transaction metadata
    - Updates ownership record
    - Distributes affiliate commissions (if applicable)
    ↓
Customer Can Export NFT to Personal Wallet
    ↓
Full Transaction History Visible on Chain
```

### Scope

#### ✅ In Scope:
- Record products as NFTs on blockchain
- Support multiple networks (Polygon, Ethereum, BSC)
- Product data immutability after recording
- NFT minting with product metadata
- Blockchain verification badge on storefront
- Customer-facing verification page/link
- Transaction history tracking on-chain
- Ownership transfer recording
- Payment distribution transparency
- Bulk recording for multiple products
- Gas fee estimation and payment
- Wallet lock per network (security)
- Export NFT to external wallet
- Read-only product state after recording

#### ❌ Out of Scope:
- Fractional ownership of products
- Product trading between customers (secondary market)
- Dynamic/product NFTs (updatable metadata)
- Cross-chain product recording
- Decentralized storage for product images (IPFS optional)
- Physical product tokenization (digital only)
- Subscription/recurring product recording
- DAO governance for product decisions

### Acceptance Criteria

- ☑ Merchant can record individual products on blockchain
- ☑ Merchant can bulk record up to 50 products at once
- ☑ Supported networks: Polygon (primary), Ethereum, BSC
- ☑ Gas fee displayed and confirmed before recording
- ☑ Product becomes read-only after recording (no edits)
- ☑ NFT minted with product metadata (name, description, image URL)
- ☑ Blockchain verification badge appears on storefront PDP
- ☑ Verification link opens blockchain explorer (Etherscan/Polygonscan)
- ☑ Wallet locked to network after first recording (same wallet required)
- ☑ Transaction hash stored and displayed
- ☑ Recording history available in product audit log
- ☑ Customer can view on-chain ownership history
- ☑ Payment splits recorded on blockchain for transparency
- ☑ Failed recordings can be retried (if gas issue)
- ☑ Recording status: Pending → Processing → Confirmed/Failed

### Technical Notes

- ERC-721 or ERC-1155 NFT standard
- Metadata stored on-chain (URI reference)
- Smart contract: `DroplinkedProductRegistry`
- Gas optimization: Batch minting for bulk operations
- Wallet lock: Prevent wallet switching after first use per network
- Event emission for all recording actions
- IPFS optional for decentralized image storage

### Dependencies
- Smart contract deployment per network
- Web3 wallet connection
- Product management system
- Blockchain node infrastructure
- Gas estimation service
- Transaction monitoring
- Shopfront rendering
- NFT metadata storage

---
