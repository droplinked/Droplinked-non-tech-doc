# Payment Methods Display - Fiat & 3rd Party

# Payment Methods Display - Fiat & 3rd Party

**Feature ID:** CHK-PAYMENT-402-A

**Category:** Checkout Domain | Payment

**Actors:** Customer

**Status:** Defined

**Owner:** Behdad

**Last Updated:** January 31, 2026

---

# **1) Summary**

### **Problem / Value**

Customers using fiat currency or third-party payment processors need a clear interface to select payment methods like Credit Card (Stripe), PayPal, or Coinbase Commerce. These methods are configured by the merchant in Shop Builder, and the storefront must display only enabled options to ensure successful transactions.

### **Desired Outcome**

- Display all merchant-enabled fiat and third-party payment methods during the checkout payment step.
- Only methods enabled by the merchant in Shop Builder are shown.
- **At least one payment method must always be enabled** (cannot be zero).
- **No country-based restrictions** on payment method availability.
- Customers can select one payment method at a time.
- **Stripe displays with "Credit Card" label** in the storefront UI.
- PayPal and Coinbase Commerce display with clear names and logos.
- Selection is saved for subsequent payment processing.
- Seamless integration with payment gateway processing.

---

# **2) Scope – In / Out**

### **In Scope**

- Fetch and display all fiat/third-party payment methods enabled by the merchant in Shop Builder.
- Supported payment methods in this category:
    - **Stripe** (displayed as "**Credit Card**" in storefront UI)
    - **PayPal** (redirect flow)
    - **Coinbase Commerce** (redirect flow, handled similarly to PayPal)
- Highlight the selected payment method visually.
- Only one method selectable at a time.
- Display method name, icon, and relevant metadata for clarity.
- Dynamically update the list if merchant configuration changes.
- Handle payment processing:
    - **Credit Card (Stripe):** Opens Stripe component inline on checkout page
    - **PayPal:** Redirects to PayPal payment page
    - **Coinbase Commerce:** Redirects to Coinbase Commerce payment page

### **Out of Scope**

- Actual payment transaction processing (handled by payment gateway).
- Multi-payment or split payment scenarios.
- Payment validation beyond frontend selection and form requirements.
- Country-specific payment method restrictions.

---

# **3) Key User Journeys**

---

## **Journey 1 – Load Fiat Payment Methods**

**Step 1:** Customer navigates to the checkout Payment Step with a cart total > $0.

**Step 2:** System fetches all enabled fiat/third-party payment methods from merchant configuration.

**Step 3:** Checkout displays only the enabled methods in a clear list with icons and names.

**Step 4:** Methods displayed with labels:

- Stripe → "Credit Card"
- PayPal → "PayPal"
- Coinbase Commerce → "Coinbase Commerce"

---

## **Journey 2 – Pay with Credit Card (Stripe)**

**Step 1:** Customer selects "Credit Card" payment method.

**Step 2:** Stripe payment component opens inline on the checkout page.

**Step 3:** Customer enters card details.

**Step 4:** Customer clicks Pay button.

**Step 5:** System enters loading state while processing.

**Step 6A:** On success → Order status set to "Paid" → Redirect to Order Success page.

**Step 6B:** On failure → Display error message → Customer can retry payment.

---

## **Journey 3 – Pay with PayPal**

**Step 1:** Customer selects "PayPal" payment method.

**Step 2:** Customer clicks Pay/Continue button.

**Step 3:** Customer is redirected to PayPal payment page.

**Step 4:** Customer completes payment on PayPal.

**Step 5A:** On success → Return to Order Success page with status "Paid".

**Step 5B:** On cancel/failure → Return to checkout page → Customer can retry or select different method.

---

## **Journey 4 – Pay with Coinbase Commerce**

**Step 1:** Customer selects "Coinbase Commerce" payment method.

**Step 2:** Customer clicks Pay/Continue button.

**Step 3:** Customer is redirected to Coinbase Commerce payment page.

**Step 4:** Customer completes payment on Coinbase.

**Step 5A:** On success → Return to Order Success page with status "Paid".

**Step 5B:** On cancel/failure → Return to checkout page → Customer can retry or select different method.

---

## **Journey 5 – Unavailable or Disabled Methods**

**Step 1:** If a fiat method is disabled by the merchant, it does not appear in the list.

**Step 2:** Only enabled methods are shown to the customer.

**Step 3:** If no fiat methods are enabled, system falls back to alternative payment options (Crypto) if available.

