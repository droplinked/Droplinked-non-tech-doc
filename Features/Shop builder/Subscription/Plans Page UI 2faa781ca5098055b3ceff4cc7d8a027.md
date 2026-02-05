# Plans Page UI

# Plans Page UI

### Feature ID:

**[SUB-002]**

### Title:

**Plans Page UI (Public & Dashboard)**

### Category:

**Subscription / UI** | **Actors**: Merchant, Visitor | **Channel**: Web (Public + Dashboard)

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
Merchants and potential customers need a clear, visually appealing page to compare subscription plans, understand pricing, and make informed decisions. The page must be accessible both publicly (for marketing) and within the merchant dashboard (for upgrades).

**Desired Outcome:**

- Display all 4 plan tiers (Starter, Pro, Premium, Enterprise) in a clear comparison layout.
- Allow toggling between billing cycles (Monthly, Yearly, 3-Year).
- Show feature comparison table for each plan.
- Provide clear CTAs: Subscribe/Upgrade for purchasable plans, Contact Us for Enterprise.
- Consistent experience on both public and dashboard versions.

---

### 2) **Scope – In / Out**

**In Scope:**

- **Page Accessibility:**
    - Public URL (e.g., `/pricing`) - accessible without login
    - Dashboard URL (e.g., `/dashboard/plans`) - accessible when logged in
- **Plan Display:**
    - 4 columns: Starter, Pro, Premium, Enterprise
    - Pricing for each billing cycle
    - Highlight "Current Plan" badge for logged-in users
    - "Most Popular" badge on recommended plan
- **Billing Cycle Toggle:**
    - Monthly | Yearly | 3-Year
    - Prices update dynamically on toggle
    - Show savings for longer cycles (e.g., "$9/mo" for yearly)
- **Feature Comparison Table:**
    - List all features per plan
    - Checkmarks/X for feature availability
    - Limits displayed (e.g., "100 products" vs "Unlimited")
- **Call-to-Action Buttons:**
    - Starter: "Current Plan" or "Downgrade" (blocked)
    - Pro/Premium: "Subscribe" (new) or "Upgrade" (existing)
    - Enterprise: "Contact Us" → Modal
- **Enterprise Modal:**
    - Form: Name, Email, Company, Message
    - Submit sends email to support + creates inquiry in Super Admin

**Out of Scope:**

- Payment processing (handled by Stripe redirect)
- Plan activation logic
- Invoice management

---

### 3) **Key User Journeys**

---

**Journey 1: Visitor Views Public Pricing Page**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Visitor | Navigates to /pricing | Public plans page loads |
| 2 | Visitor | Views default (Yearly) pricing | 4 plans displayed with yearly prices |
| 3 | Visitor | Toggles to Monthly | Prices update to monthly rates |
| 4 | Visitor | Clicks "Get Started" on Pro | Redirected to signup flow |

---

**Journey 2: Merchant Views Plans in Dashboard**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Navigates to Plans page in dashboard | Plans page loads with current plan highlighted |
| 2 | Merchant | Sees "Current Plan" badge on Starter | Cannot click Starter |
| 3 | Merchant | Clicks "Upgrade" on Pro Yearly | Redirected to Stripe Checkout |

---

**Journey 3: Enterprise Inquiry**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Clicks "Contact Us" on Enterprise | Modal opens with form |
| 2 | Merchant | Fills form and submits | Success message shown |
| 3 | System | Sends email to support | Inquiry logged in Super Admin |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria | Pass/Fail |
| --- | --- | --- |
| BAC-1 | Plans page is accessible at public URL without login |  |
| BAC-2 | Plans page is accessible in merchant dashboard when logged in |  |
| BAC-3 | All 4 plans (Starter, Pro, Premium, Enterprise) are displayed |  |
| BAC-4 | Toggle switches between Monthly, Yearly, and 3-Year pricing |  |
| BAC-5 | Prices update dynamically when toggle changes |  |
| BAC-6 | Yearly/3-Year shows per-month equivalent (e.g., "$9/mo") |  |
| BAC-7 | Feature comparison table shows all plan differences |  |
| BAC-8 | Current plan shows "Current Plan" text instead of button |  |
| BAC-9 | **All lower-tier plan buttons are disabled** (downgrade blocked) |  |
| BAC-10 | Enterprise "Contact Us" opens modal with form |  |
| BAC-11 | Enterprise form submission sends email to support |  |
| BAC-12 | Pro/Premium buttons redirect to Stripe Checkout |  |
| BAC-13 | Downgrade is completely blocked (both plan tier and cycle) |  |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-02 | Behdad | Updated button states: Current plan shows "Current Plan" text, all lower plans show disabled buttons, downgrade completely blocked | Feature Update |
| 2026-02-01 | Behdad | Initial document creation | Split from SUB-001 |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **Public Plans Page** | Marketing-facing pricing page accessible without authentication |
| **Dashboard Plans Page** | Merchant-facing pricing page within authenticated dashboard |
| **Billing Cycle Toggle** | UI control to switch between Monthly/Yearly/3-Year views |

---

### B2) Exhaustive Functional Logic

---

### **FL-1: Pricing Display**

| Plan | Monthly | Yearly | 3-Year |
| --- | --- | --- | --- |
| Starter | Free | Free | Free |
| Pro | $10/mo | $108/yr ($9/mo) | $288 ($8/mo) |
| Premium | $30/mo | $324/yr ($27/mo) | $864 ($24/mo) |
| Enterprise | Custom | Custom | Custom |

