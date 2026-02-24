# Order – Post-Payment Calculations & Payout Distribution

### Feature ID:

**PAY-SETTLE-001**

### Title:

**Settlement & Payout Distribution – After Order Paid & Confirmed**

### Category:

**Checkout / Payments / Settlement** | **Actors**: Customer, Merchant (Seller), Product Owner, Referring Shop, Droplinked, Payment Processor | **Channel**: Storefront / Payment Gateway / Settlement

### Status:

**Defined**

### Owner:

Behdad

---

### 1) **Summary**

**Problem/Value:**

Once an order is paid and confirmed, Droplinked (as the platform) receives or orchestrates the full payment from the provider (Stripe, Coinbase Commerce, or crypto smart contract) and then calculates and distributes the shares of each party. Droplinked itself is not treated as a “shop” receiving credits; it is the collector and distributor. Without deterministic rules and a predictable fallback (credits), disputes and reconciliation costs spike.

**Desired Outcome:**

- Canonical **shares** are calculated using explicit formulas.
- Funds land either directly in parties’ wallets/accounts (smart contract or connected Stripe) or in Droplinked’s account when a direct payout is not possible.
- Distribution follows **payment-method–specific** rules (direct payout when possible, otherwise **shop credit**).
- **Shortfall** scenarios (customer paid less than Droplinked share due to merchant discounts) are covered by **merchant shop credit**.
- Settlement is **idempotent**, fully logged, and auditable.

---

### 2) **Definitions & Inputs**

- **Gross Product Total (pre-discount)** = Total of all product prices before discounts.
- **Amount Paid** = Final paid amount by the customer (after discounts; includes taxes/shipping charged to customer).
- **Printful Charges** = Sum of POD product costs + POD shipping + POD product taxes.
- **EasyPost Charges** = EasyPost shipping amount.
- **Droplinked Commission (DrpComm)** = 1% × Gross Product Total (pre-discount).
- **Stripe Processor Fee (StripeFee)** = 3% × Amount Paid if payment method = Stripe; otherwise 0.
- **Precision**: compute in minor units; round to 2 decimals at output; resolve rounding remainder in favor of merchant.

---

### 3) **Share Formulas (Cart Level)**

1. **Droplinked Share**

```
DroplinkedShare = DrpComm + StripeFee + PrintfulCharges + EasyPostCharges

```

1. **Referring Shop Share**

```
ReferrerShare = max(0, 3% × (AmountPaid − DroplinkedShare))

```

> ReferrerShare is either zero or positive; never negative.
> 
1. **Merchant Share**

```
MerchantShare = AmountPaid − DroplinkedShare − ReferrerShare

```

> MerchantShare can be positive, zero, or negative (negative implies a debit to merchant’s shop credit).
> 

---

### 4) **Distribution Rules by Payment Method**

### 4.1 Coinbase Commerce

- Funds are collected into Droplinked’s Coinbase Commerce account.
- Droplinked:
    - Credits the Referrer’s shop credit with their share.
    - Credits the Merchant’s shop credit with their share (can be negative → debit).
    - Keeps its own share as actual funds in Droplinked’s account (not as shop credit).

### 4.2 Stripe (Fiat)

- Funds are collected into Droplinked’s Stripe account.
- For each party:
    - If they have a connected Stripe account → Droplinked pays their share directly via Stripe transfer.
    - If not connected → Droplinked books their share as shop credit.
- Droplinked’s share (including StripeFee) remains as actual funds in Droplinked’s Stripe account.

### 4.3 Native Crypto (multi-network, multi-token)

- When the customer selects a crypto network and token, Droplinked creates a **smart-contract payment transaction** encoding all recipient addresses and shares.
- **If every party (merchant, referrer, Droplinked) has a wallet configured for that network/token:**
    - The customer pays into the smart contract.
    - The contract automatically splits and sends each share directly on-chain to each party’s wallet.
    - Funds never pass through Droplinked’s wallet.
- **If one or more parties lack a compatible wallet:**
    - The smart contract still sends each configured recipient’s share directly on-chain to their wallet.
    - Only the shares belonging to parties without a compatible wallet are routed to Droplinked’s settlement wallet.
    - Droplinked posts equivalent shop credit for those parties without wallets (holding their funds in custody).
- Droplinked’s own share is received directly from the smart contract when a Droplinked wallet is configured; otherwise it remains in Droplinked’s settlement wallet.

---

### 5) **Shortfall Handling (Merchant Discount Underfunding)**

If **AmountPaid < DroplinkedShare**:

```
Shortfall = DroplinkedShare − AmountPaid
Debit Merchant credit by Shortfall

```

