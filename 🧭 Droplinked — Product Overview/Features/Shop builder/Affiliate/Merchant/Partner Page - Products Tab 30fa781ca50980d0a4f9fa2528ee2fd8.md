# Partner Page - Products Tab

**Feature ID:** IAA-AFF-004

**Category:** Affiliate | Shop Builder

**Actors:** Merchant (Affiliator)

**Channel:** Web

**Status:** Defined

**Owner:** Behdad

---

## Part 1: Human-Readable Spec

### Problem Statement

Affiliators need to see which products each Co-seller partner has imported and how those products are performing. The Products tab shows a detailed breakdown of products imported by a specific partner, including commission rates, sales data, and revenue metrics per product.

### User Stories

- As an Affiliator, I want to see which products a partner has imported so that I can understand their product selection.
- As an Affiliator, I want to see per-product sales performance so that I can identify top-selling products.
- As an Affiliator, I want to see net revenue per product so that I can track product profitability.
- As an Affiliator, I want to see commission rates per product so that I can verify correct commission application.

### Key User Journeys

**Journey 1: View Partner Products**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | On Partner page, clicks "Products" tab | Products tab loads |
| 2 | System | Shows products table | List of all products imported by this partner |
| 3 | System | Shows product metrics | Product name, Commission Rate, Price, Items Sold, Net Revenue |
| 4 | Affiliator | Reviews table | Can see which products are selling best |

### Scope

**✅ In Scope:**

- **Products Table**
    - Product name/image
    - Commission Rate (%)
    - Price (USD)
    - Items Sold (count)
    - Net Revenue (USD)
    - Sorted by Net Revenue (descending)
- **Summary Section (Optional)**
    - Total products imported by this partner
    - Total items sold across all products
    - Total net revenue from this partner
- **Navigation**
    - Tabs: Overview, Products (active), Payouts

**❌ Out of Scope:**

- Editing commission rates from this page
- Removing products from partner's shop
- Product detail drill-down
- Inventory/stock information
- Product edit actions
- Bulk actions

### Acceptance Criteria

- [ ]  Products tab shows table with columns:
    - Product (name + thumbnail)
    - Commission Rate (%)
    - Price (USD)
    - Items Sold (count)
    - Net Revenue (USD)
- [ ]  Table sorted by Net Revenue (highest first)
- [ ]  Empty state shown if partner has no sales yet
- [ ]  All monetary values in USD
- [ ]  Commission rate shows as percentage (e.g., "15%")
- [ ]  Product thumbnail visible in table
- [ ]  Table supports pagination if many products
- [ ]  Tabs: Overview, Products (active), Payouts

### Technical Notes

- Query affiliate_orders grouped by product
- Join with products table for names/images
- Join with affiliate_products for commission rates
- Calculate items sold = sum of quantities
- Calculate net revenue = sum of affiliator_amount

### Dependencies

- Affiliate order data
- Product catalog (for names/images)
- Affiliate product records (for commission rates)

---

## UI Flow (Source of Truth)

```
[Partner Page - Overview Tab]
    ↓
[Click Products Tab]
    ↓
[Partner Page - Products Tab]
    ┌─────────────────────────────────────────────┐
    │ Partner: [Name]                             │
    │                                             │
    │ Products Table                              │
    │ ┌─────────────┬──────────┬────────┬─────────┐│
    │ │ Product     │ Comm.    │ Price  │ Items   ││
    │ │             │ Rate     │ (USD)  │ Sold    ││
    │ │             │          │        │         ││
    │ │ [Img] Name  │ 15%      │ $29.99 │ 45      ││
    │ │ [Img] Name  │ 20%      │ $49.99 │ 32      ││
    │ │ ...         │ ...      │ ...    │ ...     ││
    │ └─────────────┴──────────┴────────┴─────────┘│
    │                                             │
    │ Tabs:                                       │
    │ [Overview] [Products] [Payouts]             │
    └─────────────────────────────────────────────┘
```

---

## Attachments

- 📎 **Linked Request:** REQ-AFF-005
- 🎨 **Design:** [Figma - Partner Products Tab]
- 🧪 **Test Cases:** TC-AFF-051 through TC-AFF-060

---

## Change Log

- 2026-02-22 — Behdad — Created based on [init.md](http://init.md/) requirements

---