# Partner Page - Payouts Tab

**Feature ID:** IAA-AFF-005

**Category:** Affiliate | Shop Builder

**Actors:** Merchant (Affiliator)

**Channel:** Web

**Status:** Defined

**Owner:** Behdad

---

## Part 1: Human-Readable Spec

### Problem Statement

Affiliators need to track all payouts (commissions paid) to each Co-seller partner. The Payouts tab shows a transaction history of all commission payments to a specific partner, providing transparency and accounting records for the affiliate relationship.

### User Stories

- As an Affiliator, I want to see a history of payouts to a partner so that I can track commission payments.
- As an Affiliator, I want to see payout amounts and dates so that I can reconcile payments.
- As an Affiliator, I want to see payout status so that I can verify completed payments.
- As an Affiliator, I want to search/filter payouts so that I can find specific transactions.

### Key User Journeys

**Journey 1: View Partner Payouts**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | On Partner page, clicks "Payouts" tab | Payouts tab loads |
| 2 | System | Shows payouts table | List of all payouts to this partner |
| 3 | System | Shows payout details | Order ID, Date, Amount, Status |
| 4 | Affiliator | Reviews table | Can see all commission payments |
| 5 | Affiliator | Uses search (optional) | Filters payouts by Order ID |

### Scope

**✅ In Scope:**

- **Payouts Table**
    - Order ID (link to order)
    - Date (payment date)
    - Amount (USD) - commission paid
    - Status (Paid/Pending)
    - Sorted by date (newest first)
- **Summary Section**
    - Total payouts count
    - Total amount paid
    - Date range filter (optional)
- **Search**
    - Search by Order ID
- **Navigation**
    - Tabs: Overview, Products, Payouts (active)

**❌ Out of Scope:**

- Manual payout actions
- Payout scheduling
- Payout method details
- Export to CSV/PDF
- Detailed payout breakdown per order
- Bulk payout operations

### Acceptance Criteria

- [ ]  Payouts tab shows table with columns:
    - Order ID (clickable link)
    - Date (formatted: "5 August, 2025")
    - Amount (USD) - commission amount
    - Status (Paid, Pending)
- [ ]  Table sorted by date (newest first)
- [ ]  Search box filters by Order ID
- [ ]  Order ID links to order detail page
- [ ]  Empty state shown if no payouts yet
- [ ]  All monetary values in USD
- [ ]  Status shows "Paid" for completed payments
- [ ]  Summary shows total payouts and total amount
- [ ]  Tabs: Overview, Products, Payouts (active)

### Technical Notes

- Payouts = commission transactions from affiliate_orders
- Status is "Paid" if order is complete
- Date is order creation date
- Amount is commission_amount from order
- Search filters on order_id field

### Dependencies

- Affiliate order data
- Order status system
- Search functionality

---

## UI Flow (Source of Truth)

```
[Partner Page - Overview/Products Tab]
    ↓
[Click Payouts Tab]
    ↓
[Partner Page - Payouts Tab]
    ┌─────────────────────────────────────────┐
    │ Partner: [Name]                         │
    │                                         │
    │ Summary:                                │
    │ Total Payouts: XX | Total Amount: $X.XX │
    │                                         │
    │ [Search by Order ID...]                 │
    │                                         │
    │ Payouts Table                           │
    │ ┌─────────────┬──────────────┬─────────┐│
    │ │ Order ID    │ Date         │ Amount  ││
    │ │             │              │ Status  ││
    │ │ #1305       │ 5 Aug 2025   │ $15.00  ││
    │ │   [link]    │              │ Paid    ││
    │ │ #1304       │ 4 Aug 2025   │ $22.50  ││
    │ │   [link]    │              │ Paid    ││
    │ │ ...         │ ...          │ ...     ││
    │ └─────────────┴──────────────┴─────────┘│
    │                                         │
    │ Tabs:                                   │
    │ [Overview] [Products] [Payouts]         │
    └─────────────────────────────────────────┘
```

---

## Attachments

- 📎 **Linked Request:** REQ-AFF-006
- 🎨 **Design:** [Figma - Partner Payouts Tab]
- 🧪 **Test Cases:** TC-AFF-061 through TC-AFF-070

---

## Change Log

- 2026-02-22 — Behdad — Created based on [init.md](http://init.md/) requirements

---