Examples:

- DroplinkedShare = $50, AmountPaid = $40 → $10 is debited from Merchant’s shop credit.
- Crypto case: $40 distributed by smart contract; $10 remaining is collected by debiting Merchant credit.

---

### 6) **Execution Sequence (Idempotent)**

1. Funds arrive into Droplinked’s Stripe account / Coinbase account / smart contract triggers on-chain split.
2. Compute shares (Droplinked, Referrer, Merchant) using Section 3 formulas.
3. Handle any shortfall by debiting Merchant shop credit.
4. Distribute:
    - Stripe transfers, on-chain sends (smart contract or Droplinked wallet), or shop-credit postings.
    - If any direct transfer fails → fallback to shop credit and log error.
5. Record ledger (inputs, outputs, tx hashes, credits).
6. Settlement complete for the order (idempotency key prevents double settlement).

---

### 7) **Key User Journeys**

- **Journey 1 – Stripe Payout Mix**
    - Merchant connected to Stripe; Referrer not.
    - **Result:** Merchant gets Stripe transfer; Referrer gets shop credit; Droplinked retains its share as actual funds.
- **Journey 2 – Coinbase Commerce**
    - Payment completes on Coinbase.
    - **Result:** Droplinked holds actual funds; Merchant and Referrer receive shop credits.
- **Journey 3 – Native Crypto Smart Contract Split**
    - All parties have wallets on the selected network/token.
    - **Result:** Smart contract pays Merchant, Referrer, and Droplinked directly on-chain; Droplinked never holds the coins.
- **Journey 4 – Native Crypto Fallback**
    - One or more parties lack wallets.
    - **Result:** Smart contract sends full amount to Droplinked wallet; Droplinked pays out on-chain to those who have wallets and credits those who don’t.
- **Journey 5 – Discount Shortfall**
    - AmountPaid < DroplinkedShare.
    - **Result:** Shortfall debited from Merchant shop credit; Droplinked’s share recovered in full.

---

### 8) **Business Acceptance Criteria (BAC)**

- **BAC 1:** System calculates DroplinkedShare, ReferrerShare, MerchantShare exactly as defined (Section 3). → Pass/Fail
- **BAC 2:** ReferrerShare ≥ 0 always; MerchantShare may be negative. → Pass/Fail
- **BAC 3:** Shortfall (if any) is debited from Merchant shop credit; settlement proceeds. → Pass/Fail
- **BAC 4:** Coinbase Commerce: funds land in Droplinked’s account; other parties get shop credits. → Pass/Fail
- **BAC 5:** Stripe: funds land in Droplinked’s Stripe account; parties with connected Stripe accounts receive direct payouts; others receive shop credits. → Pass/Fail
- **BAC 6:** Native Crypto Smart Contract: when all wallets exist, funds are split automatically on-chain and Droplinked never holds them; when wallets are missing, funds flow to Droplinked wallet with shop credit fallback. → Pass/Fail
- **BAC 7:** Failures in direct payouts fallback to shop credit with error logs; no duplicate settlements (idempotent). → Pass/Fail
- **BAC 8:** Full ledger (inputs, formulas, outputs, tx refs) is stored immutably per order. → Pass/Fail

---

### 9) **Data & Audit**

- **Order.PayoutBreakdown**: all inputs, computed shares, rounded outputs.
- **Transfers**: list of attempted & completed transfers (type, channel, tx/hash/ref, amount, status).
- **Credits**: postings per party (amount, currency, reason, ref).
- **Idempotency Key**: settlement operation key to prevent replays.
- **Timestamps**: computed_at, executed_at.

---

### 10) **Out of Scope**

- Multi-currency conversion/FX.
- Refunds/chargebacks/reversals.
- Tax determination rules (handled elsewhere).
- Post-payment fulfillment (Printful/EasyPost/NFT claim) – specified in other features.

---

### 11) **Worked Micro-Example**

- Gross Product Total (pre-discount) = $1,000
- Amount Paid (after discounts) = $940
- DrpComm = 1% × 1,000 = $10
- StripeFee (Stripe payment) = 3% × 940 = $28.20
- PrintfulCharges = $22.00
- EasyPostCharges = $5.80

DroplinkedShare = 10 + 28.20 + 22 + 5.80 = $66.00

ReferrerShare = max(0, 3% × (940 − 66)) = 3% × 874 = $26.22

MerchantShare = 940 − 66 − 26.22 = $847.78

(If the discount were larger and AmountPaid dropped to $50 while DroplinkedShare stayed $66, then $16 shortfall is debited from Merchant shop credit.)