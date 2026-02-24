# Merchant Share Engine

**Feature ID:** CHK-MER-005

**Category:** Checkout Domain → Revenue / Balance

**Actors:** Platform Engine, Merchant, Affiliate, Referrer

**Status:** Releassed

**Owner:** Behdad

---

# **1) Summary**

The **Merchant Share Engine** determines:

1. **Merchant Net Share** after subtracting all commissions & costs
2. Whether the merchant must pay additional amount (negative share)
3. Whether the merchant has enough credit to allow the order
4. How much credit is added to affiliate / referrer
5. How much credit (or wallet payout) merchant receives
6. Fails order creation if merchant credit is insufficient

This engine **runs AFTER** Cart Calculation, Shipping, Tax, and Commission Engines.

---

# **2) Inputs**

### From previous engines:

- `totalCart` (Total amount customer pays)
- `CommissionSummary.totalCommissions`
- `CommissionSummary.affiliateShare`
- `CommissionSummary.referralShare`

### From shop account:

- `merchant.creditBalance`
- `paymentMethod` (stripe / paypal / coinbase / wallet)
- `merchant.stripeConnected`
- `merchant.paypalConnected`
- `merchant.wallets` (per-chain + per-token mappings)

### From order/cart:

- Item-level details
- POD cost
- All commission components

---

# **3) Business Logic**

---

## **A) Merchant Share Formula**

Merchant gross revenue is:

```
merchantGross = totalCart

```

merchant net share:

```
merchantNet = totalCart
              - affiliateShare
              - referralShare
              - totalCommissions  // includes droplinked, podTax, podCost, shipping, provider fee…

```

Merchant net can be:

- **Positive** → merchant should receive money
- **Zero** → nothing to receive
- **Negative** → merchant must pay the difference from credit

---

## **B) Negative Merchant Share Logic**

If:

```
merchantNet < 0

```

Then merchant must cover deficit from shop credit:

```
requiredCredit = abs(merchantNet)

```

### **Rules**

- If `merchant.creditBalance >= requiredCredit`:
    
    → subtract credit & order can proceed
    
- If `merchant.creditBalance < requiredCredit`:
    
    → **order is BLOCKED**
    
    → Engine returns error `INSUFFICIENT_CREDIT`
    

This prevents “loss orders” when a merchant applies huge discounts.

---

## **C) Affiliate & Referral Payout Logic**

If referral or affiliate share > 0:

```
affiliate.credit += affiliateShare
referrer.credit += referralShare

```

These always go to **credit**, never cash-out here.

---

## **D) Merchant Payout Routing**

MerchantNet **if positive** should be routed according to payment method.

### **1) Stripe**

```
if paymentMethod == "stripe":
    if merchant.stripeConnected:
         send merchantNet to merchant.stripeAccount
    else:
         merchant.credit += merchantNet

```

### **2) PayPal**

```
if paymentMethod == "paypal":
    if merchant.paypalConnected:
         send merchantNet to merchant.paypalAccount
    else:
         merchant.credit += merchantNet

```

### **3) Coinbase Commerce**

```
if paymentMethod == "coinbase":
     merchant.credit += merchantNet

```

### **4) Wallet Payments**

If the customer paid with crypto:

```
if merchant has wallet address for selected chain+token:
       send merchantNet to merchant.walletAddress
else:
       merchant.credit += merchantNet

```

Regardless of routing, **all other shares (Droplinked, POD, shipping, etc.) go to Droplinked’s wallet.**

---

# **4) Outputs**

```json
{
  "merchantNet": 12.40,
  "creditConsumed": 0,
  "creditAddedToMerchant": 0,
  "affiliateCreditAdded": 3.20,
  "referralCreditAdded": 0.35,
  "payoutMethod": "stripe_account",
  "blockOrder": false,
  "currency": "USD"
}

```

If merchant credit is insufficient:

```json
{
  "blockOrder": true,
  "reason": "INSUFFICIENT_CREDIT",
  "merchantNet": -9.50,
  "requiredCredit": 9.50,
  "merchantCredit": 0
}

```

---

---

# **6) Business Acceptance Criteria (BAC)**

### **BAC 1** — MerchantNet = totalCart - all commissions

### **BAC 2** — Negative merchantNet requires credit deduction

### **BAC 3** — Block order if credit is insufficient

### **BAC 4** — Affiliate & referral credit are always added

### **BAC 5** — Merchant receives payout depending on payment method

### **BAC 6** — Wallet payout only works if merchant wallet exists

### **BAC 7** — If merchant is not connected to Stripe/PayPal → credited instead

### **BAC 8** — Coinbase payments → always credited

### **BAC 9** — Engine returns full routing + credit details

### **BAC 10** — No commission is ever taken from affiliate/referrer credit