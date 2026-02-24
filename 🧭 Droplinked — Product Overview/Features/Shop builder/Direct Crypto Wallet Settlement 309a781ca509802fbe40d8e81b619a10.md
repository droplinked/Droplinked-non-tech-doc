# Direct Crypto Wallet Settlement

### Feature ID:

**[IAA-WLT-006]**

### Title:

**Direct Crypto Wallet Settlement for Merchants**

### Category:

**Shop Builder | Web3 Payments** | **Actors**: Merchant, Customer | **Channel**: Web Dashboard, Checkout, Blockchain

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
Currently, when customers pay with cryptocurrency, the platform receives the funds and credits the merchant's account, requiring manual withdrawal. Merchants want the ability to connect their own crypto wallets to receive their share of crypto payments directly and immediately, eliminating the delay and platform dependency.

**Desired Outcome:**

- Merchant can connect up to 5 wallets per network
- Can configure percentage split between wallets (must total 100%)
- Can configure by token (different wallets for USDC vs USDT) or by bucket (all tokens)
- Smart contract splits payments according to configured percentages
- If no wallet configured for specific token, merchant receives credit automatically

---

### 2) **Scope – In / Out**

**In Scope:**

- **Multi-Wallet Configuration**
    - Connect multiple wallets per network with percentage splits
    - Support for up to 5 wallets per network with configurable percentages
    - Configure wallets by token (USDC, USDT, DAI, etc.) or by bucket (all tokens)
    - Support multiple networks (Polygon, Ethereum, BSC, Arbitrum, Optimism)
- **Payment Settlement**
    - Direct wallet-to-wallet settlement via smart contracts
    - Automatic payment splitting based on configured percentages
    - Automatic payment splitting for affiliate orders
    - Real-time settlement status tracking
- **Fallback Mechanism**
    - Fallback to platform credit if no wallet configured for specific token
- **Wallet Management**
    - Wallet management dashboard (add, remove, edit percentages)
    - Network-specific and token-specific wallet configurations
    - Wallet verification via signature
    - Transaction history on blockchain per wallet
- **Supported Assets**
    - Support for all stablecoins (USDC, USDT, DAI) and native tokens

**Out of Scope:**

- Fiat currency settlement to wallets
- Non-custodial wallet creation (use existing external wallets)
- Cross-chain bridging within the same transaction
- Automated wallet recovery
- Multi-signature wallet requirements
- Hardware wallet specific integrations
- Wallet insurance or fund protection
- Staking or yield generation on settled funds
- Subscription/recurring crypto payments

---

### 3) **Key User Journeys**

---

**Journey 1: Multi-Wallet Setup with Percentage Splits**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Navigates to Payment Settings → Crypto Settlement | Settlement settings page loads |
| 2 | Merchant | Selects Network (Polygon, Ethereum, BSC, etc.) | Network selected |
| 3 | Merchant | Chooses Configuration Mode | Options: By Token or By Bucket |
| 4 | Merchant | Adds First Wallet | Wallet connection modal opens |
| 5 | Merchant | Connects Wallet (MetaMask, WalletConnect, etc.) | Wallet connected |
| 6 | Merchant | Sets Percentage (e.g., 70%) | Percentage set |
| 7 | Merchant | Signs Verification | Signature verified |
| 8 | Merchant | Adds Second Wallet | Second wallet connected |
| 9 | Merchant | Validates Total = 100% | Validation passed |
| 10 | System | Wallets Linked with Split Configuration | Configuration saved |

---

**Journey 2: Multi-Wallet Payment Split Flow**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Pays with USDC on Polygon | Payment initiated |
| 2 | System | Smart Contract Receives Payment | Payment received |
| 3 | System | Identifies Token (USDC) and Merchant Configuration | Configuration loaded |
| 4 | System | Splits Payment According to Merchant's Wallet Setup | Splits executed: |

```
- 70% → Merchant Wallet A (Direct)
- 30% → Merchant Wallet B (Direct)
- [If Affiliate] X% → Co-seller Wallet (Direct)
- Platform Fee → Platform Wallet |
```

| 5 | System | All Wallets Receive Funds Simultaneously | Funds transferred |
| 6 | System | Transaction Confirmed on Blockchain | Confirmation recorded |

---

