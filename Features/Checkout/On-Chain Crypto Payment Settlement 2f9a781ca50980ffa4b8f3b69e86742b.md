On-Chain Crypto Payment Settlement (DRAFT)

### Feature Specification

- **Feature Title:** On-Chain Crypto Payment Settlement (Merchant Wallet Integration)
- **Feature ID:** IAA-CHK-501
- **Category:** Checkout / Payments
- **Actors:** Merchant (Admin), Shopper (Customer)
- **Channel:** Web
- **Status:** Defined (Beta)
- **Owner:** Product Owner

### Part 1: Human-Readable Spec

#### Problem Statement

Currently, crypto payments require manual processing or go into a pending/credit system before merchants receive their funds. This creates delays, reduces cash flow visibility, and adds operational friction for merchants who want immediate access to their crypto revenue.

#### User Stories

- **As a merchant**, I want to connect my crypto wallet to the dashboard so that I can receive payments directly on-chain without delays.
- **As a merchant**, I want to receive my share of crypto payments immediately when a customer completes checkout so that I have real-time access to my funds.
- **As a shopper**, I want to pay with crypto and know the merchant receives payment instantly so that the transaction feels trustless and transparent.

#### Key User Journeys

1) **Merchant Wallet Setup:**
   - Merchant navigates to Dashboard → Settings → Payment Methods
   - Merchant selects "Connect Crypto Wallet"
   - Merchant chooses blockchain network (e.g., Ethereum, Polygon, BSC)
   - Merchant connects wallet via Web3 wallet connector (MetaMask, WalletConnect, etc.)
   - System validates and stores wallet address
   - Merchant sees confirmation: "Wallet connected successfully"

2) **Shopper Checkout with Crypto:**
   - Shopper adds items to cart and proceeds to checkout
   - Shopper selects "Pay with Crypto" as payment method
   - System displays supported cryptocurrencies and network fees
   - Shopper confirms payment and signs transaction via their wallet
   - Transaction is submitted to blockchain
   - Upon confirmation, merchant's share is transferred immediately to their connected wallet
   - Shopper receives order confirmation
   - Merchant receives on-chain notification of payment

3) **Merchant Payment Receipt:**
   - Merchant sees real-time notification: "Payment received: [Amount] [Token] on [Network]"
   - Merchant can view transaction hash and blockchain explorer link
   - Payment appears in merchant's wallet balance immediately (no pending state)

#### Scope

- ✅ **In Scope:**
  - Merchant wallet connection flow (Web3 integration)
  - Support for major EVM-compatible chains (Ethereum, Polygon, BSC)
  - Immediate on-chain transfer of merchant share upon payment confirmation
  - Transaction status tracking and notifications
  - Dashboard UI for wallet management (connect, disconnect, view connected address)
  - Basic error handling for failed transactions
  - Beta feature flagging (opt-in only)

- ❌ **Out of Scope:**
  - Non-EVM chains (Bitcoin, Solana, etc.) - future iteration
  - Fiat currency conversion or off-ramping
  - Advanced wallet features (multi-sig, hardware wallet-specific flows)
  - Dispute resolution or chargeback handling
  - Automated accounting/tax reporting
  - Subscription or recurring payment support
  - Escrow or holding periods
  - Gas fee subsidies or optimization

#### Acceptance Criteria

- [ ] Merchant can connect one wallet per supported blockchain network
- [ ] Wallet connection persists across sessions until manually disconnected
- [ ] When shopper pays with crypto, merchant receives exactly their configured share to their wallet within 2 minutes of blockchain confirmation
- [ ] No "pending" or "credit" balance state exists for on-chain crypto payments
- [ ] System validates wallet address format before allowing connection
- [ ] Merchant can view connected wallet address and network in dashboard
- [ ] Merchant can disconnect wallet at any time
- [ ] Failed transactions display clear error messages with retry options
- [ ] Beta feature is gated behind feature flag and opt-in consent
- [ ] Transaction history shows blockchain explorer links for verification

#### Technical Notes

- Requires smart contract or payment processor integration that supports direct transfers
- Gas fees will be borne by either shopper or platform (TBD - see open questions)
- Must handle network congestion and transaction failures gracefully
- Beta period should include monitoring and alert systems for failed settlements
- Consider implementing a small testnet flow for merchant onboarding

#### Dependencies

- Web3 wallet connector library (e.g., RainbowKit, Web3Modal)
- Blockchain node provider (e.g., Alchemy, Infura)
- Smart contract deployment for payment splitting/routing
- Dashboard settings page modifications
- Checkout payment method integration
- Notification system for real-time payment alerts

