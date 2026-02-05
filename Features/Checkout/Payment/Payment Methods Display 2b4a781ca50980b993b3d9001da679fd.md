# Payment Methods Display

# Payment Methods Display

**Feature ID:** CHK-PAYMENT-402

**Category:** Checkout Domain | Payment

**Actors:** Customer

**Status:** Defined

**Owner:** Behdad

**Last Updated:** January 31, 2026

---

# **1) Summary**

### **Problem / Value**

Customers need to clearly see and select the payment methods available for a shop. These methods are configured by the merchant in Shop Builder, and the storefront must show only the enabled and active options to avoid confusion or failed payments.

### **Desired Outcome**

- Display all merchant-enabled payment methods in the checkout payment step.
- Only methods enabled by the merchant are shown.
- **At least one payment method must always be enabled** (cannot be zero).
- **No country-based restrictions** on payment method availability.
- Customers can select one payment method.
- Payment methods displayed as three categories:
    - **Credit Card** (Stripe)
    - **PayPal**
    - **Crypto** (Native token payments via dedicated Crypto Payment Page)
- Coinbase Commerce is handled similarly to PayPal (redirect flow).
- Selection is saved for subsequent payment processing.

---

# **2) Scope – In / Out**

### **In Scope**

- Fetch and display all payment methods enabled by the merchant in Shop Builder.
- Payment methods include:
    - **Credit Card** (Stripe) – Opens Stripe component inline
    - **PayPal** – Redirects to PayPal payment flow
    - **Coinbase Commerce** – Redirects to Coinbase Commerce payment flow
    - **Crypto** – Navigates to dedicated Crypto Payment Page
- Highlight the selected payment method.
- Only one method selectable at a time.
- Display method name, icon, and any relevant metadata for clarity.
- Dynamically update the list if merchant configuration changes.

### **Out of Scope**

- Actual payment transaction and gateway integration (handled by respective payment processors).
- Multi-payment or split payment scenarios.
- Payment validation beyond frontend selection.

---

# **3) Key User Journeys**

---

## **Journey 1 – Load Payment Methods**

**Step 1:** Customer navigates to the checkout Payment Step with cart total > $0.

**Step 2:** System fetches all enabled payment methods from the merchant configuration.

**Step 3:** Checkout displays only the enabled methods in a clear list with icons and names.

**Step 4:** If cart total = $0 (after discounts), payment methods are NOT displayed; only Confirm button is shown.

---

## **Journey 2 – Select Credit Card (Stripe)**

**Step 1:** Customer clicks on "Credit Card" payment method.

**Step 2:** Stripe payment component opens inline on the checkout page.

**Step 3:** Customer enters card details and completes payment.

**Step 4:** On success → Redirect to Order Success page.

**Step 5:** On failure → Remain on checkout page with error message; customer can retry.

---

## **Journey 3 – Select PayPal**

**Step 1:** Customer clicks on "PayPal" payment method.

**Step 2:** Customer is redirected to PayPal payment page.

**Step 3A:** On success → Redirect to Order Success page.

**Step 3B:** On failure/cancel → Return to checkout page; customer can retry or select different method.

---

## **Journey 4 – Select Coinbase Commerce**

**Step 1:** Customer clicks on "Coinbase Commerce" payment method.

**Step 2:** Customer is redirected to Coinbase Commerce payment page.

**Step 3A:** On success → Redirect to Order Success page.

**Step 3B:** On failure/cancel → Return to checkout page; customer can retry or select different method.

---

## **Journey 5 – Select Crypto**

**Step 1:** Customer clicks on "Crypto" payment method.

**Step 2:** Customer is navigated to the dedicated **Crypto Payment Page**.

**Step 3:** Customer completes crypto payment flow (see CHK-PAYMENT-CRYPTO-405).

**Step 4A:** On success → Redirect to Order Success page.

**Step 4B:** On failure → Return to checkout page; customer can retry or select different method.

---

## **Journey 6 – Zero Total Cart**

**Step 1:** Customer reaches checkout with cart total = $0 (due to discount codes).

