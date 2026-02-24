# Co-seller Dashboard - Overview Tab

**Feature ID:** IAA-AFF-007

**Category:** Affiliate | Shop Builder

**Actors:** Merchant (Co-seller)

**Channel:** Web

**Status:** Defined

**Owner:** Behdad

---

## Part 1: Human-Readable Spec

### Problem Statement

Co-sellers need a comprehensive view of their affiliate performance. The Overview tab provides summary metrics of total sales, earnings, products sold, and active merchants, plus a list of all merchants they work with. This helps Co-sellers track their affiliate business performance.

### User Stories

- As a Co-seller, I want to see summary metrics of my affiliate performance so that I can track my earnings.
- As a Co-seller, I want to see a list of merchants I import from so that I can manage my partnerships.
- As a Co-seller, I want to view merchant details so that I can understand my product sources.
- As a Co-seller, I want to visit merchant shops so that I can explore more products to import.

### Key User Journeys

**Journey 1: View Dashboard (First Time - Empty State)**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Co-seller | Opens Affiliate Dashboard | Dashboard loads |
| 2 | System | Shows empty state | "Start by importing products from the marketplace" |
| 3 | Co-seller | Sees CTA button | "Browse Marketplace" button |
| 4 | Co-seller | Clicks button | Redirects to marketplace |

**Journey 2: View Dashboard (With Data)**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Co-seller | Opens Affiliate Dashboard | Dashboard loads, Overview tab active |
| 2 | System | Shows summary cards | Total Sales, Earnings, Products Sold, Active Merchants |
| 3 | System | Shows merchant table | List of all merchants with imported products |
| 4 | Co-seller | Views merchant metrics | Name, Sales, Earnings, Imported Products, Status |
| 5 | Co-seller | Clicks "View Details" on merchant | Merchant detail page opens |
| 6 | Co-seller | Clicks "View Store" on merchant | Merchant's shop opens in new tab |

### Scope

**✅ In Scope:**

- **Summary Cards (Top Section)**
    - Total Sales: Total revenue from all affiliate sales
    - Earnings: Total commission earned
    - Products Sold: Count of items sold
    - Active Merchants: Number of merchants with imported products
- **Merchant List Table**
    - Columns: Merchant Name, Sales, Earnings, Imported Products, Status
    - Actions per row: View Details, View Store
    - Empty state for first-time users
    - Search functionality
- **Navigation**
    - Tabs: Overview (active), Products, Orders

**❌ Out of Scope:**

- Graphical charts/visualizations
- Export to CSV/PDF
- Date range filtering (all-time data shown)
- Product import from dashboard
- Bulk actions on merchants

### Acceptance Criteria

- [ ]  Dashboard shows summary cards with key metrics
- [ ]  Total Sales = sum of all affiliate order totals (USD)
- [ ]  Earnings = sum of all commissions earned (USD)
- [ ]  Products Sold = sum of quantities sold across all orders
- [ ]  Active Merchants = count of unique merchants with imported products
- [ ]  Merchant table shows all merchants with imported products
- [ ]  Each merchant row shows: Name, Sales, Earnings, Imported Products count, Status
- [ ]  Empty state shown when no merchants imported from
- [ ]  "View Details" button navigates to Merchant detail page
- [ ]  "View Store" button opens merchant's marketplace shop in new tab
- [ ]  All monetary values in USD
- [ ]  Tabs: Overview (active), Products, Orders

### Technical Notes

- Dashboard data calculated from affiliate_orders table
- Join with merchants for names and details
- Status = Active if has current imports, Inactive if all removed
- Empty state shown if no imported products exist

### Dependencies

- Affiliate order data
- Merchant/Affiliator data
- Product import records

---

## UI Flow (Source of Truth)

```
[Sidebar: Affiliate Network] → [Dashboard Tab]
    ↓
[Overview Tab Active]
    ↓
[Check Merchants Exist]
    ├─ No Merchants → [Empty State]
    │                   "Start by importing..."
    │                   [Browse Marketplace]
    └─ Has Merchants → [Dashboard with Data]
        ↓
    [Summary Cards]
        ├─ Total Sales: $X,XXX.XX USD
        ├─ Earnings: $X,XXX.XX USD
        ├─ Products Sold: XXX
        └─ Active Merchants: XX
        ↓
    [Merchant Table]
        Columns:
        ├─ Merchant Name
        ├─ Sales
        ├─ Earnings
        ├─ Imported Products
        ├─ Status
        └─ Actions: [View Details] [View Store]
        ↓
    [Click Merchant Row or View Details]
        ↓
    [Merchant Detail Page]
```

---

## Attachments

- 📎 **Linked Request:** REQ-AFF-008
- 🎨 **Design:** [Figma - Co-seller Dashboard Overview]
- 🧪 **Test Cases:** TC-AFF-086 through TC-AFF-100

---

## Change Log

- 2026-02-22 — Behdad — Created based on [init.md](http://init.md/) requirements

---