### Part 2: AI-Centric Layer (Functional Logic Depth)

#### Definitions & Glossary

- **Merchant Wallet**: The externally-owned account (EOA) or smart contract wallet controlled by the merchant where funds are sent
- **On-Chain Settlement**: Direct transfer of tokens to the merchant's wallet address recorded on the blockchain
- **Web3 Connector**: Library interface allowing dApps to connect to user wallets (MetaMask, WalletConnect, Coinbase Wallet, etc.)
- **Payment Split**: The portion of the total order amount that belongs to the merchant after platform fees
- **Transaction Confirmation**: State where the blockchain transaction has been mined and reached sufficient finality (e.g., 1 block confirmation)
- **Beta Flag**: Feature toggle allowing controlled rollout to select merchants

#### Exhaustive Functional Logic

**Wallet Connection Flow:**
```
IF merchant navigates to payment settings
AND selects "Connect Crypto Wallet"
THEN display supported networks list

IF merchant selects network (e.g., Polygon)
THEN initialize Web3 connector for that network

IF merchant approves connection in wallet interface
THEN:
   1. Capture wallet address
   2. Validate address format for selected network
   3. Store encrypted wallet address in merchant profile
   4. Set wallet_status = "connected"
   5. Display success confirmation
ELSE IF connection rejected
   Display error: "Connection cancelled. Please try again."

IF merchant attempts to connect second wallet on same network
   Display: "Replace existing wallet? This will disconnect [current_address]"
```

**Checkout Payment Flow:**
```
IF shopper selects "Pay with Crypto" at checkout
AND merchant has connected_wallet for selected network
THEN:
   1. Calculate merchant_share = order_total - platform_fee
   2. Generate payment transaction with:
      - recipient = merchant_connected_wallet
      - amount = merchant_share
      - token = shopper_selected_token
   3. Display transaction details to shopper
   4. Wait for shopper wallet signature
   
IF shopper confirms transaction
THEN:
   1. Submit transaction to blockchain
   2. Monitor mempool for transaction status
   3. Poll for confirmation (1 block minimum)
   
IF transaction status = "confirmed"
THEN:
   1. Transfer merchant_share to merchant_wallet (on-chain)
   2. Transfer platform_fee to platform_treasury (on-chain)
   3. Create order record with status = "paid"
   4. Emit payment_received event
   5. Send real-time notification to merchant
   6. Display confirmation to shopper
   
IF transaction status = "failed" OR timeout > 10 minutes
THEN:
   1. Log failure reason
   2. Notify merchant of failed settlement
   3. Display error to shopper with retry option
   4. Create support ticket auto-magically
```

**Wallet Management:**
```
IF merchant clicks "Disconnect Wallet"
THEN:
   1. Require password/2FA confirmation
   2. Remove wallet_address from profile
   3. Set wallet_status = "disconnected"
   4. Revoke Web3 session
   5. Display: "Wallet disconnected successfully"

IF merchant attempts checkout without connected wallet
THEN display: "Crypto payments disabled - please connect wallet in settings"
```

#### Edge Cases & Error Handling

- **Invalid Wallet Address**: Validate checksum before storing; reject malformed addresses with specific error message
- **Network Mismatch**: If shopper attempts payment on different network than merchant's connected wallet, display network selector or error
- **Insufficient Gas**: If merchant wallet is contract wallet requiring gas, handle gracefully or require EOA
- **Transaction Reversion**: If smart contract call reverts (e.g., token transfer failed), log error, notify both parties, trigger manual review process
- **Double Spend Attempt**: Implement idempotency keys for payment transactions; reject duplicate order IDs
- **Merchant Wallet Compromised**: Provide emergency disconnect feature; require 2FA for wallet changes
- **Blockchain Reorganization**: Wait for 2-3 block confirmations before marking payment as final to handle chain reorgs
- **Network Congestion**: Implement dynamic gas price estimation; allow shopper to speed up transaction
- **Token Not Supported**: Maintain whitelist of accepted tokens; reject unsupported tokens at checkout
- **Beta Opt-Out**: If merchant disables beta feature after setup, revert to legacy payment flow (credit/pending system)
- **Partial Payments**: Handle cases where shopper sends incorrect amount; allow refund or adjustment workflow

### Attachments

- 📎 Linked Tickets: (To be linked)
- 📎 Related Docs: Payment Processing Architecture, Smart Contract Specifications
- 📎 Mockups: Dashboard Wallet Settings UI, Checkout Crypto Payment Flow