**Journey 3: Fallback to Credit**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Pays with USDC | Payment initiated |
| 2 | System | Merchant Has No Wallet Connected | Configuration check fails |
| 3 | System | Smart Contract Detects No Wallet | Fallback triggered |
| 4 | System | Sends Full Amount to Platform Escrow | Funds held in escrow |
| 5 | System | Merchant Receives Credit in Dashboard | Credit balance updated |
| 6 | Merchant | Can Withdraw to Wallet Later | Withdrawal option available |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria |
| --- | --- |
| **Wallet Configuration** |  |
| BAC-1 | Merchant can connect up to 5 wallets per network |
| BAC-2 | Can configure percentage split between wallets (must total 100%) |
| BAC-3 | Can configure by token (different wallets for USDC vs USDT) or by bucket (all tokens) |
| BAC-4 | Supported wallets: MetaMask, WalletConnect, Coinbase Wallet |
| BAC-5 | Supported networks: Polygon, Ethereum, BSC, Arbitrum, Optimism |
| BAC-6 | Wallet connection requires signature verification |
| **Payment Settlement** |  |
| BAC-7 | Smart contract splits payments according to configured percentages |
| BAC-8 | Merchant sees all wallet settlements in real-time on dashboard |
| BAC-9 | If no wallet configured for specific token, merchant receives credit automatically |
| BAC-10 | Transaction hash displayed for every settlement to every wallet |
| BAC-11 | Merchant can disconnect or edit wallet percentages at any time |
| BAC-12 | Wallet address validation before saving |
| **Tracking & Transparency** |  |
| BAC-13 | Clear indication of which network and token each wallet supports |
| BAC-14 | Settlement history shows all transactions with Etherscan links per wallet |
| BAC-15 | Works for both direct sales and affiliate/co-seller scenarios |
| BAC-16 | Gas fees handled by customer (deducted from payment) |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-16 | Behdad | Converted from PRD-006 to Feature format | Documentation structure |
| 2026-02-01 | Product Management | Initial PRD creation | New feature request |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **Direct Settlement** | Payment transferred directly to merchant wallet without platform holding |
| **Percentage Split** | Dividing payment across multiple wallets by percentage |
| **Token Bucket** | Grouping all tokens under single wallet configuration |
| **Per-Token Config** | Separate wallet configuration for each token type |
| **Fallback Credit** | Platform credit received when no wallet configured |
| **Smart Contract Split** | Automated payment distribution via blockchain contract |

---

### B2) Exhaustive Functional Logic

---

### **FL-1: Wallet Configuration**

```
FUNCTION addWallet(merchant_id, config):
    // Validate merchant
    merchant = GET merchant WHERE id == merchant_id
    IF NOT merchant:
        RETURN error("Invalid merchant")

    // Validate network
    IF config.network NOT IN SUPPORTED_NETWORKS:
        RETURN error("Unsupported network")

    // Validate wallet address
    IF NOT isValidAddress(config.address, config.network):
        RETURN error("Invalid wallet address")

    // Check wallet limit
    existing_count = COUNT wallets
        WHERE merchant_id == merchant_id
        AND network == config.network

    IF existing_count >= 5:
        RETURN error("Maximum 5 wallets per network reached")

    // Validate percentage
    IF config.percentage <= 0 OR config.percentage > 100:
        RETURN error("Percentage must be between 1-100")

    // Verify with signature
    IF NOT verifySignature(config.address, config.signature):
        RETURN error("Signature verification failed")

    // Create wallet record
    wallet = CREATE Wallet({
        merchant_id: merchant_id,
        address: config.address,
        network: config.network,
        token_type: config.token_type, // 'all' or specific token
        percentage: config.percentage,
        verified: true,
        created_at: NOW
    })

    // Validate total percentage
    total = SUM percentages WHERE merchant_id == merchant_id AND network == config.network
    IF total > 100:
        DELETE wallet
        RETURN error("Total percentage exceeds 100%")

    RETURN success(wallet)

FUNCTION updateWalletPercentage(wallet_id, new_percentage):
    wallet = GET wallet WHERE id == wallet_id

    // Calculate new total
    other_wallets = GET wallets
        WHERE merchant_id == wallet.merchant_id
        AND network == wallet.network
        AND id != wallet_id

    other_total = SUM other_wallets.percentage
    new_total = other_total + new_percentage

    IF new_total > 100:
        RETURN error("New total would exceed 100%")

    wallet.percentage = new_percentage
    SAVE wallet

    RETURN success

FUNCTION removeWallet(wallet_id):
    wallet = GET wallet WHERE id == wallet_id

    // Check if any pending settlements
    pending = COUNT settlements WHERE wallet_id == wallet_id AND status == 'pending'
    IF pending > 0:
        RETURN error("Cannot remove wallet with pending settlements")

    DELETE wallet

    // Recalculate percentages for remaining wallets
    remaining = GET wallets WHERE merchant_id == wallet.merchant_id AND network == wallet.network
    IF remaining.length > 0:
        // Proportionally redistribute or require manual update
        NOTIFY merchant "Wallet removed. Please update remaining wallet percentages."

    RETURN success
```