---

### **FL-2: Toggle Behavior**

```
ON ToggleChange(cycle):
    SET selectedCycle = cycle
    FOR each plan in [Pro, Premium]:
        UPDATE displayPrice = pricing[plan][cycle]
        UPDATE perMonthText = calculatePerMonth(cycle, price)

```

**Per-Month Calculation:**

| Cycle | Formula |
| --- | --- |
| Monthly | Price as-is |
| Yearly | Price ÷ 12 |
| 3-Year | Price ÷ 36 |

---

### **FL-3: Button States (Dashboard - Logged In Users)**

| User State | Starter Button | Pro Button | Premium Button | Enterprise Button |
| --- | --- | --- | --- | --- |
| Merchant on Starter | "Current Plan" (text, no button) | "Upgrade" → Stripe | "Upgrade" → Stripe | "Contact Us" → Modal |
| Merchant on Pro | Disabled (grayed out) | "Current Plan" (text, no button) | "Upgrade" → Stripe | "Contact Us" → Modal |
| Merchant on Premium | Disabled (grayed out) | Disabled (grayed out) | "Current Plan" (text, no button) | "Contact Us" → Modal |
| Merchant on Enterprise | Disabled (grayed out) | Disabled (grayed out) | Disabled (grayed out) | "Current Plan" (text, no button) |
| Merchant in Trial | Same as active plan → "Current Plan" on trial plan, upgrades allowed |  |  |  |

**Key Rules:**

- **Current Plan:** Shows "Current Plan" text (NOT a button).
- **Lower Plans:** Button is **disabled** and grayed out (downgrade completely blocked).
- **Higher Plans:** Shows "Upgrade" button → redirects to Stripe.
- **Enterprise:** Always shows "Contact Us" unless already on Enterprise.

---

### **FL-3.5: Cycle Downgrade Blocking**

Downgrade is also blocked for billing cycle changes:

| Current Plan | Allowed Actions | Blocked Actions |
| --- | --- | --- |
| Pro Monthly | Upgrade to Pro Yearly, Pro 3-Year, Premium any | - |
| Pro Yearly | Upgrade to Pro 3-Year, Premium any | Pro Monthly (shorter cycle) |
| Pro 3-Year | Upgrade to Premium any | Pro Monthly, Pro Yearly |
| Premium Monthly | Upgrade to Premium Yearly, Premium 3-Year, Enterprise | - |
| Premium Yearly | Upgrade to Premium 3-Year, Enterprise | Premium Monthly |
| Premium 3-Year | Upgrade to Enterprise | Premium Monthly, Premium Yearly |

---

### **FL-4: Enterprise Modal Form**

| Field | Type | Required | Validation |
| --- | --- | --- | --- |
| Full Name | Text | Yes | Non-empty |
| Email | Email | Yes | Valid email format |
| Company Name | Text | No | - |
| Message | Textarea | No | Max 1000 chars |

**On Submit:**

```
IF form.isValid:
    API.sendEnterpriseInquiry(formData)
    SHOW successMessage("We'll contact you within 24 hours")
    CLOSE modal
ELSE:
    SHOW validation errors

```

---

### **FL-5: Feature Comparison Table**

| Feature | Starter | Pro | Premium | Enterprise |
| --- | --- | --- | --- | --- |
| Products | 10 | 100 | Unlimited | Unlimited |
| Storage | 1 GB | 10 GB | 50 GB | Custom |
| Custom Domain | ❌ | ✅ | ✅ | ✅ |
| Analytics | Basic | Advanced | Advanced | Custom |
| Support | Community | Email | Priority | Dedicated |
| API Access | ❌ | ✅ | ✅ | ✅ |
| White Label | ❌ | ❌ | ✅ | ✅ |

*(Feature list is example - actual features from product spec)*

---

### B3) Behavioral Flow

```
[Page Load]
    ↓
[Check: User logged in?]
    ├─ NO → [Show Public Version]
    │         └─ All buttons → "Get Started" (signup redirect)
    └─ YES → [Show Dashboard Version]
              ↓
[Fetch Current Subscription]
    ↓
[Highlight Current Plan with Badge]
    ↓
[Set Button States Based on Current Plan]
    ↓
[User Interacts]
    ├─ Toggle Cycle → Update Prices
    ├─ Click Upgrade → Redirect to Stripe
    └─ Click Contact Us → Open Modal

```

---

### B4) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** User on trial | Show "Trialing" badge, allow upgrade |
| **EC-2:** Enterprise inquiry fails | Show error, allow retry |
| **EC-3:** Stripe redirect fails | Show error message with retry option |
| **EC-4:** User already on highest plan | All upgrade buttons disabled |
| **EC-5:** Page load while subscription changing | Show loading state, refresh after |

---

### B5) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | Visit public /pricing | All 4 plans shown, no current plan badge |
| TC-2 | Toggle to Monthly | Prices update to monthly rates |
| TC-3 | Toggle to 3-Year | Prices update, show per-month savings |
| TC-4 | Logged in on Starter | Starter has "Current Plan" badge |
| TC-5 | Click Upgrade on Pro | Redirect to Stripe with Pro plan |
| TC-6 | Click Contact Us | Enterprise modal opens |
| TC-7 | Submit enterprise form | Email sent, success message shown |
| TC-8 | User on Premium clicks Pro | Button disabled (downgrade blocked) |