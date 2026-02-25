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

Before importing a product, Co-sellers need to see complete product details including images, description, price, commission information, and merchant payment methods. The Product Page provides a detailed view similar to a regular Product Detail Page (PDP) with additional affiliate-specific information like commission rate, net profit calculation, and estimated earnings based on hypothetical sales volume.

**Dynamic Profit Calculator:** A slider allows Co-sellers to see potential earnings at different sales volumes (1 to 10,000 units), helping them understand the profit potential of the product.

**Payment Method Information:** Co-sellers can see which payment methods the merchant has connected and understand how they will receive commission payouts.

### User Stories

- As a Merchant, I want to view complete product details so that I can evaluate the product before importing.
- As a Merchant, I want to see the commission rate so that I know my earnings percentage.
- As a Merchant, I want to see net profit calculation so that I know my earnings per sale.
- As a Merchant, I want to use a profit calculator to see potential earnings at different sales volumes.
- As a Merchant, I want to see merchant payment methods so that I know how I'll receive my commission.
- As a Merchant, I want to see a warning about payment method requirements so that I can connect mine before selling.
- As a Merchant, I want to import a product with one click so that I can start selling quickly.

### Key User Journeys

**Journey 1: View Product Details**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Clicks product from marketplace | Product detail page loads |
| 2 | System | Shows product images | Gallery with primary and additional images |
| 3 | System | Shows product info | Title, description, price, variants |
| 4 | System | Shows affiliate info | Shop info, commission rate, net profit |
| 5 | Merchant | Reviews all information | Complete product evaluation |

**Journey 2: Import Product (First Time - Becomes Co-seller)**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant (No Role, USD) | Clicks "Add Product" | System checks currency |
| 2 | System | Validates USD currency | Opens Settings Modal |
| 3 | Merchant | Completes profile in modal | Introduction, Social Links, Channels |
| 4 | Merchant | Clicks "Save & Import" | Profile saved, product imported |
| 5 | System | Sets role to Co-seller | Role locked permanently |
| 6 | System | Shows success | "Product imported successfully" |

**Journey 3: Import Product (Existing Co-seller)**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Co-seller (USD) | Clicks "Add Product" | Confirmation dialog |
| 2 | Co-seller | Confirms | Product imported immediately |
| 3 | System | Shows success | "Product imported successfully" |
| 4 | System | Product appears in shop | Available for sale |

**Journey 4: Cannot Import (Affiliator)**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | Clicks "Add Product" | Error message shown |
| 2 | System | Blocks import | "Affiliators cannot import products" |

**Journey 5: Cannot Import (Non-USD)**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant (Non-USD) | Clicks "Add Product" | Error message shown |
| 2 | System | Blocks import | "USD currency required" |

**Journey 6: Use Profit Calculator**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Views product page | Profit calculator section visible |
| 2 | System | Shows profit calculator | Slider set to middle (e.g., 5,000 units) |
| 3 | System | Displays calculations | Price: $10.00 - $50.00, Profit: $10.00 - $50.00, Commission Rate: 20% |
| 4 | Merchant | Drags slider | Value changes dynamically (1 to 10,000) |
| 5 | System | Updates "Sold Items" | Shows selected number: "X units sold" |
| 6 | System | Updates "Profit You Earn" | Calculates: Sold Items × Unit Profit |
| 7 | Merchant | Reviews potential earnings | Understands profit potential at different volumes |

**Journey 7: View Payment Method Information**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Views product page | Payment methods section visible |
| 2 | System | Shows merchant's payment methods | Stripe, PayPal, Crypto icons displayed |
| 3 | System | Shows info message | "These payment methods will be used when customers purchase this product. To receive your commission directly, ensure your shop has compatible payment methods connected." |
| 4 | Merchant | Clicks info icon (if available) | Tooltip or modal explains: "If you have matching payment methods connected, you'll receive commission directly. Otherwise, it will be credited to your shop." |
| 5 | Merchant | Reviews information | Understands commission payout process |

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
- **Dynamic Profit Calculator**
    - Interactive slider (range: 1 to 10,000 units)
    - Real-time "Sold Items" display (updates as slider moves)
    - Real-time "Profit You Earn" calculation in USD
    - Shows price range and profit range
    - Commission rate display
- **Payment Method Information**
    - Display merchant's connected payment methods (Stripe, PayPal, Crypto)
    - Info message explaining payout process
    - Warning if Co-seller doesn't have matching payment methods
    - Explanation of direct payout vs shop credit
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

- [ ]  Product page shows full product gallery
- [ ]  Product information shows:
    - Title
    - Description
    - Price in USD
    - Available quantity
    - Product type
    - Variants (if any)
- [ ]  Affiliate information shows:
    - Shop name and logo
    - Shop category
    - Commission rate percentage
    - Net profit calculation (Price × Commission%)
- [ ]  **Dynamic profit calculator visible with interactive slider (1-10,000)**
- [ ]  **Slider updates "Sold Items" display in real-time**
- [ ]  **"Profit You Earn" calculates and displays in USD: Sold Items × Unit Profit**
- [ ]  **Price range and profit range displayed**
- [ ]  **Payment methods section shows merchant's connected payment methods**
- [ ]  **Info message displayed: "These payment methods will be used when customers purchase this product"**
- [ ]  **Warning shown if Co-seller lacks matching payment methods: "Connect Stripe/PayPal/Crypto to receive direct payouts"**
- [ ]  "Add Product" button visible for eligible users
- [ ]  Button disabled/removed for:
    - Non-USD shops (show warning instead)
    - Affiliators (show message)
    - Already imported (show "Already Imported")
- [ ]  Clicking "Add Product":
    - First-time (No Role): Opens Settings Modal
    - Existing Co-seller: Simple confirmation
    - Invalid: Error message
- [ ]  After import:
    - Product available in Co-seller's shop
    - Success message shown
    - Button changes to "Imported" or removed
- [ ]  Link to shop page available
- [ ]  Back button to return to marketplace

### Technical Notes

- Load product from products + affiliate_products tables
- Calculate net profit = price × (commission_rate / 100)
- **Profit calculator: Slider range 1-10000, default 5000**
- **Profit calculation: sold_items × (price × commission_rate / 100)**
- Check already_imported status on load
- Validate currency before allowing import
- **Payment method validation: Check if Co-seller has matching payment methods**
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
    │                    │ ┌──────────────────────┐ │   │
    │                    │ │ Profit Calculator    │ │   │
    │                    │ │                      │ │   │
    │                    │ │ Price: $10-$50       │ │   │
    │                    │ │ Profit: $1-$5        │ │   │
    │                    │ │ Commission: 15%        │ │   │
    │                    │ │                      │ │   │
    │                    │ │ [====|====] Slider    │ │   │
    │                    │ │ Sold Items: 5,000      │ │   │
    │                    │ │ Profit You Earn:       │ │   │
    │                    │ │ $5,000.00 USD          │ │   │
    │                    │ └──────────────────────┘ │   │
    │                    │                          │   │
    │                    │ Payment Methods:         │   │
    │                    │ [Stripe] [PayPal] [Crypto]│   │
    │                    │ ℹ️ These payment methods │   │
    │                    │ will be used for sales.  │   │
    │                    │ Connect yours for direct │   │
    │                    │ commission payouts.      │   │
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

- 2026-02-25 — Behdad — Added Dynamic Profit Calculator (slider 1-10,000) and Payment Method Information section
- 2026-02-22 — Behdad — Complete rewrite based on [init.md](http://init.md/) requirements
- 2026-02-01 — Behdad — Initial document creation

---
