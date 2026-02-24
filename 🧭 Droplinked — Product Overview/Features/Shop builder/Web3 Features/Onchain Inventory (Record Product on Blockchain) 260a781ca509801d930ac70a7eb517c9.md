# Onchain Inventory (Record Product on Blockchain)

# Onchain Inventory (Record Product on Blockchain)

### Feature ID:

**[IAA-WEB3-001]**

### Title:

**Onchain Inventory – Record Products as NFTs**

### Category:

**Web3 / Product Management** | **Actors**: Merchant | **Channel**: Shop Builder Dashboard

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**

Merchants want to leverage blockchain technology to create verifiable, traceable digital records of their products. Recording products as NFTs provides authenticity verification, ownership tracking, and transparency. The **Onchain Inventory** feature provides a dedicated space for merchants to record their products on various blockchain networks.

**Desired Outcome:**

- Dedicated "Onchain Inventory" page accessible from sidebar.
- Merchants can record any product as an NFT on supported blockchain networks.
- Wallet management based on network type (non-custodial vs external wallets).
- Once recorded on a network with a specific wallet, that wallet is locked for future recordings on that network.
- Real-time gas fee estimation before recording.
- Progress tracking during the recording process.
- List view of all recorded products with filtering capabilities.
- Recorded products become read-only (cannot be edited).
- NFT verification links visible on Storefront product pages.

---

### 2) **Scope – In / Out**

**In Scope:**

- **Onchain Inventory Page**
    - Accessible from sidebar navigation
    - Empty state for first-time users
    - List view of all recorded products
    - Search and filter functionality
- **New Record Flow**
    - Network selection (Polygon, Base, Binance, Bitlayer, SKALE, Redbelly, Ethereum, Xion)
    - Wallet handling based on network type
    - Product selection via Browse Inventory modal
    - Gas fee estimation
    - Recording process with progress modal
- **Wallet Management**
    - Non-custodial networks: Default wallet address (read-only)
    - External wallet networks: Connect Metamask/Phantom
    - Wallet locking per network after first recording
- **Product Restrictions**
    - Recorded products cannot be edited
    - NFT links displayed on Storefront

**Out of Scope:**

- NFT transfer to customer after purchase (separate feature)
- NFT resale or secondary marketplace
- Automatic minting
- Royalty management (removed from this version)
- Multi-chain deployment per product

---

### 3) **Key User Journeys**

---

**Journey 1: First Visit – Empty State**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Clicks "Onchain Inventory" in sidebar | Page loads |
| 2 | System | No recorded products exist | Empty state displayed |
| 3 | System | Shows message | "No products recorded yet" |
| 4 | Merchant | Sees "New Record" button | Ready to record first product |

---

**Journey 2: Record New Product**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Clicks "New Record" | New record page opens |
| 2 | Merchant | Selects network from dropdown | Network selected |
| 3 | System | Shows wallet based on network type | See wallet logic below |
| 4 | Merchant | Clicks "Browse Inventory" | Product selection modal opens |
| 5 | Merchant | Searches/filters and selects product | Product info displayed |
| 6 | System | Shows product details | Title, variants, quantity, image, price |
| 7 | System | Calculates and shows gas fee | Gas Fee, Network, USD equivalent |
| 8 | Merchant | Clicks "Record Onchain" | Progress modal opens |
| 9 | System | Processes recording with status updates | Step-by-step progress shown |
| 10 | System | Recording completes | Success or Failed status |
| 11 | System | Redirects to list | New record visible in table |

---

**Journey 3: Wallet Handling – Non-Custodial Network**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Selects non-custodial network | Wallet section updates |
| 2 | System | Shows default wallet address | Address is read-only |
| 3 | System | Provides copy button | Merchant can copy address |
| 4 | Merchant | Cannot change wallet | Proceeds with default wallet |

---

**Journey 4: Wallet Handling – External Wallet Required**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Selects network requiring external wallet | Wallet section updates |
| 2a | System | Wallet already connected | Shows wallet address, can change |
| 2b | System | Wallet not connected | Shows "Connect Wallet" button |
| 3 | Merchant | Clicks "Connect Wallet" | Wallet connection flow (Metamask/Phantom) |
| 4 | System | Wallet connected | Address displayed |

---