### Change Log

- 2026-02-11 — Product Owner — Initial draft created

---

SPEC (FINAL)

### Feature Specification

- **Feature Title:** On-Chain Crypto Payment Settlement (Merchant Wallet Integration)
- **Feature ID:** IAA-CHK-501
- **Category:** Checkout / Payments
- **Actors:** Merchant (Admin), Shopper (Customer)
- **Channel:** Web
- **Status:** Defined
- **Owner:** Product Owner

### Part 1: Human-Readable Spec

#### Problem Statement

Currently, crypto payments require manual processing or go into a pending/credit system before merchants receive their funds. This creates delays, reduces cash flow visibility, and adds operational friction for merchants who want immediate access to their crypto revenue.

#### User Stories

- **As a merchant**, I want to connect my crypto wallet to the dashboard so that I can receive payments directly on-chain without delays.
- **As a merchant**, I want to receive my share of crypto payments immediately when a customer completes checkout so that I have real-time access to my funds.
- **As a shopper**, I want to pay with crypto and know the merchant receives payment instantly so that the transaction feels trustless and transparent.

#### Key User Journeys

1) **Merchant Wallet Setup:**
   - Merchant navigates to Dashboard → Settings → Payment Methods
   - Merchant selects "Connect Crypto Wallet"
   - Merchant chooses blockchain network (e.g., Ethereum, Polygon, BSC)
   - Merchant connects wallet via Web3 wallet connector (MetaMask, WalletConnect, etc.)
   - System validates and stores wallet address
   - Merchant sees confirmation: "Wallet connected successfully"

2) **Shopper Checkout with Crypto:**
   - Shopper adds items to cart and proceeds to checkout
   - Shopper selects "Pay with Crypto" as payment method
   - System displays supported cryptocurrencies and network fees
   - Shopper confirms payment and signs transaction via their wallet
   - Transaction is submitted to blockchain
   - Upon confirmation, merchant's share is transferred immediately to their connected wallet
   - Shopper receives order confirmation
   - Merchant receives on-chain notification of payment

3) **Merchant Payment Receipt:**
   - Merchant sees real-time notification: "Payment received: [Amount] [Token] on [Network]"
   - Merchant can view transaction hash and blockchain explorer link
   - Payment appears in merchant's wallet balance immediately (no pending state)

#### Scope

- ✅ **In Scope:**
  - Merchant wallet connection flow (Web3 integration)
  - Support for major EVM-compatible chains (Ethereum, Polygon, BSC)
  - Immediate on-chain transfer of merchant share upon payment confirmation
  - Transaction status tracking and notifications
  - Dashboard UI for wallet management (connect, disconnect, view connected address)
  - Basic error handling for failed transactions
  - Beta feature flagging (opt-in only)

- ❌ **Out of Scope:**
  - Non-EVM chains (Bitcoin, Solana, etc.) - future iteration
  - Fiat currency conversion or off-ramping
  - Advanced wallet features (multi-sig, hardware wallet-specific flows)
  - Dispute resolution or chargeback handling
  - Automated accounting/tax reporting
  - Subscription or recurring payment support
  - Escrow or holding periods
  - Gas fee subsidies or optimization

#### Acceptance Criteria

- [ ] Merchant can connect one wallet per supported blockchain network
- [ ] Wallet connection persists across sessions until manually disconnected
- [ ] When shopper pays with crypto, merchant receives exactly their configured share to their wallet within 2 minutes of blockchain confirmation
- [ ] No "pending" or "credit" balance state exists for on-chain crypto payments
- [ ] System validates wallet address format before allowing connection
- [ ] Merchant can view connected wallet address and network in dashboard
- [ ] Merchant can disconnect wallet at any time
- [ ] Failed transactions display clear error messages with retry options
- [ ] Beta feature is gated behind feature flag and opt-in consent
- [ ] Transaction history shows blockchain explorer links for verification

#### Technical Notes

- Requires smart contract or payment processor integration that supports direct transfers
- Gas fees will be borne by either shopper or platform (TBD - see open questions)
- Must handle network congestion and transaction failures gracefully
- Beta period should include monitoring and alert systems for failed settlements
- Consider implementing a small testnet flow for merchant onboarding

#### Dependencies

- Web3 wallet connector library (e.g., RainbowKit, Web3Modal)
- Blockchain node provider (e.g., Alchemy, Infura)
- Smart contract deployment for payment splitting/routing
- Dashboard settings page modifications
- Checkout payment method integration
- Notification system for real-time payment alerts

