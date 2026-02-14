# PDR-003: Affiliate Network System

**Status:** Draft  
**Author:** Product Management  
**Date:** 2026-02-12  
**Version:** 1.0  
**Related:** PDR-001 (Wallet/Payment), PDR-002 (Currency), PDR-004 (NFT)

---

## 1. Executive Summary

The Affiliate Network System enables cross-merchant commerce where "Affiliators" (product owners) allow "Co-sellers" (affiliates) to import and sell their products for a commission. This PDR addresses the complex interactions between affiliate commissions, multi-currency transactions, crypto payment splits, and product lifecycle management.

### Key Decisions
1. **Permanent role assignment**: Users are either Affiliators or Co-sellers, not both
2. **Currency normalization**: All affiliate commissions calculated in Affiliator's base currency, converted to Co-seller's currency
3. **Smart contract splits**: Crypto payments automatically split between Affiliator and Co-seller via on-chain logic
4. **Product sync model**: Co-sellers import products, prices sync from Affiliator
5. **Commission lock-in**: Commission rate locked at checkout time, not at import time

---

## 2. Problem Statement

### Current Pain Points
1. **Unclear split mechanics**: Customer pays $100. Commission 15%. Who gets what? How?
2. **Currency conflicts**: Affiliator (USD), Co-seller (EUR), Customer pays in JPY
3. **Payment routing complexity**: Single transaction needs to reach two merchants
4. **Product lifecycle management**: Affiliator changes price, deletes product, changes commission
5. **Mixed cart scenarios**: Cart has regular + affiliate + POD products
6. **Commission calculation timing**: When is commission calculated? At import? Checkout? Payout?
7. **Fulfillment responsibility**: Who ships? Who handles customer service?

### User Stories

**As an Affiliator**, I want to:
- Offer my products to Co-sellers for resale
- Set commission rates (global or per-product)
- See which Co-sellers are selling my products
- Receive my share of sales immediately or via credit
- Maintain control over product information (price, description, images)
- Change commission rates with appropriate notice

**As a Co-seller**, I want to:
- Browse and import products from other merchants
- Earn commission on every sale
- Set my own markup on imported products (optional)
- Understand my earnings in my shop's currency
- Receive payments reliably and transparently

**As a customer**, I want to:
- Buy products from any shop with consistent experience
- Not know or care about affiliate relationships
- Receive products as expected

**As a platform**, I need to:
- Track all affiliate relationships and commissions
- Handle currency conversions accurately
- Split payments correctly (fiat and crypto)
- Maintain transparency and trust

---

## 3. Scope

### In Scope ✅
- **Role Management**: Affiliator vs Co-seller assignment
- **Product Import**: Co-sellers importing Affiliator products
- **Commission Engine**: Calculation and tracking
- **Payment Splitting**: Fiat (platform-mediated) and Crypto (smart contract)
- **Product Sync**: Price and inventory synchronization
- **Analytics**: Earnings tracking for both parties
- **Lifecycle Management**: Price changes, product deletion, commission updates

### Out of Scope ❌
- Multi-level affiliate (affiliate of affiliate) - Phase 2
- Referral tracking (separate feature)
- Automated product import (AI-driven) - Future
- Cross-platform affiliate (selling Amazon products) - Legal complexity
- Commission negotiation (bidding system) - Phase 2

---

## 4. Architecture & Design

### 4.1 System Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                 AFFILIATE NETWORK ARCHITECTURE                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ROLES                                                          │
│  ┌──────────────┐                    ┌──────────────┐          │
│  │  AFFILIATOR  │◄──────offers───────│   CO-SELLER  │          │
│  │  (Merchant)  │     products       │  (Affiliate) │          │
│  │              │                    │              │          │
│  │ • Owns       │─────imports───────►│ • Imports    │          │
│  │   products   │                    │   products   │          │
│  │ • Sets price │                    │ • Sets own   │          │
│  │ • Sets       │                    │   commission │          │
│  │   commission │◄────sells──────────│ • Markets    │          │
│  │ • Ships      │                    │   to their   │          │
│  │   product    │                    │   audience   │          │
│  └──────┬───────┘                    └──────┬───────┘          │
│         │                                   │                   │
│         └───────────┬───────────────────────┘                   │
│                     ▼                                           │
│          ┌────────────────────┐                                │
│          │     CUSTOMER       │                                │
│          │  • Buys from       │                                │
│          │    Co-seller shop  │                                │
│          │  • Affiliator      │                                │
│          │    ships product   │                                │
│          └────────────────────┘                                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 4.2 Data Model