**Journey 5: Wallet Lock Enforcement**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Has previously recorded on Network X with Wallet A | Wallet A is locked for Network X |
| 2 | Merchant | Tries to record new product on Network X | System checks wallet |
| 3a | System | Same wallet (Wallet A) connected | Allowed to proceed |
| 3b | System | Different wallet (Wallet B) connected | Error shown |
| 4 | System | Error message | "You must use wallet [0x...] for this network" |

---

**Journey 6: View Recorded Products List**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Opens Onchain Inventory | List page loads |
| 2 | System | Displays table | All recorded products shown |
| 3 | Merchant | Uses search box | Filters by product title |
| 4 | Merchant | Selects wallet filter | Filters by wallet address |
| 5 | Merchant | Selects network filter | Filters by blockchain network |
| 6 | Merchant | Views product record | Sees product, network, wallet info |

---

**Journey 7: Customer Views NFT Verification**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Opens product page on Storefront | PDP loads |
| 2 | System | Product is recorded on blockchain | Verification section shown |
| 3 | Customer | Clicks verification link | Opens blockchain explorer in new tab |
| 4 | Customer | Verifies product on ledger | Sees NFT details |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria |
| --- | --- |
| **Page Access** |  |
| BAC-1 | Onchain Inventory is accessible from sidebar navigation |
| BAC-2 | Empty state is shown when no products are recorded |
| BAC-3 | "New Record" button is visible and functional |
| **Network & Wallet** |  |
| BAC-4 | Supported networks: Polygon, Base, Binance, Bitlayer, SKALE, Redbelly, Ethereum, Xion |
| BAC-5 | Non-custodial networks show default wallet address (read-only, copyable) |
| BAC-6 | External wallet networks show connect button if not connected |
| BAC-7 | Connected external wallets display address and allow changing |
| BAC-8 | After first recording on a network, wallet is locked for that network |
| BAC-9 | Error shown if merchant tries to use different wallet on same network |
| **Product Selection** |  |
| BAC-10 | Browse Inventory modal shows all products |
| BAC-11 | Modal supports search by product title |
| BAC-12 | Modal supports filter by collection |
| BAC-13 | Selected product displays: Title, Variants count, Quantity, Image, Price |
| **Gas & Recording** |  |
| BAC-14 | Gas fee is calculated based on selected product and network |
| BAC-15 | Gas fee shows: Amount, Network currency, USD equivalent |
| BAC-16 | "Record Onchain" button initiates recording process |
| BAC-17 | Progress modal shows step-by-step status during recording |
| BAC-18 | Success or Failed status is clearly displayed |
| **List View** |  |
| BAC-19 | Table displays: Product (title + image), Network, Wallet |
| BAC-20 | Search filters by product title |
| BAC-21 | Dropdown filters by wallet address |
| BAC-22 | Dropdown filters by network |
| **Product Restrictions** |  |
| BAC-23 | Recorded products CANNOT be edited |
| BAC-24 | NFT verification link is shown on Storefront product page |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-01 | Behdad | Complete rewrite - Separate page, new wallet logic | Feature Redesign |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **Onchain Inventory** | Page listing all products recorded on blockchain |
| **Record** | Process of minting an NFT for a product |
| **Non-Custodial Network** | Networks where Droplinked manages wallet automatically |
| **External Wallet** | Wallets like Metamask or Phantom that user connects |
| **Wallet Lock** | After first recording on a network, that wallet must be used for all future recordings on that network |
| **Gas Fee** | Blockchain transaction fee required for recording |

---

### B2) Network Configuration

| Network | Wallet Type | Wallet Provider |
| --- | --- | --- |
| Polygon | External | Metamask |
| Base | External | Metamask |
| Binance | External | Metamask |
| Bitlayer | External | Metamask |
| SKALE | Non-Custodial | System Managed |
| Redbelly | Non-Custodial | System Managed |
| Ethereum | External | Metamask |
| Xion | Non-Custodial | System Managed |

---

### B3) Exhaustive Functional Logic

---

### **FL-1: Page Load Logic**

```
ON NavigateTo('/onchain-inventory'):
    records = API.getOnchainRecords(shop_id)

    IF records.length == 0:
        RENDER EmptyState({
            message: "No products recorded yet",
            description: "Record your first product on the blockchain",
            button: "New Record"
        })
    ELSE:
        RENDER RecordsListPage(records)

```

