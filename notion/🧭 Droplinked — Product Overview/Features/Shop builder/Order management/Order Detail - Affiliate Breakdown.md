# Order Detail - Affiliate Breakdown

**Feature ID:** IAA-ORD-001

**Category:** Order Management | Shop Builder

**Actors:** Merchant (Affiliator)

**Channel:** Web

**Status:** Defined

**Owner:** Behdad

---

## Part 1: Human-Readable Spec

### Problem Statement

When an order contains affiliate products (products sold by a Co-seller), the Affiliator (merchant who owns the products) needs to see a detailed breakdown of the order including the commission paid to the Co-seller and their own net profit. This information is critical for financial tracking and reconciliation.

**Key Point:** The commission rate shown is the rate that was active when the customer started checkout, not the current rate (in case the merchant has since changed it).

### User Stories

- As an Affiliator, I want to see the Co-seller (Publisher) information for an order so that I know who sold my product.
- As an Affiliator, I want to see the commission breakdown so that I understand how much was paid to the Co-seller vs my net profit.
- As an Affiliator, I want to see the commission rate used at the time of purchase so that I can verify correct commission calculation.

### Key User Journeys

**Journey 1: View Affiliate Order Details**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | Opens an order from Order History | Order detail page loads |
| 2 | System | Detects affiliate product in order | Shows standard order details + Affiliate Section |
| 3 | System | Shows Affiliate Section | Publisher name, Total Value, Commission Rate Used, Publisher Revenue, Net Profit |
| 4 | Affiliator | Reviews breakdown | Understands revenue split |

**Journey 2: Commission Rate Change Scenario**

**Scenario:**
```
⏰ 10:00 AM - Customer starts checkout for an affiliate product (Commission: 15%)
           → Order stores: affiliate_commission_rate = 15%

⏰ 10:05 AM - Affiliator changes commission rate from 15% to 20%
           → New rate applies to future orders only

⏰ 10:10 AM - Customer completes purchase
           → Order uses 15% (rate at checkout start)
           → Publisher Revenue: $24.60 (15% of $164)
           → Net Profit: $139.40 (after deducting commission)

⏰ 10:15 AM - Another customer starts checkout
           → Order stores: affiliate_commission_rate = 20%
```

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Affiliator | Views order placed at 10:10 AM | Sees "Commission Rate Used: 15%" |
| 2 | Affiliator | Notes current rate is 20% | Understands this order used old rate |
| 3 | System | Shows clear indication | "Rate at time of purchase: 15% (current rate: 20%)" |

---

### Scope

**✅ In Scope:**

- **Affiliate Section in Order Detail**
    - Publisher (Co-seller) shop name and logo
    - Total Order Value (full amount paid by customer)
    - **Commission Rate Used** (rate at checkout start time)
    - Publisher Revenue (amount paid to Co-seller)
    - Net Profit (Affiliator's revenue after commission)
    - Visual indication if current rate differs from purchase-time rate
- **Order Item Level Details**
    - Per-item commission breakdown
    - Product name, quantity, unit price
    - Commission % and amount per item
- **Historical Tracking**
    - Commission rate preserved per order
    - Commission calculation stored immutably

**❌ Out of Scope:**

- Real-time commission adjustments (orders use snapshot rate)
- Commission dispute handling
- Refund commission recalculation

---

### Acceptance Criteria

- [ ]  Affiliate Section appears in order detail when order contains affiliate products
- [ ]  Section shows Publisher (Co-seller) shop name and logo
- [ ]  Section shows Total Order Value in USD
- [ ]  **Section shows Commission Rate Used (rate at time of checkout start)**
- [ ]  **Section shows Publisher Revenue (Total Value × Commission Rate)**
- [ ]  **Section shows Net Profit (Total Value - Publisher Revenue - fees)**
- [ ]  **If current commission rate differs from purchase-time rate, show both: "Rate at purchase: 15% (current: 20%)"**
- [ ]  Per-item breakdown shows individual commission amounts
- [ ]  All monetary values in USD
- [ ]  Commission calculations are immutable and stored in order data
- [ ]  Commission rate changes do NOT affect already-placed orders

---

### Technical Notes

- Affiliate data stored in `order.affiliate_commission_rate` (snapshot at checkout start)
- Publisher revenue calculated as: `total_paid × affiliate_commission_rate`
- Net profit calculated as: `total_paid - publisher_revenue - platform_fees - payment_fees`
- Order items include: `affiliate_commission_rate` and `affiliate_commission_amount` per item
- Historical preservation: Commission rate locked at order creation time
- **When merchant changes commission rate: Only affects NEW orders, existing orders remain unchanged**

---

### Dependencies

- Order management system
- Affiliate order data structure
- Commission calculation engine
- Shop profile data (for publisher info)

---

## UI Flow (Source of Truth)

```
[Order Management] → [Order List]
    ↓
[Click Order with Affiliate Product]
    ↓
[Order Detail Page]
    ┌──────────────────────────────────────────────┐
    │ Order #12345                                  │
    │ Status: Completed                             │
    │ Date: Feb 25, 2026                            │
    │                                               │
    │ Customer Information                          │
    │ [Standard customer details...]                │
    │                                               │
    │ ┌────────────────────────────────────────────┐│
    │ │ Affiliate Order Details                   ││
    │ │                                            ││
    │ │ Publisher: [Logo] Behdad Mansouri Shop    ││
    │ │                                            ││
    │ │ Total Order Value: $164.00 USD             ││
    │ │                                            ││
    │ │ Commission Rate Used: 15%                 ││
    │ │ (Current rate: 20%) ← If different        ││
    │ │                                            ││
    │ │ Publisher Revenue: $24.60 USD             ││
    │ │                                            ││
    │ │ Net Profit: $139.40 USD                   ││
    │ │ (After fees: $138.15 USD)                 ││
    │ └────────────────────────────────────────────┘│
    │                                               │
    │ Product List                                  │
    │ ┌──────────┬────────┬─────┬──────────────┐  │
    │ │ Product  │ Price  │ Qty │ Commission   │  │
    │ │          │        │     │ (15%)        │  │
    │ │ T-Shirt  │ $82.00 │  2  │ $24.60       │  │
    │ └──────────┴────────┴─────┴──────────────┘  │
    │                                               │
    │ Payment Details                               │
    │ [Standard payment info...]                    │
    └──────────────────────────────────────────────┘
```

---

## Attachments

- 📎 **Linked Request:** REQ-ORD-001
- 🎨 **Design:** [Figma - Order Detail Affiliate Section]
- 🧪 **Test Cases:** TC-ORD-001 through TC-ORD-020

---

## Change Log

- 2026-02-25 — Behdad — Initial creation: Affiliate order breakdown for merchant view with commission rate snapshot logic

---
