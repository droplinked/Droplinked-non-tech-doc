# Noncustodial Wallet Management

### Feature ID:

**[IAA-WLT-011]**

### Title:

**Noncustodial Wallet Management**

### Category:

**Shop Builder | Web3 Features** | **Actors**: Merchant, Blockchain | **Channel**: Web Dashboard, Blockchain

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
Droplinked provides noncustodial wallets for merchants using Circle and Xion technologies. Merchants need a comprehensive interface to create, view, manage, and transact with these wallets. Without proper wallet management, merchants cannot effectively receive crypto payments, manage their Web3 assets, or participate in blockchain features like on-chain inventory recording.

**Desired Outcome:**

- Merchant can create up to 5 wallets per network
- Supported wallet types: Circle Programmable Wallets, Xion
- Supported networks: Polygon, Ethereum, BSC, Arbitrum, Optimism
- Dashboard shows all wallets with balances
- Transaction history shows: date, type, amount, status, hash
- Security warnings displayed for key operations

---

### 2) **Scope – In / Out**

**In Scope:**

- **Wallet Creation**
    - Create noncustodial wallets (Circle SDK, Xion)
    - Support multiple networks (Polygon, Ethereum, BSC, Arbitrum)
    - Wallet backup and recovery phrase display
    - Export private key (encrypted, password protected)
- **Wallet Dashboard**
    - View wallet balances (native token + ERC-20 tokens)
    - Display transaction history
    - Multi-wallet view dashboard
    - Wallet labeling and organization
    - Set default wallet per network
- **Transactions**
    - Deposit functionality (receive funds)
    - Withdraw functionality (send funds)
    - QR code generation for wallet addresses
    - Gas fee estimation
    - Transaction status tracking
- **Security**
    - Security warnings and best practices
    - Private key encryption: AES-256 with merchant password
    - Wallet verification via signature

**Out of Scope:**

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

---

### 3) **Key User Journeys**

---

**Journey 1: Wallet Creation**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Navigates to Web3 → Wallets | Wallet dashboard loads |
| 2 | Merchant | Clicks "Create New Wallet" | Creation modal opens |
| 3 | Merchant | Selects Wallet Type (Circle / Xion) | Type selected |
| 4 | Merchant | Selects Network (Polygon, Ethereum, etc.) | Network selected |
| 5 | Merchant | Reviews Wallet Details | Details displayed |
| 6 | Merchant | Confirms Creation | Wallet generated |
| 7 | System | Wallet Generated with Private Key (Encrypted) | Key encrypted |
| 8 | System | Backup Phrase Displayed (One-Time) | Phrase shown |
| 9 | System | Wallet Ready for Use | Wallet active |

---

**Journey 2: Wallet Dashboard**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Opens Wallet Dashboard | Dashboard loads |
| 2 | System | Views All Wallets Grouped by Network | Wallets grouped |
| 3 | System | Sees Balances in Native Token and USD | Balances displayed |
| 4 | System | Recent Transactions Listed | Transactions shown |
| 5 | Merchant | Can Filter by: Network, Token, Date | Filter applied |
| 6 | Merchant | Click Wallet for Details | Details shown |

---

