# Merchant Dashboard

**Feature ID:** IAA-AFF-002  
**Category:** Affiliate | Shop Builder  
**Actors:** Merchant (Affiliator)  
**Channel:** Web  
**Status:** Defined  
**Owner:** Behdad

---

## Part 1: Human-Readable Spec

### Problem Statement

Affiliators need visibility into their affiliate performance. They want to track which Co-sellers are selling their products, how much revenue they're generating, and what commissions they've paid. The dashboard provides a centralized view of all affiliate activity with the ability to drill down into individual partner performance.

### User Stories

- As an Affiliator, I want to see a summary of my affiliate performance so that I can understand my revenue from Co-sellers.
- As an Affiliator, I want to see a list of my Co-seller partners so that I can track who's selling my products.
- As an Affiliator, I want to view individual partner details so that I can analyze per-partner performance.
- As an Affiliator, I want to view my partners' shops and profiles so that I can understand my sales channels.

### Key User Journeys

**Journey 1: View Dashboard (First Time - Empty State)**

| Step | Actor | Action | System Response |
|------|-------|--------|-----------------|
| 1 | Affiliator | Opens Affiliate Dashboard | Dashboard loads |
| 2 | System | Shows empty state | "No partners yet. Your products are listed on the marketplace waiting for Co-sellers to import them." |
| 3 | Affiliator | Sees info message | Link to marketplace and settings |

**Journey 2: View Dashboard (With Data)**

| Step | Actor | Action | System Response |
|------|-------|--------|-----------------|
| 1 | Affiliator | Opens Affiliate Dashboard | Dashboard loads |
| 2 | System | Shows summary cards | Total Affiliate Sales, Net Profit, Commissions Paid, Active Partners |
| 3 | System | Shows partner table | List of all Co-sellers who imported products |
| 4 | Affiliator | Views partner metrics | Name, Sales, Net Profit, Commission Paid, Imported Products, Status |
| 5 | Affiliator | Clicks "View Profile" on a partner | Partner detail page opens |
| 6 | Affiliator | Clicks "View Store" on a partner | Partner's shop opens in new tab |

### Scope

**✅ In Scope:**

- **Summary Cards (Top Section)**
  - Total Affiliate Sales: Total revenue from all affiliate sales
  - Net Profit: Total revenue minus commissions paid
  - Commissions Paid: Total commissions paid to Co-sellers
  - Active Partners: Number of Co-sellers with imported products

- **Partner List Table**
  - Columns: Partner Name, Sales (amount), Net Profit, Commission Paid, Imported Products, Status
  - Actions per row: View Profile, View Store
  - Empty state for first-time users
  - Search functionality

- **Navigation to Partner Detail**
  - Click partner name or "View Profile" button
  - Opens Partner page with 3 tabs: Overview, Products, Payouts

**❌ Out of Scope:**
- Graphical charts/visualizations
- Export to CSV/PDF
- Real-time updates (polling acceptable)
- Date range filtering (moved to Partner detail)
- Commission rate editing from dashboard
- Bulk partner actions

### Acceptance Criteria

- [ ] Dashboard shows summary cards with key metrics
- [ ] Total Affiliate Sales = sum of all affiliate order totals
- [ ] Net Profit = Total Affiliate Sales - Commissions Paid
- [ ] Commissions Paid = sum of all commissions paid to Co-sellers
- [ ] Active Partners = count of unique Co-sellers with imported products
- [ ] Partner table shows all Co-sellers who imported products
- [ ] Each partner row shows: Name, Sales, Net Profit, Commission Paid, Imported Products count, Status
- [ ] Empty state shown when no partners exist
- [ ] "View Profile" button navigates to Partner detail page
- [ ] "View Store" button opens partner's shop in new tab
- [ ] All monetary values displayed in USD

### Technical Notes

- Dashboard data should load efficiently (consider caching)
- Partner list should support pagination if many partners
- Clicking partner row navigates to Partner detail page
- Real-time updates not required, page refresh acceptable

### Dependencies

- Affiliate order data
- Partner (Co-seller) data
- Product import records
- Shop profile data

---

## UI Flow (Source of Truth)

```
[Sidebar: Affiliate Network] → [Dashboard Tab]
    ↓
[Check Partners Exist]
    ├─ No Partners → [Empty State]
    │                   "No partners yet..."
    │                   [Link to Marketplace]
    │                   [Link to Settings]
    └─ Has Partners → [Dashboard with Data]
        ↓
    [Summary Cards]
        ├─ Total Affiliate Sales: $X,XXX.XX USD
        ├─ Net Profit: $X,XXX.XX USD
        ├─ Commissions Paid: $X,XXX.XX USD
        └─ Active Partners: XX
        ↓
    [Partner Table]
        Columns:
        ├─ Partner Name
        ├─ Sales
        ├─ Net Profit
        ├─ Commission Paid
        ├─ Imported Products
        ├─ Status
        └─ Actions: [View Profile] [View Store]
        ↓
    [Click Partner Row or View Profile]
        ↓
    [Partner Detail Page]
        ├─ Overview Tab
        ├─ Products Tab
        └─ Payouts Tab
```

---

## Attachments

- 📎 **Linked Request:** REQ-AFF-003
- 🎨 **Design:** [Figma - Merchant Dashboard]
- 🧪 **Test Cases:** TC-AFF-021 through TC-AFF-040

---

## Change Log

- 2026-02-22 — Behdad — Complete rewrite based on init.md requirements (Summary cards, Partner table)
- 2026-02-01 — Behdad — Initial document creation

---