---

# **4) Business Acceptance Criteria (BAC)**

---

### **BAC 1:** Only fiat/third-party payment methods enabled by the merchant in Shop Builder are displayed.

### **BAC 2:** Supported methods in this category are:

- Stripe (labeled as "**Credit Card**" in UI)
- PayPal
- Coinbase Commerce

### **BAC 3:** Each method must display name, icon, and relevant metadata.

### **BAC 4:** Customer can select **one** fiat/third-party payment method at a time.

### **BAC 5:** Selected method is visually highlighted and saved for checkout.

### **BAC 6:** Any configuration changes by the merchant must reflect immediately for customers on the payment page.

### **BAC 7:** At least one payment method must always be enabled by the merchant.

### **BAC 8:** Payment methods have no country-based restrictions or limitations.

### **BAC 9:** Payment flows:

- **Credit Card (Stripe):** Opens Stripe component inline on checkout page
- **PayPal:** Redirects to PayPal payment page; returns to Order Success on success or checkout on failure
- **Coinbase Commerce:** Redirects to Coinbase payment page; returns to Order Success on success or checkout on failure

### **BAC 10:** Payment method selection is persistent until order is confirmed or checkout session expires.

### **BAC 11:** Successful payments set order status to "Paid".

### **BAC 12:** Failed payments allow customer to retry or select a different payment method.

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-01 | Behdad | Added AI-Centric Layer (Part 2) | SSoT completion |
| 2026-01-31 | Behdad | Renamed Stripe to "Credit Card", updated payment flows (inline vs redirect), added Paid status | Checkout simplification update |
| 2025-12-29 | Behdad | Initial document creation | Feature definition |

---

---

# [PART 2: AI-CENTRIC LAYER – Functional Logic & Rules]

---

## **B1) Definitions & Glossary**

| Term | Definition |
| --- | --- |
| **Fiat Payment** | Payment using traditional currency (USD, EUR, etc.) via card or payment processor |
| **Third-Party Payment** | Payment processed through external provider (Stripe, PayPal, Coinbase Commerce) |
| **Inline Payment Component** | Payment form embedded directly in checkout page (used by Stripe/Credit Card) |
| **Redirect Flow** | Customer leaves checkout, completes payment on external site, returns afterward |
| **Payment Gateway** | External service that processes the actual financial transaction |

---

## **B2) Detailed Functional Rules**

### **R1. Supported Fiat/Third-Party Methods**

| Method | Display Label | Flow Type | Processing |
| --- | --- | --- | --- |
| Stripe | Credit Card | Inline | Payment form on checkout page |
| PayPal | PayPal | Redirect | Customer goes to PayPal site |
| Coinbase Commerce | Coinbase Commerce | Redirect | Customer goes to Coinbase site |

### **R2. Credit Card (Stripe) Flow Rules**

- When selected: Stripe payment component appears inline on checkout page
- Customer enters: Card number, Expiry, CVC, Billing ZIP (if required)
- On "Pay" click:
    - Show loading state on button
    - Stripe processes payment
    - IF success → Order status = "Paid" → Redirect to Order Success
    - IF failure → Show error message below form → Customer can edit and retry
- Customer never leaves the checkout page

### **R3. PayPal Flow Rules**

- When selected: "Continue to PayPal" or "Pay with PayPal" button shown
- On button click:
    - Customer redirected to PayPal login/payment page
    - Customer completes payment on PayPal
    - PayPal redirects customer back
- IF payment successful → Return to Order Success page, Order status = "Paid"
- IF payment cancelled → Return to checkout page, payment methods still visible
- IF payment failed → Return to checkout page with error message

### **R4. Coinbase Commerce Flow Rules**

- Behavior identical to PayPal (redirect flow)
- When selected: "Continue to Coinbase" button shown
- On button click: Redirect to Coinbase Commerce checkout
- Same success/cancel/failure handling as PayPal

### **R5. Method Availability Rules**

- Each method shown ONLY if enabled by merchant in Shop Builder
- Methods cannot be partially enabled (either fully available or hidden)
- No geographic/country restrictions on any method
- If merchant disables all fiat methods → Crypto must be available (at least one method required)

### **R6. Order Status Transitions**

| Event | Order Status |
| --- | --- |
| Payment initiated | Pending (if applicable) |
| Credit Card success | Paid |
| PayPal returns success | Paid |
| Coinbase returns success | Paid |
| Any payment failure | Failed (logged) |
| Customer cancels redirect | No order created OR order remains pending |