**Journey 3: Deposit and Withdraw**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Selects Wallet | Wallet selected |
| 2 | Merchant | Chooses Action: Deposit or Withdraw | Options shown |
| 3 | Merchant (Deposit) | Copy Wallet Address QR Code | QR code generated |
| 4 | Merchant (Deposit) | Share Address for Payment | Address displayed |
| 5 | Merchant (Withdraw) | Enter Destination Address | Input field shown |
| 6 | Merchant (Withdraw) | Enter Amount | Amount entered |
| 7 | Merchant (Withdraw) | Confirm Transaction | Confirmation requested |
| 8 | Merchant (Withdraw) | Pay Gas Fee | Gas fee displayed |
| 9 | System | Transaction Submitted | Transaction sent |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria |
| --- | --- |
| **Wallet Creation** |  |
| BAC-1 | Merchant can create up to 5 wallets per network |
| BAC-2 | Supported wallet types: Circle Programmable Wallets, Xion |
| BAC-3 | Supported networks: Polygon, Ethereum, BSC, Arbitrum, Optimism |
| BAC-4 | Wallet creation generates unique address per network |
| BAC-5 | Private keys encrypted with merchant password |
| BAC-6 | Backup recovery phrase shown once at creation |
| **Dashboard & Management** |  |
| BAC-7 | Dashboard shows all wallets with balances |
| BAC-8 | Balances update in real-time or on refresh |
| BAC-9 | Transaction history shows: date, type, amount, status, hash |
| BAC-10 | Deposit shows QR code and copyable address |
| **Transactions** |  |
| BAC-11 | Withdraw requires: destination address, amount, gas confirmation |
| BAC-12 | Gas fee estimated before withdrawal confirmation |
| BAC-13 | Withdrawal transaction hash displayed after submission |
| BAC-14 | Wallets can be labeled (e.g., "Main", "Business", "Savings") |
| BAC-15 | Default wallet can be set per network |
| **Security** |  |
| BAC-16 | Security warnings displayed for key operations |
| BAC-17 | Wallet deletion requires confirmation and password |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-16 | Behdad | Converted from PRD-011 to Feature format | Documentation structure |
| 2026-02-01 | Product Management | Initial PRD creation | New feature request |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **Noncustodial Wallet** | Wallet where user controls private keys |
| **Recovery Phrase** | 12-24 word seed for wallet recovery |
| **Private Key** | Secret key for signing transactions |
| **ERC-20 Token** | Standard for fungible tokens on Ethereum |
| **Gas Fee** | Network transaction fee |
| **AES-256** | Encryption standard for securing private keys |

---

### B2) Exhaustive Functional Logic

---

### **FL-1: Wallet Creation**

```
FUNCTION createWallet(merchant_id, config):
    // Validate limits
    existing_count = COUNT wallets
        WHERE merchant_id == merchant_id
        AND network == config.network

    IF existing_count >= 5:
        RETURN error("Maximum 5 wallets per network reached")

    // Validate network
    IF config.network NOT IN SUPPORTED_NETWORKS:
        RETURN error("Unsupported network")

    // Create wallet based on type
    SWITCH config.wallet_type:
        CASE 'circle':
            wallet_data = circle_api.createWallet({
                network: config.network,
                type: 'merchant_wallet'
            })

        CASE 'xion':
            wallet_data = xion_sdk.createWallet({
                network: config.network
            })

    // Generate recovery phrase (BIP-39)
    recovery_phrase = bip39.generateMnemonic()

    // Derive private key from phrase
    seed = bip39.mnemonicToSeed(recovery_phrase)
    private_key = derivePrivateKey(seed, config.network)

    // Encrypt private key with merchant password
    encrypted_key = AES256.encrypt(private_key, config.password)

    // Create wallet record
    wallet = CREATE Wallet({
        merchant_id: merchant_id,
        type: config.wallet_type,
        network: config.network,
        address: wallet_data.address,
        encrypted_private_key: encrypted_key,
        public_key: wallet_data.public_key,
        recovery_phrase_shown: false,
        label: config.label OR 'Wallet ' + (existing_count + 1),
        is_default: existing_count == 0, // First wallet is default
        created_at: NOW
    })

    // Show recovery phrase (one-time)
    RETURN {
        wallet_id: wallet.id,
        address: wallet.address,
        recovery_phrase: recovery_phrase, // Only shown once
        warning: "Write down this recovery phrase. It will not be shown again!"
    }

ON RecoveryPhraseAcknowledged(wallet_id):
    wallet = GET wallet WHERE id == wallet_id
    wallet.recovery_phrase_shown = true
    wallet.recovery_phrase_acknowledged_at = NOW
    SAVE wallet

    // Clear from memory
    CLEAR recovery_phrase

FUNCTION exportPrivateKey(wallet_id, password):
    wallet = GET wallet WHERE id == wallet_id

    // Verify password
    IF NOT verifyMerchantPassword(wallet.merchant_id, password):
        RETURN error("Invalid password")

    // Decrypt private key
    TRY:
        private_key = AES256.decrypt(wallet.encrypted_private_key, password)
    CATCH:
        RETURN error("Decryption failed")

    RETURN {
        private_key: private_key,
        warning: "Never share your private key with anyone!"
    }
```

