# PDR-004: Product NFT Recording System

**Status:** Draft  
**Author:** Product Management  
**Date:** 2026-02-12  
**Version:** 1.0  
**Related:** PDR-001 (Wallet), PDR-003 (Affiliate), PDR-005 (Non-Custodial)

---

## 1. Executive Summary

The Product NFT Recording System enables merchants to tokenize their products on the blockchain, creating immutable certificates of authenticity and ownership. Each recorded product becomes an NFT with verifiable metadata, creating a transparent provenance trail from creation through sale.

### Key Decisions

1. **Hybrid architecture**: Product metadata stored on-chain (IPFS/Arweave) with business logic (price, commission) off-chain
2. **Lazy minting**: Gas fees paid only when product is actually sold
3. **Multi-network support**: Ethereum, Polygon, BSC with merchant choice
4. **Wallet lock per network**: Once wallet used for recording, it becomes the canonical wallet for that network
5. **Read-only after recording**: Product becomes immutable certificate but can be "re-recorded" as new version

---

## 2. Problem Statement

### Current Pain Points

1. **Product authenticity**: No way to prove product originality or ownership
2. **Provenance tracking**: Can't track product journey (creator → intermediaries → buyer)
3. **Counterfeit protection**: No mechanism to verify genuine products
4. **Edit vs immutability conflict**: Once recorded, product can't be edited - but what about typos?
5. **Affiliate + NFT conflict**: If product is recorded and affiliated, can commission change?
6. **Gas fee uncertainty**: Merchants don't know recording cost until transaction time
7. **Network selection**: Which blockchain to use? Ethereum (secure, expensive) vs Polygon (fast, cheap)?

### User Stories

**As a merchant**, I want to:

- Record my products as NFTs to prove authenticity
- Choose which blockchain network to use
- See estimated gas fees before recording
- Have my ownership verified and public
- Update product information if needed (corrections, improvements)
- Transfer NFT ownership if I sell my business

**As a customer**, I want to:

- Verify product authenticity on the blockchain
- See product provenance (who created it, when, transaction history)
- Trust that I'm buying genuine products
- Receive NFT certificate with my purchase

**As a platform**, we need to:

- Support multiple blockchain networks
- Handle gas fee estimation and payment
- Maintain product-NFT mapping
- Track ownership transfers
- Integrate with affiliate system

---

## 3. Scope

### In Scope ✅

- **Product Tokenization**: Convert products to NFTs
- **Multi-Network Support**: Ethereum, Polygon, BSC, Arbitrum, Base
- **Metadata Storage**: IPFS/Arweave for decentralized storage
- **Provenance Tracking**: Full ownership history
- **Lazy Minting**: Gasless listing until purchase
- **Wallet Lock**: Network-specific wallet binding
- **Verification Portal**: Public authenticity checker
- **Re-recording**: Version control for product updates

### Out of Scope ❌

- **Fractional ownership** (multiple owners per product) - Phase 2
- **Royalty enforcement** on secondary sales - Legal complexity
- **Cross-chain NFT bridging** - Technical complexity
- **Dynamic NFTs** (metadata updates automatically) - Phase 2
- **Physical product IoT integration** - Hardware requirements

---

## 4. Architecture & Design

### 4.1 System Overview