---

### **FL-2: Payment Splitting Logic**

```
FUNCTION processPayment(order):
    payment = order.payment
    merchant = order.merchant

    // Get wallet configuration
    wallets = GET wallets
        WHERE merchant_id == merchant.id
        AND network == payment.network
        AND (token_type == payment.token OR token_type == 'all')

    IF wallets.length == 0:
        // No wallet configured - fallback to credit
        RETURN processFallbackCredit(merchant, payment)

    // Validate percentages sum to 100
    total_percentage = SUM wallets.percentage
    IF total_percentage != 100:
        LOG error "Wallet percentages don't sum to 100"
        // Use proportional split
        wallets = normalizePercentages(wallets)

    // Calculate splits
    splits = []
    FOR each wallet IN wallets:
        split_amount = (payment.amount * wallet.percentage) / 100
        splits.push({
            wallet: wallet,
            amount: split_amount,
            token: payment.token
        })

    // Add platform fee
    platform_fee = (payment.amount * PLATFORM_FEE_PERCENT) / 100

    // Execute smart contract
    transaction = smart_contract.splitPayment({
        recipient_splits: splits,
        platform_fee: platform_fee,
        order_id: order.id
    })

    // Record settlements
    FOR each split IN splits:
        CREATE Settlement({
            order_id: order.id,
            wallet_id: split.wallet.id,
            amount: split.amount,
            token: split.token,
            transaction_hash: transaction.hash,
            status: 'pending',
            created_at: NOW
        })

    // Monitor for confirmation
    monitorTransaction(transaction.hash)

    RETURN transaction

FUNCTION processFallbackCredit(merchant, payment):
    // Create credit record
    credit = CREATE Credit({
        merchant_id: merchant.id,
        amount: payment.amount,
        token: payment.token,
        source: 'payment_fallback',
        order_id: order.id,
        status: 'credited',
        created_at: NOW
    })

    // Update merchant balance
    UPDATE merchant.credit_balance += payment.amount

    // Send notification
    NOTIFY merchant "Payment received as credit. Connect a wallet for direct settlement."

    RETURN credit

FUNCTION monitorTransaction(tx_hash):
    // Poll blockchain for confirmation
    max_attempts = 30
    attempt = 0

    WHILE attempt < max_attempts:
        receipt = blockchain.getTransactionReceipt(tx_hash)

        IF receipt AND receipt.status == 'confirmed':
            // Update settlements
            UPDATE settlements
                WHERE transaction_hash == tx_hash
                SET status = 'confirmed',
                    confirmed_at = NOW,
                    block_number = receipt.blockNumber

            NOTIFY merchant "Settlement confirmed on blockchain"
            RETURN

        IF receipt AND receipt.status == 'failed':
            UPDATE settlements
                WHERE transaction_hash == tx_hash
                SET status = 'failed'

            NOTIFY merchant "Settlement failed. Please contact support."
            RETURN

        WAIT 10 seconds
        attempt += 1

    // Timeout
    LOG error "Transaction monitoring timeout"
```

---

### **FL-3: Affiliate Payment Splitting**

```
FUNCTION processAffiliatePayment(order):
    // This is an affiliate order
    affiliator = order.affiliator
    co_seller = order.co_seller

    // Get affiliator wallets
    affiliator_wallets = GET wallets
        WHERE merchant_id == affiliator.id
        AND network == order.payment.network

    // Get co-seller wallets
    coseller_wallets = GET wallets
        WHERE merchant_id == co_seller.id
        AND network == order.payment.network

    // Calculate amounts
    total_amount = order.payment.amount
    commission = (total_amount * order.commission_rate) / 100
    affiliator_amount = total_amount - commission

    // Prepare splits
    splits = []

    // Affiliator's share
    IF affiliator_wallets.length > 0:
        FOR each wallet IN affiliator_wallets:
            split = (affiliator_amount * wallet.percentage) / 100
            splits.push({
                wallet: wallet,
                amount: split,
                type: 'affiliator'
            })
    ELSE:
        // Fallback to credit
        processFallbackCredit(affiliator, {
            amount: affiliator_amount,
            token: order.payment.token
        })

    // Co-seller's commission
    IF coseller_wallets.length > 0:
        FOR each wallet IN coseller_wallets:
            split = (commission * wallet.percentage) / 100
            splits.push({
                wallet: wallet,
                amount: split,
                type: 'co_seller'
            })
    ELSE:
        processFallbackCredit(co_seller, {
            amount: commission,
            token: order.payment.token
        })

    // Execute if there are splits
    IF splits.length > 0:
        transaction = smart_contract.splitPayment({
            recipient_splits: splits,
            platform_fee: (total_amount * PLATFORM_FEE_PERCENT) / 100,
            order_id: order.id
        })

        monitorTransaction(transaction.hash)

        RETURN transaction
```