```typescript
// Affiliate Relationship
interface AffiliateRelationship {
  id: string;
  affiliatorId: string;        // Product owner
  coSellerId: string;          // Affiliate
  status: 'active' | 'paused' | 'terminated';
  createdAt: Date;
  
  // Commission Configuration
  commission: {
    type: 'percentage' | 'fixed_amount';
    value: number;             // e.g., 15% or $10
    currency?: CurrencyCode;   // For fixed amount
    minCommission?: number;    // Minimum guaranteed
    maxCommission?: number;    // Commission cap
  };
  
  // Agreement terms
  terms: {
    paymentTerms: 'immediate' | 'net_15' | 'net_30';
    cookieDuration: number;    // Days (default: 30)
    exclusive: boolean;        // Only this Co-seller?
    territory?: string[];      // Geographic restrictions
  };
}

// Product Import (Co-seller's view of Affiliator product)
interface ProductImport {
  id: string;
  coSellerId: string;
  
  // Reference to original
  originalProductId: string;
  affiliatorId: string;
  
  // Sync fields (from Affiliator)
  syncedFields: {
    name: string;
    description: string;
    images: string[];
    basePrice: number;
    baseCurrency: CurrencyCode;
    inventory: number;
    lastSyncedAt: Date;
  };
  
  // Co-seller customizations
  coSellerSettings: {
    sellingPrice: number;      // Can add markup
    sellingCurrency: CurrencyCode;
    commissionRateOverride?: number; // If allowed by Affiliator
    customDescription?: string;
    customImages?: string[];
    categories: string[];
    tags: string[];
  };
  
  // Status
  status: 'active' | 'paused' | 'out_of_stock' | 'deleted_by_affiliator';
  importDate: Date;
}

// Affiliate Order (special order type)
interface AffiliateOrder {
  orderId: string;
  
  // Participants
  affiliatorId: string;
  coSellerId: string;
  customerId: string;
  
  // Product info
  productId: string;
  importId: string;
  
  // Financials
  financials: {
    orderTotal: number;
    orderCurrency: CurrencyCode;
    
    // Normalized to Affiliator's base currency
    orderInAffiliatorCurrency: number;
    affiliatorBaseCurrency: CurrencyCode;
    
    // Commission calculation
    commissionRate: number;
    commissionAmount: number;
    commissionCurrency: CurrencyCode; // Affiliator's base
    
    // After conversion to Co-seller's currency
    commissionInCoSellerCurrency: number;
    coSellerBaseCurrency: CurrencyCode;
    conversionRate: number;
    conversionRateLockedAt: Date;
    
    // Platform fee
    platformFee: number;
    platformFeeCurrency: CurrencyCode;
    
    // Final payouts
    affiliatorReceives: number;
    coSellerReceives: number;
  };
  
  // Payout tracking
  payout: {
    affiliatorPayoutStatus: 'pending' | 'paid' | 'failed';
    affiliatorPayoutAmount: number;
    affiliatorPayoutCurrency: CurrencyCode;
    affiliatorPayoutMethod: 'wallet' | 'credit' | 'stripe';
    affiliatorPayoutTxId?: string;
    
    coSellerPayoutStatus: 'pending' | 'paid' | 'failed';
    coSellerPayoutAmount: number;
    coSellerPayoutCurrency: CurrencyCode;
    coSellerPayoutMethod: 'wallet' | 'credit';
    coSellerPayoutTxId?: string;
  };
  
  // Fulfillment
  fulfillment: {
    fulfilledBy: 'affiliator';
    shippingAddress: Address;
    trackingNumber?: string;
    status: 'pending' | 'shipped' | 'delivered';
  };
  
  createdAt: Date;
  status: 'pending' | 'paid' | 'shipped' | 'delivered' | 'cancelled' | 'refunded';
}

// Commission Calculation at Checkout
interface CommissionCalculation {
  orderId: string;
  calculatedAt: Date;
  
  // Rate used (locked at this moment)
  commissionRate: number;
  rateSource: 'global' | 'product_specific' | 'negotiated';
  
  // Exchange rate info
  exchangeRate: {
    from: CurrencyCode;
    to: CurrencyCode;
    rate: number;
    provider: string;
    fetchedAt: Date;
  };
  
  // Calculated amounts
  breakdown: {
    grossOrderTotal: number;
    discountsApplied: number;
    netOrderTotal: number;
    platformFee: number;
    commissionAmount: number;
    affiliatorNet: number;
  };
}
```

### 4.3 Role Assignment Logic

```
┌─────────────────────────────────────────────────────────────┐
│              ROLE ASSIGNMENT RULES                          │
└─────────────────────────────────────────────────────────────┘

ON shop_creation:
  merchant.role = 'unassigned'

ON first_product_import:
  IF merchant.imports_other_products:
    merchant.role = 'co_seller'
    merchant.can_become_affiliator = false
    show_message("As a Co-seller, you import and sell other merchants' products")

ON activate_affiliate_network:
  IF merchant.role == 'unassigned':
    merchant.role = 'affiliator'
    merchant.can_become_co_seller = false
    show_message("As an Affiliator, other merchants can sell your products")
  
  IF merchant.role == 'co_seller':
    show_error("Co-sellers cannot become Affiliators. Create a new shop.")

RULES:
1. Role is permanent per shop (not per user)
2. User can have multiple shops with different roles
3. Co-sellers CAN import from multiple Affiliators
4. Affiliators CAN have multiple Co-sellers
5. No multi-level (Co-seller cannot have their own Co-sellers)
```

### 4.4 Commission Calculation Flow

