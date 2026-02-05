# Billing Page & Notifications

# Billing Page & Notifications

### Feature ID:

**[SUB-003]**

### Title:

**Billing Page, Plan Details & Notifications**

### Category:

**Subscription / Dashboard** | **Actors**: Merchant | **Channel**: Merchant Dashboard

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
Merchants need a dedicated space to view their billing history, understand their subscription events, download invoices, and manage their subscription. Additionally, they need timely warnings when their subscription is about to expire and renewal may fail.

**Desired Outcome:**

- Dedicated "Billing" page in merchant dashboard sidebar.
- Display billing log table with all subscription events.
- Provide Plan Details page for each billing entry.
- Send proactive notifications and display warning banner when subscription expiration is approaching and renewal is at risk.
- First-time Starter users redirected to Plans page.

---

### 2) **Scope – In / Out**

**In Scope:**

- **Billing Page in Sidebar:**
    - New navigation item "Billing" in merchant dashboard
    - Accessible to all merchants
    - **First-Time Starter Redirect:** If Starter user with no billing history visits → Redirect to Plans page
- **Billing Table:**
    - Columns: Plan Name | Event | Cycle | Date | Amount | Status
    - Status values: `paid`, `upcoming`, `cancel`
    - Event values: `trial`, `renew`, `upgrade`, `reactivate`, `new_subscription`
    - Click row → Opens Plan Details page
    - Starter plan does NOT create logs (no rows for Starter)
- **Plan Details Page:**
    - All data from billing table + Log ID
    - Payment Method (Card **** or Shop Credit) for renewals
    - Start Date and End Date
    - Invoice Download button
    - Notes field (for cancel, upgrade, special info)
    - **For Upgrades:** Credit from previous plan, amount paid
    - Cancel Subscription button (only for currently ACTIVE plan)
- **Warning Banner (Dashboard-wide):**
    - Location: Above breadcrumb, full-width
    - Appears 7 days before expiry if renewal at risk
    - Disappears when issue resolved
- **Expiration Warning Notifications:**
    - In-app + Email notifications
    - 7, 3, 1 days before expiration
    - Only when renewal is expected to fail
- **Invoice Download:**
    - PDF generation and download from Plan Details page

**Out of Scope:**

- Payment processing
- Plan selection UI (handled in Plans Page)
- Super Admin activation interface

---

### 3) **Key User Journeys**

---

**Journey 1: View Billing History**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Clicks "Billing" in sidebar | Billing page loads |
| 2 | Merchant | Views billing table | All billing logs displayed with columns: Plan Name, Event, Cycle, Date, Amount, Status |
| 3 | Merchant | Clicks on a row | Plan Details page opens |

---

**Journey 2: View Plan Details**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Clicks a billing row | Plan Details page opens |
| 2 | Merchant | Views all details | Shows: Log ID, Plan, Event, Cycle, Date, Amount, Status, Payment Method, Start/End Dates, Notes |
| 3 | Merchant | Clicks "Download Invoice" | PDF invoice downloads |
| 4 | Merchant | (If active plan) Clicks "Cancel" | Cancel confirmation modal |

---

**Journey 3: First-Time Starter User**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | New Merchant (Starter) | Clicks "Billing" in sidebar | System checks billing history |
| 2 | System | No billing logs found | Redirects to Plans page |
| 3 | Merchant | Views Plans page | Can select and purchase a plan |

---

**Journey 4: Cancel Subscription**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Opens Plan Details for active plan | Details page loads |
| 2 | Merchant | Clicks "Cancel Subscription" | Confirmation modal opens |
| 3 | Merchant | Confirms cancellation | Auto-renewal disabled |
| 4 | System | Updates logs | Upcoming log status → `cancel` |
| 5 | Merchant | Sees updated status | Plan shows "Expiring on [date]" |

---

**Journey 5: Warning Banner Display**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | System | 7 days before expiry, checks renewal status | Finds Stripe canceled OR insufficient credit |
| 2 | System | Displays warning banner | Banner appears above breadcrumb: "Your Pro plan expires in 7 days. Take action." |
| 3 | Merchant | Sees banner on any dashboard page | Banner persists until resolved |
| 4 | Merchant | Tops up credit or reactivates Stripe | Banner disappears |

---

