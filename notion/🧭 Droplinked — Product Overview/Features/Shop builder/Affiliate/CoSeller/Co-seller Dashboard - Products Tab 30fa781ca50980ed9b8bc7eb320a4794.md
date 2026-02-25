# Co-seller Dashboard - Products Tab

**Feature ID:** IAA-AFF-008

**Category:** Affiliate | Shop Builder

**Actors:** Merchant (Co-seller)

**Channel:** Web

**Status:** Defined

**Owner:** Behdad

---

## Part 1: Human-Readable Spec

### Problem Statement

Co-sellers need to manage and track the products they've imported from different merchants. The Products tab shows all imported products with their performance metrics, commission rates, and current status. This helps Co-sellers monitor their product portfolio and earnings per product.

**Remove Product:** Co-sellers can remove individual products from their shop. This removes the product only from their shop and does not affect other Co-sellers.

### User Stories

- As a Co-seller, I want to see all my imported products so that I can manage my product portfolio.
- As a Co-seller, I want to see per-product performance so that I can identify top earners.
- As a Co-seller, I want to see commission rates per product so that I can track my earnings percentage.
- As a Co-seller, I want to see product status so that I can know if products are active or unavailable.
- As a Co-seller, I want to remove products from my shop so that I can clean up underperforming products.

### Key User Journeys

**Journey 1: View Imported Products**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Co-seller | On Dashboard, clicks "Products" tab | Products tab loads |
| 2 | System | Shows products table | List of all imported products |
| 3 | System | Shows product metrics | Product name, Commission Rate, Price, Items Sold, Earnings, Status |
| 4 | Co-seller | Reviews table | Can see which products are performing best |
| 5 | Co-seller | Sees product status | Active, Out of Stock, Removed, etc. |

**Journey 2: Remove Product from Shop**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Co-seller | On Products tab, clicks "Remove" button next to product | Confirmation modal opens |
| 2 | System | Shows confirmation modal | "This product will be removed from your shop. This action cannot be undone." |
| 3 | System | Shows product details | Product name and thumbnail displayed |
| 4 | Co-seller | Reviews and clicks "Confirm Remove" | Product removed from shop |
| 5 | System | Updates product status | Status changes to "Removed" |
| 6 | System | Shows success message | "Product removed from your shop" |
| 7 | Co-seller | Product disappears from storefront | No longer visible to customers |
| 8 | Co-seller | Can re-import later | Product available in marketplace for re-import |

### Scope

**✅ In Scope:**

- **Products Table**
    - Product name/image
    - Merchant name
    - Commission Rate (%)
    - Price (USD)
    - Items Sold (count)
    - Earnings (USD)
    - Status (Active, Out of Stock, Removed, etc.)
    - Sorted by Earnings (descending)
- **Remove Product Action**
    - "Remove" button next to each product
    - Confirmation modal with product details
    - Immediate removal from Co-seller's shop only
    - Status updates to "Removed"
    - Ability to re-import removed products later
- **Product Actions**
    - View product in shop (link to storefront)
- **Navigation**
    - Tabs: Overview, Products (active), Orders

**❌ Out of Scope:**

- Editing product details (controlled by Affiliator)
- Changing commission rates
- Bulk import/remove
- Product search/filter (future enhancement)
- Product sorting by other columns

### Acceptance Criteria

- [ ]  Products tab shows table with columns:
    - Product (name + thumbnail)
    - Merchant (name)
    - Commission Rate (%)
    - Price (USD)
    - Items Sold (count)
    - Earnings (USD)
    - Status (Active, Out of Stock, Removed, etc.)
- [ ]  Table sorted by Earnings (highest first)
- [ ]  Empty state shown if no products imported
- [ ]  All monetary values in USD
- [ ]  Commission rate shows as percentage
- [ ]  Status reflects current availability:
    - Active: Product available for sale
    - Out of Stock: Affiliator disabled or stock = 0
    - Removed: Co-seller removed product
- [ ]  **"Remove" button visible for each active product**
- [ ]  **Clicking "Remove" opens confirmation modal**
- [ ]  **Modal shows product name and thumbnail**
- [ ]  **Modal displays warning: "This action cannot be undone"**
- [ ]  **Confirming removal immediately removes product from Co-seller's shop only**
- [ ]  **Other Co-sellers' shops are NOT affected**
- [ ]  **Removed product status changes to "Removed"**
- [ ]  **Removed products can be re-imported from marketplace later**
- [ ]  **Products in active cart/checkout can still be purchased (no disruption)**
- [ ]  "View in Shop" action opens product in storefront
- [ ]  Tabs: Overview, Products (active), Orders

### Technical Notes

- Products displayed from imported_products table
- Join with products for names/images
- Join with affiliate_products for commission rates
- Status calculated from multiple sources (affiliate enabled, stock, import status)
- **Remove action sets is_active = false on import record (Co-seller's shop only)**
- **Does NOT affect other Co-sellers' imported_products records**
- **Product can be re-imported by setting new imported_products record**

### Dependencies

- Imported products data
- Product catalog
- Affiliate product records
- Remove product API

---

## UI Flow (Source of Truth)

```
[Co-seller Dashboard - Overview Tab]
    ↓
[Click Products Tab]
    ↓
[Products Tab]
    ┌──────────────────────────────────────────────────┐
    │ Products Table                                   │
    │ ┌──────────┬──────────┬─────┬───────┬─────────┐│
    │ │ Product  │ Merchant │Comm.│ Price │ Items   ││
    │ │          │          │Rate │ (USD) │ Sold    ││
    │ │          │          │     │       │ Earnings││
    │ │          │          │     │       │ Status  ││
    │ │[Img]Name │ Store A  │ 15% │$29.99│ 45      ││
    │ │          │          │     │       │ $435.00 ││
    │ │          │          │     │       │ Active  ││
    │ │          │          │     │       │ [Remove]││ ← Button
    │ │[Img]Name │ Store B  │ 20% │$49.99│ 32      ││
    │ │          │          │     │       │ $320.00 ││
    │ │          │          │     │       │ Out of  ││
    │ │          │          │     │       │ Stock   ││
    │ │ ...      │ ...      │ ... │ ...   │ ...     ││
    │ └──────────┴──────────┴─────┴───────┴─────────┘│
    │                                                  │
    │ Tabs:                                            │
    │ [Overview] [Products] [Orders]                 │
    └──────────────────────────────────────────────────┘
    ↓
[Click Remove Button]
    ↓
[Confirmation Modal]
    ┌──────────────────────────────────────────────────┐
    │ Remove Product                                   │
    │                                                  │
    │ [Product Image]                                  │
    │ Product Name                                     │
    │                                                  │
    │ ⚠️ This product will be removed from your shop.   │
    │ This action cannot be undone.                    │
    │                                                  │
    │ You can re-import this product later from the    │
    │ marketplace if you change your mind.               │
    │                                                  │
    │ [Cancel]        [Confirm Remove]                 │
    └──────────────────────────────────────────────────┘
    ↓
[Click Confirm Remove]
    ↓
[Product Removed from Shop]
[Status: Removed]
[Success Message]
    ↓
[Product Removed from Storefront]
[Still Available in Other Co-seller Shops]
```

---

## Attachments

- 📎 **Linked Request:** REQ-AFF-009
- 🎨 **Design:** [Figma - Co-seller Products Tab]
- 🧪 **Test Cases:** TC-AFF-101 through TC-AFF-115

---

## Change Log

- 2026-02-25 — Behdad — Updated to add detailed Remove Product functionality (confirmation modal, status change, re-import capability)
- 2026-02-22 — Behdad — Created based on [init.md](http://init.md/) requirements

---