**Step 2:** Payment method section is completely hidden.

**Step 3:** Only "Confirm" button is displayed.

**Step 4:** Customer clicks Confirm → Order is created with status "Paid".

**Step 5:** Customer is redirected to Order Success page.

---

# **4) Business Acceptance Criteria (BAC)**

---

### **BAC 1:** Only payment methods enabled by the merchant in Shop Builder are displayed.

### **BAC 2:** Each method must show name, icon, and relevant metadata.

### **BAC 3:** Customer can select **one** payment method at a time.

### **BAC 4:** Selected method is highlighted and saved for checkout.

### **BAC 5:** Payment methods displayed with these labels:

- Stripe → "Credit Card"
- PayPal → "PayPal"
- Coinbase Commerce → "Coinbase Commerce"
- Crypto → "Crypto"

### **BAC 6:** Any configuration changes by the merchant (enabling/disabling methods) must reflect immediately for the customer.

### **BAC 7:** At least one payment method must always be enabled by the merchant.

### **BAC 8:** Payment methods have no country-based restrictions or limitations.

### **BAC 9:** If cart total = $0 after discounts, payment methods are hidden and only Confirm button is displayed.

### **BAC 10:** Zero-total orders are created with status "Paid" upon confirmation.

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-01 | Behdad | Added AI-Centric Layer (Part 2) | SSoT completion |
| 2026-01-31 | Behdad | Updated payment method naming (Stripe→Credit Card), added zero-cart flow, restructured journeys | Checkout simplification update |

---

---

# [PART 2: AI-CENTRIC LAYER – Functional Logic & Rules]

---

## **B1) Definitions & Glossary**

| Term | Definition |
| --- | --- |
| **Payment Method** | A way for customers to pay for their order (Credit Card, PayPal, Coinbase Commerce, Crypto) |
| **Inline Payment** | Payment form that opens directly on the checkout page without redirecting |
| **Redirect Payment** | Payment flow that takes customer to external provider's page |
| **Zero-Total Cart** | A cart where final amount = $0 after discounts are applied |
| **Payment Step** | The section in checkout where customer selects and completes payment |
| **Merchant Configuration** | Settings defined by shop owner in Shop Builder for enabled payment methods |

---

## **B2) Detailed Functional Rules**

### **R1. Payment Method Display Rules**

- System must fetch enabled payment methods from merchant configuration on page load
- Only methods explicitly enabled by merchant are shown
- Methods are displayed with icon + label
- Display order: Credit Card, PayPal, Coinbase Commerce, Crypto (if all enabled)
- At least one method must always be available (enforced at merchant configuration level)

### **R2. Payment Method Labels**

| Internal Name | Display Label |
| --- | --- |
| Stripe | Credit Card |
| PayPal | PayPal |
| Coinbase Commerce | Coinbase Commerce |
| Native Token | Crypto |

### **R3. Selection Behavior**

- Customer can select only ONE payment method at a time
- Clicking a method highlights it and deselects any previously selected method
- Selection persists until customer changes it or leaves checkout
- No method is pre-selected by default; customer must actively choose

### **R4. Zero-Total Cart Logic**

- IF cart total = $0 after discounts applied:
    - Hide entire payment methods section
    - Display only "Confirm" button
    - On Confirm click → Create order with status "Paid"
    - Redirect to Order Success page
- IF cart total > $0:
    - Display available payment methods
    - Require payment method selection before proceeding

### **R5. Payment Flow by Method**

| Method | Flow Type | Behavior |
| --- | --- | --- |
| Credit Card | Inline | Stripe form opens on same page, payment processed without leaving |
| PayPal | Redirect | Customer sent to PayPal site, returns after completion |
| Coinbase Commerce | Redirect | Customer sent to Coinbase site, returns after completion |
| Crypto | Navigate | Customer goes to dedicated Crypto Payment Page within app |

### **R6. Success Handling**

- All successful payments → Order status = "Paid"
- All successful payments → Redirect to Order Success page
- Success determined by: payment gateway confirmation OR webhook confirmation

