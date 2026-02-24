# Customer Support Fee

# Customer Support Fee Management

### Feature ID:

**[CSF-001]**

### Title:

**Customer Support Fee - Activation & Payment Management**

### Category:

**Super Admin / Shop Management** | **Actors**: Super Admin, Merchant | **Channel**: Super Admin Panel, Merchant Dashboard

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
Merchants need access to dedicated customer support services, but the system currently lacks a mechanism to enable and charge for these premium support services. Super Admins need a flexible way to offer custom support packages with variable pricing and payment options.

**Desired Outcome:**

- Super Admins can activate Customer Support Fee feature for specific shops from the Super Admin panel
- When activated, shops are assigned one of two payment models: Credit Balance or Credit Card
- If Credit Balance is selected and sufficient: feature auto-activates and deducts from merchant's credit
- If Credit Card is selected: merchants must add and validate their card in dashboard before activation
- Variable monthly pricing set by Super Admin for each merchant's custom support package
- Merchants can subscribe to support services with custom pricing defined by admin

---

### 2) **Scope – In / Out**

**In Scope:**

- **Super Admin Panel:**
    - Toggle to enable/disable Customer Support Fee feature per shop
    - Payment model selection (Credit Balance vs Credit Card)
    - Custom monthly pricing input field for support services
    - View of shops with active support subscriptions
- **Merchant Dashboard:**
    - Support subscription management section
    - Credit card input and validation UI (when Credit Card model selected)
    - Payment method selection and confirmation
    - Display of support service status (Active/Inactive)
    - Monthly billing information and next payment date
- **Payment Flows:**
    - Credit Balance: Automatic deduction and instant activation
    - Credit Card: Card entry → Validation → Activation
- **Backend:**
    - Support subscription status tracking
    - Payment processing for both models
    - Billing cycle management

**Out of Scope:**

- Support ticket system implementation
- Support agent assignment logic
- Refund processing for cancelled subscriptions
- Integration with external support platforms (Zendesk, Intercom, etc.)
- Support SLA/response time guarantees

---

### 3) **Key User Journeys**

---

**Journey 1: Super Admin Activates Support with Credit Balance Model**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Super Admin | Navigates to shop management in Super Admin panel | Shop list loads |
| 2 | Super Admin | Selects a shop and enables "Customer Support Fee" toggle | Feature enabled, payment model options appear |
| 3 | Super Admin | Selects "Credit Balance" as payment model | Credit balance option selected |
| 4 | Super Admin | Enters custom monthly amount (e.g., $99/month) | Amount validated and saved |
| 5 | Super Admin | Saves configuration | System checks merchant's credit balance |
| 6 | System | Validates sufficient credit exists | Support service auto-activated |
| 7 | System | Deducts first month from merchant's credit | Credit balance updated, subscription active |

---

**Journey 2: Super Admin Activates Support with Credit Card Model**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Super Admin | Enables "Customer Support Fee" toggle for a shop | Feature enabled |
| 2 | Super Admin | Selects "Credit Card" as payment model | Credit card option selected |
| 3 | Super Admin | Sets custom monthly pricing | Amount saved |
| 4 | System | Notifies merchant via email and dashboard | Notification sent |
| 5 | Merchant | Navigates to dashboard Support section | Support setup page displayed |
| 6 | Merchant | Clicks "Add Payment Method" | Card entry form opens |
| 7 | Merchant | Enters card details and submits | Card validated via payment gateway |
| 8 | System | On successful validation, activates support service | Support active, billing cycle starts |
| 9 | System | Charges first month's fee to card | Payment processed, receipt sent |

---

**Journey 3: Merchant Views Support Subscription Status**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Navigates to Support section in dashboard | Support management page loads |
| 2 | Merchant | Views subscription details | Shows: Status (Active/Inactive), Monthly fee, Next billing date, Payment method |
| 3 | Merchant | Views support service benefits | Displays included support features |

---

