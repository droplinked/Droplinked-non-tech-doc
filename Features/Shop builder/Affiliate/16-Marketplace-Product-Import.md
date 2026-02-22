# Marketplace - Product Page & Import

**Feature ID:** IAA-AFF-015  
**Category:** Affiliate | Shop Builder  
**Actors:** Merchant (Co-seller), Merchant (No Role)  
**Channel:** Web  
**Status:** Defined  
**Owner:** Behdad

---

## Part 1: Human-Readable Spec

### Problem Statement

Before importing a product, Co-sellers need to see complete product details including images, description, price, and commission information. The Product Page provides a detailed view similar to a regular Product Detail Page (PDP) with additional affiliate-specific information like commission rate and net profit calculation.

### User Stories

- As a Merchant, I want to view complete product details so that I can evaluate the product before importing.
- As a Merchant, I want to see the commission rate so that I know my earnings percentage.
- As a Merchant, I want to see net profit calculation so that I know my earnings per sale.
- As a Merchant, I want to see merchant information so that I can assess credibility.
- As a Merchant, I want to import a product with one click so that I can start selling quickly.

### Key User Journeys

**Journey 1: View Product Details**

| Step | Actor | Action | System Response |
|------|-------|--------|-----------------|
| 1 | Merchant | Clicks product from marketplace | Product detail page loads |
| 2 | System | Shows product images | Gallery with primary and additional images |
| 3 | System | Shows product info | Title, description, price, variants |
| 4 | System | Shows affiliate info | Shop info, commission rate, net profit |
| 5 | Merchant | Reviews all information | Complete product evaluation |

**Journey 2: Import Product (First Time - Becomes Co-seller)**

| Step | Actor | Action | System Response |
|------|-------|--------|-----------------|
| 1 | Merchant (No Role, USD) | Clicks "Add Product" | System checks currency |
| 2 | System | Validates USD currency | Opens Settings Modal |
| 3 | Merchant | Completes profile in modal | Introduction, Social Links, Channels |
| 4 | Merchant | Clicks "Save & Import" | Profile saved, product imported |
| 5 | System | Sets role to Co-seller | Role locked permanently |
| 6 | System | Shows success | "Product imported successfully" |

**Journey 3: Import Product (Existing Co-seller)**

| Step | Actor | Action | System Response |
|------|-------|--------|-----------------|
| 1 | Co-seller (USD) | Clicks "Add Product" | Confirmation dialog |
| 2 | Co-seller | Confirms | Product imported immediately |
| 3 | System | Shows success | "Product imported successfully" |
| 4 | System | Product appears in shop | Available for sale |

**Journey 4: Cannot Import (Affiliator)**

| Step | Actor | Action | System Response |
|------|-------|--------|-----------------|
| 1 | Affiliator | Clicks "Add Product" | Error message shown |
| 2 | System | Blocks import | "Affiliators cannot import products" |

**Journey 5: Cannot Import (Non-USD)**

| Step | Actor | Action | System Response |
|------|-------|--------|-----------------|
| 1 | Merchant (Non-USD) | Clicks "Add Product" | Error message shown |
| 2 | System | Blocks import | "USD currency required" |

### Scope

**✅ In Scope:**

- **Product Images**
  - Primary image (large)
  - Thumbnail gallery (click to change main image)
  - Image zoom on hover (optional)

- **Product Information**
  - Title
  - Description
  - Price (USD)
  - Available quantity
  - Product type (Physical/Digital)
  - Variants (if applicable)

- **Affiliate Information**
  - Shop name and logo
  - Shop category
  - Commission rate (e.g., "15%")
  - Net profit calculation (e.g., "You earn $4.50 per sale")
  - Link to shop page

- **Import Functionality**
  - "Add Product" button
  - Currency validation (USD only)
  - Role validation (not Affiliator)
  - Duplicate check (not already imported)
  - First-time Settings Modal
  - Success/error feedback

- **Related Actions**
  - View shop page
  - Browse more from this shop
  - Share product

**❌ Out of Scope:**
- Customer reviews/ratings
- Related products
- Product comparison
- Wishlist/Favorites
- Product Q&A
- Inventory quantity alerts