```
┌─────────────────────────────────────────────────────────────────┐
│              PRODUCT NFT RECORDING ARCHITECTURE                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  MERCHANT FLOW                                                  │
│  ┌──────────────┐     ┌──────────────┐     ┌──────────────┐   │
│  │   Select     │────►│  Configure   │────►│   Record     │   │
│  │   Product    │     │   Metadata   │     │   Product    │   │
│  └──────────────┘     └──────────────┘     └──────┬───────┘   │
│                                                   │            │
│                         ┌─────────────────────────┘            │
│                         ▼                                      │
│              ┌────────────────────┐                           │
│              │  Select Network    │                           │
│              │  - Ethereum        │                           │
│              │  - Polygon         │                           │
│              │  - BSC             │                           │
│              └────────┬───────────┘                           │
│                       │                                        │
│         ┌─────────────┼─────────────┐                         │
│         ▼             ▼             ▼                         │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐                    │
│  │ IPFS/    │  │  Smart   │  │  Wallet  │                    │
│  │ Arweave  │  │ Contract │  │   Lock   │                    │
│  │ Storage  │  │ Deploy   │  │ Per Net  │                    │
│  └──────────┘  └──────────┘  └──────────┘                    │
│                                                                 │
│  CUSTOMER VERIFICATION                                          │
│  ┌──────────────┐     ┌──────────────┐     ┌──────────────┐   │
│  │   Scan QR    │────►│  Blockchain  │────►│   Verify     │   │
│  │    Code      │     │   Lookup     │     │ Authenticity │   │
│  └──────────────┘     └──────────────┘     └──────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 4.2 Data Model

```typescript
// Product NFT Record
interface ProductNFTRecord {
  id: string;
  productId: string;
  merchantId: string;
  
  // Blockchain reference
  blockchain: {
    networkId: string;        // 'ethereum', 'polygon', etc.
    chainId: number;          // 1, 137, 56
    contractAddress: string;  // NFT contract
    tokenId: string;          // Token ID
    transactionHash: string;  // Minting transaction
    blockNumber: number;
    mintedAt: Date;
  };
  
  // Metadata
  metadata: {
    name: string;
    description: string;
    image: string;            // IPFS hash
    externalUrl: string;      // Product page URL
    attributes: NFTAttribute[];
    createdAt: Date;
    createdBy: string;        // Wallet address
  };
  
  // Storage
  storage: {
    type: 'ipfs' | 'arweave';
    metadataUri: string;      // ipfs://... or ar://...
    files: {
      images: string[];       // IPFS hashes
      documents?: string[];   // Certificates, manuals
    };
  };
  
  // Version control
  version: {
    versionNumber: number;    // 1, 2, 3...
    previousVersion?: string; // ID of previous NFT
    isLatestVersion: boolean;
    supersededAt?: Date;
  };
  
  // Status
  status: 'active' | 'transferred' | 'burned' | 'superseded';
  
  // Provenance
  provenance: ProvenanceEvent[];
}

// NFT Attribute (metadata)
interface NFTAttribute {
  traitType: string;
  value: string | number;
  displayType?: 'string' | 'number' | 'date' | 'boost_percentage';
}

// Provenance Event
interface ProvenanceEvent {
  eventType: 'minted' | 'listed' | 'sold' | 'transferred' | 'affiliated';
  fromAddress?: string;
  toAddress: string;
  transactionHash: string;
  timestamp: Date;
  price?: number;
  currency?: string;
  marketplace?: string;
  metadata?: {
    affiliateId?: string;     // If sold via affiliate
    platformFee?: number;
    royaltyAmount?: number;
  };
}

// Merchant Wallet Lock
interface MerchantWalletLock {
  merchantId: string;
  networkId: string;
  walletAddress: string;
  lockedAt: Date;
  lockReason: 'product_recording' | 'manual';
  
  // Products recorded with this wallet
  recordedProducts: string[]; // Product IDs
  
  // Can be unlocked if no active products
  unlockable: boolean;
  unlockConditions: {
    requiresBurnAll: boolean;
    requiresTransferAll: boolean;
    adminOverridePossible: boolean;
  };
}

// Recording Request
interface RecordingRequest {
  id: string;
  productId: string;
  merchantId: string;
  status: 'draft' | 'pending_payment' | 'uploading' | 'minting' | 'completed' | 'failed';
  
  // Configuration
  config: {
    networkId: string;
    storageType: 'ipfs' | 'arweave';
    metadata: ProductMetadata;
  };
  
  // Gas estimation
  gasEstimate: {
    estimatedGas: number;
    gasPrice: bigint;
    estimatedCost: number;    // In native token
    estimatedCostUsd: number;
    calculatedAt: Date;
  };
  
  // Progress tracking
  progress: {
    currentStep: number;
    totalSteps: number;
    currentStepName: string;
    startedAt: Date;
    estimatedCompletion: Date;
  };
  
