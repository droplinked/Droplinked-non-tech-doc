# Subscription Plans Management

# Subscription Plans Management

> 📁 This is the Overview Document. Detailed specifications are split into:
> 
> - **[SUB-002] Plans Page UI** - Public & Dashboard plans display
> - **[SUB-003] Subscription Dashboard & Notifications** - Dashboard tab, history, invoices, expiration warnings

# Subscriptions

### Feature ID:

**[SUB-001]**

### Title:

**Subscription Plans Management (Overview)**

### Category:

**[Credits & Payouts / Admin & Compliance]** | **Actors**: [Merchant, Super Admin] | **Channel**: [Merchant Dashboard, Super Admin]

### Status:

**[Defined]**

### Owner:

[Behdad]

---

### Related Documents

| Document | Feature ID | Description |
| --- | --- | --- |
| [Plans Page UI](https://www.notion.so/Subscription/Plans%20Page%20UI%20260a781ca5098089a895e1ef82aa1378.md) | SUB-002 | Plans display, pricing toggle, feature comparison |
| [Subscription Dashboard & Notifications](https://www.notion.so/Subscription/Subscription%20Dashboard%20%26%20Notifications%20260a781ca5098089a895e1ef82aa1379.md) | SUB-003 | Dashboard tab, history, invoices, expiration warnings |

---

### 1) **Summary**

**Problem/Value:**
Merchants require a tiered subscription system to access advanced features/limits. The system must support flexible durations (Monthly, Yearly, 3-Year), precise billing cycles, and seamless upgrades. Payment processing is offloaded to Stripe (hosted checkout), while retaining support for manual administrative activation via Shop Credit. Transparency is key, requiring detailed logs, downloadable PDF invoices, and clear prorated calculations for upgrades.

**Desired Outcome:**

- **Tiered Plans:** Starter (Default/Free), Pro, Premium, Enterprise.
- **Flexible Durations:** Monthly, Yearly, 3-Year.
- **Stripe Integration:** Payments handled via Stripe Hosted Checkout; no on-site sensitive data handling.
- **Immediate Activation:** Plans activate immediately upon purchase with no trial period.
- **Smart Renewals:** Stripe auto-renew for card purchases; Shop Credit deduction for Admin-activated plans.
- **Upgrade Logic:** Immediate upgrades with prorated credit for unused time on previous plans. Previous paid logs remain unchanged.
- **Billing Page:** Dedicated billing page with comprehensive transaction logs showing all billing events.
- **Upcoming Billing:** From the moment a plan activates, the next renewal is logged with "upcoming" status for full transparency.
- **Visibility:** Dedicated "Billing" page with history, status, and PDF invoices. Separate "Plan Details" page for each subscription event.

---

### 2) **Scope – In / Out**

**In:**

- **Plan Types:** Starter, Pro, Premium, Enterprise.
- **Durations:** Monthly, Yearly, 3-Year.
- **Purchase Flow:** Stripe Hosted Checkout (Redirection).
- **Enterprise Flow:** "Request Info" modal → Super Admin inquiry (email support).
- **Admin Activation:** Super Admin activates plans; renewals deduct from Merchant Shop Credit.
- **Immediate Activation:** Plans activate immediately upon purchase. No trial period is offered.
- **Upgrade/Downgrade:**
    - Upgrades: Immediate, prorated credit applied, previous logs remain as "paid", upcoming log becomes "cancel".
    - Downgrades: **BLOCKED entirely** - both plan tier and cycle/date downgrades are prohibited.
- **Billing Page:** Dedicated page with transaction log table.
- **Billing Log Statuses:** `paid`, `upcoming`, `cancel`.
- **Billing Events:** `trial`, `renew`, `upgrade`, `reactivate`, `new_subscription`.
- **Plan Details Page:** Detailed view of each billing entry with invoice download.
- **Invoices:** PDF generation and download for every transaction.
- **Logging:** Comprehensive "Billing Log" visible to merchant (Starter plan excluded).
- **Expiration Warning Banner:** Dashboard banner 7 days before expiry if renewal at risk.

**Out:**

- **Enterprise Card Purchase:** Enterprise is manual/admin activation only.
- **Mid-term Downgrades:** Refunds or partial downgrades are strictly reduced.
- **Multi-currency display:** USD only.

---

### 3) **Key User Journeys**

**Journey 1: Public/Dashboard Plan Purchase (Immediate Activation)**

- Merchant views the Plans page (Public or Dashboard).
- Selects "Pro - Yearly" ($108).
- Redirected to Stripe Checkout.
- Card is charged immediately for the full plan price.
- Redirected back to Dashboard → "Success Modal" (static, shown once).
- **Outcome:**
    - Pro Yearly plan activates immediately for full 1-year duration.
    - **Billing Log Created:**
        - Log 1: `event=new_subscription`, `status=paid`, `amount=$108`, `date=Day 1`
        - Log 2: `event=renew`, `status=upcoming`, `amount=$108`, `date=Day 1 + 1 year` (scheduled)

---

### 📊 **Example: Ali's First Subscription Journey (Immediate Activation)**

| Day | Event | Log Entry | Status | Amount | Notes |
| --- | --- | --- | --- | --- | --- |
| 1 | Ali signs up & buys Pro Yearly | New Subscription | paid | $108 | Card charged immediately |
| 1 | System creates next billing | Pro Yearly Renewal | upcoming | $108 | Scheduled for Day 366 |
| 366 | Auto-renewal | Pro Yearly Renewal | paid | $108 | If not canceled |
| 366 | System creates next billing | Pro Yearly Renewal | upcoming | $108 | Scheduled for Day 731 |

**Visual Timeline:**

```
Day 1                              Day 366                           Day 731
   │                                  │                                 │
   ▼                                  ▼                                 ▼
[New Subscription]───────────────►[Auto-Renewal $108]─────────────►[Auto-Renewal $108]
   │                                  │                                 │
 Card Charged $108              1 Year PAID                      1 Year PAID
   │                                  │                                 │
   Log: new_subscription/paid         Log: renew/paid                   Log: renew/paid
   Log: renew/upcoming                Log: renew/upcoming               Log: renew/upcoming

```

**Journey 2: Upgrading an Active Plan**

- Merchant is on "Pro - Yearly" (Month 6). Decides to upgrade to "Premium - Yearly".
- System calculates "Unused Value" of currently active Pro plan.
- **Checkout:** Price = (Premium Price) - (Unused Pro Value).
- Merchant pays the difference.
- **Outcome:**
    - Pro plan's **paid log remains unchanged** (historical record).
    - Pro plan's **upcoming log changes to `cancel`**.
    - New **Premium paid log** created with upgrade event.
    - New **Premium upcoming log** created for next renewal.
    - Invoice generated showing credit from Pro and amount paid.

---

### 📊 **Example: Upgrade from Pro to Premium (Month 6)**

**Scenario:** Ali bought Pro Yearly on Jan 1, 2026. Upgrades to Premium Yearly on July 1, 2026.

**Calculation:**

- Pro Yearly paid: $108
- Time used: 6 months (181 days)
- Time remaining: 6 months (184 days)
- Credit = $108 × (184/365) = **$54.41**
- Premium Yearly: $324
- **Amount to Pay: $324 - $54.41 = $269.59**

| Before Upgrade |  |  |  |  |
| --- | --- | --- | --- | --- |
| Log ID | Event | Plan | Status | Amount |
| 001 | renew | Pro Yearly | paid | $108 |
| 002 | renew | Pro Yearly | upcoming | $108 |

| After Upgrade |  |  |  |  |
| --- | --- | --- | --- | --- |
| Log ID | Event | Plan | Status | Amount |
| 001 | renew | Pro Yearly | paid | $108 |
| 002 | renew | Pro Yearly | **cancel** | $108 |
| 003 | upgrade | Premium Yearly | paid | $269.59 |
| 004 | renew | Premium Yearly | upcoming | $324 |

**Journey 3: Enterprise Request**

- Merchant selects "Enterprise".
- "Request Info" modal appears.
- Merchant submits request.
- **Outcome:** Notification sent to Super Admin/Support Email. No automatic activation.

**Journey 4: Cancellation**

- Merchant clicks "Cancel Subscription" → Auto-renewal disabled.
- Plan remains active until Expiry Date.
- **Upcoming log → `cancel`**, plan status → "Expiring".
- On expiry → Starter (no new logs).

**Journey 5: Super Admin Activation**

- Super Admin activates plan for a Shop → Immediate activation.
- Renewal via Shop Credit deduction.

**Journey 6: Failed Renewal**

- Card fails or Shop Credit insufficient → Upcoming log → `cancel` → Downgrade to Starter.

**Journey 7: Reactivation**

- After cancel/expiry, user buys new plan → **NO trial**, event=`reactivate`.

---

> See [SUB-003] for Billing Page details, Plan Details page, and Invoice download flow.
> 

---

### 4) **Business Acceptance Criteria (BAC)**

**Purchase & Activation**

- **BAC 1:** User is redirected to Stripe for payments; success callback correctly activates the plan locally. → Pass/Fail.
- **BAC 2:** Plans activate immediately upon successful payment; NO trial period is offered. → Pass/Fail.
- **BAC 3:** First billing creates `new_subscription` event log with `status=paid` and full plan amount. → Pass/Fail.
- **BAC 4:** Upcoming renewal log is created immediately with `status=upcoming`. → Pass/Fail.

**Upgrade & Downgrade**

- **BAC 6:** Upgrades calculate prorated credit correctly (down to the day) and deduct it from the new plan cost. → Pass/Fail.
- **BAC 7:** On upgrade: previous paid log unchanged, previous upcoming log becomes `cancel`, new paid and upcoming logs created. → Pass/Fail.
- **BAC 8:** Downgrades are **completely blocked** - both plan tier and cycle/date downgrades prohibited. → Pass/Fail.

**Billing Logs & Statuses**

- **BAC 9:** Billing log statuses are: `paid`, `upcoming`, `cancel`. → Pass/Fail.
- **BAC 10:** Billing events are: `trial`, `renew`, `upgrade`, `reactivate`, `new_subscription`. → Pass/Fail.
- **BAC 11:** From moment of plan activation, an `upcoming` log is created for next renewal. → Pass/Fail.
- **BAC 12:** Starter plan does NOT create billing logs. → Pass/Fail.

**Billing Page**

- **BAC 13:** Billing page shows table with: Plan Name, Event, Cycle, Date, Amount, Status. → Pass/Fail.
- **BAC 14:** Billing page redirects Starter-only users (first visit) to Plans page. → Pass/Fail.
- **BAC 15:** Clicking a billing row opens Plan Details page. → Pass/Fail.

**Cancellation & Renewal**

- **BAC 16:** When user cancels, upcoming log status changes to `cancel` (not deleted). → Pass/Fail.
- **BAC 17:** When renewal fails, upcoming log becomes `cancel` and user is downgraded to Starter. → Pass/Fail.
- **BAC 18:** Canceled plan remains active until original expiry date. → Pass/Fail.

**Other**

- **BAC 19:** Enterprise plan is locked behind a "Contact/Request" flow (cannot buy directly). → Pass/Fail.
- **BAC 20:** Admin-activated plans renew via "Shop Credit" deduction; Logs appear in Shop Credit history. → Pass/Fail.
- **BAC 21:** PDF Invoices are generated and downloadable for every transaction. → Pass/Fail.
- **BAC 22:** Default plan for new signups is "Starter" (Unlimited duration, limited features, NO billing logs). → Pass/Fail.

**Warnings & Notifications**

- **BAC 23:** Warning banner appears in dashboard (above breadcrumb) 7 days before expiry if renewal at risk. → Pass/Fail.
- **BAC 24:** Warning banner disappears when issue is resolved (credit added or Stripe reactivated). → Pass/Fail.
- **BAC 25:** Merchants receive expiration warning notifications 7, 3, and 1 days before subscription ends (only if auto-renewal will fail). → Pass/Fail.
- **BAC 26:** Warning notifications are NOT sent if renewal is expected to succeed (Stripe active OR sufficient Shop Credit). → Pass/Fail.

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-16 | Behdad | REMOVED: 30-day trial period. All plans now activate immediately upon purchase. Updated all related logic, BAC, and examples. | Business decision - Remove trial period |
| 2026-02-02 | Behdad | Major update: New Trial logic (30-day free then charge), Billing page, Log statuses (paid/upcoming/cancel), Events (trial/renew/upgrade/reactivate/new_subscription), Upgrade logic (previous paid unchanged, upcoming→cancel), Downgrade fully blocked, Failed renewal handling, Reactivation flow, Warning banner, Plan details page, Comprehensive examples with tables | Feature Overhaul |
| 2026-02-01 | Behdad | Added Expiration Warning Notifications (R24-R29, BAC9-10, T6-T9) | New Feature Request |
| 2026-01-31 | AI | Updated Pricing, Upgrade Logic, Stripe Flow, Enterprise Request, Logs | Feature Update Request |

---

---

# [PART 2: AI REFERENCE SPEC]

*This section must contain the "No-Guessing" logic for developers and AI tools.*

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **Plan** | A tier of service (Starter, Pro, Premium, Enterprise). |
| **Cycle/Duration** | The billing period (Monthly, Yearly, 3-Year). |
| **Trial** | A 30-day FREE period applied *once per merchant* upon their FIRST subscription purchase. Card is registered but NOT charged. Full plan duration starts after trial ends. |
| **Active Subscription** | A paid plan with a future expiration date. |
| **Billing Log** | A record of every billing event. Starter plan does NOT create logs. |
| **Billing Status** | One of: `paid` (completed transaction), `upcoming` (scheduled future billing), `cancel` (will not process). |
| **Billing Event** | One of: `trial`, `renew`, `upgrade`, `reactivate`, `new_subscription`. |
| **Upcoming Log** | A log entry created immediately when a plan activates, representing the NEXT scheduled billing. |
| **Shop Credit Renewal** | A payment method where the system deducts the plan cost from the merchant's internal wallet (DropLink Credit) instead of a card. Used for Admin-activated plans. |
| **Prorated Credit** | The monetary value of the *unused time* remaining on a current plan, applied as a discount to an upgrade. Cannot be negative or refunded as cash. |
| **Reactivation** | When a merchant who previously had a paid plan (canceled/expired) purchases a new plan. NO trial given. |
| **Warning Banner** | A persistent alert above the breadcrumb in dashboard when renewal is at risk (7 days before expiry). |

### B2) Detailed Functional Rules (Numbered)

**Pricing Configuration**

- **R1.** **Starter:** Free, Default.
- **R2.** **Pro:** $25/mo | $270/yr | $675/3yr.
- **R3.** **Premium:** $50/mo | $540/yr | $1,350/3yr.
- **R4.** **Enterprise:** Custom Pricing. Activation via Super Admin only.

**Activation Logic (Immediate - No Trial)**

- **R5.** All plan purchases activate immediately upon successful payment. **NO trial period is offered.**
- **R6.** On successful Stripe checkout:
    - Card is **charged immediately** for the full plan price.
    - A `new_subscription` event log is created with `status=paid`.
    - An `upcoming` log is created for the next renewal date.
- **R7.** Reactivation (after cancel/expiry) follows the same immediate activation flow as new subscriptions.

**Stripe & Payment Flow**

- **R11.** All user-initiated purchases must redirect to Stripe Hosted Checkout.
- **R12.** On successful Stripe checkout:
    - Create `new_subscription` event log with `status=paid`.
    - Create `upcoming` log for next renewal.
    - If upgrade: Trigger Upgrade Logic (R16-R20).
    - If reactivation: Create `reactivate` event log + upcoming log.
    - If renewal: Update upcoming log to `paid`, create new upcoming log.

---

### 📊 **Billing Log Quick Reference**

| Status | Meaning |
| --- | --- |
| `paid` | Transaction completed |
| `upcoming` | Scheduled for future |
| `cancel` | Will not process |

| Event | When Used |
| --- | --- |
| `new_subscription` | New subscription purchase |
| `renew` | Renewal (auto or manual) |
| `upgrade` | Plan upgrade |
| `reactivate` | Return from Starter |
| `new_subscription` | Admin activation |

> See [SUB-003] for full Billing Table structure and Plan Details page fields.
> 

**Upgrade Logic (Immediate)**

- **R16.** Upgrade Definition: Moving to a plan with a **higher tier** OR **longer duration** that results in a higher value.
- **R17.** **Credit Calculation:**
$$ Credit = \frac{PaidAmount}{TotalDurationDays} \times RemainingDays $$
- **R18.** **Payment Amount:** $Payment = NewPlanPrice - Credit$.
- **R19.** If $Payment \le 0$, payment is $0 (edge case handling).
- **R20.** **Log Handling on Upgrade:**
    1. Previous plan's `paid` log: **UNCHANGED**
    2. Previous plan's `upcoming` log: Status → `cancel`
    3. New plan: Create `upgrade` event log with `status=paid`
    4. New plan: Create `upcoming` log for next renewal
    5. Invoice generated with credit breakdown

**Downgrade & Cancellation**

- **R21.** **Downgrade: COMPLETELY BLOCKED**
    - Cannot downgrade to a lower-tier plan (e.g., Premium → Pro).
    - Cannot downgrade to a shorter cycle (e.g., Yearly → Monthly).
    - Both plan and cycle downgrades are **prohibited at all times**.
- **R22.** **Cancellation:**
    - Sets `auto_renew = false`.
    - Plan remains valid until `expiry_date`.
    - **Upcoming log status changes to `cancel`** (log is NOT deleted).
    - Plan status changes to `Expiring`.
- **R23.** Expired plans revert automatically to **Starter** (no logs created for Starter).

**Failed Renewal**

- **R24.** If renewal fails at due date (card declined, insufficient Shop Credit):
    - **Upcoming log status changes to `cancel`**.
    - Merchant is immediately downgraded to **Starter**.
    - Notification sent about failed renewal.
    - No new logs created (Starter has no billing logs).

**Admin Activation & Shop Credit**

- **R25.** Super Admin activates plan -> `payment_method = 'Shop Credit'`.
- **R26.** Admin-activated plans: **NO trial**, immediate activation.
- **R27.** On Admin activation:
    - Create `new_subscription` event log with `status=paid`.
    - Create `upcoming` log for next renewal.
- **R28.** Renewal logic for 'Shop Credit' method:
    - Check `Shop Balance`.
    - If `Balance >= PlanPrice`: Deduct amount, Log in Credit History, Log in Subscription History, Extend Plan, Create new upcoming log.
    - If `Balance < PlanPrice`: Fail renewal, **upcoming log → cancel**, Downgrade to Starter, Notify Merchant.
- **R29.** Logs for Admin-activated plans must appear in **both** Subscription Logs and Wallet/Credit Logs (with correct flags).

**Enterprise Flow**

- **R30.** Enterprise requires "Contact Sales" modal → No direct checkout.

**Expiration Warning**

- **R31.** Warning banner + notifications at 7, 3, 1 days before expiry if renewal at risk.
- **R32.** Warnings suppressed when issue resolved.

> See [SUB-003] for detailed notification logic and banner behavior.
> 

### B3) UI/UX States & Triggers

> Note: Detailed UI specifications are in:
> 
> - **[SUB-002]** Plans Page UI - Button states, pricing display, toggle behavior
> - **[SUB-003]** Billing Page & Notifications - Table structure, Plan Details page, Warning Banner

**Key UI Rules (Summary):**

- Plans Page: Current plan shows text (not button), lower plans disabled
- Billing Page: Starter users with no history → redirect to Plans
- Warning Banner: Above breadcrumb, 7 days before expiry if at risk

### B4) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC1. Stripe Payment Fail on Renewal** | Upcoming log → `cancel`, downgrade to Starter immediately. |
| **EC2. Zero Cost Upgrade** | If Credit >= New Price, charge $0, still create logs normally. |
| **EC3. Double Purchase Attempt** | FE should block button or redirect to "Change Plan" logic. |
| **EC4. Shop Credit Topped Up After Warning** | 3-day and 1-day warnings are suppressed, banner disappears. |
| **EC5. Stripe Subscription Reactivated** | Subsequent warnings suppressed, banner disappears. |
| **EC6. Subscription Upgraded During Warning Period** | All pending warnings for old plan are canceled, old upcoming → cancel. |
| **EC7. User Cancels Then Wants to Reactivate** | Must start new subscription with `reactivate` event (no trial). |
| **EC8. Trial User Upgrades** | Upgrade allowed during trial, prorated credit = $0 (nothing paid yet). |
| **EC9. Starter User Visits Billing Page (First Time)** | Redirect to Plans page. |
| **EC10. Multiple Upgrades in Same Cycle** | Each upgrade follows same pattern: previous upcoming → cancel, new logs created. |

### B5) Data Requirements & API Contracts

### B6) Logical Flow & Pseudo-code

**Upgrade Calculation:**

```python
def calculate_upgrade(current_sub, new_plan_price):
    total_seconds = (current_sub.end_date - current_sub.start_date).total_seconds()
    used_seconds = (now - current_sub.start_date).total_seconds()
    remaining_seconds = max(0, total_seconds - used_seconds)

    paid_amount = current_sub.amount_paid
    # Credit is calculated on what was PAID
    credit_value = (paid_amount / total_seconds) * remaining_seconds

    final_price = max(0, new_plan_price - credit_value)

    return {
        "credit_applied": credit_value,
        "final_to_pay": final_price
    }

```

### B7) Testing Matrix for AI

> Note: UI-specific tests are in [SUB-002] and [SUB-003].
> 

**Trial Flow:**

- **T1.** First-time buy → Status=Trialing, Card NOT charged, Log: trial/paid/$0 + renew/upcoming
- **T2.** Day 31 → Card charged, upcoming→paid, new upcoming created

**Upgrade Flow:**

- **T3.** Mid-cycle upgrade → Credit calculated, old upcoming→cancel, new paid+upcoming logs
- **T4.** Upgrade during trial → Credit=$0, pay full price

**Cancellation & Renewal:**

- **T5.** Cancel → upcoming→cancel, plan active until expiry
- **T6.** Failed renewal → upcoming→cancel, downgrade to Starter

**Reactivation:**

- **T7.** Reactivate after cancel → event=reactivate

**Admin:**

- **T8.** Admin activation → NO trial, event=new_subscription
- **T9.** Admin renewal insufficient funds → upcoming→cancel, Starter

---

# Final Ambiguity Check

- **Unclear terms:** None.
- **Missing rules:** None.
- **Top 3 likely bug sources:**
    1. Time zone mismatches in "Day 30" trial calc vs Stripe billing cycle.
    2. Prorated credit calculation off by small decimals (rounding errors).
    3. Shop Credit renewal failing to trigger or double charging.