---

### **FL-2: Balance & Transaction Management**

```
FUNCTION getWalletBalances(wallet_id):
    wallet = GET wallet WHERE id == wallet_id

    // Get native token balance
    native_balance = blockchain.getBalance(
        wallet.address,
        wallet.network
    )

    // Get ERC-20 token balances
    token_balances = []
    FOR each token IN SUPPORTED_TOKENS[wallet.network]:
        balance = blockchain.getTokenBalance(
            wallet.address,
            token.contract_address,
            wallet.network
        )

        IF balance > 0:
            // Get USD value
            usd_value = getTokenPrice(token.symbol) * balance

            token_balances.push({
                token: token.symbol,
                balance: balance,
                decimals: token.decimals,
                usd_value: usd_value
            })

    // Calculate total USD value
    total_usd = native_balance.usd_value + SUM(token_balances.usd_value)

    RETURN {
        wallet_id: wallet_id,
        address: wallet.address,
        network: wallet.network,
        native_token: {
            symbol: getNativeSymbol(wallet.network),
            balance: native_balance.amount,
            usd_value: native_balance.usd_value
        },
        tokens: token_balances,
        total_usd_value: total_usd,
        last_updated: NOW
    }

FUNCTION getTransactionHistory(wallet_id, filters = {}):
    wallet = GET wallet WHERE id == wallet_id

    // Get transactions from blockchain
    transactions = blockchain.getTransactions(
        address: wallet.address,
        network: wallet.network,
        from_block: filters.from_block,
        to_block: filters.to_block
    )

    // Enrich with local data
    enriched = []
    FOR each tx IN transactions:
        local_tx = GET transaction WHERE blockchain_hash == tx.hash

        enriched.push({
            hash: tx.hash,
            type: tx.type, // 'incoming', 'outgoing'
            amount: tx.value,
            token: tx.token_symbol,
            from: tx.from_address,
            to: tx.to_address,
            status: tx.status,
            gas_used: tx.gas_used,
            gas_price: tx.gas_price,
            timestamp: tx.timestamp,
            block_number: tx.block_number,
            explorer_url: getExplorerUrl(wallet.network, tx.hash),
            label: local_tx.label OR null,
            notes: local_tx.notes OR null
        })

    // Apply filters
    IF filters.token:
        enriched = FILTER enriched WHERE token == filters.token

    IF filters.type:
        enriched = FILTER enriched WHERE type == filters.type

    // Sort by date
    enriched = SORT enriched BY timestamp DESC

    RETURN enriched
```

---

### **FL-3: Withdrawal Flow**