### Acceptance Criteria

- [ ] Product page shows full product gallery
- [ ] Product information shows:
  - Title
  - Description
  - Price in USD
  - Available quantity
  - Product type
  - Variants (if any)
- [ ] Affiliate information shows:
  - Shop name and logo
  - Shop category
  - Commission rate percentage
  - Net profit calculation (Price × Commission%)
- [ ] "Add Product" button visible for eligible users
- [ ] Button disabled/removed for:
  - Non-USD shops (show warning instead)
  - Affiliators (show message)
  - Already imported (show "Already Imported")
- [ ] Clicking "Add Product":
  - First-time (No Role): Opens Settings Modal
  - Existing Co-seller: Simple confirmation
  - Invalid: Error message
- [ ] After import:
  - Product available in Co-seller's shop
  - Success message shown
  - Button changes to "Imported" or removed
- [ ] Link to shop page available
- [ ] Back button to return to marketplace

### Technical Notes

- Load product from products + affiliate_products tables
- Calculate net profit = price × (commission_rate / 100)
- Check already_imported status on load
- Validate currency before allowing import
- First import triggers Settings Modal
- Import creates record in imported_products table

### Dependencies

- Product catalog
- Affiliate product data
- Shop data
- Import API
- Co-seller settings modal
- Currency validation

---

## UI Flow (Source of Truth)

```
[Marketplace or Shop Page]
    ↓
[Click Product Card]
    ↓
[Product Detail Page]
    ┌────────────────────────────────────────────────────┐
    │ ← Back                                             │
    │                                                    │
    │ ┌────────────────┐ ┌──────────────────────────┐   │
    │ │                │ │ Product Title            │   │
    │ │   [Primary     │ │                          │   │
    │ │    Image]      │ │ $XX.XX USD               │   │
    │ │                │ │                          │   │
    │ │                │ │ Description              │   │
    │ │                │ │ Lorem ipsum dolor sit    │   │
    │ │                │ │ amet, consectetur...     │   │
    │ │                │ │                          │   │
    │ │ [Thumb][Thumb] │ │ Variants: [Size ▼]       │   │
    │ │ [Thumb][Thumb] │ │                          │   │
    │ └────────────────┘ │ Available: XX in stock │   │
    │                    │                          │   │
    │                    │ Sold by: [Logo] Shop Name│   │
    │                    │ Category: Fashion        │   │
    │                    │                          │   │
    │                    │ 💰 Commission: 15%       │   │
    │                    │ 💵 You earn: $X.XX/sale  │   │
    │                    │                          │   │
    │                    │ [View Shop →]            │   │
    │                    │                          │   │
    │                    │ [Add Product]            │   │
    │                    │  or                      │   │
    │                    │ [Already Imported]       │   │
    │                    │  or                      │   │
    │                    │ ⚠️ USD Required         │   │
    │                    │  or                      │   │
    │                    │ 🚫 Affiliators Cannot    │   │
    │                    │    Import                │   │
    │                    │                          │   │
    │                    │ [Share Product]          │   │
    │ └──────────────────────────┘   │
    └────────────────────────────────────────────────────┘
    ↓
[Click Add Product]
    ↓
[Check Eligibility]
    ├─ Non-USD → [Error: "USD currency required"]
    ├─ Affiliator → [Error: "Affiliators cannot import"]
    ├─ Already Imported → [Error: "Already imported"]
    ├─ No Role (First Time) → [Settings Modal]
    │                            ↓
    │                         [Complete Profile]
    │                            ↓
    │                         [Save & Import]
    │                            ↓
    │                         [Role = Co-seller]
    │                            ↓
    │                         [Product Imported]
    └─ Co-seller (Existing) → [Simple Confirm]
                               ↓
                            [Product Imported]
```

---

## Attachments

- 📎 **Linked Request:** REQ-AFF-016
- 🎨 **Design:** [Figma - Product Page]
- 🧪 **Test Cases:** TC-AFF-206 through TC-AFF-230

---

## Change Log

- 2026-02-22 — Behdad — Complete rewrite based on init.md requirements
- 2026-02-01 — Behdad — Initial document creation

---
