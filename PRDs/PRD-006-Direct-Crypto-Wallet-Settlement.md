# PRD-006: Direct Crypto Wallet Settlement for Merchants

---

## Feature Specification

- **Feature Title:** Direct Crypto Wallet Settlement for Merchants
- **Feature ID:** IAA-WLT-006
- **Category:** Shop Builder | Web3 Payments
- **Actors:** Merchant, Customer
- **Channel:** Web Dashboard, Checkout, Blockchain
- **Status:** Defined
- **Owner:** Product Management
- **Linked Request:** Request 6 - امکان وصل کردن ولت برای دریافت سود مبلغ پرداخت کریپتویی

---

## Part 1: Human-Readable Spec

### Problem Statement

Currently, when customers pay with cryptocurrency, the platform receives the funds and credits the merchant's account, requiring manual withdrawal. Merchants want the ability to connect their own crypto wallets to receive their share of crypto payments directly and immediately, eliminating the delay and platform dependency. If a merchant hasn't connected a wallet, they should receive credit as a fallback. This gives merchants full control over their crypto revenue.

### User Stories

- As a merchant, I want to connect multiple wallets for the same network so that I can split payments between different wallets.
- As a merchant, I want to set percentage splits between wallets so that I can automatically allocate funds (e.g., 70% to wallet A, 30% to wallet B).
- As a merchant, I want to configure wallets based on specific tokens or buckets so that different payment types go to different wallets.
- As a merchant, I want my share of each sale split and sent directly to my configured wallets so that I have immediate custody of funds.
- As a merchant, I want a fallback to platform credit if no wallet is configured for a specific token/bucket so that I never miss a sale.

### Key User Journeys

**Journey 1: Multi-Wallet Setup with Percentage Splits**
```
Merchant Navigates to Payment Settings → Crypto Settlement
    ↓
Selects Network (Polygon, Ethereum, BSC, etc.)
    ↓
Chooses Configuration Mode:
    ├─ By Token: Configure per token (USDC, USDT, DAI, etc.)
    └─ By Bucket: Configure for all tokens (single bucket)
    ↓
Adds First Wallet:
    ├─ Connects Wallet (MetaMask, WalletConnect, etc.)
    ├─ Sets Percentage (e.g., 70%)
    └─ Signs Verification
    ↓
Adds Second Wallet:
    ├─ Connects Second Wallet
    ├─ Sets Percentage (e.g., 30%)
    └─ Signs Verification
    ↓
Validates Total = 100%
    ↓
Wallets Linked with Split Configuration
```

**Journey 2: Multi-Wallet Payment Split Flow**
```
Customer Pays with USDC on Polygon
    ↓
Smart Contract Receives Payment
    ↓
Identifies Token (USDC) and Merchant Configuration
    ↓
Splits Payment According to Merchant's Wallet Setup:
    ├─ 70% → Merchant Wallet A (Direct)
    ├─ 30% → Merchant Wallet B (Direct)
    ├─ [If Affiliate] X% → Co-seller Wallet (Direct)
    └─ Platform Fee → Platform Wallet
    ↓
All Wallets Receive Funds Simultaneously
    ↓
Transaction Confirmed on Blockchain
```

**Journey 3: Fallback to Credit**
```
Customer Pays with USDC
    ↓
Merchant Has No Wallet Connected
    ↓
Smart Contract Detects No Wallet
    ↓
Sends Full Amount to Platform Escrow
    ↓
Merchant Receives Credit in Dashboard
    ↓
Merchant Can Withdraw to Wallet Later
```

### Scope

#### ✅ In Scope:
- Connect multiple wallets per network with percentage splits
- Support for up to 5 wallets per network with configurable percentages
- Configure wallets by token (USDC, USDT, DAI, etc.) or by bucket (all tokens)
- Support multiple networks (Polygon, Ethereum, BSC, Arbitrum, Optimism)
- Direct wallet-to-wallet settlement via smart contracts
- Automatic payment splitting based on configured percentages
- Automatic payment splitting for affiliate orders
- Fallback to platform credit if no wallet configured for specific token
- Wallet management dashboard (add, remove, edit percentages)
- Network-specific and token-specific wallet configurations
- Wallet verification via signature
- Transaction history on blockchain per wallet
- Real-time settlement status tracking
- Support for all stablecoins (USDC, USDT, DAI) and native tokens

#### ❌ Out of Scope:
- Fiat currency settlement to wallets
- Non-custodial wallet creation (use existing external wallets)
- Cross-chain bridging within the same transaction
- Automated wallet recovery
- Multi-signature wallet requirements
- Hardware wallet specific integrations
- Wallet insurance or fund protection
- Staking or yield generation on settled funds
- Subscription/recurring crypto payments

### Acceptance Criteria

- ☑ Merchant can connect up to 5 wallets per network
- ☑ Can configure percentage split between wallets (must total 100%)
- ☑ Can configure by token (different wallets for USDC vs USDT) or by bucket (all tokens)
- ☑ Supported wallets: MetaMask, WalletConnect, Coinbase Wallet
- ☑ Supported networks: Polygon, Ethereum, BSC, Arbitrum, Optimism
- ☑ Wallet connection requires signature verification
- ☑ Smart contract splits payments according to configured percentages
- ☑ Merchant sees all wallet settlements in real-time on dashboard
- ☑ If no wallet configured for specific token, merchant receives credit automatically
- ☑ Transaction hash displayed for every settlement to every wallet
- ☑ Merchant can disconnect or edit wallet percentages at any time
- ☑ Wallet address validation before saving
- ☑ Clear indication of which network and token each wallet supports
- ☑ Settlement history shows all transactions with Etherscan links per wallet
- ☑ Works for both direct sales and affiliate/co-seller scenarios
- ☑ Gas fees handled by customer (deducted from payment)

### Technical Notes

- Use smart contracts for trustless payment splitting
- Implement EIP-1271 for signature verification
- Wallet addresses stored encrypted in database
- Monitor blockchain for settlement confirmations
- Fallback logic in smart contract for missing wallet addresses
- Support ERC-20 tokens (USDC, USDT, DAI) primarily
- Event listeners for transaction status updates

### Dependencies

- Smart contract deployment on each supported network
- Web3 wallet connection libraries (Web3Modal, RainbowKit)
- Blockchain node infrastructure (Alchemy/Infura)
- Existing crypto payment system
- Merchant credit system
- Affiliate network (for split payments)
- Transaction monitoring service

---