```
┌─────────────────────────────────────────────────────────────────┐
│            COMMISSION CALCULATION ENGINE                        │
└─────────────────────────────────────────────────────────────────┘

Step 1: Order initiated
┌─────────────────────────────────────────────────────────────┐
│ Customer adds affiliate product to cart                     │
│ Co-seller shop: €85 EUR (display price)                    │
│                                                             │
│ Checkout initiated                                          │
└─────────────────────────────────────────────────────────────┘

Step 2: Lock commission rate
┌─────────────────────────────────────────────────────────────┐
│ Fetch current commission rate                               │
│ - Check product-specific rate first                        │
│ - Fall back to global affiliate rate                       │
│ - Rate locked for this order                               │
│                                                             │
│ Commission rate: 15%                                        │
│ Rate locked at: 2026-02-12T10:30:00Z                       │
│ Rate source: product_specific                             │
└─────────────────────────────────────────────────────────────┘

Step 3: Currency normalization
┌─────────────────────────────────────────────────────────────┐
│ Customer pays: €85 EUR                                     │
│ Co-seller base: EUR                                        │
│ Affiliator base: USD                                       │
│                                                             │
│ Convert to Affiliator currency:                            │
│ €85 EUR × 1.10 = $93.50 USD                                │
│ Exchange rate locked: 1 EUR = 1.10 USD                     │
└─────────────────────────────────────────────────────────────┘

Step 4: Calculate commission
┌─────────────────────────────────────────────────────────────┐
│ Order total (Affiliator currency): $93.50 USD              │
│ Commission rate: 15%                                        │
│                                                             │
│ Commission (Affiliator currency):                          │
│ $93.50 × 0.15 = $14.03 USD                                 │
│                                                             │
│ Platform fee (1%): $0.94 USD                              │
│                                                             │
│ Affiliator receives: $78.53 USD                            │
│   - After platform fee: $77.59 USD                        │
│                                                             │
│ Co-seller receives: $14.03 USD                             │
│   - Converted to EUR: €12.75 EUR                          │
└─────────────────────────────────────────────────────────────┘

Step 5: Payment routing
┌─────────────────────────────────────────────────────────────┐
│ FIAT PAYMENT:                                              │
│ Platform receives €85 EUR                                  │
│ Platform splits:                                           │
│   → Affiliator: $77.59 USD credit or payout               │
│   → Co-seller: €12.75 EUR credit or payout                │
│                                                             │
│ CRYPTO PAYMENT:                                            │
│ Customer sends USDC to Smart Contract                      │
│ Smart Contract splits:                                     │
│   → Affiliator wallet: $77.59 USDC                        │
│   → Co-seller wallet: €12.75 equivalent USDC              │
│   → Platform treasury: $0.94 USDC                         │
└─────────────────────────────────────────────────────────────┘
```

---

## 5. Payment Split Mechanisms

### 5.1 Fiat Payment Split (Platform-Mediated)

```
┌─────────────────────────────────────────────────────────────┐
│                 FIAT PAYMENT FLOW                           │
└─────────────────────────────────────────────────────────────┘

Customer pays $100 USD via Stripe
         │
         ▼
┌──────────────────────────────────────────────┐
│  STRIPE PLATFORM ACCOUNT                     │
│  Droplinked receives full $100               │
│  (Using Stripe Connect destination charges) │
└─────────────────┬────────────────────────────┘
                  │
    ┌─────────────┼─────────────┐
    ▼             ▼             ▼
┌────────┐  ┌──────────┐  ┌──────────┐
│Platform│  │Affiliator│  │Co-seller │
│  $1.00 │  │  $84.00  │  │  $15.00  │
│  (1%)  │  │  (84%)   │  │  (15%)   │
└────────┘  └──────────┘  └──────────┘

Implementation:
- Use Stripe Connect "Separate Charges and Transfers"
- Platform receives full amount
- Creates transfers to Affiliator and Co-seller
- Both must have Stripe Connect accounts
- Transfer grouped to reduce fees
```

### 5.2 Crypto Payment Split (Smart Contract)

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