  // Results
  result?: {
    tokenId: string;
    transactionHash: string;
    contractAddress: string;
    metadataUri: string;
    explorerUrl: string;
  };
  
  // Error handling
  error?: {
    code: string;
    message: string;
    retryable: boolean;
    failedAt: Date;
  };
}
```

### 4.3 Smart Contract Architecture

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/utils/Counters.sol";

contract DroplinkedProductNFT is ERC721, ERC721URIStorage, Ownable {
    using Counters for Counters.Counter;
    Counters.Counter private _tokenIds;
    
    // Platform wallet for fees
    address public platformWallet;
    uint256 public recordingFee = 0.01 ether;
    
    // Product data
    struct ProductData {
        string productId;           // Internal product ID
        address merchant;           // Original creator
        uint256 createdAt;
        bool isRecorded;
        string metadataURI;
    }
    
    mapping(uint256 => ProductData) public products;
    mapping(string => uint256) public productIdToToken;
    
    // Provenance tracking
    struct Provenance {
        address from;
        address to;
        uint256 timestamp;
        string eventType;
        uint256 price;
    }
    
    mapping(uint256 => Provenance[]) public provenance;
    
    // Events
    event ProductRecorded(
        uint256 indexed tokenId,
        string productId,
        address indexed merchant,
        string metadataURI
    );
    
    event ProvenanceUpdated(
        uint256 indexed tokenId,
        address indexed from,
        address indexed to,
        string eventType
    );
    
    constructor(address _platformWallet) ERC721("Droplinked Product", "DPLINK") {
        platformWallet = _platformWallet;
    }
    
    // Record a product as NFT
    function recordProduct(
        string memory productId,
        string memory metadataURI,
        address merchant
    ) public payable returns (uint256) {
        require(msg.value >= recordingFee, "Insufficient recording fee");
        require(bytes(productId).length > 0, "Invalid product ID");
        require(productIdToToken[productId] == 0, "Product already recorded");
        
        // Transfer fee to platform
        payable(platformWallet).transfer(recordingFee);
        
        // Refund excess
        if (msg.value > recordingFee) {
            payable(msg.sender).transfer(msg.value - recordingFee);
        }
        
        // Mint NFT
        _tokenIds.increment();
        uint256 newTokenId = _tokenIds.current();
        
        _safeMint(merchant, newTokenId);
        _setTokenURI(newTokenId, metadataURI);
        
        // Store product data
        products[newTokenId] = ProductData({
            productId: productId,
            merchant: merchant,
            createdAt: block.timestamp,
            isRecorded: true,
            metadataURI: metadataURI
        });
        
        productIdToToken[productId] = newTokenId;
        
        // Add provenance
        provenance[newTokenId].push(Provenance({
            from: address(0),
            to: merchant,
            timestamp: block.timestamp,
            eventType: "minted",
            price: 0
        }));
        
        emit ProductRecorded(newTokenId, productId, merchant, metadataURI);
        emit ProvenanceUpdated(newTokenId, address(0), merchant, "minted");
        
        return newTokenId;
    }
    
    // Update provenance on sale
    function updateProvenanceOnSale(
        uint256 tokenId,
        address from,
        address to,
        uint256 price
    ) external onlyOwner {
        require(_exists(tokenId), "Token does not exist");
        
        provenance[tokenId].push(Provenance({
            from: from,
            to: to,
            timestamp: block.timestamp,
            eventType: "sold",
            price: price
        }));
        
        emit ProvenanceUpdated(tokenId, from, to, "sold");
    }
    
    // Batch record products (gas optimization)
    function batchRecordProducts(
        string[] memory productIds,
        string[] memory metadataURIs,
        address merchant
    ) public payable returns (uint256[] memory) {
        require(
            productIds.length == metadataURIs.length,
            "Array length mismatch"
        );
        require(
            msg.value >= recordingFee * productIds.length,
            "Insufficient batch fee"
        );
        
        uint256[] memory tokenIds = new uint256[](productIds.length);
        
        for (uint256 i = 0; i < productIds.length; i++) {
            tokenIds[i] = recordProduct(productIds[i], metadataURIs[i], merchant);
        }
        
        return tokenIds;
    }
    
    // Update recording fee (platform only)
    function setRecordingFee(uint256 newFee) external onlyOwner {
        recordingFee = newFee;
    }
    
    // Get provenance history
    function getProvenance(uint256 tokenId) external view returns (Provenance[] memory) {
        require(_exists(tokenId), "Token does not exist");
        return provenance[tokenId];
    }
    
    // Check if product is recorded
    function isProductRecorded(string memory productId) external view returns (bool) {
        return productIdToToken[productId] != 0;
    }
    
    // Override required functions
    function _burn(uint256 tokenId) internal override(ERC721, ERC721URIStorage) {
        super._burn(tokenId);
    }
    
    function tokenURI(uint256 tokenId)
        public
        view
        override(ERC721, ERC721URIStorage)
        returns (string memory)
    {
        return super.tokenURI(tokenId);
    }
}
```