---

### B3) Behavioral Flow

```
[Merchant Navigates to Settings]
    ↓
[Add Wallet]
    ↓
[Select Network]
    ↓
[Connect Wallet]
    ├─ MetaMask
    ├─ WalletConnect
    └─ Coinbase Wallet
    ↓
[Sign Verification]
    ↓
[Set Percentage]
    ↓
[Validate Total = 100%]
    ├─ Yes → [Save Configuration]
    └─ No → [Show Error]
            ↓
        [Adjust Percentages]

[Customer Pays]
    ↓
[Check Merchant Wallets]
    ├─ Wallets Configured → [Split Payment]
    │                           ↓
    │                       [Execute Smart Contract]
    │                           ↓
    │                       [Monitor Confirmation]
    │                           ↓
    │                       [Update Settlement Status]
    │
    └─ No Wallets → [Fallback to Credit]
                        ↓
                    [Create Credit Record]
                        ↓
                    [Notify Merchant]
```

---

### B4) State Transitions

| Current State | Event | New State | Side Effects |
| --- | --- | --- | --- |
| No Wallets | Wallet added | Configured | Wallet stored |
| Configured | Payment received | Processing | Smart contract called |
| Processing | Transaction confirmed | Settled | Funds in wallet |
| Processing | Transaction failed | Failed | Retry or fallback |
| Configured | Wallet removed | Partial Config | Percentages need update |
| Fallback | Wallet added | Configured | Future payments split |

---

### B5) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** Wallet address invalid | Reject during configuration |
| **EC-2:** Signature verification fails | Require re-signing |
| **EC-3:** Percentages don't sum to 100 | Block save, show error |
| **EC-4:** Transaction fails on blockchain | Retry once, then fallback |
| **EC-5:** Network congestion | Extend monitoring timeout |
| **EC-6:** Merchant removes all wallets | Fallback to credit for all future |
| **EC-7:** Gas estimation fails | Use fixed gas limit |
| **EC-8:** Affiliate order, co-seller has no wallet | Co-seller gets credit |

---

### B6) Data Requirements

**Wallet Configuration:**

| Field | Type | Description |
| --- | --- | --- |
| id | UUID | Wallet identifier |
| merchant_id | UUID | Reference to merchant |
| address | String | Wallet address |
| network | Enum | Polygon, Ethereum, BSC, Arbitrum, Optimism |
| token_type | String | 'all' or specific token symbol |
| percentage | Integer | 1-100 |
| verified | Boolean | Signature verified |
| created_at | DateTime | Creation timestamp |

**Settlement Records:**

| Field | Type | Description |
| --- | --- | --- |
| id | UUID | Settlement identifier |
| order_id | UUID | Reference to order |
| wallet_id | UUID | Reference to wallet |
| amount | Decimal | Settlement amount |
| token | String | Token symbol |
| transaction_hash | String | Blockchain transaction hash |
| status | Enum | pending, confirmed, failed |
| created_at | DateTime | When initiated |
| confirmed_at | DateTime | When confirmed on chain |
| block_number | Integer | Confirmation block |

---

### B7) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | Connect MetaMask wallet | Wallet connected and verified |
| TC-2 | Set percentage split (70/30) | Split saved successfully |
| TC-3 | Payment received | Split according to percentages |
| TC-4 | No wallet configured | Credit created automatically |
| TC-5 | View settlement history | All transactions listed with hashes |
| TC-6 | Remove wallet | Wallet removed, notification sent |
| TC-7 | Affiliate order payment | Both parties receive split |
| TC-8 | Transaction confirmed | Status updated to confirmed |
| TC-9 | Transaction fails | Retry initiated or fallback |
| TC-10 | Update percentage | New split applied to future payments |