# PRD-011: Noncustodial Wallet Management

---

## Feature Specification

- **Feature Title:** Noncustodial Wallet Management
- **Feature ID:** IAA-WLT-011
- **Category:** Shop Builder | Web3 Features
- **Actors:** Merchant, Blockchain
- **Channel:** Web Dashboard, Blockchain
- **Status:** Defined
- **Owner:** Product Management
- **Linked Ticket:** Ticket 5 - Management of noncustodial wallets

---

## Part 1: Human-Readable Spec

### Problem Statement

Droplinked provides noncustodial wallets for merchants using Circle and Xion technologies. Merchants need a comprehensive interface to create, view, manage, and transact with these wallets. Without proper wallet management, merchants cannot effectively receive crypto payments, manage their Web3 assets, or participate in blockchain features like on-chain inventory recording.

### User Stories

- As a merchant, I want to create noncustodial wallets so that I can receive crypto payments without relying on external wallets.
- As a merchant, I want to view all my wallets in one dashboard so that I can monitor balances across networks.
- As a merchant, I want to deposit funds to my wallets so that I can top up for gas fees or operations.
- As a merchant, I want to withdraw funds from my wallets so that I can move crypto to external accounts.
- As a merchant, I want to see transaction history so that I can track my crypto activity.

### Key User Journeys

**Journey 1: Wallet Creation**
```
Merchant Navigates to Web3 → Wallets
    ↓
Clicks "Create New Wallet"
    ↓
Selects Wallet Type (Circle / Xion)
    ↓
Selects Network (Polygon, Ethereum, etc.)
    ↓
Reviews Wallet Details
    ↓
Confirms Creation
    ↓
Wallet Generated with Private Key (Encrypted)
    ↓
Backup Phrase Displayed (One-Time)
    ↓
Wallet Ready for Use
```

**Journey 2: Wallet Dashboard**
```
Merchant Opens Wallet Dashboard
    ↓
Views All Wallets Grouped by Network
    ↓
Sees Balances in Native Token and USD
    ↓
Recent Transactions Listed
    ↓
Can Filter by: Network, Token, Date
    ↓
Click Wallet for Details
```

**Journey 3: Deposit and Withdraw**
```
Merchant Selects Wallet
    ↓
Chooses Action: Deposit or Withdraw
    ↓
Deposit:
    └─ Copy Wallet Address QR Code
    └─ Share Address for Payment
    ↓
Withdraw:
    └─ Enter Destination Address
    └─ Enter Amount
    └─ Confirm Transaction
    └─ Pay Gas Fee
    └─ Transaction Submitted
```

### Scope

#### ✅ In Scope:
- Create noncustodial wallets (Circle SDK, Xion)
- Support multiple networks (Polygon, Ethereum, BSC, Arbitrum)
- View wallet balances (native token + ERC-20 tokens)
- Display transaction history
- Deposit functionality (receive funds)
- Withdraw functionality (send funds)
- QR code generation for wallet addresses
- Wallet backup and recovery phrase display
- Export private key (encrypted, password protected)
- Wallet labeling and organization
- Set default wallet per network
- Multi-wallet view dashboard
- Gas fee estimation
- Transaction status tracking
- Security warnings and best practices

#### ❌ Out of Scope:
- Fiat on/off ramp (buy/sell crypto with bank)
- Staking or yield farming
- DeFi integrations (lending, borrowing)
- Multi-signature wallets
- Hardware wallet integration
- Social recovery mechanisms
- Wallet insurance
- Cross-chain bridging within wallet
- Token swapping/trading
- NFT viewing (separate feature)

### Acceptance Criteria

- ☑ Merchant can create up to 5 wallets per network
- ☑ Supported wallet types: Circle Programmable Wallets, Xion
- ☑ Supported networks: Polygon, Ethereum, BSC, Arbitrum, Optimism
- ☑ Wallet creation generates unique address per network
- ☑ Private keys encrypted with merchant password
- ☑ Backup recovery phrase shown once at creation
- ☑ Dashboard shows all wallets with balances
- ☑ Balances update in real-time or on refresh
- ☑ Transaction history shows: date, type, amount, status, hash
- ☑ Deposit shows QR code and copyable address
- ☑ Withdraw requires: destination address, amount, gas confirmation
- ☑ Gas fee estimated before withdrawal confirmation
- ☑ Withdrawal transaction hash displayed after submission
- ☑ Wallets can be labeled (e.g., "Main", "Business", "Savings")
- ☑ Default wallet can be set per network
- ☑ Security warnings displayed for key operations
- ☑ Wallet deletion requires confirmation and password

### Technical Notes

- Circle Wallet API integration
- Xion wallet SDK integration
- Private key encryption: AES-256 with merchant password
- Recovery phrase: BIP-39 standard
- Transaction signing: ECDSA
- Balance queries via blockchain RPC or indexer
- Gas estimation via network APIs
- WebSocket for real-time transaction updates

### Dependencies
- Circle Wallet API credentials
- Xion SDK
- Blockchain node infrastructure
- Encryption service
- Transaction monitoring
- Merchant authentication system
- Gas price oracle

---