### 4.4 Recording Workflow

```
┌─────────────────────────────────────────────────────────────────┐
│              PRODUCT RECORDING WORKFLOW                         │
└─────────────────────────────────────────────────────────────────┘

Step 1: Merchant selects product
┌─────────────────────────────────────────────────────────────┐
│ Record Product as NFT                                        │
│                                                              │
│ Product: Premium Headphones XYZ                             │
│ Price: $149.99                                              │
│                                                              │
│ ⚠️ Once recorded:                                           │
│ • Product becomes immutable certificate                     │
│ • Cannot edit core details (name, images)                  │
│ • Can create new version if needed                         │
│                                                              │
│ [Start Recording] [Cancel]                                  │
└─────────────────────────────────────────────────────────────┘

Step 2: Configure metadata
┌─────────────────────────────────────────────────────────────┐
│ Configure NFT Metadata                                       │
│                                                              │
│ Product Name: Premium Headphones XYZ                        │
│ Description: High-quality wireless headphones...            │
│                                                              │
│ Attributes:                                                  │
│ • Brand: AudioTech                                          │
│ • Category: Electronics                                     │
│ • Warranty: 2 years                                         │
│ • SKU: ATH-XYZ-001                                          │
│                                                              │
│ Images uploading to IPFS...                                 │
│ [=====>    ] 50%                                            │
└─────────────────────────────────────────────────────────────┘

Step 3: Select network
┌─────────────────────────────────────────────────────────────┐
│ Select Blockchain Network                                    │
│                                                              │
│ ⚡ Recommended: Polygon                                      │
│    • Gas fee: ~$0.05                                        │
│    • Speed: ~2 seconds                                      │
│    • Eco-friendly                                           │
│    [Select]                                                 │
│                                                              │
│ 🔒 Ethereum (Mainnet)                                       │
│    • Gas fee: ~$15-50                                       │
│    • Speed: ~12 seconds                                     │
│    • Maximum security                                       │
│    [Select]                                                 │
│                                                              │
│ 🚀 BSC (Binance Smart Chain)                                │
│    • Gas fee: ~$0.20                                        │
│    • Speed: ~3 seconds                                      │
│    [Select]                                                 │
│                                                              │
│ ℹ️ Your wallet on this network will be locked for          │
│    recording products.                                      │
└─────────────────────────────────────────────────────────────┘

Step 4: Review and confirm
┌─────────────────────────────────────────────────────────────┐
│ Review Recording Details                                     │
│                                                              │
│ Network: Polygon                                            │
│ Wallet: 0x742d...9f2a                                       │
│                                                              │
│ Estimated Costs:                                            │
│ • Gas fee: 0.005 MATIC (~$0.05)                            │
│ • Platform fee: $1.00                                       │
│ • Total: ~$1.05                                             │
│                                                              │
│ ⚠️ This wallet will be locked for Polygon recordings       │
│                                                              │
│ [Edit] [Confirm & Record]                                   │
└─────────────────────────────────────────────────────────────┘

Step 5: Execute recording
┌─────────────────────────────────────────────────────────────┐
│ Recording in Progress...                                     │
│                                                              │
│ Step 1/4: Uploading metadata to IPFS ✅                     │
│ Step 2/4: Confirming wallet connection ✅                   │
│ Step 3/4: Minting NFT on Polygon ⏳                         │
│ Step 4/4: Recording provenance ⏳                           │
│                                                              │
│ Transaction: 0x8f3a...2b9c                                  │
│ [View on Polygonscan]                                       │
│                                                              │
│ Please keep this window open...                             │
└─────────────────────────────────────────────────────────────┘

Step 6: Completion
┌─────────────────────────────────────────────────────────────┐
│ ✅ Product Successfully Recorded!                            │
│                                                              │
│ Token ID: #12345                                            │
│ Contract: 0xabc...def                                       │
│ Network: Polygon                                            │
│                                                              │
│ View on:                                                    │
│ [OpenSea] [Polygonscan] [Droplinked Verify]                 │
│                                                              │
│ QR Code for verification:                                   │
│ [QR CODE]                                                   │
│                                                              │
│ Download Certificate: [PDF] [Image]                         │
└─────────────────────────────────────────────────────────────┘
```