---

## **B3) UI/UX States & Triggers**

### **Credit Card States**

| State | UI Display | Trigger |
| --- | --- | --- |
| Selected | Stripe form expands/appears | Customer clicks Credit Card |
| Entering Details | Form fields active, editable | Customer typing |
| Processing | Button shows spinner, form disabled | Customer clicks Pay |
| Success | Form disappears, redirect occurs | Stripe confirms |
| Error | Red error message below form | Stripe rejects |

### **PayPal/Coinbase States**

| State | UI Display | Trigger |
| --- | --- | --- |
| Selected | "Continue to [Provider]" button shown | Customer clicks method |
| Redirecting | Loading indicator or immediate redirect | Customer clicks continue |
| Processing | Customer on external site | N/A (not on our page) |
| Return Success | Redirect to Order Success | Provider confirms |
| Return Cancel | Back to checkout, method deselected | Customer cancels on provider |
| Return Failed | Back to checkout with error | Provider rejects |

---

## **B4) Edge Cases & Error Handling**

| Edge Case | Expected Behavior |
| --- | --- |
| **Credit Card declined** | Show "Card declined. Please try another card." Allow retry. |
| **Credit Card expired** | Stripe shows inline validation error before submission |
| **Insufficient funds** | Show "Insufficient funds. Please try another payment method." |
| **PayPal account issue** | PayPal handles on their end; returns failure to checkout |
| **Customer closes PayPal popup/tab** | Customer stuck on PayPal; must manually return or restart |
| **Coinbase session expires** | Customer must restart payment flow |
| **Network error during Stripe** | Show "Connection error. Please try again." with retry option |
| **3D Secure required** | Stripe handles popup; customer completes verification |
| **Redirect takes too long** | Show timeout message after 60 seconds |
| **Customer uses browser back from provider** | Return to checkout; no order created; can restart |

---

## **B5) Validation Rules**

### **Before Showing Payment Methods:**

- Cart must have items
- Cart total must be > $0 (otherwise hide fiat methods, show Confirm only)
- Shipping must be completed (if required by cart items)
- Contact information must be provided

### **Before Allowing Credit Card Submission:**

- Card number: Valid format, passes Luhn check (handled by Stripe)
- Expiry: Future date (handled by Stripe)
- CVC: 3-4 digits (handled by Stripe)
- Billing ZIP: If required by merchant config

### **Before Redirect to PayPal/Coinbase:**

- Payment method must be selected
- Order information prepared for external provider

---

## **B6) Business Logic Summary**

```
CUSTOMER SELECTS FIAT PAYMENT METHOD
│
├─► Credit Card selected
│   │
│   ├─► Stripe form appears inline
│   ├─► Customer enters card details
│   ├─► Customer clicks "Pay"
│   │     │
│   │     ├─► Success → Order = Paid → Success Page
│   │     └─► Failure → Show Error → Allow Retry
│   │
│   └─► Customer can switch to other method before paying
│
├─► PayPal selected
│   │
│   ├─► "Continue to PayPal" button shown
│   ├─► Customer clicks → Redirect to PayPal
│   │     │
│   │     ├─► Completes payment → Return → Order = Paid → Success Page
│   │     ├─► Cancels → Return to Checkout
│   │     └─► Fails → Return to Checkout with error
│   │
│   └─► Customer can switch method before clicking Continue
│
└─► Coinbase Commerce selected
    │
    └─► Same flow as PayPal

```

---

## **B7) Test Scenarios for QA**

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| **T1** | Select Credit Card, enter valid card, pay | Payment succeeds, order = Paid, success page |
| **T2** | Select Credit Card, enter expired card | Inline error: "Card expired" |
| **T3** | Select Credit Card, payment declined | Error message, can retry with different card |
| **T4** | Select PayPal, complete payment | Redirect to PayPal, return to success page |
| **T5** | Select PayPal, cancel on PayPal | Return to checkout, can select different method |
| **T6** | Select Coinbase, complete payment | Same as PayPal flow |
| **T7** | Only Stripe enabled by merchant | Only Credit Card option visible |
| **T8** | Switch from Credit Card to PayPal | Stripe form closes, PayPal button appears |
| **T9** | Network error during Credit Card payment | Error: "Connection error", retry available |
| **T10** | 3D Secure verification required | Popup appears, customer completes, payment proceeds |