---

### **FL-2: Network Selection Logic**

```
networks = [
    { id: 'polygon', name: 'Polygon', walletType: 'external', provider: 'metamask' },
    { id: 'base', name: 'Base', walletType: 'external', provider: 'metamask' },
    { id: 'binance', name: 'Binance', walletType: 'external', provider: 'metamask' },
    { id: 'bitlayer', name: 'Bitlayer', walletType: 'external', provider: 'metamask' },
    { id: 'skale', name: 'SKALE', walletType: 'non-custodial', provider: 'system' },
    { id: 'redbelly', name: 'Redbelly', walletType: 'non-custodial', provider: 'system' },
    { id: 'ethereum', name: 'Ethereum', walletType: 'external', provider: 'metamask' },
    { id: 'xion', name: 'Xion', walletType: 'non-custodial', provider: 'system' }
]

ON NetworkSelect(networkId):
    selectedNetwork = networks.find(n => n.id == networkId)

    IF selectedNetwork.walletType == 'non-custodial':
        // Get system-managed wallet for this shop
        wallet = API.getShopWallet(shop_id, networkId)
        RENDER WalletDisplay({
            address: wallet.address,
            readonly: true,
            copyable: true
        })
    ELSE:
        // Check for connected external wallet
        connectedWallet = WalletProvider.getConnectedWallet(selectedNetwork.provider)

        IF connectedWallet:
            RENDER WalletDisplay({
                address: connectedWallet.address,
                readonly: false,
                canChange: true
            })
        ELSE:
            RENDER ConnectWalletButton({
                provider: selectedNetwork.provider
            })

    // Check for wallet lock
    checkWalletLock(networkId)

```

---

### **FL-3: Wallet Lock Logic**

```
FUNCTION checkWalletLock(networkId):
    // Get first record on this network for this shop
    firstRecord = API.getFirstRecordOnNetwork(shop_id, networkId)

    IF firstRecord == null:
        // No previous records, any wallet allowed
        SET walletLocked = false
        RETURN

    lockedWallet = firstRecord.wallet_address
    SET walletLocked = true
    SET lockedWalletAddress = lockedWallet

ON AttemptRecord:
    IF walletLocked:
        currentWallet = getCurrentWalletAddress()

        IF currentWallet != lockedWalletAddress:
            SHOW error("You must use wallet " + truncate(lockedWalletAddress) + " for " + selectedNetwork.name)
            BLOCK recording
            RETURN false

    RETURN true

```

---

### **FL-4: Browse Inventory Modal**

```
ON ClickBrowseInventory:
    products = API.getProducts(shop_id, {
        status: ['public', 'unlisted', 'draft'],
        exclude_recorded: false  // Can record already-recorded products on different networks
    })

    OPEN Modal({
        title: "Select Product",
        searchPlaceholder: "Search products...",
        filters: [
            { type: 'collection', label: 'Collection' }
        ],
        items: products
    })

ON ProductSearch(query):
    filteredProducts = products.filter(p =>
        p.title.toLowerCase().includes(query.toLowerCase())
    )
    RENDER productList(filteredProducts)

ON CollectionFilter(collectionId):
    filteredProducts = products.filter(p =>
        p.collections.includes(collectionId)
    )
    RENDER productList(filteredProducts)

ON ProductSelect(productId):
    selectedProduct = products.find(p => p.id == productId)
    CLOSE modal

    // Display product info
    RENDER ProductDetails({
        title: selectedProduct.title,
        variantsCount: selectedProduct.variants.length,
        quantity: selectedProduct.total_quantity,
        image: selectedProduct.primary_image,
        price: selectedProduct.price
    })

    // Calculate gas fee
    calculateGasFee(selectedProduct, selectedNetwork)

```

---

### **FL-5: Gas Fee Calculation**

```
FUNCTION calculateGasFee(product, network):
    gasFee = API.estimateGasFee({
        network_id: network.id,
        product_id: product.id,
        variants_count: product.variants.length,
        quantity: product.total_quantity
    })

    RENDER GasFeePanel({
        gasFee: gasFee.amount,
        networkCurrency: gasFee.currency,
        usdEquivalent: gasFee.usd_equivalent
    })

    // Example display:
    // Gas Fee: 0.05 ETH
    // Network: Ethereum
    // USD Equivalent: $10.00

```

