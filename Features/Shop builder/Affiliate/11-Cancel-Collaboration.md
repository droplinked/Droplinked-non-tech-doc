# Cancel Collaboration

**Feature ID:** IAA-AFF-010  
**Category:** Affiliate | Shop Builder  
**Actors:** Merchant (Co-seller)  
**Channel:** Web  
**Status:** Defined  
**Owner:** Behdad

---

## Part 1: Human-Readable Spec

### Problem Statement

Co-sellers may want to stop working with a specific merchant and remove all imported products from that merchant. The Cancel Collaboration feature allows Co-sellers to end their partnership with a merchant, removing all associated products from their shop at once. This is useful when a Co-seller wants to discontinue selling a particular merchant's products.

### User Stories

- As a Co-seller, I want to cancel collaboration with a merchant so that I can stop selling their products.
- As a Co-seller, I want to remove all products from a merchant at once so that I don't have to remove them individually.
- As a Co-seller, I want to see what will be removed before confirming so that I don't accidentally remove products.
- As a Co-seller, I want to be able to re-import products later if I change my mind.

### Key User Journeys

**Journey 1: Cancel Collaboration**

| Step | Actor | Action | System Response |
|------|-------|--------|-----------------|
| 1 | Co-seller | On Merchant detail page | Views merchant information |
| 2 | Co-seller | Clicks "Cancel Collaboration" button | Confirmation modal opens |
| 3 | System | Shows products to be removed | Lists all products from this merchant |
| 4 | System | Shows warning | "X products will be removed from your shop" |
| 5 | Co-seller | Reviews list | Can see which products will be removed |
| 6 | Co-seller | Clicks "Confirm Cancel" | Collaboration cancelled |
| 7 | System | Removes all products | All imports from this merchant deleted |
| 8 | System | Updates merchant status | Status changes to "Inactive" |
| 9 | System | Shows success message | "Collaboration cancelled. X products removed." |
| 10 | Co-seller | Redirected to dashboard | Dashboard shows updated merchant list |

**Journey 2: Re-import After Cancellation**

| Step | Actor | Action | System Response |
|------|-------|--------|-----------------|
| 1 | Co-seller | Goes to Marketplace | Browses products |
| 2 | Co-seller | Finds product from cancelled merchant | Product detail shows |
| 3 | Co-seller | Clicks "Add Product" | Product imports normally |
| 4 | System | Re-imports product | Product available in shop again |
| 5 | System | Updates merchant status | Status changes back to "Active" |

### Scope

**✅ In Scope:**

- **Cancel Collaboration Button**
  - Available on Merchant detail page
  - Visible for active merchants

- **Confirmation Modal**
  - List of products to be removed
  - Product count
  - Warning message
  - Confirm and Cancel buttons

- **Removal Logic**
  - Set all import records to inactive
  - Products removed from storefront immediately
  - Historical sales data preserved
  - Merchant status updated to Inactive

- **Re-import Capability**
  - Can re-import products from cancelled merchant
  - Status updates back to Active on re-import

**❌ Out of Scope:**
- Partial cancellation (remove some products only)
- Cancellation reason collection
- Notification to merchant
- Refund handling
- Bulk cancellation across multiple merchants

### Acceptance Criteria

- [ ] "Cancel Collaboration" button visible on Merchant detail page for active merchants
- [ ] Clicking button opens confirmation modal
- [ ] Modal shows list of all products that will be removed
- [ ] Modal shows product count: "X products will be removed"
- [ ] Modal shows warning: "This action cannot be undone"
- [ ] Modal has "Confirm Cancel" and "Cancel" buttons
- [ ] Clicking "Confirm Cancel" removes all products from this merchant
- [ ] Products removed immediately from storefront
- [ ] Historical sales data preserved in orders
- [ ] Merchant status changes to "Inactive"
- [ ] Success message shown after cancellation
- [ ] User redirected to dashboard
- [ ] Can re-import products from cancelled merchant later
- [ ] Re-importing updates merchant status back to "Active"

### Technical Notes

- Set is_active = false on all imported_products records for this merchant
- Don't delete records - preserve historical data
- Update merchant status calculation
- Immediate effect - products disappear from storefront

### Dependencies

- Imported products data
- Merchant detail page
- Product removal API
- Status calculation logic

---

## UI Flow (Source of Truth)

```
[Co-seller Dashboard]
    ↓
[Click Merchant]
    ↓
[Merchant Detail Page]
    ┌───────────────────────────────────────────┐
    │ Merchant Information                      │
    │ Performance                              │
    │                                          │
    │ [Cancel Collaboration] ← Button visible   │
    │                                          │
    │ Products Tab | Orders Tab                 │
    └───────────────────────────────────────────┘
    ↓
[Click Cancel Collaboration]
    ↓
[Confirmation Modal]
    ┌───────────────────────────────────────────┐
    │ Cancel Collaboration                      │
    │                                          │
    │ You are about to cancel collaboration     │
    │ with [Merchant Name].                     │
    │                                          │
    │ The following X products will be          │
    │ removed from your shop:                   │
    │                                          │
    │ • Product A                              │
    │ • Product B                              │
    │ • Product C                              │
    │ • ...                                    │
    │                                          │
    │ ⚠️ This action cannot be undone.          │
    │ You can re-import these products later.   │
    │                                          │
    │ [Cancel]        [Confirm Cancel]          │
    └───────────────────────────────────────────┘
    ↓
[Click Confirm Cancel]
    ↓
[Products Removed]
[Merchant Status: Inactive]
[Success Message]
    ↓
[Redirect to Dashboard]
```

---

## Attachments

- 📎 **Linked Request:** REQ-AFF-011
- 🎨 **Design:** [Figma - Cancel Collaboration]
- 🧪 **Test Cases:** TC-AFF-131 through TC-AFF-140

---

## Change Log

- 2026-02-22 — Behdad — Created based on init.md requirements

---