contract AffiliatePaymentRouter {
    struct AffiliateOrder {
        address affiliator;
        address coSeller;
        uint256 affiliatorShare;
        uint256 coSellerShare;
        uint256 platformFee;
        bool paid;
    }
    
    mapping(bytes32 => AffiliateOrder) public orders;
    address public platformTreasury;
    uint256 public platformFeeBasisPoints; // 100 = 1%
    
    event AffiliatePaymentSplit(
        bytes32 indexed orderId,
        address indexed affiliator,
        address indexed coSeller,
        uint256 affiliatorAmount,
        uint256 coSellerAmount,
        uint256 platformFee
    );
    
    function processAffiliatePayment(
        bytes32 orderId,
        address affiliatorWallet,
        address coSellerWallet,
        uint256 totalAmount,
        uint256 commissionBasisPoints // e.g., 1500 for 15%
    ) external payable {
        require(msg.value == totalAmount, "Incorrect payment amount");
        require(!orders[orderId].paid, "Order already paid");
        
        // Calculate splits
        uint256 platformFee = (totalAmount * platformFeeBasisPoints) / 10000;
        uint256 commission = (totalAmount * commissionBasisPoints) / 10000;
        uint256 affiliatorShare = totalAmount - platformFee - commission;
        
        // Transfer to platform
        payable(platformTreasury).transfer(platformFee);
        
        // Transfer to affiliator
        payable(affiliatorWallet).transfer(affiliatorShare);
        
        // Transfer commission to co-seller
        payable(coSellerWallet).transfer(commission);
        
        // Record order
        orders[orderId] = AffiliateOrder({
            affiliator: affiliatorWallet,
            coSeller: coSellerWallet,
            affiliatorShare: affiliatorShare,
            coSellerShare: commission,
            platformFee: platformFee,
            paid: true
        });
        
        emit AffiliatePaymentSplit(
            orderId,
            affiliatorWallet,
            coSellerWallet,
            affiliatorShare,
            commission,
            platformFee
        );
    }
    
    // For ERC20 tokens (USDC, USDT, etc.)
    function processAffiliatePaymentERC20(
        bytes32 orderId,
        address token,
        address affiliatorWallet,
        address coSellerWallet,
        uint256 totalAmount,
        uint256 commissionBasisPoints
    ) external {
        require(!orders[orderId].paid, "Order already paid");
        
        IERC20 paymentToken = IERC20(token);
        require(
            paymentToken.transferFrom(msg.sender, address(this), totalAmount),
            "Transfer failed"
        );
        
        // Calculate splits
        uint256 platformFee = (totalAmount * platformFeeBasisPoints) / 10000;
        uint256 commission = (totalAmount * commissionBasisPoints) / 10000;
        uint256 affiliatorShare = totalAmount - platformFee - commission;
        
        // Transfer to all parties
        paymentToken.transfer(platformTreasury, platformFee);
        paymentToken.transfer(affiliatorWallet, affiliatorShare);
        paymentToken.transfer(coSellerWallet, commission);
        
        orders[orderId].paid = true;
        
        emit AffiliatePaymentSplit(
            orderId,
            affiliatorWallet,
            coSellerWallet,
            affiliatorShare,
            commission,
            platformFee
        );
    }
}

interface IERC20 {
    function transfer(address to, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}
```

---

## 6. Conflict Resolution

### Conflict 1: Commission Calculation Timing

**Scenario**: Co-seller imports product at 15% commission. Affiliator changes to 10% before customer checks out. Which rate applies?

**Resolution**:
```javascript
// Commission rate is locked at checkout time, not import time
function calculateCommission(order, product, import) {
  // Get current rate from Affiliator's settings
  const currentRate = getAffiliatorCommissionRate(
    product.affiliatorId,
    product.id
  );
  
  // Lock this rate for the order
  order.commissionRate = currentRate;
  order.commissionRateLockedAt = new Date();
  
  return order.commissionRate;
}
```

**Best Practice**: Rate at checkout time (like Amazon Associates cookie window)

### Conflict 2: Currency Change with Active Imports

**Scenario**: Affiliator (USD) has 10 Co-sellers. Affiliator changes currency to EUR.

**Resolution**:
```
ON affiliator.currency_change:
  // Notify all Co-sellers
  FOR co_seller IN affiliator.coSellers:
    send_notification(
      to: co_seller,
      subject: "Affiliator currency change notification",
      body: `
        ${affiliator.name} changed their currency to EUR.
        
        What this means for you:
        • Future commissions will be calculated in EUR
        • Your existing imports remain active
        • Commission will be converted to your currency (${co_seller.currency})
        
        Current exchange rate: 1 USD = 0.90 EUR
      `
    );
  
  // Existing orders complete in original currency
  // New orders use new currency
  // Commission calculations updated
```

### Conflict 3: Product Deletion by Affiliator

**Scenario**: Affiliator deletes product that 5 Co-sellers are selling. Customers have it in cart.

**Resolution**:
```
ON product.delete:
  // Update all imports
  imports = getImportsOfProduct(product.id);
  
  FOR import IN imports:
    import.status = 'deleted_by_affiliator';
    import.deletedAt = now();
    
    // Notify Co-seller
    notify_co_seller(import.coSellerId, 
      "Product ${product.name} has been deleted by the Affiliator. " +
      "It has been removed from your shop."
    );
  
  // Handle active carts
  activeCarts = getCartsWithProduct(product.id);
  FOR cart IN activeCarts:
    removeProductFromCart(cart, product.id);
    notify_customer(cart.customerId,
      "An item in your cart is no longer available and has been removed."
    );
  