**Journey 4: Credit Balance Runs Low - Auto-Renewal Failure**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | System | Monthly renewal date arrives | Attempts to deduct from credit balance |
| 2 | System | Detects insufficient credit balance | Renewal fails |
| 3 | System | Updates subscription status to "Suspended - Payment Failed" | Status changed |
| 4 | System | Sends email notification to merchant | Notification sent |
| 5 | System | Displays warning in merchant dashboard | Alert shown with "Add Credit" CTA |
| 6 | Merchant | Adds credit to balance | Credit updated |
| 7 | System | Auto-retries payment | If sufficient, subscription reactivated |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria | Pass/Fail |
| --- | --- | --- |
| BAC-1 | Super Admin can enable Customer Support Fee feature per shop |  |
| BAC-2 | Super Admin can select between Credit Balance and Credit Card payment models |  |
| BAC-3 | Super Admin can set custom monthly pricing for each shop |  |
| BAC-4 | When Credit Balance selected and sufficient, feature auto-activates |  |
| BAC-5 | When Credit Balance selected, amount is deducted from merchant credit |  |
| BAC-6 | When Credit Card selected, merchant must enter valid card before activation |  |
| BAC-7 | Credit card validation occurs via secure payment gateway |  |
| BAC-8 | Support subscription status is displayed in merchant dashboard |  |
| BAC-9 | Monthly billing cycle is tracked and displayed |  |
| BAC-10 | Failed payments trigger notifications and dashboard alerts |  |
| BAC-11 | Subscription reactivates automatically after payment is resolved |  |
| BAC-12 | Super Admin can view all shops with active support subscriptions |  |
| BAC-13 | Super Admin can disable support feature for any shop |  |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-18 | Behdad | Initial document creation | New feature specification |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **Customer Support Fee** | Premium service subscription that grants merchants access to dedicated support |
| **Payment Model** | Method used to charge for support services: Credit Balance (internal) or Credit Card (external) |
| **Credit Balance** | Internal store credit that merchants can use for platform services |
| **Custom Pricing** | Variable monthly fee set by Super Admin for each merchant's support package |

---

### B2) Exhaustive Functional Logic

---

### **FL-1: Support Feature Activation Flow**

```
ON SuperAdminEnableSupport(shopId, paymentModel, monthlyFee):
    SET shop.supportFeature.enabled = true
    SET shop.supportFeature.paymentModel = paymentModel
    SET shop.supportFeature.monthlyFee = monthlyFee
    SET shop.supportFeature.status = "Pending"

    IF paymentModel == "CREDIT_BALANCE":
        CHECK merchant.creditBalance >= monthlyFee
        IF sufficient:
            DEDUCT monthlyFee FROM merchant.creditBalance
            SET shop.supportFeature.status = "Active"
            SET shop.supportFeature.startDate = NOW()
            SET shop.supportFeature.nextBillingDate = NOW() + 30 days
            NOTIFY merchant("Support activated, first month deducted from credit")
        ELSE:
            SET shop.supportFeature.status = "Pending - Insufficient Credit"
            NOTIFY merchant("Support pending - please add credit to activate")

    ELSE IF paymentModel == "CREDIT_CARD":
        NOTIFY merchant("Please add payment method to activate support")
        CREATE dashboard notification card
```

---

### **FL-2: Credit Card Activation Flow**

```
ON MerchantAddCard(shopId, cardDetails):
    VALIDATE card format (number, expiry, CVV)
    IF invalid:
        RETURN error("Invalid card details")

    CALL paymentGateway.validateCard(cardDetails)
    IF validation fails:
        RETURN error("Card validation failed")

    STORE card token (securely, PCI compliant)
    SET shop.supportFeature.paymentMethod = cardToken

    CHARGE monthlyFee TO card
    IF charge successful:
        SET shop.supportFeature.status = "Active"
        SET shop.supportFeature.startDate = NOW()
        SET shop.supportFeature.nextBillingDate = NOW() + 30 days
        NOTIFY merchant("Support activated successfully")
        RETURN success
    ELSE:
        RETURN error("Payment failed - please try another card")
```

---

### **FL-3: Monthly Billing Cycle**

