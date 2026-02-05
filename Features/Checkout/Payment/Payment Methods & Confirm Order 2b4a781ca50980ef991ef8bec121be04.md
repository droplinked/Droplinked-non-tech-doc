# Payment Methods & Confirm Order

# Payment Methods & Confirm Order

**Feature ID:** CHK-PAYMENT-404

**Category:** Checkout Domain | Payment

**Actors:** Customer

**Status:** Defined

**Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

Customers need a reliable way to pay for their cart using any of the merchant-enabled payment methods. The system must handle both zero-total carts and paid carts, process payments with various gateways and tokens, and track order status based on payment outcome.

### **Desired Outcome**

- If the cart total is **0** after discounts, skip payment methods and allow order confirmation.
- If the cart total > 0, show only merchant-enabled payment methods.
- Support multiple payment types including fiat gateways (Stripe, PayPal, Coinbase) and crypto tokens across different networks.
- Once a payment method is selected and submitted, create an order and allow up to **2 hours** for payment completion.
    - **No timer displayed** on confirmation page
    - If customer attempts payment after 2 hours → **New order is automatically created**
    - Expired orders have no impact on inventory or cart state
- Confirm the order automatically if payment succeeds; mark as failed if payment fails and log the failure.

---

# **2) Scope – In / Out**

### **In Scope**

- Display only merchant-enabled payment methods.
- Payment methods include:
    - **Fiat Gateways:** Stripe, PayPal, Coinbase
    - **Native Tokens:**
        - BNB, USD Coin (Networks: BINANCE, SKALE, BITLAYER, POLYGON, ETH)
        - Tether USDt (Network: XRPLSIDECHAIN)
        - XRP
        - ETH (Network: Base)
- Allow customers to select a payment method.
- Handle zero-total carts by hiding payment selection and showing **Confirm Order** directly.
- Create an order upon clicking “Pay / Confirm Order” and start a 2-hour payment window. - No timer displayed to customer
    - After 2 hours: New order created if payment attempted
    - No impact on inventory or cart- Update order status:
        - Payment success → Confirmed
        - Payment failure → Failed + log entry
- Display success/failure feedback to the customer.

### **Out of Scope**

- Partial payments or split payments.
- Refund or chargeback flows.
- Manual payment processing by merchant.
- External wallet management outside supported tokens/networks.

---

# **3) Key User Journeys**

---

## **Journey 1 – Zero-Total Cart**

**Step 1:** Customer proceeds to checkout.

**Step 2:** Cart total after discounts = 0.

**Step 3:** Payment methods are hidden.

**Step 4:** Customer clicks **Confirm Order**.

**Step 5:** Order is created and immediately confirmed.

---

## **Journey 2 – Paid Cart with Merchant-Enabled Payment Methods**

**Step 1:** Customer proceeds to checkout.

**Step 2:** Cart total > 0.

**Step 3:** System displays only enabled payment methods.

**Step 4:** Customer selects a payment method.

**Step 5:** Customer clicks **Pay / Confirm Order**.

**Step 6:** Order is created and 2-hour payment window starts (no timer shown).

**Step 7a:** Payment successful → order status updated to **Confirmed**.

**Step 7b:** Payment fails → order status updated to **Failed** and log entry created.

**Step 7c:** Payment attempted after 2 hours → new order automatically created.

**Step 8:** Customer receives payment result feedback.

---

# **4) Business Acceptance Criteria (BAC)**

---

### **BAC 1:** Zero-total carts must skip payment selection and allow direct order confirmation.

### **BAC 2:** Only merchant-enabled payment methods are displayed to the customer.

### **BAC 3:** Supported payment methods include:

- **Fiat:** Stripe, PayPal, Coinbase
- **Tokens:**
    - BNB, USD Coin (BINANCE, SKALE, BITLAYER, POLYGON, ETH)
    - Tether USDt (XRPLSIDECHAIN)
    - XRP
    - ETH (Base)

### **BAC 4:** Customer can select a payment method and submit payment.

### **BAC 5:** An order is created upon submission. The 2-hour payment window starts (no timer displayed). If payment attempted after 2 hours, a new order is created.

### **BAC 6:** Successful payment → order status **Confirmed**.

### **BAC 7:** Failed payment → order status **Failed** and logged in system.

### **BAC 8:** Customer receives immediate feedback about payment success or failure.