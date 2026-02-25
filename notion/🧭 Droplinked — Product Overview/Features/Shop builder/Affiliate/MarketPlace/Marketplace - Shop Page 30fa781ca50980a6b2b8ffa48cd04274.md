# Marketplace - Shop Page

**Feature ID:** IAA-AFF-014

**Category:** Affiliate | Shop Builder

**Actors:** Merchant (Co-seller), Merchant (No Role), Customer

**Channel:** Web

**Status:** Defined

**Owner:** Behdad

---

## Part 1: Human-Readable Spec

### Problem Statement

Co-sellers want to explore specific merchants and see all the products they offer. The Shop Page provides a merchant profile with their full product catalog, payment methods, and key statistics, making it easy for Co-sellers to browse and import multiple products from a trusted merchant.

**Payment Methods Information:** Co-sellers can see which payment methods the merchant has connected. This helps Co-sellers understand how they will receive their commission payouts.

### User Stories

- As a Merchant, I want to view a merchant's profile so that I can assess their credibility.
- As a Merchant, I want to see all products from a specific merchant so that I can browse their catalog.
- As a Merchant, I want to import products from a shop page so that I can add multiple products easily.
- As a Merchant, I want to see merchant statistics so that I can evaluate their performance.

### Key User Journeys

**Journey 1: View Shop Page**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Clicks shop name on product card | Shop page loads |
| 2 | System | Shows shop profile | Logo, name, category, description |
| 3 | System | Shows product grid | All affiliate products from this shop |
| 4 | Merchant | Browses products | Product cards with same info as marketplace |
| 5 | Merchant | Clicks product | Product detail page opens |

**Journey 2: Import from Shop Page**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | On Shop page, clicks "Add Product" on a product | Import flow starts |
| 2 | System | Checks currency and role | Validates USD and not Affiliator |
| 3 | System | Imports product | Product added to merchant's shop |

### Scope

**✅ In Scope:**

- **Shop Profile Section**
    - Shop logo
    - Shop name
    - Shop category
    - Shop description
    - "View on Marketplace" link (if applicable)
- **Payment Methods Section**
    - Display merchant's connected payment methods (Stripe, PayPal, Crypto)
    - Show which methods are active
    - Help text: "You will receive commission payouts via these methods"
- **Statistics Section**
    - Total products available
    - Average commission rate
- **Product Grid**
    - All affiliate products from this shop
    - Same product cards as marketplace
    - Click to view product detail
    - "Add Product" button per card
- **Navigation**
    - Back button to marketplace
    - Product cards link to product detail

**❌ Out of Scope:**

- Merchant rating/reviews
- Contact merchant button
- Merchant policies/terms
- Follow/subscribe to merchant
- Shop sorting/filtering
- Shop analytics for public view

### Acceptance Criteria

- [ ]  Shop page shows shop profile with:
    - Shop logo (or default if none)
    - Shop name
    - Shop category
    - Shop description
- [ ]  **Payment Methods section shows merchant's connected payment methods:**
    - **Stripe (Credit Card)** - shown if connected
    - **PayPal** - shown if connected
    - **Crypto Wallet** - shown if connected
- [ ]  **Display at least one active payment method (required for affiliate)**
- [ ]  **Show help text: "Commission payouts will be made via these payment methods"**
- [ ]  Shows grid of all active affiliate products from this shop
- [ ]  Product cards show:
    - Product image
    - Product title
    - Price (USD)
    - Commission percentage
    - **Commission amount in USD**
- [ ]  Clicking product card navigates to product detail page
- [ ]  "Add Product" button on each card (if Co-seller and USD)
- [ ]  Back button returns to marketplace
- [ ]  Non-USD shops see warning
- [ ]  Affiliators cannot import (button disabled)
- [ ]  If shop disabled affiliate, show "Shop Not Available" message
- [ ]  If no products available, show "No products available" message

### Technical Notes

- Load shop from shops table
- Load products where shop_id = {id} AND is_active = true AND affiliator_enabled = true
- Reuse product card component from marketplace
- Check merchant currency before showing import button

### Dependencies

- Shop data
- Affiliate products data
- Product catalog
- Import product API

---

## UI Flow (Source of Truth)

```
[Marketplace Main Page]
    ↓
[Click Shop Name]
    ↓
[Shop Page]
    ┌──────────────────────────────────────────────────┐
    │ ← Back to Marketplace                            │
    │                                                   │
    │ [Shop Logo]                                       │
    │ Shop Name                                         │
    │ Category: Fashion & Apparel                       │
    │                                                   │
    │ About this shop:                                  │
    │ Lorem ipsum dolor sit amet, consetetur            │
    │ sadipscing elitr...                               │
    │                                                   │
    │ Payment Methods                                   │
    │ ┌───────────────────────────────────────────────┐│
    │ │ [Credit Card] [PayPal] [Crypto]             ││
    │ │                                               ││
    │ │ Commission payouts will be made via these   ││
    │ │ payment methods.                              ││
    │ └───────────────────────────────────────────────┘│
    │                                                   │
    │ XX Products | Avg. Commission: XX%               │
    │                                                   │
    │ Product Grid                                      │
    │ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ │
    │ │ [Img]   │ │ [Img]   │ │ [Img]   │ │ [Img]   │ │
    │ │         │ │         │ │         │ │         │ │
    │ │Product  │ │Product  │ │Product  │ │Product  │ │
    │ │Title   │ │Title   │ │Title   │ │Title   │ │
    │ │         │ │         │ │         │ │         │ │
    │ │$29.99  │ │$49.99  │ │$19.99  │ │$39.99  │ │
    │ │ 15%     │ │ 20%     │ │ 10%     │ │ 25%     │ │
    │ │+$4.50   │ │+$9.99   │ │+$2.00   │ │+$9.75   │ │
    │ │[Add]   │ │[Add]   │ │[Add]   │ │[Add]   │ │
    │ └─────────┘ └─────────┘ └─────────┘ └─────────┘ │
    └──────────────────────────────────────────────────┘
    ↓
[Click Product Card]
    ↓
[Product Detail Page]
```

---

## Attachments

- 📎 **Linked Request:** REQ-AFF-015
- 🎨 **Design:** [Figma - Shop Page]
- 🧪 **Test Cases:** TC-AFF-191 through TC-AFF-205

---

## Change Log

- 2026-02-25 — Behdad — Added Payment Methods section showing merchant's connected payment methods (Stripe, PayPal, Crypto)
- 2026-02-22 — Behdad — Complete rewrite based on [init.md](http://init.md/) requirements
- 2026-02-01 — Behdad — Initial document creation

---