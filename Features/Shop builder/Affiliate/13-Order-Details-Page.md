# Order Details Page (Co-seller View)

**Feature ID:** IAA-AFF-012  
**Category:** Affiliate | Shop Builder  
**Actors:** Merchant (Co-seller)  
**Channel:** Web  
**Status:** Defined  
**Owner:** Behdad

---

## Part 1: Human-Readable Spec

### Problem Statement

Co-sellers need detailed information about individual affiliate orders to understand their earnings and track their sales. The Order Details page shows comprehensive information about a specific order, including product details, financial breakdown, and payment information. Unlike the Affiliator view, Co-sellers don't see customer information since fulfillment is handled by the Affiliator.

### User Stories

- As a Co-seller, I want to view order details so that I can verify my earnings.
- As a Co-seller, I want to see product information so that I can understand what was sold.
- As a Co-seller, I want to see financial breakdown so that I can verify commission calculation.
- As a Co-seller, I want to see merchant information so that I can know who fulfilled the order.
- As a Co-seller, I want to see payment status so that I can confirm commission receipt.

### Key User Journeys

**Journey 1: View Order Details**

| Step | Actor | Action | System Response |
|------|-------|--------|-----------------|
| 1 | Co-seller | Clicks order from Orders tab | Order detail page loads |
| 2 | System | Shows order information | Order #, Date, Commission Status |
| 3 | System | Shows merchant information | Merchant Name |
| 4 | System | Shows payment details | Total Cart, My Earnings |
| 5 | System | Shows cart details | Products list with details |
| 6 | Co-seller | Reviews all information | Complete order breakdown visible |

### Scope

**✅ In Scope:**

- **Order Information Section**
  - Order Number
  - Date (formatted)
  - Commission Status (Paid)

- **Merchant Information Section**
  - Merchant Name
  - Link to merchant detail page

- **Payment Details Section**
  - Total Cart: Full order amount (for reference)
  - My Earnings: Commission earned

- **Cart Details Section**
  - Product list with:
    - Product title
    - Subtitle/variant info
    - Quantity
    - Unit Price (USD)
    - Commission Rate (%)
    - My Earnings per product

- **Navigation**
  - Back button to Orders tab
  - Link to Merchant detail page

**❌ Out of Scope:**
- Customer information (not visible to Co-seller)
- Shipping details (handled by Affiliator)
- Order status tracking (pending/shipped/delivered)
- Refund information
- Download invoice (Co-seller doesn't invoice)
- Order actions (edit, cancel)

### Acceptance Criteria

- [ ] Order detail page shows:
  - Order Number (e.g., "Order #1305")
  - Date (formatted: "5 August, 2025")
  - Commission Status: "Paid"
- [ ] Merchant Information shows:
  - Merchant Name
  - Link to view merchant details
- [ ] Payment Details shows:
  - Total Cart: $XXX.XX USD (full order amount)
  - My Earnings: $XXX.XX USD (commission)
- [ ] Cart Details shows product list with:
  - Product title and subtitle
  - Quantity (e.g., "10 Items")
  - Unit Price in USD
  - Commission Rate (e.g., "20%")
  - My Earnings per product
- [ ] "Show More" button if multiple products (expandable)
- [ ] "View Payment Details" link (if applicable)
- [ ] All monetary values in USD
- [ ] Back button to return to Orders tab

### Technical Notes

- Order data from affiliate_orders table
- Join with products for cart details
- Co-seller only sees their commission info
- Customer data excluded for privacy

### Dependencies

- Affiliate order data
- Product data
- Merchant data
- Merchant detail page

---

## UI Flow (Source of Truth)

```
[Co-seller Dashboard - Orders Tab]
    ↓
[Click Order ID]
    ↓
[Order Detail Page]
    ┌────────────────────────────────────────────┐
    │ ← Back to Orders                             │
    │                                              │
    │ Order #1305                                  │
    │                                              │
    │ Order Details                                │
    │ ├─ Order ID: Order #3121                    │
    │ ├─ Date: 5 August, 2025                     │
    │ └─ Commission Status: Paid                   │
    │                                              │
    │ Merchant Information                         │
    │ ├─ Merchant Name: [Store Name]              │
    │ └─ [View Merchant Details →]               │
    │                                              │
    │ Payment Details                              │
    │ ├─ Total Cart: $10.00 USD                   │
    │ ├─ My Earnings: $10.00 USD                  │
    │ └─ [View Payment Details]                    │
    │                                              │
    │ Cart Details                                 │
    │ ┌───────────────────────────────────────┐  │
    │ │ Product Title (Required)             │  │
    │ │ Subtitle here                        │  │
    │ │ 10 Items                             │  │
    │ │                                       │  │
    │ │ Unit Price: $10.50 USD               │  │
    │ │ Commission Rate: 20%                 │  │
    │ │ My Earnings: $10.50 USD              │  │
    │ │                                       │  │
    │ │ Product Title 2 (Required)           │  │
    │ │ Subtitle here                        │  │
    │ │ ...                                   │  │
    │ │                                       │  │
    │ │ [Show More]                          │  │
    │ └───────────────────────────────────────┘  │
    └────────────────────────────────────────────┘
```

---

## Attachments

- 📎 **Linked Request:** REQ-AFF-013
- 🎨 **Design:** [Figma - Order Details]
- 🧪 **Test Cases:** TC-AFF-156 through TC-AFF-170

---

## Change Log

- 2026-02-22 — Behdad — Created based on init.md requirements

---