---

## 5. Conflict Resolution

### Conflict 1: Product Edit After Recording

**Scenario**: Merchant records product. Discovers typo in description. Product is immutable.

**Resolution Options**:

**Option A: Re-record as New Version (Chosen)**
```
- Create new NFT with corrected metadata
- Mark old NFT as "superseded"
- Link old → new in provenance
- Both remain on blockchain (historical record)
- New version gets new token ID

Cost: Pay gas fees again for new recording
Tradeoff: Immutable history vs ability to correct
```

**Option B: Mutable Metadata (Rejected)**
```
- Use upgradeable contracts
- Allow metadata updates
- Breaks immutability promise
- Customers can't trust certificate
```

**Best Practice**: Follow OpenSea's approach - immutable with versioning

### Conflict 2: Affiliate Product Recording

**Scenario**: Product is being sold by 5 Co-sellers. Affiliator wants to record it.

**Resolution**:
```
ON record_request:
  activeImports = getActiveImports(product.id);
  
  IF activeImports.length > 0:
    show_warning(
      "This product is being sold by ${activeImports.length} Co-sellers. " +
      "Recording will not affect their ability to sell, but:"
    );
    
    considerations = [
      "Product becomes immutable (cannot edit)",
      "Co-sellers will see 'NFT Verified' badge",
      "Commission rates remain editable (off-chain)",
      "You remain responsible for fulfillment"
    ];
    
    // Allow recording but notify Co-sellers
    FOR coSeller IN activeImports:
      notify_co_seller(
        "Affiliator recorded product as NFT. " +
        "Your sales continue normally."
      );
```

**Key Decision**: Recording doesn't break affiliate relationships

### Conflict 3: Wallet Lock Conflicts

**Scenario**: Merchant uses Wallet A to record on Polygon. Later wants to use Wallet B for non-custodial payments.

**Resolution**:
```
// Wallet lock is PER NETWORK and PER FEATURE
interface WalletPermissions {
  networkId: string;
  walletAddress: string;
  
  // What this wallet is authorized for
  permissions: {
    recording: boolean;      // Locked after first recording
    receivingPayments: boolean;
    nonCustodialManagement: boolean;
  };
  
  // Can add more wallets for same network
  // Each with different permissions
}

// Example:
// Network: Polygon
// Wallet A: 0x123... (locked for recording, can receive payments)
// Wallet B: 0x456... (can receive payments, non-custodial management)

// Rules:
// - Only one wallet can be locked for recording per network
// - Multiple wallets can receive payments
// - Recording wallet automatically gets payment permission
// - Non-custodial is separate permission
```

### Conflict 4: Gas Fee Fluctuation

**Scenario**: Merchant approves $5 gas fee. Transaction takes 5 minutes. Gas prices spike to $50.

