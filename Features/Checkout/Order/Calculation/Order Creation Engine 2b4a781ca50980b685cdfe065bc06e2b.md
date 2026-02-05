# Order Creation Engine

**Feature ID:** ORD-CREATE-007

**Category:** Checkout / Backend | Order System

**Actors:** Customer, System

**Status:** Draft

**Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

When a customer completes checkout, the system needs to validate the order, lock necessary credits, and create an official order record. This engine ensures orders cannot proceed if the merchant credit is insufficient or if other validations fail. It acts as a bridge between the cart calculation, commission calculation, payment routing, and payment execution engines.

### **Desired Outcome**

- Validate order totals, applied discounts, taxes, and shipping fees.
- Ensure merchant, affiliate, and referral shares can be paid or credited correctly.
- Lock merchant credit if necessary before order creation.
- Prevent order creation if merchant credit is insufficient for negative balances.
- Generate complete order records for subsequent payment execution.
- Trigger routing instructions for all payouts (merchant, affiliate, referral, Droplinked, external service costs).

---

# **2) Scope – In / Out**

### **In Scope**

- Validate **Total Cart Calculation** (from PRD 001 – Cart Calculation Engine).
- Validate **Shipping, Tax, Discounts, and Promotions**.
- Check **merchant credit** to cover potential negative payouts.
- Trigger **Payment Routing Engine** (PRD 006) to determine all payout destinations.
- Lock merchant credit temporarily until payment is confirmed.
- Create **Order Record** in the system with:
    - Cart snapshot
    - Payment routing instructions
    - Calculated shares (merchant, affiliate, referral, Droplinked, external costs)
- Reject order creation if validation fails.
- Expose structured order data for Payment Execution Engine (PRD 008).

### **Out of Scope**

- Actual payment processing (handled in PRD 008 – Payment Execution Engine).
- Customer notifications/email (handled by separate service).
- Refunds or cancellations.
- Modifying cart items after order creation.

---

# **3) Key User Journeys**

---

## **Journey 1 – Successful Order Creation**

**Step 1:** Customer clicks **Checkout** on storefront.

**Step 2:** System validates:

- TotalItems, TotalShipping, TotalTax, TotalDiscount
- Payment method availability
- Merchant credit for potential negative balance

**Step 3:** If validation passes:

- Lock merchant credit (if needed)
- Trigger Payment Routing Engine
- Create order record with all necessary data

**Step 4:** Return order confirmation to frontend / Order Confirmation Page (PRD 008).

---

## **Journey 2 – Merchant Credit Insufficient**

**Step 1:** Customer clicks **Checkout**.

**Step 2:** System calculates merchant share after deductions (affiliate, referral, Droplinked, external costs).

**Step 3:** If merchant share is negative and merchant credit is insufficient:

- Block order creation
- Return error to frontend explaining insufficient credit
- No order record is created

---

## **Journey 3 – Routing Trigger**

**Step 1:** Order is validated and ready to create.

**Step 2:** Engine triggers Payment Routing Engine (PRD 006) to generate payout instructions.

**Step 3:** Routing object is attached to order record for Payment Execution Engine.

---

## **Journey 4 – Failed Validation**

**Step 1:** System detects:

- Invalid cart total
- Invalid discounts
- Invalid or missing shipping information

**Step 2:** Order creation is blocked.

**Step 3:** Frontend receives clear error messages to fix issues before retrying checkout.

---

# **4) Business Acceptance Criteria (BAC)**

---

### **BAC 1:** Order cannot be created if merchant credit is insufficient to cover negative balance.

### **BAC 2:** Order creation must lock merchant credit temporarily until payment confirmation.

### **BAC 3:** Payment routing instructions must be generated and attached to order.

### **BAC 4:** Order record must include:

- Cart snapshot
- Calculated shares (merchant, affiliate, referral, Droplinked, external costs)
- Discounts, taxes, shipping, promotions

### **BAC 5:** System must return structured order data to frontend for the Order Confirmation page.

### **BAC 6:** Invalid carts, discounts, or shipping info must block order creation with appropriate error messages.

### **BAC 7:** Order creation engine must be idempotent — re-triggering creation for the same cart cannot create duplicate orders.