**Journey 6: Receive Expiration Warning Notification**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | System | 7 days before expiry, checks renewal status | Finds renewal at risk |
| 2 | System | Sends notification | In-app + Email warning sent |
| 3 | Merchant | Sees notification | "Your Pro plan expires in 7 days" |
| 4 | Merchant | Takes action (top up credit or reactivate) | Subsequent warnings suppressed |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria | Pass/Fail |
| --- | --- | --- |
| **Billing Page** |  |  |
| BAC-1 | "Billing" appears in dashboard sidebar |  |
| BAC-2 | First-time Starter user is redirected to Plans page |  |
| BAC-3 | Billing table shows columns: Plan Name, Event, Cycle, Date, Amount, Status |  |
| BAC-4 | Table displays all billing logs (Starter excluded) |  |
| BAC-5 | Status column shows: `paid`, `upcoming`, or `cancel` |  |
| BAC-6 | Event column shows: `trial`, `renew`, `upgrade`, `reactivate`, `new_subscription` |  |
| BAC-7 | Clicking a row opens Plan Details page |  |
| **Plan Details Page** |  |  |
| BAC-8 | Shows all billing table data + Log ID |  |
| BAC-9 | Shows Payment Method (Card **** or Shop Credit) |  |
| BAC-10 | Shows Start Date and End Date |  |
| BAC-11 | Invoice Download button works |  |
| BAC-12 | Notes field displays special info (cancel reason, upgrade credit, etc.) |  |
| BAC-13 | For upgrades: shows credit from previous plan and amount paid |  |
| BAC-14 | Cancel button visible only for currently ACTIVE plan |  |
| BAC-15 | Cancel button updates upcoming log to `cancel` status |  |
| **Warning Banner** |  |  |
| BAC-16 | Banner appears above breadcrumb 7 days before expiry if renewal at risk |  |
| BAC-17 | Banner shows on all dashboard pages |  |
| BAC-18 | Banner disappears when issue resolved (credit topped up or Stripe reactivated) |  |
| **Notifications** |  |  |
| BAC-19 | Warning sent 7 days before expiry if renewal will fail |  |
| BAC-20 | Warning sent 3 days before expiry if still at risk |  |
| BAC-21 | Warning sent 1 day before expiry if still at risk |  |
| BAC-22 | Warnings NOT sent if renewal is expected to succeed |  |
| BAC-23 | Warnings suppressed if issue is resolved |  |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-02 | Behdad | Major update: Renamed to Billing Page, Added Plan Details page, Updated log statuses (paid/upcoming/cancel), Events (trial/renew/upgrade/reactivate/new_subscription), Warning Banner, First-time Starter redirect, Cancel updates upcoming log | Feature Overhaul |
| 2026-02-01 | Behdad | Initial document creation | Split from SUB-001, merged with notifications |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **Billing Page** | Dashboard page for viewing billing history and managing subscription |
| **Billing Log** | A record of a billing event (Starter plan does NOT create logs) |
| **Billing Status** | One of: `paid`, `upcoming`, `cancel` |
| **Billing Event** | One of: `trial`, `renew`, `upgrade`, `reactivate`, `new_subscription` |
| **Plan Details Page** | Detailed view of a single billing log entry |
| **Warning Banner** | Persistent alert above breadcrumb when renewal is at risk (7 days before expiry) |
| **Auto-Renewal** | Flag indicating if subscription will automatically renew |
| **Expiration Warning** | Notification sent when subscription is expiring and renewal may fail |
| **Invoice** | PDF document detailing a billing transaction |

---

### B2) Exhaustive Functional Logic

---

### **FL-1: Page Layout States**

| State | Condition | Display |
| --- | --- | --- |
| Redirect | First-time Starter user (no billing history) | Redirect to Plans page |
| Empty | Had billing history but now on Starter | "No active subscription" message |
| Active | Has active subscription | Billing table + current plan indicator |
| Trialing | In trial period | Trial badge + days remaining |
| Expiring | Canceled but not yet expired | "Expiring on [date]" indicator |

---

### **FL-1.5: Billing Table Columns**

| Column | Description | Example |
| --- | --- | --- |
| Plan Name | Name of the subscription plan | Pro, Premium |
| Event | Type of billing event | trial, renew, upgrade, reactivate, new_subscription |
| Cycle | Billing cycle duration | Monthly, Yearly, 3-Year |
| Date | Date of the event | Feb 1, 2026 |
| Amount | Charged or scheduled amount | $108.00 |
| Status | Current status of this log | `paid`, `upcoming`, `cancel` |

---

### **FL-2: Plan Details Page Content**

| Field | Description | Source |
| --- | --- | --- |
| Log ID | Unique identifier for billing log | billing_log.id |
| Plan Name | Name of the subscription plan | billing_log.plan_name |
| Event | Type of billing event | billing_log.event |
| Cycle | Billing cycle duration | billing_log.cycle |
| Date | Date of the event | billing_log.date |
| Amount | Charged or scheduled amount | billing_log.amount |
| Status | Current status of this log | billing_log.status |
| Payment Method | Card details or Shop Credit | billing_log.payment_method |
| Start Date | When this plan period starts | billing_log.start_date |
| End Date | When this plan period ends | billing_log.end_date |
| Notes | Special info (cancel reason, upgrade credit, etc.) | billing_log.notes |
| **For Upgrades:** |  |  |
| Credit from Previous Plan | Amount credited from unused time | billing_log.upgrade_credit |
| Amount Paid | Actual amount charged after credit | billing_log.amount_paid |