---

### **FL-6: Record Onchain Process**

```
ON ClickRecordOnchain:
    // Validate wallet lock
    IF NOT checkWalletLock(selectedNetwork.id):
        RETURN

    // Open progress modal
    OPEN ProgressModal({
        title: "Recording on Blockchain",
        steps: [
            { id: 'prepare', label: 'Preparing transaction...', status: 'pending' },
            { id: 'sign', label: 'Waiting for signature...', status: 'pending' },
            { id: 'broadcast', label: 'Broadcasting to network...', status: 'pending' },
            { id: 'confirm', label: 'Confirming transaction...', status: 'pending' },
            { id: 'finalize', label: 'Finalizing record...', status: 'pending' }
        ]
    })

    TRY:
        // Step 1: Prepare
        UPDATE step('prepare', 'in-progress')
        txData = API.prepareRecordTransaction(selectedProduct.id, selectedNetwork.id)
        UPDATE step('prepare', 'completed')

        // Step 2: Sign
        UPDATE step('sign', 'in-progress')
        IF selectedNetwork.walletType == 'external':
            signature = await WalletProvider.signTransaction(txData)
        ELSE:
            signature = await API.signWithCustodialWallet(txData)
        UPDATE step('sign', 'completed')

        // Step 3: Broadcast
        UPDATE step('broadcast', 'in-progress')
        txHash = await API.broadcastTransaction(signature)
        UPDATE step('broadcast', 'completed')

        // Step 4: Confirm
        UPDATE step('confirm', 'in-progress')
        confirmation = await API.waitForConfirmation(txHash)
        UPDATE step('confirm', 'completed')

        // Step 5: Finalize
        UPDATE step('finalize', 'in-progress')
        record = await API.finalizeRecord({
            product_id: selectedProduct.id,
            network_id: selectedNetwork.id,
            wallet_address: currentWalletAddress,
            tx_hash: txHash,
            nft_data: confirmation.nft_data
        })
        UPDATE step('finalize', 'completed')

        // Mark product as recorded
        API.markProductAsRecorded(selectedProduct.id)

        SHOW success("Product recorded successfully!")
        CLOSE modal
        NAVIGATE to '/onchain-inventory'

    CATCH error:
        UPDATE currentStep('failed')
        SHOW error(error.message)
        SHOW retryButton

```

---

### **FL-7: Records List Page**

```
RecordsListPage:
    ├── Header:
    │   ├── Title: "Onchain Inventory"
    │   └── Button: "New Record"
    │
    ├── Filters:
    │   ├── SearchInput: placeholder="Search..."
    │   ├── WalletDropdown: "All Wallets" + list of used wallets
    │   └── NetworkDropdown: "All Networks" + list of used networks
    │
    └── Table:
        ├── Column: Records (Product Title + Image)
        ├── Column: Network
        └── Column: Wallet (truncated address)

ON Search(query):
    filteredRecords = records.filter(r =>
        r.product.title.toLowerCase().includes(query.toLowerCase())
    )
    RENDER table(filteredRecords)

ON WalletFilter(walletAddress):
    IF walletAddress == 'all':
        filteredRecords = records
    ELSE:
        filteredRecords = records.filter(r => r.wallet_address == walletAddress)
    RENDER table(filteredRecords)

ON NetworkFilter(networkId):
    IF networkId == 'all':
        filteredRecords = records
    ELSE:
        filteredRecords = records.filter(r => r.network_id == networkId)
    RENDER table(filteredRecords)

```

---

### **FL-8: Product Edit Lock**

```
// In Product Edit Page
ON LoadProductEditPage(productId):
    product = API.getProduct(productId)

    IF product.is_recorded == true:
        SHOW readOnlyBanner("This product is recorded on blockchain and cannot be edited.")
        DISABLE all edit controls
        RENDER viewOnlyMode(product)
    ELSE:
        RENDER editMode(product)

```

---

### B4) Behavioral Flow