### **R7. Failure Handling**

- Credit Card failure → Show error on checkout page, allow retry
- PayPal cancel/failure → Return to checkout page, allow retry or method change
- Coinbase cancel/failure → Return to checkout page, allow retry or method change
- Crypto failure → Return to checkout page OR stay on Crypto page with retry option

### **R8. Real-time Configuration Updates**

- If merchant enables/disables a payment method:
    - Change must reflect immediately on customer's checkout page
    - No page refresh required (real-time or on next action)
    - If currently selected method is disabled → Deselect and show remaining options

---

## **B3) UI/UX States & Triggers**

| State | Description | Trigger |
| --- | --- | --- |
| **Methods Loading** | Spinner while fetching available methods | Page load |
| **Methods Displayed** | List of payment method options shown | Fetch complete |
| **Method Selected** | One method highlighted/active | Customer clicks method |
| **Payment Processing** | Loading overlay during payment | Customer submits payment |
| **Payment Success** | Redirect to success page | Gateway confirms payment |
| **Payment Failed** | Error message displayed | Gateway rejects payment |
| **Zero Cart Mode** | Only Confirm button visible, no payment methods | Cart total = $0 |

---

## **B4) Edge Cases & Error Handling**

| Edge Case | Expected Behavior |
| --- | --- |
| **All payment methods disabled** | Should never happen (BAC 7 prevents this); if occurs, show error "No payment methods available" |
| **Customer refreshes during payment** | Payment state lost; return to method selection |
| **Network error loading methods** | Show retry button; "Unable to load payment options. Please try again." |
| **Method disabled after selection** | Deselect method; show message "Selected payment method is no longer available" |
| **PayPal/Coinbase popup blocked** | Show message "Please allow popups to complete payment" |
| **Customer uses back button during redirect** | Return to checkout; previous selection cleared |
| **Session expires during payment** | Start fresh checkout; cart preserved |
| **Discount makes cart $0 after method selection** | Hide payment methods; show only Confirm button |

---

## **B5) Business Logic Decision Tree**

```
CUSTOMER ARRIVES AT PAYMENT STEP
│
├─► Is Cart Total = $0?
│   ├─► YES → Hide payment methods → Show "Confirm" button only
│   │         └─► On Confirm → Order status = "Paid" → Success page
│   │
│   └─► NO → Fetch enabled payment methods
│             │
│             ├─► Display available methods
│             │
│             └─► Customer selects method
│                   │
│                   ├─► Credit Card → Open Stripe inline
│                   │     ├─► Success → Paid → Success page
│                   │     └─► Fail → Show error → Retry
│                   │
│                   ├─► PayPal → Redirect to PayPal
│                   │     ├─► Success → Paid → Success page
│                   │     └─► Cancel/Fail → Return to checkout
│                   │
│                   ├─► Coinbase → Redirect to Coinbase
│                   │     ├─► Success → Paid → Success page
│                   │     └─► Cancel/Fail → Return to checkout
│                   │
│                   └─► Crypto → Go to Crypto Payment Page
│                         ├─► Success → Paid → Success page
│                         └─► Fail → Retry or return to checkout

```

---

## **B6) Test Scenarios for QA**

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| **T1** | Load checkout with all 4 payment methods enabled | All 4 methods displayed with correct labels |
| **T2** | Load checkout with only Credit Card enabled | Only "Credit Card" option shown |
| **T3** | Apply 100% discount code | Payment methods hidden, only Confirm button visible |
| **T4** | Click Confirm on $0 cart | Order created with status "Paid", redirect to success |
| **T5** | Select Credit Card, complete payment | Stripe form works, order = Paid, success page shown |
| **T6** | Select PayPal, complete payment | Redirect works, return to success page |
| **T7** | Select PayPal, click cancel | Return to checkout, can select different method |
| **T8** | Credit Card payment fails | Error shown, customer can retry |
| **T9** | Merchant disables PayPal mid-checkout | PayPal option disappears from list |
| **T10** | Select Crypto, complete flow | Navigates to Crypto page, success returns to success page |