---

### **FL-3: Plan Details Page Actions**

| Action | Visibility | Behavior |
| --- | --- | --- |
| Download Invoice | Always (if status=paid) | Download PDF invoice |
| Cancel Subscription | Only for currently ACTIVE plan | Opens cancel confirmation modal |
| Back to Billing | Always | Navigate back to billing table |

---

### **FL-4: Cancel Subscription Logic**

```
ON CancelClick:
    SHOW ConfirmModal("Are you sure? Plan remains active until [expiry_date]")

ON ConfirmCancel:
    IF payment_method == 'Stripe':
        API.cancelStripeSubscription(subscription_id)

    SET subscription.auto_renew = false
    SET subscription.status = 'Expiring'

    // CRITICAL: Update the upcoming log status to 'cancel'
    FIND billing_log WHERE status = 'upcoming' AND plan = current_plan
    SET billing_log.status = 'cancel'
    SET billing_log.notes = "Canceled by user on [date]"

    LOG event = "Subscription canceled"
    SHOW success("Subscription will not renew after [expiry_date]")

```

---

### **FL-4.5: Warning Banner Logic**

```
// Runs on every dashboard page load
ON DashboardPageLoad:
    IF subscription.exists AND subscription.expiry_date <= now + 7days:

        renewal_at_risk = false

        IF payment_method == 'Stripe':
            IF stripe_subscription.cancel_at_period_end == true:
                renewal_at_risk = true

        ELSE IF payment_method == 'Shop Credit':
            IF shop.credit_balance < subscription.renewal_amount:
                renewal_at_risk = true

        IF renewal_at_risk:
            SHOW WarningBanner(
                location: "above-breadcrumb",
                message: "Your [Plan] subscription expires in [X] days. Take action.",
                dismissible: false  // Only disappears when issue resolved
            )
        ELSE:
            HIDE WarningBanner

```

---

### **FL-5: Billing Event Types**

| Event | Description | Creates Invoice |
| --- | --- | --- |
| `trial` | 30-day free trial started | No (amount=$0) |
| `renew` | Plan renewed (or scheduled to renew) | Yes (if status=paid) |
| `upgrade` | Upgraded from lower plan | Yes |
| `reactivate` | Returned from Starter to paid plan | Yes |
| `new_subscription` | Admin-activated subscription | Yes |

---

### **FL-6: Invoice Generation**

```
ON InvoiceDownloadClick(event_id):
    invoice = API.getInvoice(event_id)

    IF invoice.exists:
        DOWNLOAD as PDF
    ELSE:
        GENERATE PDF with:
            - Invoice Number
            - Date
            - Merchant Details
            - Plan Details
            - Amount Breakdown
            - Payment Method
            - Transaction ID
        SAVE and DOWNLOAD

```

---

### **FL-7: Expiration Warning Notification Logic**

**Daily Cron Job:**

```
FOR each subscription WHERE expiry_date BETWEEN now AND now+7days:

    should_warn = false

    IF payment_method == 'Stripe':
        IF stripe_subscription.cancel_at_period_end == true:
            should_warn = true

    ELSE IF payment_method == 'Shop Credit':
        IF shop.credit_balance < subscription.renewal_amount:
            should_warn = true

    IF should_warn:
        days_remaining = (expiry_date - now).days

        IF days_remaining == 7 AND NOT notification_sent_7d:
            SEND notification(7)
            SET notification_sent_7d = true

        ELSE IF days_remaining == 3 AND NOT notification_sent_3d:
            SEND notification(3)
            SET notification_sent_3d = true

        ELSE IF days_remaining == 1 AND NOT notification_sent_1d:
            SEND notification(1)
            SET notification_sent_1d = true

    ELSE:
        // Conditions resolved, reset flags to suppress future warnings
        RESET notification_sent flags

```

---

### **FL-8: Notification Content**

| Days | Subject | Body |
| --- | --- | --- |
| 7 | Your subscription expires soon | "Your [Plan] subscription will expire in 7 days. [Action needed: Add credit / Update payment method]" |
| 3 | Subscription expiring in 3 days | "Your [Plan] subscription expires in 3 days. To avoid interruption, please [action]." |
| 1 | Subscription expires tomorrow | "Your [Plan] subscription expires tomorrow. Take action now to continue your access." |

**Notification Channels:**

- In-app notification (bell icon)
- Email to billing email address

---

### **FL-9: Notification Suppression Conditions**

