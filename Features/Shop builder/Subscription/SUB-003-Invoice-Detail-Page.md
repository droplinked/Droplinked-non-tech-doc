# Invoice Detail Page

**Feature ID:** SUB-003  
**Category:** Subscription / Shop Builder  
**Actors:** Merchant  
**Channel:** Merchant Dashboard  
**Status:** Writing Spec (Draft)  
**Owner:** Behdad  

---

## Part 1: Human-Readable Spec

### Problem Statement

Merchants need a detailed view of individual invoices to review billing information, download receipts, and manage their subscription (including cancellation for active plans). This page provides complete transparency for each billing transaction.

### User Stories

- As a **merchant**, I want to view complete details of a specific invoice so that I can verify billing information.
- As a **merchant**, I want to download my invoice as PDF so that I can keep records for accounting.
- As a **merchant with an active subscription**, I want to cancel my plan so that it won't auto-renew.
- As a **merchant**, I want to see the subscription period clearly so that I know when my plan expires.

### Key User Journeys

**Journey 1: View Invoice Details**

1. Merchant clicks an invoice row from Billing page
2. Invoice Detail page loads
3. Page displays:
   - Invoice ID (e.g., INV-1304)
   - Subscription Overview section
   - Payment Details section
   - Subscription Period
   - Amount breakdown

**Journey 2: Download Invoice PDF**

1. Merchant views invoice details
2. Clicks "Download PDF" button
3. System generates and downloads PDF invoice
4. PDF includes all invoice details

**Journey 3: Cancel Active Subscription**

1. Merchant views invoice for currently active plan
2. Clicks three-dot menu (⋮) in top-right
3. Selects "Cancel" from dropdown
4. Confirmation modal opens with message:
   - "Are you sure? Plan remains active until [expiry_date]"
5. Merchant confirms cancellation
6. Auto-renewal disabled
7. Plan continues until end of current period
8. Success message displayed

### Scope

**✅ In Scope:**
- Invoice detail page accessible from Billing page
- Complete invoice information display
- Subscription Overview section with all plan details
- Payment Details section with billing method
- Subscription Period display (start date - end date)
- Amount breakdown (Subtotal, Total Amount)
- Invoice ID display (e.g., INV-XXXX)
- Download PDF functionality
- Three-dot menu for active plans
- Cancel subscription option in menu
- Cancel confirmation modal
- Auto-renewal disabling on cancel
- Plan remains active until expiry after cancel

**❌ Out of Scope:**
- Edit invoice details
- Refund processing
- Upgrade from this page (redirects to Pricing)
- Multiple invoice bulk actions

### Acceptance Criteria

- [ ] Invoice Detail page accessible by clicking any invoice from Billing page
- [ ] Page displays Invoice ID prominently (e.g., INV-1304)
- [ ] Subscription Overview section shows:
  - [ ] Current Plan name
  - [ ] Cycle (Monthly/Yearly/3-Year)
  - [ ] Transaction type (New/Renew/Upgrade)
- [ ] Payment Details section shows:
  - [ ] Due Date
  - [ ] Payment Method (Card **** or Shop Credit)
  - [ ] Status (Paid/Upcoming/Cancel)
  - [ ] Billing Email
  - [ ] Auto-Renewal status (Active/Inactive)
- [ ] Subscription Period displayed: [Start Date] - [End Date]
- [ ] Amount breakdown shows:
  - [ ] Subtotal
  - [ ] Total Amount with currency (USD)
- [ ] "Download PDF" button generates and downloads PDF invoice
- [ ] Three-dot menu (⋮) visible only for currently active plan invoices
- [ ] Menu contains "Cancel" option
- [ ] Clicking Cancel opens confirmation modal
- [ ] Modal displays: "Are you sure? Plan remains active until [expiry_date]"
- [ ] Confirming cancellation disables auto-renewal
- [ ] Plan remains active until original expiry date after cancellation
- [ ] Success message shown after cancellation
- [ ] Back button returns to Billing page

### Technical Notes

- Invoice data fetched by ID from billing API
- PDF generation using existing invoice service
- Cancel action updates subscription auto_renew flag to false
- Cancel only available when:
  - Invoice is for current active plan
  - Auto-renewal is currently enabled
  - Plan is not already canceled

### Dependencies

- Billing page (SUB-002)
- Subscription management API
- Invoice PDF generation service
- User authentication

---

## UI Flow (Source of Truth)

```
[Click Invoice Row from Billing Page]
    ↓
[Invoice Detail Page]
    ↓
[Choose Action]
    ├─ [Back] → [Billing Page]
    ├─ [Download PDF] → [PDF Downloaded]
    └─ [Three-dot Menu - If Active Plan]
              ↓
        [Select Cancel]
              ↓
        [Confirmation Modal]
              ├─ [Confirm] → [Auto-Renewal Disabled] → [Success Message]
              └─ [Cancel] → [Close Modal]
```

---

## Attachments

- 📎 **Linked Request:** Subscription Management
- 📄 **Spec Document:** Features/Shop builder/Subscription/
- 🎨 **Design:** Figma - Invoice Detail Page
- 🧪 **Test Cases:** TBD

---

## Change Log

- 2026-02-22 — Behdad — Initial spec creation
- 2026-02-22 — Behdad — Added cancel subscription flow with three-dot menu