**Resolution**:
```
// Gas estimation with buffer
function estimateGasWithBuffer(network) {
  const baseEstimate = getCurrentGasEstimate(network);
  const buffer = 1.5; // 50% buffer
  
  return {
    estimated: baseEstimate,
    maxAllowed: baseEstimate * buffer,
    validFor: 2 minutes // Refresh if older
  };
}

// Transaction flow
IF gasPrice > estimatedMaxAtApproval:
  // Show warning
  show_modal(
    "Gas prices have increased",
    "Original estimate: $5.00",
    "Current cost: $12.00",
    actions: ["Continue anyway", "Wait for lower prices", "Cancel"]
  );

// Alternatively: Use gas price ceiling
IF gasPrice > merchantMaxGasPrice:
  queueTransactionForLater();
  notify_merchant("Transaction queued for lower gas prices");
```

---

## 6. Integration Points

### 6.1 Wallet Connection (PDR-001)

```javascript
// Wallet validation before recording
async function validateWalletForRecording(merchant, networkId) {
  const wallet = merchant.wallets[networkId];
  
  IF !wallet:
    return {
      valid: false,
      error: `No wallet connected for ${networkId}`,
      action: 'connect_wallet'
    };
  
  // Check if wallet already locked for recording
  const existingLock = await getWalletLock(merchant.id, networkId);
  IF existingLock && existingLock.walletAddress !== wallet.address:
    return {
      valid: false,
      error: `Network ${networkId} locked to wallet ${existingLock.walletAddress}`,
      action: 'use_locked_wallet'
    };
  
  // Verify wallet has sufficient balance for gas
  const balance = await getWalletBalance(wallet.address, networkId);
  const estimatedGas = await estimateRecordingGas(networkId);
  
  IF balance < estimatedGas:
    return {
      valid: false,
      error: `Insufficient balance. Need ${estimatedGas} ETH, have ${balance}`,
      action: 'add_funds'
    };
  
  return { valid: true };
}
```

### 6.2 Affiliate Network (PDR-003)

```javascript
// Display NFT badge on Co-seller's imported product
function enrichImportedProduct(importData) {
  const originalProduct = getProduct(importData.originalProductId);
  
  IF originalProduct.nftRecord:
    return {
      ...importData,
      nftVerified: true,
      nftDetails: {
        network: originalProduct.nftRecord.networkId,
        contractAddress: originalProduct.nftRecord.contractAddress,
        tokenId: originalProduct.nftRecord.tokenId,
        verificationUrl: `/verify/${originalProduct.nftRecord.tokenId}`
      },
      badges: [...importData.badges, 'NFT Verified']
    };
  
  return importData;
}
```

### 6.3 Non-Custodial Wallets (PDR-005)

```javascript
// Non-custodial wallet can also be recording wallet
// But recording locks it permanently for that network

function configureWallet(wallet, purpose) {
  IF purpose === 'recording':
    wallet.permissions = {
      recording: true,           // Locked
      receivingPayments: true,   // Auto-enabled
      nonCustodial: true,        // Optional but recommended
      withdrawable: false        // Never - platform doesn't hold funds
    };
    wallet.lockType = 'permanent';
    wallet.canBeChanged = false;
  
  IF purpose === 'payments_only':
    wallet.permissions = {
      recording: false,
      receivingPayments: true,
      nonCustodial: true,
      withdrawable: false
    };
    wallet.lockType = 'none';
    wallet.canBeChanged = true;
}
```

---

## 7. Best Practices from Industry

### From OpenSea:

- ✅ **Lazy minting**: Gasless until purchase
- ✅ **Collection standards**: Follow ERC-721/1155
- ✅ **Metadata standards**: Follow OpenSea metadata format
- ✅ **Verification**: Clear authenticity indicators

**Our Implementation**:

- Lazy minting for gas efficiency
- Full ERC-721 compliance
- Standard metadata with custom attributes
- Verification portal with QR codes

### From Nike/RNFTFKT:

- ✅ **Physical-digital connection**: NFT represents physical product
- ✅ **Authentication**: QR codes for verification
- ✅ **Limited editions**: Scarcity drives value
- ✅ **Community access**: NFT as membership

**Our Implementation**:

- Product-NFT binding
- QR verification system
- Optional limited editions
- Future: Community features

### From Origin Protocol:

