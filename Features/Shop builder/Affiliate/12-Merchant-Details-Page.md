# Merchant Details Page (Co-seller View)

**Feature ID:** IAA-AFF-011  
**Category:** Affiliate | Shop Builder  
**Actors:** Merchant (Co-seller)  
**Channel:** Web  
**Status:** Defined  
**Owner:** Behdad

---

## Part 1: Human-Readable Spec

### Problem Statement

Co-sellers need detailed information about the merchants they import from. The Merchant Details page provides comprehensive information about a specific merchant, including their profile, performance metrics, and available products. This helps Co-sellers understand their product sources and make informed decisions about which merchants to work with.

### User Stories

- As a Co-seller, I want to view detailed information about a merchant so that I can understand my product source.
- As a Co-seller, I want to see merchant performance metrics so that I can track my earnings from them.
- As a Co-seller, I want to see all products I've imported from a merchant so that I can manage my portfolio.
- As a Co-seller, I want to browse more products from a merchant so that I can expand my offerings.
- As a Co-seller, I want to cancel collaboration with a merchant so that I can stop working with them.

### Key User Journeys

**Journey 1: View Merchant Details**

| Step | Actor | Action | System Response |
|------|-------|--------|-----------------|
| 1 | Co-seller | Clicks merchant from dashboard | Merchant detail page loads |
| 2 | System | Shows merchant information | Store Name, Email, Join Date, Status, About |
| 3 | System | Shows performance metrics | Imported Products, Products Sold, Sales, Earnings |
| 4 | System | Shows imported products list | Products with status and earnings |
| 5 | Co-seller | Reviews all information | Can see complete merchant profile |

**Journey 2: Browse More Products**

| Step | Actor | Action | System Response |
|------|-------|--------|-----------------|
| 1 | Co-seller | On Merchant page, clicks "View on Marketplace" | Marketplace shop page opens |
| 2 | Co-seller | Browses products | Sees all merchant's affiliate products |
| 3 | Co-seller | Imports new product | Product added to shop |

**Journey 3: Cancel Collaboration**

| Step | Actor | Action | System Response |
|------|-------|--------|-----------------|
| 1 | Co-seller | On Merchant page, clicks "Cancel Collaboration" | Confirmation modal opens |
| 2 | Co-seller | Confirms cancellation | All products removed |
| 3 | System | Redirects to dashboard | Dashboard shows updated list |

### Scope

**✅ In Scope:**

- **Merchant Information Section**
  - Store Name
  - Email Address
  - Join Date
  - Status (Active/Inactive)
  - About (description)
  - Social Media Links (if available)

- **Performance Section**
  - Imported Products: Count
  - Products Sold: Count
  - Sales: Total revenue
  - Earnings: Total commission

- **Imported Products List**
  - Product name/image
  - Commission Rate
  - Price
  - Items Sold
  - Earnings
  - Status

- **Actions**
  - View on Marketplace (opens shop page)
  - Cancel Collaboration
  - Import More Products (link to marketplace)

**❌ Out of Scope:**
- Editing merchant information (read-only)
- Messaging merchant directly
- Negotiating commission rates
- Requesting new products

### Acceptance Criteria

- [ ] Merchant detail page shows:
  - Store Name, Email Address, Join Date, Status, About
  - About section shows merchant's description
- [ ] Performance metrics show:
  - Imported Products: Number of products imported from this merchant
  - Products Sold: Total items sold
  - Sales: Total $ revenue
  - Earnings: Total $ commission
- [ ] Shows list of imported products with:
  - Product name, Commission Rate, Price, Items Sold, Earnings, Status
- [ ] "View on Marketplace" button opens merchant's marketplace shop
- [ ] "Cancel Collaboration" button opens confirmation modal
- [ ] All monetary values in USD
- [ ] Social media links displayed if merchant provided them
- [ ] Back button to return to dashboard

### Technical Notes

- Load merchant profile from affiliator data
- Calculate metrics from affiliate_orders
- Join with products for current imports
- Status based on affiliate_enabled and stock

### Dependencies

- Merchant profile data
- Affiliate order data
- Product import records
- Marketplace shop page
- Cancel collaboration feature

---

## UI Flow (Source of Truth)

```
[Co-seller Dashboard]
    ↓
[Click Merchant Row]
    ↓
[Merchant Detail Page]
    ┌───────────────────────────────────────────────┐
    │ ← Back to Dashboard                           │
    │                                               │
    │ Merchant Information                          │
    │ ├─ Store Name: [Name]                        │
    │ ├─ Email Address: [Email]                    │
    │ ├─ Join Date: [Date]                         │
    │ ├─ Status: [Active/Inactive]                  │
    │ ├─ About: [Description]                       │
    │ └─ Social Links: [Icons if available]         │
    │                                               │
    │ Performance                                   │
    │ ├─ Imported Products: XX                   │
    │ ├─ Products Sold: XX                         │
    │ ├─ Sales: $X,XXX.XX USD                      │
    │ └─ Earnings: $X,XXX.XX USD                   │
    │                                               │
    │ Actions                                       │
    │ [View on Marketplace]  [Cancel Collaboration] │
    │                                               │
    │ Imported Products                             │
    │ ┌─────────────┬────────┬───────┬───────────┐│
    │ │ Product     │ Comm.  │ Price │ Earnings  ││
    │ │             │ Rate   │       │ Status    ││
    │ │ [Img] Name  │ 15%    │$29.99 │ $XXX.00  ││
    │ │             │        │       │ Active    ││
    │ └─────────────┴────────┴───────┴───────────┘│
    └───────────────────────────────────────────────┘
```

---

## Attachments

- 📎 **Linked Request:** REQ-AFF-012
- 🎨 **Design:** [Figma - Merchant Details]
- 🧪 **Test Cases:** TC-AFF-141 through TC-AFF-155

---

## Change Log

- 2026-02-22 — Behdad — Created based on init.md requirements

---