```
[Merchant Opens Onchain Inventory]
    ↓
[Check Records]
    ├─ None → [Empty State] → [New Record Button]
    └─ Has Records → [Display List with Filters]
        ↓
[Click New Record]
    ↓
[Select Network]
    ├─ Non-Custodial → [Show System Wallet (Read-only)]
    └─ External → [Check Connected]
                    ├─ Connected → [Show Wallet Address]
                    └─ Not Connected → [Connect Button]
                        ↓
[Check Wallet Lock]
    ├─ First Recording → [Any Wallet OK]
    └─ Previous Recording → [Must Use Same Wallet]
        ├─ Same Wallet → [Continue]
        └─ Different Wallet → [ERROR]
            ↓
[Browse Inventory] → [Select Product]
    ↓
[Show Product Details + Gas Fee]
    ↓
[Click Record Onchain]
    ↓
[Progress Modal]
    ├─ Prepare → Sign → Broadcast → Confirm → Finalize
    ├─ Success → [Return to List]
    └─ Failed → [Show Error, Retry Option]
        ↓
[Product Marked as Recorded]
    ↓
[Product Cannot Be Edited]

```

---

### B5) State Transitions

| Current State | Event | New State | Side Effects |
| --- | --- | --- | --- |
| Not Recorded | Start Recording | Pending | Progress modal shown |
| Pending | Recording Succeeds | Recorded | Product locked, NFT link available |
| Pending | Recording Fails | Not Recorded | Error shown, can retry |
| Recorded | (immutable) | Recorded | Cannot edit product |

---

### B6) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** Wrong wallet connected | Error: "You must use wallet [address] for this network" |
| **EC-2:** Wallet disconnected during recording | Pause, prompt to reconnect |
| **EC-3:** Network congestion (high gas) | Show warning, allow to proceed or cancel |
| **EC-4:** Transaction fails on chain | Mark as failed, show error, allow retry |
| **EC-5:** User closes browser during recording | On return, check tx status, update accordingly |
| **EC-6:** Product already recorded on same network | Allow (re-record/update) or block (based on business rule) |
| **EC-7:** Insufficient gas balance | Show error before starting |
| **EC-8:** Network unavailable | Show network status, disable that option |

---

### B7) Data Requirements

**Onchain Record Table:**

| Field | Type | Description |
| --- | --- | --- |
| id | UUID | Unique record ID |
| shop_id | UUID | Shop that owns the record |
| product_id | UUID | Recorded product |
| network_id | String | Blockchain network |
| wallet_address | String | Wallet used for recording |
| tx_hash | String | Transaction hash |
| nft_contract | String | NFT contract address |
| nft_token_id | String | Token ID on chain |
| status | Enum | pending, recorded, failed |
| recorded_at | DateTime | When recording completed |

**Shop Wallet Lock Table:**

| Field | Type | Description |
| --- | --- | --- |
| shop_id | UUID | Shop ID |
| network_id | String | Network ID |
| locked_wallet | String | Wallet address locked to this network |
| locked_at | DateTime | When first recording happened |

**API Endpoints:**

| Endpoint | Method | Purpose |
| --- | --- | --- |
| `/api/onchain/records` | GET | List all records for shop |
| `/api/onchain/records` | POST | Create new record |
| `/api/onchain/estimate-gas` | POST | Estimate gas fee |
| `/api/onchain/wallet-lock` | GET | Check wallet lock for network |
| `/api/shop/wallet/{network}` | GET | Get shop's wallet for network |

---

### B8) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | First visit, no records | Empty state shown |
| TC-2 | Select non-custodial network | System wallet shown, read-only |
| TC-3 | Select external wallet network, not connected | Connect button shown |
| TC-4 | Connect external wallet | Wallet address displayed |
| TC-5 | First recording on network | Any wallet allowed |
| TC-6 | Second recording, same wallet | Allowed |
| TC-7 | Second recording, different wallet | Error shown |
| TC-8 | Browse inventory modal | Products searchable and filterable |
| TC-9 | Select product | Details and gas fee shown |
| TC-10 | Complete recording | Progress steps complete, record created |
| TC-11 | Recording fails | Error shown, retry available |
| TC-12 | View recorded product in edit mode | Read-only, cannot edit |
| TC-13 | Filter records by wallet | Only matching records shown |
| TC-14 | Filter records by network | Only matching records shown |
| TC-15 | Customer views recorded product | Verification link visible |