### Part 2: AI-Centric Layer (Functional Logic Depth)

#### Definitions & Glossary

- **Merchant Wallet**: The externally-owned account (EOA) or smart contract wallet controlled by the merchant where funds are sent
- **On-Chain Settlement**: Direct transfer of tokens to the merchant's wallet address recorded on the blockchain
- **Web3 Connector**: Library interface allowing dApps to connect to user wallets (MetaMask, WalletConnect, Coinbase Wallet, etc.)
- **Payment Split**: The portion of the total order amount that belongs to the merchant after platform fees
- **Transaction Confirmation**: State where the blockchain transaction has been mined and reached sufficient finality (e.g., 1 block confirmation)
- **Beta Flag**: Feature toggle allowing controlled rollout to select merchants

#### Exhaustive Functional Logic

**Wallet Connection Flow:**
```
IF merchant navigates to payment settings
AND selects "Connect Crypto Wallet"
THEN display supported networks list

IF merchant selects network (e.g., Polygon)
THEN initialize Web3 connector for that network

IF merchant approves connection in wallet interface
THEN:
   1. Capture wallet address
   2. Validate address format for selected network
   3. Store encrypted wallet address in merchant profile
   4. Set wallet_status = "connected"
   5. Display success confirmation
ELSE IF connection rejected
   Display error: "Connection cancelled. Please try again."

IF merchant attempts to connect second wallet on same network
   Display: "Replace existing wallet? This will disconnect [current_address]"
```

**Checkout Payment Flow:**
```
IF shopper selects "Pay with Crypto" at checkout
AND merchant has connected_wallet for selected network
THEN:
   1. Calculate merchant_share = order_total - platform_fee
   2. Generate payment transaction with:
      - recipient = merchant_connected_wallet
      - amount = merchant_share
      - token = shopper_selected_token
   3. Display transaction details to shopper
   4. Wait for shopper wallet signature
   
IF shopper confirms transaction
THEN:
   1. Submit transaction to blockchain
   2. Monitor mempool for transaction status
   3. Poll for confirmation (1 block minimum)
   
IF transaction status = "confirmed"
THEN:
   1. Transfer merchant_share to merchant_wallet (on-chain)
   2. Transfer platform_fee to platform_treasury (on-chain)
   3. Create order record with status = "paid"
   4. Emit payment_received event
   5. Send real-time notification to merchant
   6. Display confirmation to shopper
   
IF transaction status = "failed" OR timeout > 10 minutes
THEN:
   1. Log failure reason
   2. Notify merchant of failed settlement
   3. Display error to shopper with retry option
   4. Create support ticket auto-magically
```

**Wallet Management:**
```
IF merchant clicks "Disconnect Wallet"
THEN:
   1. Require password/2FA confirmation
   2. Remove wallet_address from profile
   3. Set wallet_status = "disconnected"
   4. Revoke Web3 session
   5. Display: "Wallet disconnected successfully"

IF merchant attempts checkout without connected wallet
THEN display: "Crypto payments disabled - please connect wallet in settings"
```

#### Edge Cases & Error Handling

- **Invalid Wallet Address**: Validate checksum before storing; reject malformed addresses with specific error message
- **Network Mismatch**: If shopper attempts payment on different network than merchant's connected wallet, display network selector or error
- **Insufficient Gas**: If merchant wallet is contract wallet requiring gas, handle gracefully or require EOA
- **Transaction Reversion**: If smart contract call reverts (e.g., token transfer failed), log error, notify both parties, trigger manual review process
- **Double Spend Attempt**: Implement idempotency keys for payment transactions; reject duplicate order IDs
- **Merchant Wallet Compromised**: Provide emergency disconnect feature; require 2FA for wallet changes
- **Blockchain Reorganization**: Wait for 2-3 block confirmations before marking payment as final to handle chain reorgs
- **Network Congestion**: Implement dynamic gas price estimation; allow shopper to speed up transaction
- **Token Not Supported**: Maintain whitelist of accepted tokens; reject unsupported tokens at checkout
- **Beta Opt-Out**: If merchant disables beta feature after setup, revert to legacy payment flow (credit/pending system)
- **Partial Payments**: Handle cases where shopper sends incorrect amount; allow refund or adjustment workflow

### Attachments

- 📎 Linked Tickets: (To be linked)
- 📎 Related Docs: Payment Processing Architecture, Smart Contract Specifications
- 📎 Mockups: Dashboard Wallet Settings UI, Checkout Crypto Payment Flow

### Change Log

- 2026-02-11 — Product Owner — Finalized spec
