# Calculation

---

## 1. User Journey & Purchase Flow

**The path from product selection to checkout:**

**Step 1: Product Selection**

- Users choose products from 3 categories: Physical, Digital, and Print on Demand (POD).

**Step 2: Customer Information**

- Email and phone number entry (mandatory).

**Step 3: Address & Taxation**

- If the cart contains Physical or POD products → shipping address is required.
- Tax is calculated based on the destination country. (Digital products are tax-exempt)

**Step 4: Shipping Selection**

- System groups products with similar shipping logic.
- Users select a shipping method for each group.

**Step 5: Discounts & Rulesets**

- General Discount: Percentage (1–100%) or fixed amount (Gift Cards)
- Smart Ruleset: Discounts applied to a specific product based on customer criteria

**Step 6: Payment**

- Users select the payment method (Crypto or Fiat) and complete the transaction

---

## 2. Calculation Engine (Logic Core)

### A) Mandatory Deductions (Droplinked Share)

Covers operational costs and is prioritized:

- Platform Fee: 1% of the Original Price (pre-discount)
- Payment Provider Fee: Variable (e.g., Stripe ~3%)
- Production Cost (POD): Manufacturing cost for POD items
- Shipping Cost: Logistics fees for non-custom shipping
- POD Tax: Taxes collected on POD items

**Droplinked Share** = Platform Fee + Payment Provider Fee + Shipping Cost + POD Production Cost + POD Tax

### B) Updated Revenue Split

After calculating Droplinked Share, the remaining balance is split between Referrer and Merchant:

1. **Referral Share**
- If a referral exists → 3% of (Total Paid by User minus Droplinked Share)
- If no referral exists → 0

**Referral Share** = (Total Paid by User – Droplinked Share) × 3%

1. **Merchant Share**
- Remaining amount after Platform Fee and Referral Share

**Merchant Share** = Total Paid by User – (Droplinked Share + Referral Share)

---

## 3. Payout Logic (Fund Flow)

| Scenario | Condition | Fund Routing |
| --- | --- | --- |
| Fiat Payment | Account Connected | Automated split: Merchant & Droplinked receive funds in their accounts |
| Fiat Payment | Not Connected | Total amount goes to Droplinked; Merchant's share added to Shop Credit |
| Crypto Payment | Wallet Connected | Smart Contract routes shares directly to Merchant’s and Droplinked’s wallets |
| Crypto Payment | Not Connected | Total amount goes to Droplinked; Merchant's share added to Shop Credit |

---

## 4. Negative Balance Protection

If customer payment (due to high discounts) is less than Droplinked Share:

- Deduct the difference from Merchant’s Shop Credit

**Deduction from Credit** = Droplinked Share – Total Paid by User

---

## Executive Summary

1. Droplinked Share is secured first to cover production and fees
2. 3% Referral Fee is deducted if applicable
3. Remaining balance goes to Merchant
4. If accounts are not connected, Merchant’s earnings are stored as Shop Credit

---

[**Cart Calculation Engine**
](Calculation/Cart%20Calculation%20Engine%202b4a781ca5098024a880c53136ee10d1.md)

[Shipping Engine](Calculation/Shipping%20Engine%202b4a781ca50980c286f8e7d2d5476844.md)

[Tax Engine](Calculation/Tax%20Engine%202b4a781ca50980cc855ed340d9a8e477.md)

[Commission Engine](Calculation/Commission%20Engine%202b4a781ca5098035a8ecf52ae53d0d22.md)

[Merchant Share Engine](Calculation/Merchant%20Share%20Engine%202b4a781ca509805898b4c1a19baa4fa2.md)

[Payment Routing Engine](Calculation/Payment%20Routing%20Engine%202b4a781ca5098004907bf80352f8b7ad.md)

[Order Creation Engine](Calculation/Order%20Creation%20Engine%202b4a781ca50980b685cdfe065bc06e2b.md)

[Payment Execution Engine](Calculation/Payment%20Execution%20Engine%202b4a781ca509809ba74bd48b29fcbab5.md)