```
SCHEDULE monthlyBillingJob:
    FOR each shop WITH supportFeature.status == "Active":
        IF shop.supportFeature.nextBillingDate <= NOW():
            fee = shop.supportFeature.monthlyFee

            IF shop.supportFeature.paymentModel == "CREDIT_BALANCE":
                IF merchant.creditBalance >= fee:
                    DEDUCT fee FROM merchant.creditBalance
                    SET shop.supportFeature.nextBillingDate = NOW() + 30 days
                    NOTIFY merchant("Support fee deducted from credit")
                ELSE:
                    SET shop.supportFeature.status = "Suspended - Insufficient Credit"
                    NOTIFY merchant("Support suspended - add credit to reactivate")

            ELSE IF shop.supportFeature.paymentModel == "CREDIT_CARD":
                CHARGE fee TO stored card
                IF charge successful:
                    SET shop.supportFeature.nextBillingDate = NOW() + 30 days
                    SEND receipt to merchant
                ELSE:
                    SET shop.supportFeature.status = "Suspended - Payment Failed"
                    NOTIFY merchant("Support suspended - payment failed")
                    SET retryCount = 1
```

---

### **FL-4: Payment Retry Logic**

```
SCHEDULE paymentRetryJob(daily):
    FOR each shop WITH supportFeature.status == "Suspended - Payment Failed":
        IF retryCount < 3:
            retryCount++
            CHARGE monthlyFee TO stored card
            IF successful:
                SET shop.supportFeature.status = "Active"
                SET shop.supportFeature.nextBillingDate = NOW() + 30 days
                RESET retryCount
            ELSE:
                NOTIFY merchant("Payment attempt failed, will retry")
        ELSE:
            NOTIFY merchant("Support cancelled after 3 failed attempts")
            SET shop.supportFeature.enabled = false
```

---

### **FL-5: Super Admin Management**

| Action | Permission | Effect |
| --- | --- | --- |
| Enable Support Feature | Super Admin only | Activates feature for specific shop |
| Disable Support Feature | Super Admin only | Immediately cancels subscription, stops billing |
| Change Payment Model | Super Admin only | Switches between Credit Balance and Credit Card |
| Update Monthly Fee | Super Admin only | Changes subscription cost (effective next billing cycle) |
| View All Active Subscriptions | Super Admin only | List view with filter/search capabilities |

---

### B3) Behavioral Flow

```
[Super Admin Panel]
    ↓
[Select Shop]
    ↓
[Enable Support Toggle]
    ↓
[Select Payment Model]
    ├─ CREDIT_BALANCE → [Check Credit] → [Auto-Activate if sufficient]
    └─ CREDIT_CARD → [Notify Merchant] → [Dashboard Card Entry]
                                    ↓
[Merchant Dashboard]
    ↓
[Support Section]
    ├─ View Status
    ├─ Add Payment Method (if needed)
    └─ View Billing History
    ↓
[Monthly Billing]
    ├─ Success → [Continue Active]
    └─ Failure → [Suspend] → [Retry] → [Notify]
```

---

### B4) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** Merchant cancels/disputes credit card | Mark payment failed, suspend service, notify merchant |
| **EC-2:** Credit card expires | Proactive email 30 days before expiry, suspend if not updated |
| **EC-3:** Super Admin changes pricing mid-cycle | New price effective at next billing date, current cycle unchanged |
| **EC-4:** Merchant disputes credit deduction | Log dispute, hold future deductions, notify Super Admin |
| **EC-5:** Payment gateway is down | Queue payment attempts, retry when gateway available |
| **EC-6:** Merchant already has support from another source | Allow override, but warn about duplicate billing |
| **EC-7:** Negative credit balance | Treat as zero, block activation until positive |

---

### B5) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | Enable support with sufficient credit | Auto-activated, credit deducted, status Active |
| TC-2 | Enable support with insufficient credit | Pending status, notification sent to merchant |
| TC-3 | Enable support with credit card model | Merchant sees card entry prompt, activation pending |
| TC-4 | Merchant adds valid card | Card validated, first charge processed, status Active |
| TC-5 | Monthly billing with credit balance | Credit deducted, next billing date updated |
| TC-6 | Monthly billing with expired card | Payment failed, retry scheduled, merchant notified |
| TC-7 | Credit balance runs out during billing | Service suspended, merchant can add credit to reactivate |
| TC-8 | Super Admin disables support feature | Subscription cancelled immediately, no refund |
| TC-9 | Super Admin changes monthly fee | New price shown, effective next billing cycle |
| TC-10 | View support subscription in dashboard | Status, fee, next billing date, payment method displayed |

---

**Document End**