```
FUNCTION initiateWithdrawal(wallet_id, params):
    wallet = GET wallet WHERE id == wallet_id

    // Validate parameters
    IF NOT isValidAddress(params.to_address, wallet.network):
        RETURN error("Invalid destination address")

    IF params.amount <= 0:
        RETURN error("Amount must be positive")

    // Get balance
    balance = getWalletBalances(wallet_id)

    // Check sufficient balance
    IF params.token == 'native':
        available = balance.native_token.balance
    ELSE:
        token_balance = FIND balance.tokens WHERE token == params.token
        available = token_balance.balance IF token_balance ELSE 0

    IF params.amount > available:
        RETURN error("Insufficient balance")

    // Estimate gas
    gas_estimate = blockchain.estimateGas({
        from: wallet.address,
        to: params.to_address,
        value: params.amount,
        token: params.token,
        network: wallet.network
    })

    // Check gas balance for native token
    IF balance.native_token.balance < gas_estimate.total_cost:
        RETURN error("Insufficient native token for gas fees")

    RETURN {
        status: 'ready',
        estimated_gas: gas_estimate,
        total_cost: params.amount + gas_estimate.total_cost,
        requires_confirmation: true
    }

FUNCTION confirmWithdrawal(wallet_id, params, password):
    wallet = GET wallet WHERE id == wallet_id

    // Verify merchant password
    IF NOT verifyMerchantPassword(wallet.merchant_id, password):
        RETURN error("Invalid password")

    // Decrypt private key
    private_key = AES256.decrypt(wallet.encrypted_private_key, password)

    // Prepare transaction
    transaction = {
        from: wallet.address,
        to: params.to_address,
        value: params.amount,
        gas_limit: params.gas_estimate.gas_limit,
        gas_price: params.gas_estimate.gas_price,
        nonce: blockchain.getNonce(wallet.address, wallet.network)
    }

    // Sign transaction
    signed_tx = signTransaction(transaction, private_key)

    // Broadcast to network
    tx_hash = blockchain.broadcastTransaction(signed_tx, wallet.network)

    // Record transaction
    CREATE Transaction({
        wallet_id: wallet_id,
        blockchain_hash: tx_hash,
        type: 'outgoing',
        amount: params.amount,
        token: params.token,
        to_address: params.to_address,
        gas_estimate: params.gas_estimate,
        status: 'pending',
        created_at: NOW
    })

    // Monitor transaction
    monitorTransaction(tx_hash, wallet.network)

    RETURN {
        transaction_hash: tx_hash,
        status: 'pending',
        explorer_url: getExplorerUrl(wallet.network, tx_hash)
    }

FUNCTION generateQRCode(wallet_id):
    wallet = GET wallet WHERE id == wallet_id

    // Generate EIP-681 compatible QR
    qr_data = {
        scheme: 'ethereum',
        target_address: wallet.address,
        chain_id: getChainId(wallet.network)
    }

    qr_code = generateQR(JSON.stringify(qr_data))

    RETURN {
        address: wallet.address,
        qr_code: qr_code,
        network: wallet.network,
        instructions: "Scan to deposit " + getNativeSymbol(wallet.network)
    }
```

---

### **FL-4: Wallet Management**

```
FUNCTION setDefaultWallet(wallet_id):
    wallet = GET wallet WHERE id == wallet_id

    // Remove default from other wallets on same network
    UPDATE wallets
        WHERE merchant_id == wallet.merchant_id
        AND network == wallet.network
        SET is_default = false

    // Set new default
    wallet.is_default = true
    SAVE wallet

    RETURN success

FUNCTION updateWalletLabel(wallet_id, new_label):
    wallet = GET wallet WHERE id == wallet_id
    wallet.label = new_label
    SAVE wallet

    RETURN success

FUNCTION deleteWallet(wallet_id, password):
    wallet = GET wallet WHERE id == wallet_id

    // Check if default
    IF wallet.is_default:
        RETURN error("Cannot delete default wallet. Set another wallet as default first.")

    // Check balance
    balance = getWalletBalances(wallet_id)
    IF balance.total_usd_value > 0:
        RETURN error("Wallet has balance. Transfer all funds before deleting.")

    // Verify password
    IF NOT verifyMerchantPassword(wallet.merchant_id, password):
        RETURN error("Invalid password")

    // Archive instead of delete for audit
    wallet.status = 'deleted'
    wallet.deleted_at = NOW
    SAVE wallet

    RETURN success
```

---

### B3) Behavioral Flow

