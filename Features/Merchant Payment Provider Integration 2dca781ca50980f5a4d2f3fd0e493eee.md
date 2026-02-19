# Merchant Payment Provider Integration

# Merchant Payment Provider Integration & Shop Activation Control

### Feature ID:

**[MPPI-001]**

### Title:

**Mandatory Payment Provider Integration for Merchant Sales**

### Category:

**Shop Management / Compliance** | **Actors**: Merchant, System, Super Admin | **Channel**: Merchant Dashboard, Shop Front, Onboarding Flow

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
Currently, merchants can create shops and start selling without connecting their own Stripe, PayPal, or Payment Provider accounts. This creates issues with payment routing, compliance, and platform reliability. We need to enforce that merchants have valid, connected, and approved payment provider accounts before they can accept orders.

**Desired Outcome:**

- Merchants MUST connect their Stripe or PayPal account to enable sales
- Shops are created but placed in "Inactive/Deactivated" state if payment provider not connected
- Shop Front displays "Add to Cart" lock/disable state when shop is inactive
- Existing legacy shops must be gradually migrated or grandfathered with clear deadlines
- Merchants see clear guidance on required payment provider setup during onboarding
- Shop activation status is visible in merchant dashboard with actionable steps to enable

---

### 2) **Scope – In / Out**

**In Scope:**

- **Shop Creation Flow:**
    - New shops created with default status "Pending Activation"
    - Shop becomes "Active" only after payment provider verification
- **Payment Provider Integration:**
    - Stripe Connect OAuth flow for merchant onboarding
    - PayPal merchant account connection and verification
    - Payment Provider account status checking (connected, verified, approved)
- **Shop Front Behavior:**
    - Display shop products but disable "Add to Cart" functionality when inactive
    - Show informational banner: "This shop is not accepting orders"
    - Optional: CTA to contact merchant or platform support
- **Merchant Dashboard:**
    - Clear activation checklist showing payment provider status
    - Step-by-step guide to connect Stripe/PayPal
    - Real-time status updates when connection is successful
- **Legacy Shop Migration:**
    - Identify all existing shops without payment providers
    - Grace period notification system
    - Deadline-based enforcement with escalation warnings
    - Gradual rollout: Warning → Soft Block (orders reduced) → Hard Block (no orders)

**Out of Scope:**

