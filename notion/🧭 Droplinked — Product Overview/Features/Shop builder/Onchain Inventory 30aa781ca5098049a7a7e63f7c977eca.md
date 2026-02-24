# Onchain Inventory

### Feature ID:

**[IAA-WEB3-010]**

### Title:

**Onchain Inventory - Record Products on Blockchain**

### Category:

**Shop Builder | Web3 Features** | **Actors**: Merchant, Customer, Blockchain | **Channel**: Web Dashboard, Shopfront, Blockchain

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
Merchants need the ability to record their products on the blockchain to provide transparency, provenance, and authenticity verification. This creates an immutable record of product creation, ownership, and transaction history.

**Desired Outcome:**

- Merchant can record individual products on blockchain
- Merchant can bulk record up to 50 products at once
- Supported networks: Polygon (primary), Ethereum, BSC
- Product becomes read-only after recording (no edits)
- NFT minted with product metadata (name, description, image URL)
- Blockchain verification badge appears on storefront PDP
- Customer can view on-chain ownership history

---

### 2) **Scope – In / Out**

**In Scope:**

- **Product Recording**
    - Record products as NFTs on blockchain
    - Support multiple networks (Polygon, Ethereum, BSC)
    - Product data immutability after recording
    - NFT minting with product metadata
    - Bulk recording for multiple products (up to 50)
- **Verification & Display**
    - Blockchain verification badge on storefront
    - Customer-facing verification page/link
    - Transaction history tracking on-chain
    - Ownership transfer recording
    - Payment distribution transparency
- **Wallet & Gas Management**
    - Gas fee estimation and payment
    - Wallet lock per network (security)
    - Export NFT to external wallet
- **Product State**
    - Read-only product state after recording
    - Recording status: Pending → Processing → Confirmed/Failed

**Out of Scope:**

- Fractional ownership of products
- Product trading between customers (secondary market)
- Dynamic/product NFTs (updatable metadata)
- Cross-chain product recording
- Decentralized storage for product images (IPFS optional)
- Physical product tokenization (digital only)
- Subscription/recurring product recording
- DAO governance for product decisions

---

### 3) **Key User Journeys**

---

**Journey 1: Recording a Product on Blockchain**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Opens Product Detail | Product page loads |
| 2 | Merchant | Clicks "Record on Blockchain" | Recording modal opens |
| 3 | Merchant | Selects Network (Polygon, Ethereum, etc.) | Network selected |
| 4 | System | Reviews Product Data to be Recorded | Data displayed for review |
| 5 | Merchant | Confirms and Pays Gas Fee | Transaction initiated |
| 6 | System | Smart Contract Creates NFT | NFT minting in progress |
| 7 | System | Product Becomes Read-Only (Immutable) | Product locked for edits |
| 8 | System | NFT Verification Link Available on Storefront | Verification enabled |

---

**Journey 2: Customer Verification**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Views Product on Storefront | Product page loads |
| 2 | Customer | Sees "Verified on Blockchain" Badge | Badge displayed |
| 3 | Customer | Clicks Verification Link | Verification page opens |
| 4 | System | Views Blockchain Record | Details shown: |

```
- Creator wallet address
- Creation timestamp
- Transaction history
- Ownership transfers
- Payment distributions |
```

| 5 | Customer | Verifies Authenticity via Etherscan | External link provided |

---

**Journey 3: Post-Sale On-Chain Transfer**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Purchases Recorded Product | Payment initiated |
| 2 | System | Payment Confirmed on Blockchain | Confirmation received |
| 3 | System | Smart Contract: |  |

```
- Transfers payment to merchant
- Records transaction metadata
- Updates ownership record
- Distributes affiliate commissions (if applicable) | All actions recorded |
```

