# Co-seller Dashboard - Orders Tab

**Feature ID:** IAA-AFF-009

**Category:** Affiliate | Shop Builder

**Actors:** Merchant (Co-seller)

**Channel:** Web

**Status:** Defined

**Owner:** Behdad

---

## Part 1: Human-Readable Spec

### Problem Statement

Co-sellers need to track their affiliate orders and earnings. The Orders tab shows a complete history of all orders placed for imported products, including order details, earnings per order, and commission payment status. This helps Co-sellers monitor their sales and reconcile their earnings.

### User Stories

- As a Co-seller, I want to see my order history so that I can track my sales.
- As a Co-seller, I want to see earnings per order so that I can verify my commissions.
- As a Co-seller, I want to see order details so that I can understand what was sold.
- As a Co-seller, I want to check commission status so that I can verify payment status.

### Key User Journeys

**Journey 1: View Orders**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Co-seller | On Dashboard, clicks "Orders" tab | Orders tab loads |
| 2 | System | Shows orders table | List of all affiliate orders |
| 3 | System | Shows order details | Merchant, Date, Earnings, Commission Status |
| 4 | Co-seller | Reviews table | Can see all order history |
| 5 | Co-seller | Clicks on order row | Order detail page opens |

### Scope

**✅ In Scope:**

- **Orders Table**
    - Order ID (link to detail)
    - Merchant name
    - Date (order date)
    - Earnings (USD) - commission amount
    - Commission Status (Paid)
    - Sorted by date (newest first)
- **Summary Section**
    - Total orders count
    - Total earnings
    - Optional date filter
- **Navigation**
    - Click order row → Order detail page
    - Tabs: Overview, Products, Orders (active)

**❌ Out of Scope:**

- Order status tracking (fulfillment handled by Affiliator)
- Customer information (not visible to Co-seller)
- Shipping details (handled by Affiliator)
- Refund status
- Export to CSV
- Bulk actions

### Acceptance Criteria

- [ ]  Orders tab shows table with columns:
    - Order ID (clickable link)
    - Merchant (name)
    - Date (formatted: "5 August, 2025")
    - Earnings (USD) - commission earned
    - Commission Status (Paid)
- [ ]  Table sorted by date (newest first)
- [ ]  Empty state shown if no orders yet
- [ ]  All monetary values in USD
- [ ]  Commission Status shows "Paid" for completed orders
- [ ]  Clicking Order ID navigates to Order detail page
- [ ]  Summary shows total orders and total earnings
- [ ]  Tabs: Overview, Products, Orders (active)

### Technical Notes

- Orders from affiliate_orders table where coseller_shop_id matches
- Join with merchants for names
- Earnings = commission_amount
- Status = "Paid" if payment_status = 'paid'
- Co-seller doesn't see customer info (privacy/security)

### Dependencies

- Affiliate order data
- Merchant data
- Order detail page

---

## UI Flow (Source of Truth)

```
[Co-seller Dashboard - Overview/Products Tab]
    ↓
[Click Orders Tab]
    ↓
[Orders Tab]
    ┌──────────────────────────────────────────────┐
    │ Summary: Total Orders: XX | Earnings: $XXX  │
    │                                              │
    │ Orders Table                                 │
    │ ┌──────────┬──────────┬──────────┬─────────┐│
    │ │ Order ID │ Merchant │ Date     │ Earnings││
    │ │          │          │          │ Status  ││
    │ │ #1305    │ Store A  │ 5 Aug 25 │ $15.00  ││
    │ │  [link]  │          │          │ Paid    ││
    │ │ #1304    │ Store B  │ 4 Aug 25 │ $22.50  ││
    │ │  [link]  │          │          │ Paid    ││
    │ │ ...      │ ...      │ ...      │ ...     ││
    │ └──────────┴──────────┴──────────┴─────────┘│
    │                                              │
    │ Tabs:                                        │
    │ [Overview] [Products] [Orders]               │
    └──────────────────────────────────────────────┘
```

---

## Attachments

- 📎 **Linked Request:** REQ-AFF-010
- 🎨 **Design:** [Figma - Co-seller Orders Tab]
- 🧪 **Test Cases:** TC-AFF-116 through TC-AFF-130

---

## Change Log

- 2026-02-22 — Behdad — Created based on [init.md](http://init.md/) requirements

---