  // Handle pending orders
  pendingOrders = getPendingOrdersWithProduct(product.id);
  FOR order IN pendingOrders:
    IF order.status == 'pending_payment':
      cancelOrder(order, reason: 'Product unavailable');
      refundCustomer(order);
    ELSE IF order.status == 'paid':
      // Honor the order
      fulfillOrder(order);
```

### Conflict 4: Mixed Cart with Affiliate Products

**Scenario**: Cart has:
- Regular product ($50) - Merchant keeps all
- Affiliate product ($100) - Split with Co-seller
- POD product ($30) - Printful cost deducted

**Resolution**:
```
┌─────────────────────────────────────────────────────────────┐
│  Mixed Cart Checkout                                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Cart Items:                                                │
│  • Product A (Regular)          $50.00  → Merchant A      │
│  • Product B (Affiliate)        $100.00 → Split           │
│  • Product C (POD)              $30.00  → Printful        │
│                                                             │
│  Subtotal:                      $180.00                    │
│  Shipping:                      $15.00                     │
│  Tax:                           $16.20                     │
│  ─────────────────────────────────────                     │
│  Total:                         $211.20                    │
│                                                             │
│  Payment Routing:                                           │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ Customer pays: $211.20                               │   │
│  │                                                       │   │
│  │ Platform receives full amount                        │   │
│  │                                                       │   │
│  │ Splits:                                              │   │
│  │ • Merchant A (regular): $50.00 - fees               │   │
│  │ • Affiliator B: $85.00 (after Co-seller $15)        │   │
│  │ • Co-seller B: $15.00 commission                    │   │
│  │ • Printful: $30.00 (POD cost)                       │   │
│  │ • Platform: Fees from all                           │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
└─────────────────────────────────────────────────────────────┘

Implementation:
- Each line item tracked separately
- Commission calculated per affiliate item
- Multiple payouts created from single order
- Each recipient sees relevant portion in dashboard
```

### Conflict 5: Crypto Payment - Which Wallet?

**Scenario**: Customer pays with USDC for affiliate product. Both Affiliator and Co-seller have wallets connected.

**Resolution**:
```solidity
// Smart contract receives payment
// Both wallets must be connected for crypto affiliate sales

function validateWallets(order) {
  require(
    order.affiliator.wallet != address(0),
    "Affiliator wallet not connected"
  );
  require(
    order.coSeller.wallet != address(0),
    "Co-seller wallet not connected"
  );
  
  // If either wallet not connected, fall back to credit
  IF !walletsConnected:
    order.paymentMethod = 'fiat_fallback';
    platform.receivesPayment(order.total);
    // Platform credits both merchants
}

// UX: At checkout, if crypto selected for affiliate product
// Check both wallets connected
// If not, show: "Crypto unavailable - Affiliator hasn't connected wallet"
```

---

## 7. Best Practices from Industry

### From Amazon Associates:
- ✅ **24-hour cookie window**: Attribution window for commissions
- ✅ **Standardized rates by category**: Clear commission structure
- ✅ **Cross-border tracking**: Works across country sites
- ✅ **Real-time reporting**: Instant commission visibility

**Our Implementation**:
- 30-day cookie window (more generous)
- Flexible rates (per-product or global)
- Real-time earnings dashboard
- Currency normalization

### From Shopify Collabs:
- ✅ **Influencer discovery**: Find partners to promote products
- ✅ **Automated commission tracking**: No manual reconciliation
- ✅ **Product gifting**: Send products to affiliates
- ✅ **Performance analytics**: Track affiliate effectiveness

**Our Implementation**:
- Product marketplace for discovery
- Automated commission via smart contracts
- Import system (instead of gifting)
- Earnings analytics for both parties

### From ShareASale:
- ✅ **Merchant approval**: Affiliates apply, merchants approve
- ✅ **Custom commission terms**: Negotiated per relationship
- ✅ **Reversal rules**: Handle returns and cancellations
- ✅ **Attribution models**: First-click, last-click options

**Our Implementation**:
- Open import (lower friction) + optional approval
- Flexible commission structure
- Clear reversal policies
- Last-click attribution

---

## 8. User Experience

### 8.1 Co-seller Dashboard

```
┌─────────────────────────────────────────────────────────────┐
│  🤝 Affiliate Network (Co-seller View)                       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  💰 Your Affiliate Earnings                                 │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  This Month: €1,245.00                                 │  │
│  │  Pending:    €340.00                                   │  │
│  │  All Time:   €8,920.00                                 │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                              │
│  📦 Your Imported Products                                  │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  [Product Image] Headphones XYZ                        │  │
│  │  From: TechStore (Affiliator)                          │  │
│  │  Your Price: €85.00                                    │  │
│  │  Base Price: $90.00                                    │  │
│  │  Commission: 15% (€12.75 per sale)                    │  │
│  │  Sales: 23 units | Earnings: €293.25                  │  │
│  │  Status: ✅ Active                                     │  │
│  │  [Edit] [Pause] [Remove]                               │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                              │
│  🔍 Browse Products to Import                               │
│  [Search products...] [Filter by category ▼]                │
│                                                              │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  [Product Image] Smart Watch                           │  │
│  │  By: GadgetWorld | Commission: 20%                     │
│  │  Base Price: $150.00 | Your est. earn: $30.00         │  │
│  │  [Import Product]                                      │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                              │
│  📊 Performance                                             │
│  [Sales Chart] [Top Products] [Conversion Rate]             │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### 8.2 Affiliator Dashboard

```
┌─────────────────────────────────────────────────────────────┐
│  🏪 Affiliate Network (Affiliator View)                      │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  📈 Network Overview                                        │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  Active Co-sellers: 12                                 │  │
│  │  Products in Network: 45                               │  │
│  │  Network Sales (30d): $12,450.00                       │  │
│  │  Commission Paid Out: $1,867.50                        │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                              │
│  💵 Commission Settings                                     │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  Default Commission Rate: 15%                         │  │
│  │  [Edit Default Rate]                                   │  │
│  │                                                        │  │
│  │  Per-Product Overrides:                                │  │
│  │  • Headphones XYZ: 20% [Edit]                         │  │
│  │  • Smart Watch: 10% [Edit]                            │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                              │
│  👥 Your Co-sellers                                         │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  FashionHub                                            │  │
│  │  Products selling: 5 | Monthly sales: $2,340          │  │
│  │  Commission paid: $351.00 | Status: Active            │  │
│  │  [View Details] [Message] [Adjust Commission]         │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                              │
│  📦 Products Available to Co-sellers                        │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  ☑️ Headphones XYZ (20% commission)                   │  │
│  │  ☑️ Smart Watch (10% commission)                      │  │
│  │  ☐ Gaming Mouse (not in network) [Add]                │  │
│  │                                                        │  │
│  │  [Add Products to Network]                            │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                              │
│  [⚠️ Deactivate Affiliate Network]                         │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 9. Implementation Roadmap

### Phase 1: Core Relationships (Weeks 1-4)
- [ ] Role assignment system
- [ ] Product import flow
- [ ] Commission configuration
- [ ] Basic tracking

### Phase 2: Payment Integration (Weeks 5-8)
- [ ] Fiat split implementation
- [ ] Smart contract development
- [ ] Crypto split deployment
- [ ] Payout system

### Phase 3: Currency & Sync (Weeks 9-12)
- [ ] Currency normalization
- [ ] Product sync engine
- [ ] Price update propagation
- [ ] Multi-currency commission calc

### Phase 4: Lifecycle Management (Weeks 13-16)
- [ ] Product deletion handling
- [ ] Commission change notifications
- [ ] Cart/checkout integration
- [ ] Order fulfillment tracking

### Phase 5: Analytics & Scale (Weeks 17-20)
- [ ] Earnings dashboards
- [ ] Performance metrics
- [ ] Bulk operations
- [ ] Advanced reporting

---

## 10. Success Metrics

1. **Activation Rate**: >30% of eligible merchants activate affiliate network
2. **Import Success**: >90% of imports result in at least one sale
3. **Commission Accuracy**: 99.9% accurate commission calculations
4. **Payout Reliability**: <1% failed payouts
5. **Network Growth**: Average 5 Co-sellers per Affiliator
6. **Revenue Impact**: Affiliate sales represent >20% of platform GMV

---

## 11. Open Questions

1. Should we allow Co-sellers to negotiate commission rates?
2. How do we handle returns/refunds with split payments?
3. Should we support tiered commissions (volume-based)?
4. Do we need geographic restrictions for affiliates?
5. How do we prevent fraud (fake sales for commissions)?

---

## Appendix A: Conflict Scenario Solutions

This appendix provides specific solutions for affiliate-related conflicts identified in the Feature Conflicts Report.

### A.1 Affiliate Network Conflicts

#### Scenario 7: Commission Rate Change Timing
**Problem:** Affiliator changes commission from 15% to 5%. Co-seller has product in cart. Customer checks out 1 minute after change. Which rate applies?

**Solution: Lock Rate at Checkout Time**
- Commission rate from time of checkout (not import)
- Protects both parties from mid-checkout changes
- Clear display of rate used

**Implementation:**
```javascript
order.commissionRate = getCurrentRate(product);
order.commissionLockedAt = new Date();
// Rate valid for 24 hours
```

**Trade-off:** Current rate vs stability. Chosen: Stability.

---

#### Scenario 8: Affiliator Disables Affiliate
**Problem:** Affiliator has 50 Co-sellers. Disables affiliate network. What happens to pending orders?

**Solution: 7-Day Grace Period**
- Existing orders fulfilled normally
- New imports blocked immediately
- Grace period for pending payments

**Implementation:**
```
ON disable_affiliate:
  - Block new imports immediately
  - Allow 7 days for pending orders
  - Notify all Co-sellers
  - Products marked "Out of Stock" after grace period
```

**Trade-off:** Speed vs fairness. Chosen: Fairness.

---

#### Scenario 9: Affiliator Deletes Product
**Problem:** Affiliator deletes product. 10 Co-sellers selling it. Customers have it in cart.

**Solution: Immediate Removal with Cart Notification**
- Remove from Co-seller shops immediately
- Notify customers with product in cart
- Pending orders: fulfill if paid, cancel if not

**Implementation:**
```
ON product_delete:
  - Update all imports to 'deleted' status
  - Remove from active carts
  - Notify affected customers
  - Handle pending orders based on payment status
```

**Trade-off:** Speed vs customer experience. Chosen: Transparency.

---

#### Scenario 10: Affiliator Changes Price
**Problem:** Affiliator changes price from $100 to $120, then to $130. Customer checks out. What price?

**Solution: Price Locked at Checkout**
- Customer pays price at checkout time
- Real-time sync to Co-sellers
- Commission calculated on final price

**Implementation:**
```javascript
order.price = getCurrentPrice(product);
order.priceLockedAt = new Date();
// Sync price to all Co-sellers in real-time
```

**Trade-off:** Sync speed vs accuracy. Chosen: Real-time sync.

---

#### Scenario 11: Co-seller Removes Import
**Problem:** Co-seller removes import. Customer has it in cart. What happens at checkout?

**Solution: Allow Checkout if in Cart**
- Don't remove from active carts
- Complete order normally
- Remove from storefront only

**Implementation:**
```
ON remove_import:
  - Hide from storefront
  - Keep in active carts
  - Allow checkout completion
  - Commission still paid
```

**Trade-off:** Inventory accuracy vs customer experience. Chosen: Customer experience.

---

#### Scenario 12: Role Across Multiple Shops
**Problem:** Merchant is Affiliator in Shop A. Creates Shop B. Can they be Co-seller in Shop B?

**Solution: Role Per Shop (Not Per User)**
- Can be Affiliator in Shop A
- Can be Co-seller in Shop B
- Role is shop-level, not account-level

**Implementation:**
```typescript
interface Shop {
  id: string;
  ownerId: string;
  role: 'affiliator' | 'co_seller' | 'unassigned';
}
// Same user can have different roles in different shops
```

**Trade-off:** Simplicity vs flexibility. Chosen: Flexibility.

---

#### Scenario 13: Affiliator Tries to Import
**Problem:** Affiliator wants to import another product. Affiliators cannot import.

**Solution: Create Separate Shop or Temporarily Disable**
- Option 1: Create new shop for Co-selling
- Option 2: Temporarily pause Affiliator status
- Option 3: Use different user account

**Implementation:**
```
IF user.isAffiliator():
  show_options([
    "Create new shop for Co-selling",
    "Temporarily pause Affiliator status (14-day cooldown)",
    "Use different account"
  ]);
```

**Trade-off:** Restrictions vs flexibility. Chosen: Flexibility with clear options.

---

### A.2 Mixed Cart Conflicts

#### Scenario 14: Coupon with Mixed Cart
**Problem:** Cart has regular (coupon allowed) + affiliate (coupon blocked) + recorded products. Customer tries coupon.

**Solution: Show Clear Exclusion Message**
- Calculate discount on eligible items only
- Show which items excluded
- Clear UX explanation

**Implementation:**
```
Coupon applied: $10 off

Eligible items:
✓ Regular Product: -$10.00

Excluded items:
✗ Affiliate Product (coupons not applicable)
✗ Recorded NFT Product (coupons not applicable)

Total discount: $10.00
```

**Trade-off:** Complexity vs clarity. Chosen: Clarity.

---

#### Scenario 15: Mixed Cart Checkout Flow
**Problem:** Cart has regular + affiliate + POD. 3 different payout flows in one order.

**Solution: Line-Item Payout Routing**
- Each item routed separately
- Regular → Merchant
- Affiliate → Split
- POD → Printful

**Implementation:**
```typescript
interface OrderPayout {
  lineItems: [
    { type: 'regular', recipient: merchantA, amount: 50 },
    { type: 'affiliate_affiliator', recipient: affiliatorB, amount: 85 },
    { type: 'affiliate_coSeller', recipient: coSellerB, amount: 15 },
    { type: 'pod', recipient: printful, amount: 30 }
  ];
}
```

**Trade-off:** Complexity vs accuracy. Chosen: Accuracy.

---

#### Scenario 16: Shipping Calculation Mixed Cart
**Problem:** Physical product (merchant ships) + POD (Printful ships) + affiliate (Affiliator ships). Who pays shipping?

**Solution: Item-Level Shipping Display**
- Show shipping per item
- Each item ships from respective source
- Customer pays combined shipping

**Implementation:**
```
Shipping breakdown:
• Regular Product: $5.00 (shipped by Merchant A)
• POD Product: $4.50 (shipped by Printful)
• Affiliate Product: $6.00 (shipped by Affiliator B)

Total shipping: $15.50
```

**Trade-off:** Complexity vs transparency. Chosen: Transparency.

---

### A.3 Order Processing Conflicts

#### Scenario 17: Merchant Credit Insufficient
**Problem:** Order creates negative share (merchant owes $10). Merchant has $5 credit. Order blocked. Customer sees error.

**Solution: Graceful Error Message**
- Generic error: "Payment processing issue"
- Don't expose internal credit system
- Alert merchant separately

**Implementation:**
```
Customer sees: "Unable to process order. Please try again or contact support."

Merchant sees: "Order blocked: Insufficient credit balance. Current: $5.00, Required: $10.00"
```

**Trade-off:** Transparency vs system protection. Chosen: System protection.

---

#### Scenario 18: Order with Zero Total
**Problem:** Cart total $0 after discounts. Payment skipped. Affiliate commission $0. Co-seller gets nothing.

**Solution: Allow $0 Orders**
- Commission = $0 (fair)
- Track as marketing cost
- Co-seller gets nothing (expected)

**Implementation:**
```
Order total: $0.00
Payment: Skipped
Commission: $0.00
Status: Complete (marketing promotion)
```

**Trade-off:** Fairness vs complexity. Chosen: Fairness.

---

#### Scenario 19: Payment Method Change During Checkout
**Problem:** Customer switches from Stripe to PayPal. Commission calculations differ. What if merchant gets negative?

**Solution: Recalculate on Method Change**
- Update commission with new fees
- Show merchant share preview
- Block if would go negative

**Implementation:**
```
Payment method changed to PayPal

Updated calculation:
• Order total: $100.00
• PayPal fees: $3.20
• Affiliate commission: $15.00
• Platform fee: $1.00
• Your share: $80.80

[Continue] [Change Method]
```

**Trade-off:** Complexity vs accuracy. Chosen: Accuracy.

---

#### Scenario 20: Affiliate Order Customer Info
**Problem:** Affiliator sees shipping info (they ship). Co-seller sees order without details. Co-seller may need to contact customer.

**Solution: Show Co-seller with Restrictions**
- Co-seller sees customer info
- Cannot contact directly
- Platform mediates if needed

**Implementation:**
```
Co-seller Dashboard:
Order #12345
Customer: John D. (john@example.com)
Shipping: 123 Main St, City, Country

⚠️ Privacy Notice: Do not contact customer directly. Use platform messaging.
```

**Trade-off:** Privacy vs functionality. Chosen: Functionality with restrictions.

---

#### Scenario 21: Fulfillment Status Sync
**Problem:** Affiliator marks "shipped". Co-seller sees same order. Does status sync? What if Affiliator never updates?

**Solution: Real-Time Sync with Timeout**
- Status syncs automatically
- If no update in 7 days: alert Co-seller
- Escalation path available

**Implementation:**
```
Day 0: Affiliator marks shipped
Day 0: Co-seller sees "Shipped"
Day 7: If no update: Co-seller gets alert
Day 10: Auto-escalate to support
```

**Trade-off:** Automation vs oversight. Chosen: Automation with oversight.

---

### A.4 Commission Calculation Conflicts

#### Scenario 22: Commission on Discounted Price
**Problem:** Product $100, commission 15%. Customer uses 20% coupon. Final $80. Commission on $100 or $80?

**Solution: Commission on Original Price**
- Co-seller gets full commission ($15)
- Affiliator absorbs discount
- Documented clearly

**Implementation:**
```
Order: $100.00
Discount: -$20.00 (20% off)
Final: $80.00

Commission: $15.00 (15% of original $100)
Affiliator receives: $65.00
```

**Trade-off:** Affiliator vs Co-seller fairness. Chosen: Co-seller protection.

---

#### Scenario 23: Referral Share + Affiliate
**Problem:** Shop has referral (3%) + affiliate (15%). Customer referred. Who gets what?

**Solution: Referral on Final Profit**
- Calculate affiliate commission first
- Calculate referral on remaining amount
- Transparent breakdown

**Implementation:**
```
Order: $100.00
Affiliate commission: $15.00 (15%)
Remaining: $85.00
Referral (3% of $85): $2.55
Platform fee (1%): $1.00
Merchant receives: $81.45
```

**Trade-off:** Complexity vs fairness. Chosen: Fairness.

---

#### Scenario 24: Droplinked Commission Stacking
**Problem:** 1% platform + 3% Stripe + 15% affiliate + 3% referral = 22%. Merchant gets little. Show breakdown?

**Solution: Full Transparency with Calculator**
- Show complete breakdown before activation
- Commission calculator tool
- Clear expectations

**Implementation:**
```
Commission Breakdown Calculator:
• Product price: $100.00
• Platform fee (1%): -$1.00
• Payment fee (3%): -$3.00
• Affiliate commission (15%): -$15.00
• Referral (3%): -$3.00
─────────────────────────────
You receive: $78.00

⚠️ Is this margin acceptable for your business?
```

**Trade-off:** Complexity vs transparency. Chosen: Transparency.

---

#### Scenario 25: Negative Commission Edge Case
**Problem:** POD costs $10, sells for $12. Commission 15% = $1.80. Product profit only $2. Affiliator loses money.

**Solution: Block Low-Margin Products**
- Warn if margin < 20%
- Suggest higher prices
- Allow override with acknowledgment

**Implementation:**
```
⚠️ Low Margin Warning

Product: T-Shirt
Cost: $10.00 | Price: $12.00 | Margin: $2.00 (20%)
Affiliate commission: $1.80 (15%)

Your profit after affiliate: $0.20

Options:
• Increase price to $15.00+ (recommended)
• Lower commission to 10%
• Accept low margin and proceed
```

**Trade-off:** Restrictions vs merchant freedom. Chosen: Warnings with freedom.

---

### A.5 Summary of Trade-offs

| Conflict | Solution | Trade-off |
|----------|----------|-----------|
| Commission Timing | Lock at checkout | Stability |
| Disable Affiliate | 7-day grace period | Fairness |
| Product Deletion | Immediate removal | Transparency |
| Price Changes | Real-time sync | Accuracy |
| Import Removal | Allow cart checkout | Customer experience |
| Role Across Shops | Per-shop roles | Flexibility |
| Affiliator Importing | Create separate shop | Flexibility |
| Mixed Cart Coupon | Show exclusions | Clarity |
| Mixed Cart Payout | Line-item routing | Accuracy |
| Mixed Cart Shipping | Item-level display | Transparency |
| Credit Insufficient | Generic error | System protection |
| Zero Total Orders | Allow $0 | Fairness |
| Payment Method Change | Recalculate | Accuracy |
| Customer Info | Show with restrictions | Balance |
| Fulfillment Sync | Auto-sync with timeout | Automation |
| Commission on Discount | Original price | Co-seller protection |
| Referral + Affiliate | Referral on profit | Fairness |
| Commission Stacking | Full transparency | Transparency |
| Low Margin | Warning system | Balance |

---

*Last Updated: 2026-02-12*  
*Next Review: After Phase 2 completion*