| 4 | Customer | Can Export NFT to Personal Wallet | Export option provided |
| 5 | System | Full Transaction History Visible on Chain | History accessible |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria |
| --- | --- |
| **Product Recording** |  |
| BAC-1 | Merchant can record individual products on blockchain |
| BAC-2 | Merchant can bulk record up to 50 products at once |
| BAC-3 | Supported networks: Polygon (primary), Ethereum, BSC |
| BAC-4 | Gas fee displayed and confirmed before recording |
| BAC-5 | Product becomes read-only after recording (no edits) |
| BAC-6 | NFT minted with product metadata (name, description, image URL) |
| **Verification & Display** |  |
| BAC-7 | Blockchain verification badge appears on storefront PDP |
| BAC-8 | Verification link opens blockchain explorer (Etherscan/Polygonscan) |
| BAC-9 | Wallet locked to network after first recording (same wallet required) |
| BAC-10 | Transaction hash stored and displayed |
| BAC-11 | Recording history available in product audit log |
| **Ownership & History** |  |
| BAC-12 | Customer can view on-chain ownership history |
| BAC-13 | Payment splits recorded on blockchain for transparency |
| BAC-14 | Failed recordings can be retried (if gas issue) |
| BAC-15 | Recording status: Pending → Processing → Confirmed/Failed |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-16 | Behdad | Converted from PRD-010 to Feature format | Documentation structure |
| 2026-02-01 | Product Management | Initial PRD creation | New feature request |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **Onchain** | Data recorded on the blockchain |
| **NFT** | Non-Fungible Token - unique digital asset |
| **Minting** | Creating a new NFT on the blockchain |
| **Gas Fee** | Transaction fee paid to network validators |
| **Immutable** | Cannot be changed after creation |
| **Provenance** | Record of ownership history |

---

### B2) Exhaustive Functional Logic

---

### **FL-1: NFT Minting**

```
FUNCTION mintProductNFT(product, network, wallet):
    // Validate product
    IF product.blockchain_recorded:
        RETURN error("Product already recorded on blockchain")

    // Validate network
    IF network NOT IN SUPPORTED_NETWORKS:
        RETURN error("Unsupported network")

    // Estimate gas
    gas_estimate = smart_contract.estimateGas('mintProduct', {
        product_data: product,
        network: network
    })

    // Check wallet balance
    wallet_balance = blockchain.getBalance(wallet.address, network)
    IF wallet_balance < gas_estimate.total_cost:
        RETURN error("Insufficient funds for gas fee")

    // Prepare metadata
    metadata = {
        name: product.name,
        description: product.description,
        image: product.image_url,
        attributes: [
            { trait_type: 'SKU', value: product.sku },
            { trait_type: 'Category', value: product.category },
            { trait_type: 'Created', value: product.created_at },
            { trait_type: 'Price', value: product.price + ' ' + product.currency }
        ]
    }

    // Upload metadata to IPFS (optional)
    IF use_ipfs:
        metadata_uri = ipfs.upload(JSON.stringify(metadata))
    ELSE:
        metadata_uri = cdn.store(metadata)

    // Execute mint transaction
    transaction = smart_contract.mintProduct({
        to: wallet.address,
        metadata_uri: metadata_uri,
        product_id: product.id,
        network: network
    }, {
        from: wallet.address,
        gas: gas_estimate.gas_limit,
        gas_price: gas_estimate.gas_price
    })

    // Record pending status
    recording = CREATE BlockchainRecording({
        product_id: product.id,
        network: network,
        transaction_hash: transaction.hash,
        status: 'pending',
        wallet_address: wallet.address,
        metadata_uri: metadata_uri,
        gas_paid: gas_estimate.total_cost,
        created_at: NOW
    })

    // Monitor transaction
    monitorRecording(recording.id)

    RETURN {
        recording_id: recording.id,
        transaction_hash: transaction.hash,
        status: 'pending'
    }

FUNCTION bulkMintProducts(products, network, wallet):
    IF products.length > 50:
        RETURN error("Maximum 50 products per bulk recording")

    // Estimate batch gas
    gas_estimate = smart_contract.estimateGas('batchMintProducts', {
        count: products.length,
        network: network
    })

    // Check balance
    wallet_balance = blockchain.getBalance(wallet.address, network)
    IF wallet_balance < gas_estimate.total_cost:
        RETURN error("Insufficient funds for bulk gas fee")

    // Prepare all metadata
    metadata_uris = []
    FOR each product IN products:
        metadata = prepareMetadata(product)
        uri = ipfs.upload(JSON.stringify(metadata))
        metadata_uris.push(uri)

    // Execute batch mint
    transaction = smart_contract.batchMintProducts({
        to: wallet.address,
        metadata_uris: metadata_uris,
        product_ids: MAP products TO product.id
    })

    // Create recording records for all
    FOR each product, index IN products:
        CREATE BlockchainRecording({
            product_id: product.id,
            network: network,
            transaction_hash: transaction.hash,
            status: 'pending',
            batch_index: index,
            wallet_address: wallet.address,
            metadata_uri: metadata_uris[index],
            created_at: NOW
        })

    RETURN {
        transaction_hash: transaction.hash,
        products_count: products.length,
        status: 'pending'
    }
```