```
[Merchant Opens Wallet Dashboard]
    ↓
[View All Wallets]
    ↓
[Choose Action]
    ├─ Create Wallet → [Select Type]
    │                     ↓
    │                 [Select Network]
    │                     ↓
    │                 [Generate Wallet]
    │                     ↓
    │                 [Show Recovery Phrase]
    │                     ↓
    │                 [Acknowledge Backup]
    │
    ├─ View Balance → [Fetch from Blockchain]
    │                   ↓
    │               [Display Native + Tokens]
    │                   ↓
    │               [Show USD Values]
    │
    ├─ Deposit → [Show QR Code]
    │             ↓
    │         [Display Address]
    │
    └─ Withdraw → [Enter Details]
                    ↓
                [Estimate Gas]
                    ↓
                [Confirm Password]
                    ↓
                [Sign Transaction]
                    ↓
                [Broadcast]
                    ↓
                [Monitor Confirmation]
```

---

### B4) State Transitions

| Current State | Event | New State | Side Effects |
| --- | --- | --- | --- |
| Non-existent | Create initiated | Creating | Wallet generation in progress |
| Creating | Phrase shown | Active | Recovery phrase acknowledged |
| Active | Deposit received | Active | Balance updated |
| Active | Withdrawal initiated | Pending | Transaction submitted |
| Pending | Confirmed | Active | Balance updated |
| Pending | Failed | Active | Error logged, funds returned |
| Active | Delete requested | Deleted | Wallet archived |
| Active | Set as default | Default | Other wallets unset |

---

### B5) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** Insufficient gas for withdrawal | Show error, require deposit |
| **EC-2:** Invalid destination address | Reject before signing |
| **EC-3:** Password incorrect | Allow 3 retries, then lock temporarily |
| **EC-4:** Network congestion | High gas estimate, allow override |
| **EC-5:** Transaction stuck pending | Provide speed up/cancel options |
| **EC-6:** Phrase lost | Display warning, cannot recover |
| **EC-7:** Delete with pending tx | Block until confirmed |
| **EC-8:** Wallet limit reached | Error message, suggest deletion |

---

### B6) Data Requirements

**Wallet Storage:**

| Field | Type | Description |
| --- | --- | --- |
| id | UUID | Wallet identifier |
| merchant_id | UUID | Reference to merchant |
| type | Enum | 'circle', 'xion' |
| network | Enum | Polygon, Ethereum, BSC, Arbitrum, Optimism |
| address | String | Public wallet address |
| encrypted_private_key | String | AES-256 encrypted key |
| public_key | String | Public key |
| label | String | Display name |
| is_default | Boolean | Default for network |
| status | Enum | active, deleted |
| created_at | DateTime | Creation timestamp |
| recovery_phrase_shown | Boolean | Whether phrase was acknowledged |

**Transactions:**

| Field | Type | Description |
| --- | --- | --- |
| id | UUID | Transaction identifier |
| wallet_id | UUID | Reference to wallet |
| blockchain_hash | String | Tx hash on chain |
| type | Enum | incoming, outgoing |
| amount | Decimal | Transaction amount |
| token | String | Token symbol |
| to_address | String | Destination |
| status | Enum | pending, confirmed, failed |
| gas_used | Decimal | Gas consumed |
| created_at | DateTime | When initiated |

---

### B7) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | Create Circle wallet | Wallet created with address |
| TC-2 | Create Xion wallet | Wallet created with address |
| TC-3 | View recovery phrase | Phrase displayed once |
| TC-4 | View balances | Native + ERC-20 balances shown |
| TC-5 | Generate QR code | QR code displayed |
| TC-6 | Withdraw funds | Transaction submitted and confirmed |
| TC-7 | View transaction history | All transactions listed |
| TC-8 | Set default wallet | Default set, others unset |
| TC-9 | Export private key | Key decrypted and shown |
| TC-10 | Delete wallet | Wallet archived, not accessible |
| TC-11 | Create 6th wallet | Error: max 5 per network |
| TC-12 | Withdraw with wrong password | Error: invalid password |