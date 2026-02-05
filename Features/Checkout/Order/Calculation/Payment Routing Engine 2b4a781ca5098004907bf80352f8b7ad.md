# Payment Routing Engine

**Feature ID:** CHK-PAY-006

**Category:** Checkout Domain → Payment Processing

**Actors:** Customer, Payment Provider, Platform Engine

**Status:** Draft

**Owner:** Behdad

---

# **1) Summary**

### **Problem / Value**

After commission distribution and merchant share calculation, the system must route all payouts (merchant share, referral share, affiliate share, service provider fees, Droplinked commission, external service costs) to the correct destinations.

Each payment method (Stripe, PayPal, Coinbase Commerce, Wallet) has different rules for where funds get delivered.

The routing engine must centralize this logic, ensure correct fund flows, and guarantee consistent handling across all payment types.

### **Desired Outcome**

- Route merchant share, referral share, and affiliate share correctly based on the selected payment method.
- Ensure external service costs and Droplinked commissions always move to Droplinked wallets.
- Correctly determine when merchant should receive **payout** vs **credit top-up**.
- Handle merchants with or without connected payment accounts.
- Handle wallet payments with network & token-specific routing.
- Guarantee routing consistency across all order types (POD, physical, digital).
- Expose a clean API interface for Order Creation Engine (007).

---

# **2) Scope – In / Out**

### **In Scope**

- Routing logic for **all payout destinations**:
    - Merchant Share
    - Affiliate Share
    - Referral Share
    - Droplinked Commission
    - External Service Costs
        - Printful Costs
        - External Shipping Costs (easypost, printful shipping)
- Routing logic per **payment method**:
    - Stripe
    - PayPal
    - Coinbase Commerce
    - Wallet
- Detect whether merchant has a connected account.
- Fallback to **credit** when payout destination is not connected/available.
- Produce final routing instructions for Order Creation Engine.
- Handle negative merchant shares (merchant owes money → reduce credit).

### **Out of Scope**

- Actual charge collection (handled in **008 – Payment Execution Engine**)
- Credit locking or validation (handled in **007 – Order Creation Engine**)
- Commission calculation (handled in **004 – Commission Engine**)
- Merchant share calculation (handled in **005 – Merchant Share Engine**)
- Creation of withdrawable merchant payouts (handled in wallet/financial subsystem)

---

# **3) Key User Journeys**

---

## **Journey 1 – Stripe Payment (Merchant Has Connected Stripe)**

**Step 1:** Customer pays using Stripe.

**Step 2:** Engine checks merchant’s Stripe connection.

**Step 3:** Merchant share is routed **directly to merchant’s Stripe account**.

**Step 4:** Affiliate & referral shares → **increase user credit**.

**Step 5:** Droplinked commission + external costs → **Droplinked wallet**.

**Step 6:** Routing summary returned to Order Creation Engine.

---

## **Journey 2 – Stripe Payment (Merchant Does NOT Have Connected Stripe)**

**Step 1:** Customer pays with Stripe.

**Step 2:** Merchant has no connected Stripe → fallback.

**Step 3:** Merchant share → **merchant credit**.

**Step 4:** Affiliate & referral → **credit**.

**Step 5:** Droplinked commission + external costs → **Droplinked wallet**.

---

## **Journey 3 – PayPal Payment**

**Logic same as Stripe**:

- If merchant has connected PayPal → payout to PayPal.
- Otherwise → add to merchant credit.
- All other payouts follow normal rules.

---

## **Journey 4 – Coinbase Commerce Payment**

**Step 1:** Customer pays via Coinbase.

**Step 2:** Merchant payout rules:

- Merchant **never receives crypto directly**, unless wallet connected.
- Merchant share → **credit** (default behavior).
    
    **Step 3:** Affiliate & referral → **credit**.
    
    **Step 4:** Droplinked + external costs → **Droplinked wallet**.
    

---

## **Journey 5 – Wallet Payment**

**Step 1:** Customer pays with Droplinked Wallet.

**Step 2:** Payment token + chain identified.

**Step 3:** Merchant wallet routing:

- If merchant wallet for **same chain & token exists** → payout to merchant wallet.
- If no merchant wallet → payout goes to **merchant credit**.
    
    **Step 4:** Affiliate & referral → credit.
    
    **Step 5:** Droplinked fees & external costs → Droplinked wallet.
    

---

# **4) Business Acceptance Criteria (BAC)**

---

### **BAC 1:** Routing Engine must produce deterministic routing results for every order.

### **BAC 2:** Merchant share must route to merchant’s connected payment account when possible:

- Stripe Connect
- PayPal partner
- Wallet (same network + token)

### **BAC 3:** If merchant payout destination is unavailable, share must be added to merchant credit.

### **BAC 4:** Affiliate share must always be paid to affiliate credit.

### **BAC 5:** Referral share must always be paid to referrer credit.

### **BAC 6:** Droplinked commission, external shipping costs, Printful costs must always route to Droplinked wallet.

### **BAC 7:** Coinbase Commerce payments must never directly pay merchants; merchant share must always route to credit.

### **BAC 8:** Wallet payments must enforce chain + token matching before allowing direct merchant payout.

### **BAC 9:** Routing Engine must generate a structured routing object for the Order Creation Engine.

### **BAC 10:** Negative merchant share (merchant owes Droplinked) must result in:

- Deducting merchant credit
- Failing order creation if credit insufficient (checked in PRD 007)