---

### **FL-2: Transaction Monitoring**

```
FUNCTION monitorRecording(recording_id):
    recording = GET blockchain_recording WHERE id == recording_id
    max_attempts = 60 // 10 minutes with 10-second intervals
    attempt = 0

    WHILE attempt < max_attempts:
        receipt = blockchain.getTransactionReceipt(
            recording.transaction_hash,
            recording.network
        )

        IF receipt:
            IF receipt.status == 'success':
                // Transaction confirmed
                recording.status = 'confirmed'
                recording.block_number = receipt.blockNumber
                recording.confirmed_at = NOW
                recording.nft_token_id = receipt.tokenId

                // Update product
                product = GET product WHERE id == recording.product_id
                product.blockchain_recorded = true
                product.blockchain_network = recording.network
                product.nft_contract_address = receipt.contractAddress
                product.nft_token_id = receipt.tokenId
                product.is_editable = false // Lock product

                SAVE recording
                SAVE product

                NOTIFY merchant "Product recorded on blockchain successfully"
                RETURN

            ELSE IF receipt.status == 'failed':
                // Transaction failed
                recording.status = 'failed'
                recording.error = receipt.error OR 'Transaction failed'
                recording.failed_at = NOW

                SAVE recording

                NOTIFY merchant "Recording failed. You can retry."
                RETURN

        WAIT 10 seconds
        attempt += 1

    // Timeout
    recording.status = 'timeout'
    recording.error = 'Confirmation timeout'
    SAVE recording

    LOG error "Recording monitoring timeout for ${recording.id}"

FUNCTION retryRecording(recording_id):
    recording = GET blockchain_recording WHERE id == recording_id

    IF recording.status NOT IN ['failed', 'timeout']:
        RETURN error("Can only retry failed or timed out recordings")

    product = GET product WHERE id == recording.product_id

    // Create new recording attempt
    RETURN mintProductNFT(product, recording.network, recording.wallet_address)
```

---

### **FL-3: Verification & Display**

```
FUNCTION getVerificationData(product_id):
    product = GET product WHERE id == product_id

    IF NOT product.blockchain_recorded:
        RETURN null

    recording = GET blockchain_recording
        WHERE product_id == product_id
        AND status == 'confirmed'
        ORDER BY confirmed_at DESC
        LIMIT 1

    // Get ownership history from blockchain
    transfers = blockchain.getTokenTransfers(
        contract_address: product.nft_contract_address,
        token_id: product.nft_token_id,
        network: product.blockchain_network
    )

    // Get transaction details
    creation_tx = blockchain.getTransaction(
        recording.transaction_hash,
        product.blockchain_network
    )

    RETURN {
        is_verified: true,
        network: product.blockchain_network,
        contract_address: product.nft_contract_address,
        token_id: product.nft_token_id,
        creator_address: recording.wallet_address,
        creation_timestamp: recording.confirmed_at,
        transaction_hash: recording.transaction_hash,
        explorer_url: getExplorerUrl(
            product.blockchain_network,
            recording.transaction_hash
        ),
        ownership_history: transfers,
        metadata: {
            name: product.name,
            description: product.description,
            image: product.image_url,
            created_at: product.created_at
        }
    }

FUNCTION getExplorerUrl(network, transaction_hash):
    explorers = {
        'polygon': '<https://polygonscan.com/tx/>',
        'ethereum': '<https://etherscan.io/tx/>',
        'bsc': '<https://bscscan.com/tx/>'
    }

    RETURN explorers[network] + transaction_hash

// Middleware to add verification data to product responses
FUNCTION injectVerificationData(product):
    IF product.blockchain_recorded:
        product.verification = getVerificationData(product.id)
        product.verification_badge = {
            show: true,
            text: 'Verified on Blockchain',
            network: product.blockchain_network
        }
    ELSE:
        product.verification_badge = {
            show: false
        }

    RETURN product
```

---

### **FL-4: Wallet Lock**

