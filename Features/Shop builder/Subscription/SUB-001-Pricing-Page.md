# Pricing Page (Public & Dashboard)

**Feature ID:** SUB-001  
**Category:** Subscription / Shop Builder  
**Actors:** Merchant, Visitor (Public), Merchant (Dashboard)  
**Channel:** Web (Public + Dashboard)  
**Status:** Writing Spec (Draft)  
**Owner:** Behdad  

---

## Part 1: Human-Readable Spec

### Problem Statement

Merchants and potential customers need a clear, accessible page to view and compare subscription plans. The page must be available both publicly (for marketing and new user acquisition) and within the dashboard (for existing merchants to upgrade). Users need to see plan features, pricing, and their current active plan status to make informed decisions.

### User Stories

- As a **visitor**, I want to view pricing plans so that I can understand what features are available before signing up.
- As a **logged-in merchant**, I want to see all available plans and which one I currently have active so that I can upgrade if needed.
- As a **merchant on a lower-tier plan**, I want to select a higher plan and be redirected to payment so that I can upgrade my subscription.
- As a **visitor selecting a plan**, I want to be redirected to the registration page so that I can create an account and complete the purchase.

### Key User Journeys

**Journey 1: Visitor Views Public Pricing Page**

1. Visitor navigates to public pricing page (e.g., `/pricing`)
2. Page displays 4 plan columns: Starter, Pro, Premium, Enterprise
3. Visitor sees monthly/yearly/3-year toggle with pricing
4. Visitor sees feature comparison table
5. Visitor clicks "Select" on any paid plan → Redirected to `/register`

**Journey 2: Logged-in Merchant Views Dashboard Pricing**

1. Merchant clicks "Plans" in sidebar (or views public pricing while logged in)
2. Page displays same 4 plans with current plan highlighted
3. Active plan shows "Current Plan" badge (not clickable)
4. Higher-tier plans show "Select" button → Redirects to Stripe checkout
5. Lower-tier plans show "Unavailable" (downgrade blocked)

**Journey 3: Successful Payment Flow**

1. Merchant completes Stripe payment
2. System activates the selected plan immediately
3. Merchant redirected back to previous page
4. Success modal displays: "[Plan Name] has been activated successfully"

**Journey 4: Enterprise Plan Inquiry**

1. User clicks "Contact Us" on Enterprise column
2. Modal opens with form: Name, Email, Company, Message
3. Form submission sends inquiry to support

### Scope

**✅ In Scope:**
- Public pricing page accessible without login (`/pricing`)
- Dashboard pricing page accessible when logged in
- 4 plan tiers: Starter, Pro, Premium, Enterprise
- 3 billing cycles: Monthly, Yearly, 3-Year
- Plan comparison table with all features
- Active plan highlighting for logged-in users
- "Current Plan" badge for active subscription
- "Select" button for purchasable plans (redirects to Stripe)
- "Unavailable" state for lower-tier plans (downgrade blocked)
- "Contact Us" modal for Enterprise plan
- Success modal after payment completion
- Redirect to `/register` for non-logged users selecting a plan

**❌ Out of Scope:**
- Trial periods (removed per business decision)
- Payment processing logic (handled by Stripe)
- Invoice generation (handled in Billing page)
- Subscription cancellation (handled in Invoice Detail page)

### Acceptance Criteria

- [ ] Public pricing page loads at `/pricing` without requiring authentication
- [ ] Dashboard pricing page accessible via sidebar when logged in
- [ ] All 4 plans (Starter, Pro, Premium, Enterprise) are displayed in columns
- [ ] Toggle exists to switch between Monthly, Yearly, and 3-Year pricing
- [ ] Prices update dynamically when toggle changes
- [ ] Feature comparison table shows all plan differences with checkmarks
- [ ] Logged-in users see "Current Plan" badge on their active plan
- [ ] Current plan button is disabled/not clickable
- [ ] Lower-tier plans show "Unavailable" (grayed out, non-clickable)
- [ ] Higher-tier plans show "Select" button that redirects to Stripe
- [ ] Non-logged users clicking "Select" are redirected to `/register`
- [ ] After successful Stripe payment, user returns to page with success modal
- [ ] Success modal shows: "[Plan Name] has been activated successfully"
- [ ] Enterprise "Contact Us" opens modal with form
- [ ] Enterprise form submission sends email to support team

### Technical Notes

- Use existing Stripe integration for checkout redirects
- Plan data should come from backend API
- Current plan status should be fetched from user subscription endpoint
- Pricing configuration:
  - Starter: Free
  - Pro: $25/mo | $270/yr | $675/3yr
  - Premium: $50/mo | $540/yr | $1,350/3yr
  - Enterprise: Custom (contact sales)

### Dependencies

- Stripe payment integration
- User authentication system
- Subscription management API
- Email service for Enterprise inquiries

---

## UI Flow (Source of Truth)

```
[Pricing Page]
    ↓
[Check: User logged in?]
    ├─ NO (Public) → [Select Plan] → [/register]
    │                  └─ [Contact Us] → [Enterprise Modal]
    └─ YES (Dashboard)
           ↓
    [View Current Plan Highlighted]
           ↓
    [Choose Action]
           ├─ [Current Plan] → (No Action)
           ├─ [Unavailable Plan] → (Blocked)
           ├─ [Select Higher Plan] → [Stripe Checkout] → [Return] → [Success Modal]
           └─ [Contact Us Enterprise] → [Enterprise Modal] → [Submit]
```

---

## Attachments

- 📎 **Linked Request:** Subscription Management
- 📄 **Spec Document:** Features/Shop builder/Subscription/
- 🎨 **Design:** Figma - Pricing Page
- 🧪 **Test Cases:** TBD

---

## Change Log

- 2026-02-22 — Behdad — Initial spec creation based on requirements
- 2026-02-22 — Behdad — Consolidated public and dashboard pricing into single spec