| Condition | Result |
| --- | --- |
| Stripe auto-renew reactivated | Suppress all pending warnings |
| Shop Credit topped up above renewal amount | Suppress all pending warnings |
| Plan upgraded to new plan | Cancel all warnings for old plan |
| Plan renewed successfully | Clear all warning flags |

---

### B3) Behavioral Flow

```
[Merchant Opens Subscription Tab]
    ↓
[Check Subscription Status]
    ├─ None/Starter → [Show Empty State]
    │                   └─ [View Plans] button
    └─ Has Subscription → [Show Full View]
                           ↓
[Render Header Section]
    ├─ Plan Name, Cycle, Amount
    ├─ Next Billing Date
    ├─ Payment Method
    └─ Auto-Renewal Status
    ↓
[Render Action Buttons]
    ├─ View Details → [Open Modal]
    ├─ Upgrade Plan → [Navigate to Plans]
    └─ Cancel → [Confirm Modal] → [Disable Renewal]
    ↓
[Render History Table]
    └─ For each event: Date, Type, Plan, Amount, [Invoice]
    ↓
[Background: Daily Cron]
    ↓
[Check Expiring Subscriptions]
    ├─ Renewal OK → [No Action]
    └─ Renewal At Risk → [Send Warning]

```

---

### B4) State Transitions

| Current State | Event | New State | Side Effects |
| --- | --- | --- | --- |
| Active | Cancel clicked | Expiring | Auto-renew disabled, log created |
| Expiring | Expiry date reached | Expired | Downgrade to Starter, log created |
| Active | Renewal successful | Active | Extend expiry, log created |
| Active | Upgrade completed | Active (new plan) | Old plan logged as "Upgraded" |
| Trialing | Trial ends | Active | First charge processed |

---

### B5) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** Invoice generation fails | Show error, allow retry |
| **EC-2:** Cancel fails (Stripe error) | Show error, keep current state |
| **EC-3:** User cancels then wants to reactivate | Must start new subscription |
| **EC-4:** Multiple subscriptions in history | Show all in chronological order |
| **EC-5:** Shop Credit topped up after 7-day warning | 3-day and 1-day warnings suppressed |
| **EC-6:** Stripe reactivated after warning | Subsequent warnings suppressed |
| **EC-7:** Upgrade during warning period | Old plan warnings canceled |
| **EC-8:** Notification email fails | Retry 3 times, log failure |
| **EC-9:** User has email notifications disabled | Still show in-app notification |

---

### B6) Data Requirements

**Subscription Object:**

| Field | Type | Description |
| --- | --- | --- |
| id | String | Unique identifier |
| plan_name | String | Starter, Pro, Premium, Enterprise |
| duration | Enum | Monthly, Yearly, 3-Year |
| status | Enum | Active, Trialing, Expiring, Expired |
| start_date | DateTime | Subscription start |
| end_date | DateTime | Subscription expiry |
| next_billing_date | DateTime | Next charge date |
| amount | Decimal | Renewal amount |
| payment_method | String | stripe_card, shop_credit |
| auto_renew | Boolean | Auto-renewal enabled |
| stripe_subscription_id | String | Stripe reference (if applicable) |

**Notification Tracking:**

| Field | Type | Description |
| --- | --- | --- |
| subscription_id | String | Related subscription |
| notification_7d_sent | Boolean | 7-day warning sent |
| notification_3d_sent | Boolean | 3-day warning sent |
| notification_1d_sent | Boolean | 1-day warning sent |

---

### B7) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| **Dashboard Tab** |  |  |
| TC-1 | Starter user opens tab | Empty state with "View Plans" |
| TC-2 | Pro user opens tab | Full details shown |
| TC-3 | Click View Details | Modal with all fields |
| TC-4 | Click Upgrade Plan | Navigate to Plans page |
| TC-5 | Cancel subscription | Auto-renew disabled, status "Expiring" |
| **History & Invoices** |  |  |
| TC-6 | View history | All events listed |
| TC-7 | Download invoice | PDF downloads |
| TC-8 | Event without invoice (trial) | No download icon |
| **Notifications** |  |  |
| TC-9 | 7 days to expiry, Stripe canceled | Warning notification sent |
| TC-10 | 7 days to expiry, Shop Credit low | Warning notification sent |
| TC-11 | 7 days to expiry, Stripe active | NO warning sent |
| TC-12 | Credit topped up after 7-day warning | 3-day warning NOT sent |
| TC-13 | Stripe reactivated after warning | Subsequent warnings NOT sent |
| TC-14 | Upgrade during warning period | Old plan warnings canceled |
| TC-15 | 3 days to expiry, still at risk | 3-day warning sent |
| TC-16 | 1 day to expiry, still at risk | 1-day warning sent |