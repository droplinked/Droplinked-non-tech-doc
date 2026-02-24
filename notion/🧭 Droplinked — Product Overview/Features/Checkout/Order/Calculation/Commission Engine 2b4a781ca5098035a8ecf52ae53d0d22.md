# Commission Engine

**Feature ID:** CHK-COM-004

**Category:** Checkout Domain → Cost/Commission Engines

**Actors:** Checkout Service, Tax Providers, Product Engine

**Status:** Released

**Owner:** Behdad

---

# **1) Summary**

The **Commission Engine** calculates all non-merchant shares, including:

- Droplinked commission
- External services costs (shipping providers, POD, etc.)
- Payment provider fees (Stripe / PayPal / Coinbase Commerce)
- Printful cost (POD manufacturing cost)
- Affiliate share
- Referral share

This engine **does not calculate merchant share** — that is Engine 005.

It only outputs **all costs that must be deducted from merchant totalCart revenue**.

---

# **2) Inputs**

### From other engines:

- `TotalItems`
- `totalShipping`
- `totalTax`
- `totalDiscount`
- `totalCart`
- `ShippingEngine.externalShippingCost`
- `TaxEngine.podTax`
- `PODBuildingCost` (sum of cost for all POD items)

### From cart items:

- item: `originalPrice`
- item: `quantity`
- item: `affiliateCommissionPercent`
- item: `productType` (physical | pod | digital)
- item: `cost` (for POD items)

### From shop:

- referralEnabled?
- affiliateEnabled?
- referralOfShop? (referrer user ID)

### From payment:

- paymentMethod = stripe | paypal | coinbase | wallet

---

# **3) Commission Components (Business Logic)**

## **A) Droplinked Commission**

Always:

```
DroplinkedCommission = TotalItems * 0.01

```

---

## **B) External Service Costs**

### **1) Shipping Providers**

From Shipping Engine:

```
ExternalShippingCost = externalShipping.total   // Easypost, Printful, etc.

```

---

### **2) POD Tax Paid to Printful**

From Tax Engine:

```
PODTaxToPrintful = podTax

```

---

### **3) POD Manufacturing Cost (Printful)**

From item costs:

```
PODCost = Σ (item.cost * item.quantity) WHERE item.type = 'pod'

```

---

## **C) Payment Provider Fees**

Based on **totalCart** after discount:

| Provider | Fee |
| --- | --- |
| Stripe | 3% |
| PayPal | 3% |
| Coinbase Commerce | 2% |
| Wallet | 0% |

Formula:

```
PaymentFee = totalCart * providerRate

```

---

## **D) Referral Share (1% of profit per product)**

Profit per product:

```
profit = (originalPrice - item.costIfPOD) * quantity

```

Referral share only if the shop WAS referred:

```
ReferralShare = Σ ( profit * 0.03 )

```

Digital product cost is considered **0**.

---

## **E) Affiliate Share**

Affiliate commission is based on each product’s **original price**, not discounted price.

```
AffiliateShare = Σ (originalPrice * quantity * commissionPercent)

```

Only if `affiliateCommissionPercent > 0`.

---

# **4) Outputs**

```json
{
  "droplinkedCommission": 1.25,
  "externalServiceCosts": {
    "shippingProviders": 4.00,
    "podTaxPaidToPrintful": 0.70,
    "podManufacturingCost": 10.50
  },
  "paymentProviderFee": 2.07,
  "referralShare": 0.35,
  "affiliateShare": 3.20,

  "totalCommissions": 21.07,
  "currency": "USD"
}

```

`totalCommissions` is:

```
DroplinkedCommission
+ ExternalShippingCost
+ PODTax
+ PODCost
+ PaymentProviderFee
+ ReferralShare
+ AffiliateShare

```

---

# **5) Commission Engine Output Schema**

```tsx
type CommissionSummary = {
  droplinkedCommission: number;

  externalServiceCosts: {
    shippingProviders: number;
    podTaxPaidToPrintful: number;
    podManufacturingCost: number;
  };

  paymentProviderFee: number;

  referralShare: number;
  affiliateShare: number;

  totalCommissions: number;
  currency: string;
}

```

---

# **6) Business Acceptance Criteria**

### **BAC 1** — Droplinked commission = 1% of TotalItems

### **BAC 2** — Payment provider fee rate is based on payment method

### **BAC 3** — Affiliate share uses original item price (not discounted)

### **BAC 4** — Referral share uses 1% of product profit only

### **BAC 5** — External shipping costs passed directly from Shipping Engine

### **BAC 6** — POD tax goes to Printful

### **BAC 7** — POD manufacturing cost is included as an external cost

### **BAC 8** — TotalCommissions is sum of all components

### **BAC 9** — This engine does NOT calculate merchant share

### **BAC 10** — Digital products never generate tax or cost, only affiliate/referral profit logic applies