```
FUNCTION checkWalletLock(merchant_id, network):
    // Check if merchant has recorded products on this network
    recordings = GET blockchain_recordings
        WHERE merchant_id == merchant_id
        AND network == network
        AND status == 'confirmed'

    IF recordings.length > 0:
        // Return the locked wallet address
        RETURN {
            locked: true,
            wallet_address: recordings[0].wallet_address,
            message: 'Wallet locked to ' + recordings[0].wallet_address +
                     ' for ' + network + ' network'
        }

    RETURN { locked: false }

FUNCTION enforceWalletLock(merchant_id, network, requested_wallet):
    lock_status = checkWalletLock(merchant_id, network)

    IF lock_status.locked:
        IF requested_wallet.address != lock_status.wallet_address:
            RETURN error(
                "Wallet already locked to " + lock_status.wallet_address +
                " for " + network + ". Use the same wallet for all recordings."
            )

    RETURN { allowed: true }
```

---

### B3) Behavioral Flow

```
[Merchant Opens Product]
    ↓
[Clicks "Record on Blockchain"]
    ↓
[Check Wallet Lock]
    ├─ Locked → [Verify Same Wallet]
    │             ↓
    │         [Proceed]
    │
    └─ Not Locked → [Select Network]
                    ↓
                [Estimate Gas]
                    ↓
                [Confirm Payment]
                    ↓
                [Execute Mint Transaction]
                    ↓
                [Monitor Confirmation]
                    ├─ Confirmed → [Update Product]
                    │                 ↓
                    │             [Lock Product for Editing]
                    │                 ↓
                    │             [Show Verification Badge]
                    │
                    └─ Failed → [Show Error]
                                  ↓
                              [Allow Retry]
```

---

### B4) State Transitions

| Current State | Event | New State | Side Effects |
| --- | --- | --- | --- |
| Unrecorded | Record initiated | Pending | Transaction submitted |
| Pending | Confirmed on chain | Confirmed | Product locked, badge shown |
| Pending | Failed | Failed | Error logged, retry available |
| Pending | Timeout | Timeout | Retry available |
| Failed | Retry clicked | Pending | New transaction |
| Confirmed | Edit attempted | Blocked | Edit denied, show reason |
| Confirmed | Sale occurs | Transferred | Ownership updated on chain |

---

### B5) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** Gas price spikes during mint | Show updated estimate, require re-confirmation |
| **EC-2:** Wallet rejects transaction | Show error, allow retry |
| **EC-3:** Product deleted after recording | Keep blockchain record, mark as "delisted" |
| **EC-4:** Network congestion | Extend monitoring timeout |
| **EC-5:** Duplicate mint attempt | Reject, show existing token ID |
| **EC-6:** Metadata upload fails | Retry 3 times, then fail |
| **EC-7:** Wallet switches mid-recording | Block, enforce lock |
| **EC-8:** Token URI becomes unreachable | Keep record, show warning |

---

### B6) Data Requirements

**Blockchain Recording:**

| Field | Type | Description |
| --- | --- | --- |
| id | UUID | Recording identifier |
| product_id | UUID | Reference to product |
| network | Enum | Polygon, Ethereum, BSC |
| transaction_hash | String | Blockchain tx hash |
| status | Enum | pending, confirmed, failed, timeout |
| wallet_address | String | Creator wallet |
| metadata_uri | String | IPFS or CDN URI |
| nft_token_id | String | Token ID on chain |
| gas_paid | Decimal | Gas fee amount |
| block_number | Integer | Confirmation block |
| created_at | DateTime | When initiated |
| confirmed_at | DateTime | When confirmed |

**Product Blockchain Fields:**

| Field | Type | Description |
| --- | --- | --- |
| blockchain_recorded | Boolean | Whether recorded |
| blockchain_network | String | Network used |
| nft_contract_address | String | Contract address |
| nft_token_id | String | Token ID |
| is_editable | Boolean | Whether edits allowed |

---

### B7) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | Record single product | NFT minted, product locked |
| TC-2 | Bulk record 50 products | All products recorded |
| TC-3 | View verification badge | Badge displayed on PDP |
| TC-4 | Click verification link | Blockchain explorer opens |
| TC-5 | Check ownership history | Full history displayed |
| TC-6 | Failed recording | Retry option available |
| TC-7 | Edit locked product | Edit blocked with message |
| TC-8 | Different wallet attempt | Rejected with lock message |
| TC-9 | Polygon network | Recording on Polygon |
| TC-10 | Export NFT | Export functionality works |