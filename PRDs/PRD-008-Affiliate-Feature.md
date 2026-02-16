# PRD-008: Affiliate Feature (Merchant-to-Merchant)

---

## Feature Specification

- **Feature Title:** Affiliate Feature (Merchant-to-Merchant)
- **Feature ID:** IAA-AFF-008
- **Category:** Shop Builder | Partnership
- **Actors:** Affiliator (Product Owner), Co-seller (Affiliate Partner), Customer, Platform
- **Channel:** Web Dashboard, Shopfront, Checkout, API
- **Status:** Defined
- **Owner:** Product Management
- **Linked Ticket:** Ticket 2 - Affiliate Feature (Merchant-to-Merchant)

---

## Part 1: Human-Readable Spec

### Problem Statement

Merchants need to allow other merchants to sell their products and earn commissions, enabling cross-merchant affiliate relationships. This creates a network effect where product owners (Affiliators) can expand their reach through partners (Co-sellers), while Co-sellers can monetize their audience without managing inventory. The system must handle complex scenarios including currency differences, payment splitting, and tax/shipping attribution.

### User Stories

- As an Affiliator, I want to enable other merchants to sell my products so that I can expand my distribution.
- As a Co-seller, I want to import products from other merchants so that I can offer more products without inventory.
- As an Affiliator, I want to set commission rates so that I control my profit margins.
- As a Co-seller, I want to receive my commission automatically so that I get paid fairly and promptly.
- As a merchant, I want payments split correctly even with different currencies so that accounting remains accurate.

### Key User Journeys

**Journey 1: Affiliator Enables Affiliate Network**
```
Affiliator Opens Shop Settings
    ↓
Navigates to Affiliate Network Section
    ↓
Enables Affiliate Feature (requires USD currency)
    ↓
Sets Default Commission Rate (e.g., 15%)
    ↓
Selects Which Products Are Available for Import
    ↓
Products Appear in Marketplace for Co-sellers
```

**Journey 2: Co-seller Imports Product**
```
Co-seller Browses Marketplace
    ↓
Finds Product from Affiliator
    ↓
Reviews Commission Rate and Terms
    ↓
Imports Product to Their Shop
    ↓
Product Appears in Co-seller's Storefront
    ↓
Price Syncs from Affiliator
```

**Journey 3: Affiliate Sale and Payment Split**
```
Customer Buys from Co-seller's Shop
    ↓
Checkout Processes Payment
    ↓
Smart Contract/Platform Splits Payment:
    ├─ Co-seller: 15% Commission
    ├─ Affiliator: 83% (after platform fee)
    └─ Platform: 2% Fee
    ↓
All Parties Receive Funds/Credit Instantly
    ↓
Order Fulfilled by Affiliator
```

### Scope

#### ✅ In Scope:
- Enable/disable affiliate network per shop
- Set commission rates (global and per-product)
- Product marketplace for Co-sellers to browse
- Import products from Affiliators to Co-seller shops
- Automatic price synchronization
- Commission calculation and payment splitting
- Currency conversion for cross-currency transactions
- Tax attribution (based on Co-seller location)
- Shipping attribution (Affiliator ships)
- Real-time commission tracking dashboard
- Affiliate order management
- Commission payout to connected wallets or credit
- Disable/enable individual product affiliate availability

#### ❌ Out of Scope:
- Multi-level affiliate (MLM) structures
- Affiliate links for non-merchants (influencers)
- Coupon-based affiliate tracking
- Custom affiliate agreements/contracts
- White-label/branded fulfillment
- Bulk commission rate updates
- Time-limited commission promotions
- Affiliate leaderboards/gamification

### Acceptance Criteria

- ☑ Affiliator can enable affiliate network in settings
- ☑ Affiliate activation requires USD currency (for now)
- ☑ Affiliator sets default commission rate (5-50% range)
- ☑ Affiliator can override commission per product
- ☑ Products marked as "Available for Affiliate" appear in marketplace
- ☑ Co-sellers can browse and search marketplace
- ☑ Co-seller can import product with one click
- ☑ Imported product syncs price from Affiliator automatically
- ☑ Commission calculated on final sale price
- ☑ Payment splits automatically at transaction time
- ☑ Tax calculated based on Co-seller's tax settings
- ☑ Shipping handled by Affiliator
- ☑ Co-seller sees affiliate orders in dashboard
- ☑ Affiliator sees all Co-seller orders for their products
- ☑ Commission appears in real-time after order confirmation
- ☑ If Co-seller has wallet connected, receives crypto directly
- ☑ If no wallet, Co-seller receives credit
- ☑ Affiliator can disable affiliate for specific Co-sellers
- ☑ Removing imported product removes from Co-seller shop

### Technical Notes

- Affiliate data stored in `affiliate_products` table
- Commission rates: default in shop settings, override in product_affiliate_settings
- Price sync: webhook or polling from Affiliator to Co-seller
- Payment splitting: smart contract for crypto, platform logic for fiat
- Currency conversion at order time using current rates
- Order attribution: track both Affiliator and Co-seller IDs
- Inventory managed by Affiliator only

### Dependencies
- Shop currency system (USD requirement)
- Product management system
- Marketplace/discovery system
- Checkout and payment processing
- Tax calculation engine
- Shipping management
- Wallet/credit system
- Order management system
- Smart contract infrastructure

---
