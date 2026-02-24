# Payment Execution Engine

**Feature ID:** PAY-EXEC-008

**Category:** Checkout / Backend | Payment System

**Actors:** Customer, System, Payment Gateways

**Status:** Draft

**Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

After an order is created, the system must process payment reliably and confirm its success or failure. This engine handles multiple payment methods (cards, crypto, wallets), polls for asynchronous confirmation, and updates order status. It ensures that merchant, affiliate, referral, and Droplinked payouts are executed correctly based on payment outcome.

### **Desired Outcome**

- Process payments via all enabled methods: Stripe, PayPal, Coinbase, and network tokens.
- Poll for payment status until confirmed or failed.
- Update order status in real-time (Pending → Confirmed / Failed).
- Trigger credit or payout adjustments for merchant, affiliate, referral, Droplinked, and external service providers.
- Handle zero-total orders and applied discounts correctly.
- Maintain accurate logs for auditing and troubleshooting.

---

# **2) Scope – In / Out**

### **In Scope**

- Payment processing for:
    - Stripe
    - PayPal
    - Coinbase Commerce
    - Native tokens on supported blockchain networks
- Handle zero-total orders (skip payment, directly confirm).
- Poll asynchronous payments for final status.
- Confirm or fail orders based on payment outcome.
- Trigger payout logic for all stakeholders.
- Update frontend/order confirmation page with payment status.
- Log all payment events for traceability.

### **Out of Scope**

- Payment gateway integration setup/configuration (handled separately).
- Refunds, chargebacks, or disputes.
- Partial payments or multi-order splitting.
- Customer notifications beyond order status updates.

---

# **3) Key User Journeys**

---

## **Journey 1 – Successful Payment**

**Step 1:** Customer selects payment method and submits payment.

**Step 2:** System initiates payment request to the selected gateway or wallet.

**Step 3:** Payment succeeds:

- Order status changes to **Confirmed**
- Payouts executed to merchant, affiliate, referral, Droplinked, and external services as calculated
- Frontend is updated with confirmation

---

## **Journey 2 – Zero-Total Orders**

**Step 1:** Cart total after discounts = $0

**Step 2:** Payment step skipped

**Step 3:** Order status immediately set to **Confirmed**

**Step 4:** Payouts adjusted (if negative merchant share, deduct from credit)

---

## **Journey 3 – Failed Payment**

**Step 1:** Payment is attempted via selected gateway/wallet.

**Step 2:** Payment fails (insufficient funds, network error, expired card, etc.)

**Step 3:** System sets order status to **Failed**

**Step 4:** Log entry created with reason for failure

**Step 5:** Merchant credit lock released (if any)

---

## **Journey 4 – Pending Payment**

**Step 1:** Payment requires asynchronous confirmation (e.g., crypto transaction pending).

**Step 2:** System sets order status to **Pending**

**Step 3:** Polls gateway/network until final status

**Step 4a:** Payment succeeds → status changes to **Confirmed**

**Step 4b:** Payment fails → status changes to **Failed**

**Step 5:** Trigger payout or credit adjustments based on outcome

---

# **4) Business Acceptance Criteria (BAC)**

---

### **BAC 1:** Payment execution must support all enabled merchant payment methods.

### **BAC 2:** Zero-total orders bypass payment gateways and confirm immediately.

### **BAC 3:** Order status must reflect real-time payment outcome: Pending → Confirmed / Failed.

### **BAC 4:** Stakeholder payouts (merchant, affiliate, referral, Droplinked, external services) must execute according to rules in Payment Routing Engine (PRD 006).

### **BAC 5:** Failed payments must create detailed logs for troubleshooting.

### **BAC 6:** System must poll asynchronous payments until final confirmation or timeout.

### **BAC 7:** Customer-facing frontend/order confirmation must update automatically with payment status.

### **BAC 8:** Merchant credit locks applied during order creation (PRD 007) must be released if payment fails.