- Automatic payment provider account creation for merchants
- Platform-hosted payment processing (we're moving AWAY from this)
- Manual verification of payment provider accounts by our team
- Support for additional payment providers beyond Stripe and PayPal
- Complex grandfathering rules for high-volume legacy merchants

---

### 3) **Key User Journeys**

---

**Journey 1: New Merchant Onboarding with Payment Provider**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Completes basic shop creation form | Shop created with "Pending Activation" status |
| 2 | System | Redirects to payment provider setup | Onboarding wizard displayed |
| 3 | Merchant | Selects "Connect with Stripe" | Stripe Connect OAuth flow initiated |
| 4 | Merchant | Completes Stripe account setup/connection | Stripe redirects back to platform |
| 5 | System | Verifies Stripe account status (connected + verified + approved) | Status checked via API |
| 6 | System | On successful verification, activates shop | Shop status changes to "Active" |
| 7 | System | Shows success message, redirects to dashboard | "Your shop is now live!" message displayed |
| 8 | System | Enables "Add to Cart" on shop front | Shop now accepting orders |

---

**Journey 2: New Merchant Without Immediate Payment Provider Setup**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Creates shop but skips payment provider step | Shop created, status "Pending Activation" |
| 2 | System | Allows dashboard access but shows prominent banner | "Complete setup to start selling" banner |
| 3 | Merchant | Views shop front | Products visible, "Add to Cart" disabled/locked |
| 4 | Merchant | Returns to dashboard later | Payment provider setup still required |
| 5 | Merchant | Clicks "Connect Payment Provider" | Setup wizard opens |
| 6 | Merchant | Connects PayPal account | PayPal OAuth flow completed |
| 7 | System | Verifies PayPal account status | Merchant account verified |
| 8 | System | Activates shop | Status Active, Add to Cart enabled |

---

**Journey 3: Customer Views Inactive Shop**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Navigates to shop URL | Shop front loads |
| 2 | Customer | Browses products | Products displayed normally |
| 3 | Customer | Attempts to add item to cart | Button disabled or shows lock icon |
| 4 | Customer | Hovers over disabled button | Tooltip: "This shop is not currently accepting orders" |
| 5 | Customer | Sees informational banner at top of page | "Shop temporarily unavailable for orders" |

---

**Journey 4: Legacy Shop Migration - Warning Phase**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | System | Identifies legacy shop without payment provider | Flagged for migration |
| 2 | System | Sends email to merchant | "Action Required: Connect payment provider by [DATE]" |
| 3 | Merchant | Logs into dashboard | Large banner: "Connect payment provider to continue selling" |
| 4 | Merchant | Clicks "Learn More" | Migration info page displayed |
| 5 | Merchant | Has 30-day grace period | Shop continues to accept orders during warning phase |

---

**Journey 5: Legacy Shop Migration - Enforcement Phase**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | System | Grace period expires, payment provider still not connected | Enforcement triggered |
| 2 | System | Deactivates shop | Shop status changes to "Inactive" |
| 3 | System | Disables Add to Cart on shop front | Cart functionality locked |
| 4 | Customer | Visits shop | Products visible, Add to Cart disabled |
| 5 | Merchant | Receives final notification | "Your shop has been deactivated - connect payment provider to reactivate" |
| 6 | Merchant | Connects payment provider | Shop reactivated immediately |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria | Pass/Fail |
| --- | --- | --- |
| BAC-1 | New shops are created with "Pending Activation" status |  |
| BAC-2 | Shops become "Active" only after verified payment provider connection |  |
| BAC-3 | Stripe Connect OAuth flow is available for merchants |  |
| BAC-4 | PayPal merchant connection is available for merchants |  |
| BAC-5 | System verifies payment provider account is connected + verified + approved |  |
| BAC-6 | Shop front shows products but disables Add to Cart when inactive |  |
| BAC-7 | Clear informational messaging displayed on inactive shop fronts |  |
| BAC-8 | Merchant dashboard shows activation checklist with payment provider status |  |
| BAC-9 | Legacy shops without payment providers are identified |  |
| BAC-10 | Legacy shop owners receive warning emails before enforcement |  |
| BAC-11 | Legacy shops enter deactivation after grace period expires |  |
| BAC-12 | Deactivated shops can be reactivated by connecting payment provider |  |
| BAC-13 | Shop activation status is visible to Super Admin |  |
| BAC-14 | Super Admin can manually override activation status for special cases |  |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-18 | Behdad | Initial document creation | New compliance feature |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **Pending Activation** | Shop status where products are visible but orders cannot be placed |
| **Active** | Shop status allowing full e-commerce functionality including orders |
| **Inactive** | Shop status where shop front is viewable but checkout is disabled |
| **Legacy Shop** | Existing shop created before this feature implementation |
| **Grace Period** | Warning period allowing legacy shops time to comply before enforcement |
| **Stripe Connect** | Stripe's platform onboarding solution for connected accounts |
| **PayPal Verification** | PayPal's merchant account approval and verification process |

---

### B2) Exhaustive Functional Logic

---

### **FL-1: Shop Status State Machine**

```
STATES:
  - PENDING_ACTIVATION
  - ACTIVE
  - INACTIVE
  - SUSPENDED

TRANSITIONS:
  1. Shop Created → PENDING_ACTIVATION
  2. PENDING_ACTIVATION + Payment Provider Verified → ACTIVE
  3. ACTIVE + Payment Provider Disconnected → PENDING_ACTIVATION
  4. PENDING_ACTIVATION + Grace Period Expired (legacy) → INACTIVE
  5. INACTIVE + Payment Provider Connected → ACTIVE
  6. ACTIVE + Super Admin Override → SUSPENDED
  7. SUSPENDED + Admin Action → ACTIVE or INACTIVE
```

---

### **FL-2: Payment Provider Verification Check**

```
FUNCTION verifyPaymentProvider(shopId, provider):
    
    merchantAccount = GET merchant_account WHERE shop_id = shopId
    
    IF provider == "STRIPE":
        stripeAccount = GET stripe_connected_account WHERE merchant_id = merchantAccount.id
        
        IF NOT stripeAccount.exists:
            RETURN { valid: false, reason: "NOT_CONNECTED" }
        
        CALL stripeAPI.getAccountStatus(stripeAccount.stripeAccountId)
        
        IF stripeAccount.charges_enabled == false:
            RETURN { valid: false, reason: "CHARGES_DISABLED" }
        
        IF stripeAccount.details_submitted == false:
            RETURN { valid: false, reason: "DETAILS_INCOMPLETE" }
        
        RETURN { valid: true, provider: "STRIPE" }
    
    ELSE IF provider == "PAYPAL":
        paypalAccount = GET paypal_merchant_account WHERE merchant_id = merchantAccount.id
        
        IF NOT paypalAccount.exists:
            RETURN { valid: false, reason: "NOT_CONNECTED" }
        
        CALL paypalAPI.getMerchantStatus(paypalAccount.merchantId)
        
        IF paypalAccount.status != "VERIFIED":
            RETURN { valid: false, reason: "NOT_VERIFIED" }
        
        RETURN { valid: true, provider: "PAYPAL" }
    
    RETURN { valid: false, reason: "INVALID_PROVIDER" }
```

---

### **FL-3: Shop Activation Logic**

```
FUNCTION attemptShopActivation(shopId):
    
    shop = GET shop WHERE id = shopId
    
    // Check Stripe first
    stripeResult = verifyPaymentProvider(shopId, "STRIPE")
    IF stripeResult.valid:
        SET shop.status = "ACTIVE"
        SET shop.payment_provider = "STRIPE"
        SET shop.activated_at = NOW()
        LOG activation event
        NOTIFY merchant("Shop activated with Stripe")
        RETURN success
    
    // If Stripe not valid, check PayPal
    paypalResult = verifyPaymentProvider(shopId, "PAYPAL")
    IF paypalResult.valid:
        SET shop.status = "ACTIVE"
        SET shop.payment_provider = "PAYPAL"
        SET shop.activated_at = NOW()
        LOG activation event
        NOTIFY merchant("Shop activated with PayPal")
        RETURN success
    
    // Neither provider valid
    SET shop.status = "PENDING_ACTIVATION"
    SET shop.activation_error = stripeResult.reason OR paypalResult.reason
    NOTIFY merchant("Payment provider setup incomplete")
    RETURN failure
```

---

### **FL-4: Legacy Shop Migration Logic**

```
FUNCTION processLegacyShops():
    
    legacyShops = GET ALL shops WHERE 
        created_at < '2026-03-01' AND 
        payment_provider IS NULL AND
        migration_status IS NULL
    
    FOR each shop IN legacyShops:
        
        // Phase 1: Warning (Day 0-30)
        IF NOW() < shop.migration_deadline - 30 days:
            SET shop.migration_status = "WARNING"
            SEND email("Action Required: Connect payment provider by [deadline]")
            SHOW dashboard banner with countdown
        
        // Phase 2: Urgent Warning (Day 31-60)
        ELSE IF NOW() < shop.migration_deadline:
            SET shop.migration_status = "URGENT"
            SEND email("URGENT: [X] days left to connect payment provider")
            SHOW prominent dashboard warning
            REDUCE shop visibility in marketplace (optional)
        
        // Phase 3: Enforcement (After deadline)
        ELSE:
            SET shop.migration_status = "ENFORCED"
            SET shop.status = "INACTIVE"
            SEND email("Shop deactivated - connect payment provider to reactivate")
            DISABLE Add to Cart on shop front
```

---

### **FL-5: Shop Front Cart Behavior**

```
FUNCTION getShopFrontStatus(shopId):
    
    shop = GET shop WHERE id = shopId
    
    IF shop.status == "ACTIVE":
        RETURN {
            canPurchase: true,
            addToCartEnabled: true,
            message: null,
            banner: null
        }
    
    ELSE IF shop.status == "PENDING_ACTIVATION":
        RETURN {
            canPurchase: false,
            addToCartEnabled: false,
            addToCartTooltip: "Shop setup incomplete - merchant is adding payment options",
            message: "This shop is not currently accepting orders",
            banner: {
                type: "info",
                text: "Shop setup in progress - check back soon!"
            }
        }
    
    ELSE IF shop.status == "INACTIVE":
        RETURN {
            canPurchase: false,
            addToCartEnabled: false,
            addToCartTooltip: "This shop is temporarily unavailable",
            message: "This shop is not accepting orders at this time",
            banner: {
                type: "warning",
                text: "This shop is currently unavailable for orders"
            }
        }
```

---

### B3) Behavioral Flow

```
[Merchant Onboarding]
    ↓
[Create Shop]
    ↓
[Status: PENDING_ACTIVATION]
    ↓
[Payment Provider Setup]
    ├─ Connect Stripe → [OAuth Flow] → [Verify] → [Activate Shop]
    └─ Connect PayPal → [OAuth Flow] → [Verify] → [Activate Shop]
    ↓
[Status: ACTIVE]
    ↓
[Shop Front Live with Full Checkout]

[Legacy Shops]
    ↓
[Identify Shops w/o Payment Provider]
    ↓
[Phase 1: Warning] → Email + Banner + 30 days
    ↓
[Phase 2: Urgent] → Escalated warnings + Reduced visibility
    ↓
[Phase 3: Enforcement] → Shop INACTIVE + Add to Cart disabled
    ↓
[Merchant Connects Provider] → Shop reactivated
```

---

### B4) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** Stripe account connected but not verified | Show specific error: "Complete your Stripe verification to activate" |
| **EC-2:** PayPal account under review | Allow shop creation, show "Verification pending" status |
| **EC-3:** Merchant disconnects payment provider after activation | Shop returns to PENDING_ACTIVATION, orders blocked after 24h grace period |
| **EC-4:** Payment provider API is down | Queue verification checks, retry every 15 minutes |
| **EC-5:** Merchant has both Stripe and PayPal connected | Use Stripe as primary, allow merchant to set preference |
| **EC-6:** Legacy high-volume merchant refuses to comply | Super Admin manual review and potential grandfathering |
| **EC-7:** Payment provider account suspended by provider | Immediate shop deactivation, notify merchant |
| **EC-8:** Shop has active orders when deactivating | Complete existing orders, block new orders |

---

### B5) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | Create new shop without payment provider | Shop status PENDING_ACTIVATION, Add to Cart disabled |
| TC-2 | Connect verified Stripe account | Shop status ACTIVE, Add to Cart enabled |
| TC-3 | Connect unverified Stripe account | Error shown, shop remains PENDING_ACTIVATION |
| TC-4 | Connect verified PayPal account | Shop status ACTIVE, payment via PayPal works |
| TC-5 | View inactive shop as customer | Products visible, Add to Cart locked, banner shown |
| TC-6 | Legacy shop enters warning phase | Email sent, dashboard banner appears, shop still active |
| TC-7 | Legacy shop grace period expires | Shop status INACTIVE, Add to Cart disabled |
| TC-8 | Reactivate inactive shop by connecting provider | Shop status ACTIVE immediately |
| TC-9 | Disconnect payment provider from active shop | Status changes to PENDING_ACTIVATION after grace period |
| TC-10 | Verify both Stripe and PayPal connected | Stripe used as default, merchant can switch preference |
| TC-11 | Check dashboard activation checklist | Shows completed/incomplete steps clearly |
| TC-12 | Super Admin override for special merchant | Manual activation bypasses payment provider check |

---

**Document End**
