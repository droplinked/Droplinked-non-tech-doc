# Affiliate Network Overview

**Feature ID:** IAA-AFF-000

**Category:** Affiliate | Shop Builder

**Actors:** Merchant (Affiliator), Merchant (Co-seller), Customer

**Channel:** Web

**Status:** Defined

**Owner:** Behdad

---

## Part 1: Human-Readable Spec

### Problem Statement

Merchants want to expand their sales reach without additional marketing efforts. Product owners (Affiliators) want others to sell their products and pay commission only on successful sales. Sellers (Co-sellers) want to earn money by selling existing products without inventory management or product creation. The Affiliate Network connects these two parties in a mutually beneficial marketplace.

**Key Constraint:** Both Affiliator and Co-seller shops MUST have USD currency to participate in the Affiliate Network.

### User Stories

- As a Merchant, I want to activate Affiliate Network as an Affiliator so that other merchants can sell my products and I can expand my reach.
- As a Merchant, I want to import products from the Affiliate Marketplace so that I can sell them in my shop and earn commission.
- As an Affiliator, I want to manage my partners (Co-sellers) and see their performance so that I can track my affiliate revenue.
- As a Co-seller, I want to manage my imported products and see my earnings so that I can track my affiliate income.
- As a Customer, I want to purchase products from Co-seller shops seamlessly so that I can buy affiliate products like any other product.

### Key User Journeys

**Journey 1: Merchant Becomes Affiliator**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Clicks "Affiliate Network" in sidebar | System validates shop currency is USD |
| 2 | System | Shows empty state page with activation info | "Learn More" link to help center |
| 3 | Merchant | Clicks "Activate Affiliate Network" | Modal opens with explanation |
| 4 | Merchant | Confirms activation in modal | Affiliate activated, role set to Affiliator |
| 5 | System | Redirects to Settings page | Settings page with configuration options |
| 6 | Merchant | Configures: Category, Description, Commission Mode | Settings saved |
| 7 | Merchant | Selects products and sets commission rates | Products listed on marketplace |

**Journey 2: Merchant Becomes Co-seller**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Clicks "Affiliate Network" → "Marketplace" tab | Marketplace loads |
| 2 | Merchant | Browses and finds product to import | Product detail page shown |
| 3 | Merchant | Clicks "Add Product" | Settings modal opens (first time) |
| 4 | Merchant | Fills: Introduction & Strategy, Social Media Links, Active Channels | Co-seller profile created |
| 5 | System | Imports product to shop | Product available in storefront |
| 6 | System | Sets role to Co-seller | Role locked permanently |

**Journey 3: Customer Purchases Affiliate Product**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Views Co-seller's shop | Imported products displayed |
| 2 | Customer | Clicks imported product | PDP loads with full details |
| 3 | Customer | Adds to cart | Product added normally |
| 4 | Customer | Tries to apply coupon | Error: "Coupons not valid for affiliate products" |
| 5 | Customer | Completes checkout | Payment processed |
| 6 | System | Splits payment | Commission to Co-seller, remainder to Affiliator |
| 7 | System | Creates orders | Orders appear in both dashboards |

### Scope

**✅ In Scope:**

**Merchant (Affiliator) Side:**

- Activation page with explanation and modal
- Settings page (Category, Description, Commission Mode)
- Dashboard showing: Total Sales, Net Profit, Commissions Paid, Active Partners
- Partner list with: Name, Sales, Net Profit, Commission Paid, Imported Products, Status
- Partner detail page with 3 tabs:
    - Overview: Store info, Performance metrics, Social links
    - Products: Commission rate, Price, Items Sold, Net Revenue per product
    - Payouts: Order ID, Date, Amount, Status

**Merchant (Co-seller) Side:**

- Activation modal (Introduction & Strategy, Social Media, Active Channels)
- Dashboard with 3 tabs:
    - Overview: Total Sales, Earnings, Products Sold, Active Merchants, Merchant list
    - Products: Commission Rate, Price, Items Sold, Earnings, Status per product
    - Orders: Merchant, Date, Earnings, Commission Status
- Merchant detail page with info and performance
- Order detail page with full breakdown
- Cancel collaboration functionality

**Marketplace:**

- Main page: All products grid with search and filters
- Shop page: Shop profile with product listings
- Product page: Full details with import functionality

**❌ Out of Scope:**

- Multiple roles per merchant (Affiliator OR Co-seller, never both)
- Role switching after assignment
- Direct payment (uses shop credits only)
- Refund handling for affiliate orders
- Coupon codes on affiliate products
- Non-USD currency shops
- Imported products in inventory list
- Bulk import operations

### Acceptance Criteria

- [ ]  Both Affiliator and Co-seller shops must have USD currency
- [ ]  Merchant can only have ONE role: Affiliator OR Co-seller (exclusive)
- [ ]  Role is permanent and cannot be changed
- [ ]  Affiliator can set commission rates 1-99% for products
- [ ]  Co-seller can import products with one click
- [ ]  Imported products sync automatically with Affiliator's changes
- [ ]  Payment is split automatically on sale
- [ ]  Both parties see orders in their dashboards
- [ ]  Coupon codes are blocked for affiliate products

### Technical Notes

- Currency validation at activation/import time
- Real-time sync for product changes (price, commission, stock)
- Automatic credit distribution on payment confirmation
- Imported products are references, not copies
- Webhook or polling for product status updates

### Dependencies

- Shop currency system (must support USD validation)
- Credit/wallet system for payment distribution
- Product catalog system
- Order management system
- User role management system

---

## UI Flow (Source of Truth)

**Affiliator Flow:**

```
[Sidebar: Affiliate Network]
    ↓
[Activation Page] → [Modal: Confirm Activation]
    ↓
[Settings Page]
    ├─ General Info (URL, Category, Description)
    ├─ Commission Mode (Fixed/Manual)
    └─ Enable/Disable Toggle
    ↓
[Dashboard]
    ├─ Summary Cards (Sales, Profit, Commissions, Partners)
    └─ Partner List Table
        ↓
    [Click Partner]
        ↓
    [Partner Detail Page]
        ├─ Overview Tab
        ├─ Products Tab
        └─ Payouts Tab
```

**Co-seller Flow:**

```
[Sidebar: Affiliate Network] → [Marketplace Tab]
    ↓
[Marketplace Main Page]
    ├─ Product Grid
    ├─ Search & Filters
    ├─ [Click Product] → [Product Detail + Import]
    └─ [Click Shop] → [Shop Page]
    ↓
[First Import] → [Settings Modal]
    ├─ Introduction & Strategy
    ├─ Social Media Links
    └─ Active Channels
    ↓
[Dashboard]
    ├─ Overview Tab (Sales, Earnings, Merchants)
    ├─ Products Tab (Imported Products)
    └─ Orders Tab (Order History)
        ↓
    [Click Merchant] → [Merchant Detail Page]
    [Click Order] → [Order Detail Page]
```

---

## Attachments

- 📎 **Linked Request:** REQ-AFF-001
- 🎨 **Design:** [Figma - Affiliate Network]
- 🧪 **Test Cases:** TC-AFF-001 through TC-AFF-100

---

## Change Log

- 2026-02-22 — Behdad — Complete rewrite based on new structure (Merchant Dashboard, Partner tabs, Co-seller tabs, Marketplace)
- 2026-02-16 — Behdad — Added USD currency requirement for both parties
- 2026-02-01 — Behdad — Initial document creation

---