- ✅ **Decentralized marketplaces**: P2P without platform
- ✅ **Escrow contracts**: Trustless transactions
- ✅ **Reputation systems**: On-chain reviews
- ✅ **Fractional ownership**: Shared ownership

**Our Implementation**:

- Platform-mediated with optional direct transfers
- Smart contract escrow for disputes
- On-chain provenance as reputation
- Phase 2: Fractional ownership

---

## 8. User Experience

### 8.1 Product Detail - NFT Section

```
┌─────────────────────────────────────────────────────────────┐
│  🔐 Authenticity Guaranteed                                  │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  This product is recorded on the blockchain                 │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  [NFT Badge Image]                                     │  │
│  │                                                        │  │
│  │  Token ID: #12345                                      │  │
│  │  Network: Polygon                                      │  │
│  │  Created: Jan 15, 2026                                │  │
│  │  Creator: 0x742d...9f2a                                │  │
│  │                                                        │  │
│  │  [Verify Authenticity]  [View on Blockchain]          │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                              │
│  What this means:                                           │
│  ✅ Product authenticity verified                           │
│  ✅ Ownership trail transparent                             │
│  ✅ Immutable product details                               │
│  ✅ Resale history tracked                                  │
│                                                              │
│  Scan to verify:                                            │
│  [QR CODE]                                                  │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### 8.2 Verification Portal

```
┌─────────────────────────────────────────────────────────────┐
│  🔍 NFT Verification                                         │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Enter Token ID or scan QR code                             │
│  [____________________]  [Verify]                           │
│                                                              │
│  ─────────────────────────────────────────────────────────  │
│                                                              │
│  Product: Premium Headphones XYZ                            │
│  Status: ✅ Authentic                                        │
│                                                              │
│  Blockchain Details:                                        │
│  • Network: Polygon                                         │
│  • Token ID: #12345                                         │
│  • Contract: 0xabc...def                                    │
│  • Created: Jan 15, 2026 by 0x742d...9f2a                   │
│                                                              │
│  Provenance:                                                │
│  ┌───────────────────────────────────────────────────────┐  │
│  │ Jan 15, 2026   Minted by Creator                      │  │
│  │ Feb 01, 2026   Sold to 0x123...456 for $149.99       │  │
│  │                via Droplinked                         │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                              │
│  [View Full History] [Report Issue]                         │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 9. Implementation Roadmap

### Phase 1: Core Recording (Weeks 1-4)

- [ ] Smart contract development
- [ ] IPFS integration
- [ ] Basic recording flow
- [ ] Polygon support

### Phase 2: Multi-Network (Weeks 5-8)

- [ ] Ethereum mainnet support
- [ ] BSC integration
- [ ] Network selection UX
- [ ] Gas estimation

### Phase 3: Provenance & Verification (Weeks 9-12)

- [ ] Provenance tracking
- [ ] Verification portal
- [ ] QR code generation
- [ ] Public API

### Phase 4: Advanced Features (Weeks 13-16)

- [ ] Batch recording
- [ ] Versioning system
- [ ] Certificate generation
- [ ] Affiliate integration

### Phase 5: Scale & Optimization (Weeks 17-20)

- [ ] Performance optimization
- [ ] Gas optimization
- [ ] Security audit
- [ ] Documentation

---

## 10. Success Metrics

1. **Recording Adoption**: >20% of products recorded
2. **Gas Efficiency**: <$.50 average recording cost (Polygon)
3. **Verification Usage**: >1000 verifications/month
4. **Success Rate**: >98% recording success rate
5. **Network Distribution**: 60% Polygon, 30% Ethereum, 10% BSC

---

## 11. Open Questions

1. Should we support soulbound NFTs (non-transferable) for authenticity only?
2. How do we handle product returns with NFT transfers?
3. Should we charge ongoing "storage fees" for IPFS pinning?
4. Do we need to support ERC-1155 (semi-fungible) for product variants?
5. How do we handle network migrations (e.g., Polygon 1.0 → 2.0)?

---

*Last Updated: 2026-02-12*  
*Next Review: After Phase 1 completion*
