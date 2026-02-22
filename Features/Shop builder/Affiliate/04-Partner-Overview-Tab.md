# Partner Page - Overview Tab

**Feature ID:** IAA-AFF-003  
**Category:** Affiliate | Shop Builder  
**Actors:** Merchant (Affiliator)  
**Channel:** Web  
**Status:** Defined  
**Owner:** Behdad

---

## Part 1: Human-Readable Spec

### Problem Statement

Affiliators need detailed information about each Co-seller partner. The Overview tab provides comprehensive partner information including store details, performance metrics, and social media links. This helps Affiliators understand who is selling their products and how well they're performing.

### User Stories

- As an Affiliator, I want to view detailed information about a partner so that I can understand my sales channels.
- As an Affiliator, I want to see partner performance metrics so that I can track their sales contribution.
- As an Affiliator, I want to access partner's social media links so that I can see their marketing channels.
- As an Affiliator, I want to visit partner's store directly so that I can see how my products are presented.

### Key User Journeys

**Journey 1: View Partner Overview**

| Step | Actor | Action | System Response |
|------|-------|--------|-----------------|
| 1 | Affiliator | Clicks on partner from dashboard | Partner page loads, Overview tab active |
| 2 | System | Shows partner information | Store Name, Email, Join Date, Status, About |
| 3 | System | Shows performance metrics | Imported Products, Products Sold, Affiliate Sales, Net Revenue, Commission Paid |
| 4 | System | Shows social media links | Links to partner's social channels |
| 5 | Affiliator | Clicks "View Store" button | Partner's shop opens in new tab |

### Scope

**✅ In Scope:**

- **Partner Information Section**
  - Store Name
  - Email Address
  - Join Date
  - Status (Active/Inactive)
  - About (description)
  - View Store button

- **Performance Section**
  - Imported Products: Count of products imported by this partner
  - Products Sold: Count of items sold
  - Affiliate Sales: Total revenue from this partner
  - Net Revenue: Revenue after commission
  - Commission Paid: Total commission paid to this partner

- **Social Media Links**
  - Display partner's social media links (if provided)
  - Links open in new tabs

- **Navigation**
  - Tabs: Overview (active), Products, Payouts

**❌ Out of Scope:**
- Editing partner information (read-only)
- Messaging partner directly
- Commission rate negotiation
- Partner approval/rejection
- Historical graphs/charts
- Date range filtering (all-time data)

### Acceptance Criteria

- [ ] Partner page shows Store Name, Email Address, Join Date, Status, About
- [ ] About section shows partner's description/introduction
- [ ] Performance metrics show:
  - Imported Products: Number of products this partner imported
  - Products Sold: Total items sold by this partner
  - Affiliate Sales: Total $ amount of sales
  - Net Revenue: Total $ amount after commission
  - Commission Paid: Total $ commission paid
- [ ] All monetary values in USD
- [ ] Social media links displayed if partner provided them
- [ ] "View Store" button opens partner's shop in new tab
- [ ] Three tabs visible: Overview (active), Products, Payouts
- [ ] Clicking other tabs navigates to those sections

### Technical Notes

- Load partner data from Co-seller profile
- Calculate metrics from affiliate_orders table
- Join date = when partner first imported a product
- Status = Active if partner has imported products, Inactive if all removed

### Dependencies

- Partner profile data (Co-seller settings)
- Affiliate order data
- Social media links from Co-seller profile
- Shop URL for View Store button

---

## UI Flow (Source of Truth)

```
[Merchant Dashboard]
    ↓
[Click Partner Row]
    ↓
[Partner Page - Overview Tab]
    ┌─────────────────────────────────────┐
    │ Partner Information                   │
    │ ├─ Store Name: [Name]               │
    │ ├─ Email Address: [Email]           │
    │ ├─ Join Date: [Date]                │
    │ ├─ Status: [Active/Inactive]        │
    │ ├─ About: [Description]             │
    │ └─ [View Store] button              │
    │                                     │
    │ Performance                         │
    │ ├─ Imported Products: XX            │
    │ ├─ Products Sold: XX                │
    │ ├─ Affiliate Sales: $X,XXX.XX USD   │
    │ ├─ Net Revenue: $X,XXX.XX USD       │
    │ └─ Commission Paid: $X,XXX.XX USD   │
    │                                     │
    │ Social Media                        │
    │ ├─ [Instagram] [Twitter] etc.       │
    │                                     │
    │ Tabs:                               │
    │ [Overview] [Products] [Payouts]     │
    └─────────────────────────────────────┘
```

---

## Attachments

- 📎 **Linked Request:** REQ-AFF-004
- 🎨 **Design:** [Figma - Partner Overview]
- 🧪 **Test Cases:** TC-AFF-041 through TC-AFF-050

---

## Change Log

- 2026-02-22 — Behdad — Created based on init.md requirements

---
