# Marketplace - Main Page

**Feature ID:** IAA-AFF-013

**Category:** Affiliate | Shop Builder

**Actors:** Merchant (Co-seller), Merchant (No Role), Customer

**Channel:** Web

**Status:** Defined

**Owner:** Behdad

---

## Part 1: Human-Readable Spec

### Problem Statement

Merchants who want to sell products without creating inventory need a centralized place to discover and import products from other merchants. The Marketplace Main Page provides a browsable catalog of all available affiliate products with search and filtering capabilities, making it easy for Co-sellers to find products that match their niche and target audience.

### User Stories

- As a Merchant, I want to browse all available affiliate products so that I can find products to sell.
- As a Merchant, I want to search for specific products so that I can find items matching my niche.
- As a Merchant, I want to filter products by criteria so that I can narrow down options.
- As a Merchant, I want to see product details at a glance so that I can quickly evaluate products.
- As a Merchant, I want to view shop information so that I can assess merchant credibility.

### Key User Journeys

**Journey 1: Browse Marketplace**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Clicks "Affiliate Network" → "Marketplace" | Marketplace main page loads |
| 2 | System | Shows product grid | All affiliate products displayed |
| 3 | System | Shows product cards | Image, Shop Name, Shop Logo, Title, Price, Commission % |
| 4 | Merchant | Scrolls through products | More products load (pagination/infinite scroll) |
| 5 | Merchant | Clicks on product card | Product detail page opens |

**Journey 2: Search Products**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Enters search term | System searches product titles and shop names |
| 2 | System | Updates results | Matching products displayed |
| 3 | Merchant | Clears search | All products shown again |

**Journey 3: Filter Products**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Selects product type filter | Shows only Physical or Digital |
| 2 | Merchant | Sets price range | Shows products within range |
| 3 | Merchant | Sets commission range | Shows products with matching commission % |
| 4 | Merchant | Selects shop category | Shows products from shops in that category |
| 5 | System | Updates results | Filtered products displayed |
| 6 | Merchant | Clicks "Clear Filters" | All filters removed, all products shown |

### Scope

**✅ In Scope:**

- **Product Grid**
    - Grid layout of product cards
    - Pagination or infinite scroll (20 products per page)
    - Empty state if no products match filters
- **Product Cards**
    - Product image
    - Shop name and logo
    - Product title
    - Price (USD)
    - Commission percentage
    - Click to view product detail
- **Search**
    - Search by product title
    - Search by shop name
    - Real-time or debounced search
    - Clear search button
- **Filters**
    - Product type: Physical, Digital
    - Price range: Min-Max input
    - Commission range: Min-Max % input
    - Shop category: Dropdown (10 categories)
    - Clear all filters button
- **Currency Check**
    - Warning for non-USD shops
    - Import disabled for non-USD

**❌ Out of Scope:**

- Product sorting (by price, commission, etc.)
- Advanced filters (tags, collections)
- Product comparison
- Wishlist/Favorites
- Recently viewed
- Recommended products

### Acceptance Criteria

- [ ]  Marketplace shows grid of all active affiliate products
- [ ]  Product cards display:
    - Product image (primary image)
    - Shop name and shop logo
    - Product title
    - Price in USD
    - Commission percentage
- [ ]  Products paginated or infinite scroll (20 per page)
- [ ]  Only products from enabled Affiliators shown
- [ ]  Only products with stock > 0 shown
- [ ]  Search works for:
    - Product title (partial match)
    - Shop name (partial match)
- [ ]  Filters available:
    - Product type: Physical, Digital
    - Price range: Min-Max inputs
    - Commission range: Min-Max % inputs
    - Shop category: 10 predefined categories
- [ ]  Multiple filters can be combined
- [ ]  "Clear Filters" button removes all filters
- [ ]  Non-USD shops see warning: "Your shop currency must be USD to import products"
- [ ]  Affiliators see products but cannot import (button disabled or hidden)
- [ ]  Clicking product card navigates to product detail page
- [ ]  Clicking shop name navigates to shop page

### Technical Notes

- Query from affiliate_products table where is_active = true
- Join with shops for shop info
- Join with products for product details
- Filter: affiliator_enabled = true AND quantity > 0
- Search: product title OR shop name (LIKE query)
- Pagination with offset/limit

### Dependencies

- Affiliate products data
- Product catalog
- Shop data
- Search and filter APIs

---

## UI Flow (Source of Truth)

```
[Sidebar: Affiliate Network] → [Marketplace Tab]
    ↓
[Marketplace Main Page]
    ┌────────────────────────────────────────────────────┐
    │ Affiliate Marketplace                              │
    │                                                    │
    │ [Search products...                    ] [Search] │
    │                                                    │
    │ Filters:                                           │
    │ Type: [All ▼]  Category: [All ▼]                  │
    │ Price: [$Min] - [$Max]                            │
    │ Commission: [Min%] - [Max%]                        │
    │ [Clear Filters]                                    │
    │                                                    │
    │ ⚠️ Your shop currency must be USD to import       │
    │                                                    │
    │ Product Grid (20 per page)                        │
    │ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐  │
    │ │ [Img]   │ │ [Img]   │ │ [Img]   │ │ [Img]   │  │
    │ │ [Logo]  │ │ [Logo]  │ │ [Logo]  │ │ [Logo]  │  │
    │ │ Shop A  │ │ Shop B  │ │ Shop C  │ │ Shop D  │  │
    │ │         │ │         │ │         │ │         │  │
    │ │Product  │ │Product  │ │Product  │ │Product  │  │
    │ │Title   │ │Title   │ │Title   │ │Title   │  │
    │ │         │ │         │ │         │ │         │  │
    │ │$29.99  │ │$49.99  │ │$19.99  │ │$39.99  │  │
    │ │ 15%    │ │ 20%    │ │ 10%    │ │ 25%    │  │
    │ └─────────┘ └─────────┘ └─────────┘ └─────────┘  │
    │                                                    │
    │ [Load More] or Pagination                         │
    └────────────────────────────────────────────────────┘
    ↓
[Click Product Card]
    ↓
[Product Detail Page]
```

---

## Attachments

- 📎 **Linked Request:** REQ-AFF-014
- 🎨 **Design:** [Figma - Marketplace Main Page]
- 🧪 **Test Cases:** TC-AFF-171 through TC-AFF-190

---

## Change Log

- 2026-02-22 — Behdad — Complete rewrite based on [init.md](http://init.md/) requirements
- 2026-02-01 — Behdad — Initial document creation

---