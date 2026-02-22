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

### User Stories

- As a Co-seller, I want to see all my imported products so that I can manage my product portfolio.
- As a Co-seller, I want to see per-product performance so that I can identify top earners.
- As a Co-seller, I want to see commission rates per product so that I can track my earnings percentage.
- As a Co-seller, I want to see product status so that I can know if products are active or unavailable.

### Key User Journeys

**Journey 1: View Imported Products**

| Step | Actor | Action | System Response |
|------|-------|--------|-----------------|
| 1 | Co-seller | On Dashboard, clicks "Products" tab | Products tab loads |
| 2 | System | Shows products table | List of all imported products |
| 3 | System | Shows product metrics | Product name, Commission Rate, Price, Items Sold, Earnings, Status |
| 4 | Co-seller | Reviews table | Can see which products are performing best |
| 5 | Co-seller | Sees product status | Active, Out of Stock, Removed, etc. |

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

- **Product Actions**
  - Remove product from shop
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

- [ ] Products tab shows table with columns:
  - Product (name + thumbnail)
  - Merchant (name)
  - Commission Rate (%)
  - Price (USD)
  - Items Sold (count)
  - Earnings (USD)
  - Status (Active, Out of Stock, Removed, etc.)
- [ ] Table sorted by Earnings (highest first)
- [ ] Empty state shown if no products imported
- [ ] All monetary values in USD
- [ ] Commission rate shows as percentage
- [ ] Status reflects current availability:
  - Active: Product available for sale
  - Out of Stock: Affiliator disabled or stock = 0
  - Removed: Co-seller removed product
- [ ] "Remove" action available for active products
- [ ] "View in Shop" action opens product in storefront
- [ ] Confirmation modal before removing product
- [ ] Tabs: Overview, Products (active), Orders

### Technical Notes

- Products displayed from imported_products table
- Join with products for names/images
- Join with affiliate_products for commission rates
- Status calculated from multiple sources (affiliate enabled, stock, import status)
- Remove action sets is_active = false on import record

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
    │ │          │          │     │       │ [Remove]││
    │ │[Img]Name │ Store B  │ 20% │$49.99│ 32      ││
    │ │          │          │     │       │ $320.00 ││
    │ │          │          │     │       │ Out of  ││
    │ │          │          │     │       │ Stock   ││
    │ │ ...      │ ...      │ ... │ ...   │ ...     ││
    │ └──────────┴──────────┴─────┴───────┴─────────┘│
    │                                                  │
    │ Tabs:                                            │
    │ [Overview] [Products] [Orders]                   │
    └──────────────────────────────────────────────────┘
```

---

## Attachments

- 📎 **Linked Request:** REQ-AFF-009
- 🎨 **Design:** [Figma - Co-seller Products Tab]
- 🧪 **Test Cases:** TC-AFF-101 through TC-AFF-115

---

## Change Log

- 2026-02-22 — Behdad — Created based on init.md requirements

---
