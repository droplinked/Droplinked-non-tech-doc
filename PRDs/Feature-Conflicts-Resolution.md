# Feature Conflicts Resolution & Integration Strategy

---

**Date:** 2026-02-16  
**PRDs Referenced:** PRD-001 through PRD-011  
**Conflicts Source:** Feature_Conflicts_Report.md


Based on the 68 conflict scenarios identified in the Feature Conflicts Report, this document provides resolution strategies and architectural decisions for implementing the 11 PRDs without critical conflicts. The features have been designed with awareness of interdependencies and edge cases.
---
## Critical Conflict Resolutions

### 1. Currency + Affiliate Network Conflict

**Conflict:** Affiliate Network requires USD, but merchants may want other currencies.

**Resolution:**
- **PRD-007 (Shop Currency):** Affiliate activation blocked if currency ≠ USD
- **PRD-008 (Affiliate):** Clear error message: "Affiliate requires USD. Change currency first."
- **Future Enhancement:** Multi-currency affiliate support (Phase 2)

**Implementation Order:**
1. Currency configuration (PRD-007)
2. Affiliate network (PRD-008) - with USD validation

---

### 2. Currency Change After Business Activities

**Conflicts:** #4, #8, #12, #50 - Currency changes with products, affiliates, pending orders

**Resolution:**
- **PRD-007 (Shop Currency):** Hard lock after:
  - First product created
  - Affiliate network activated
  - Any pending orders
- Clear messaging: "Create new shop for different currency"
- No exceptions without support intervention

**Data Strategy:**
- Credit balances tracked by currency (dual balance support)
- Historical orders keep original currency
- New orders use new currency (if change somehow allowed)

---

### 3. Payment Provider Currency Compatibility

**Conflicts:** #33, #34 - Currency change disables payment methods

**Resolution:**
- **PRD-007 (Shop Currency):** Pre-validation before currency change
- Display warning: "These providers don't support [CURRENCY]: PayPal, ..."
- Auto-disable incompatible providers on confirmation
- Prevent checkout errors by validating at source

**Provider Matrix:**
| Provider | USD | EUR | GBP | AED | TRY | JPY |
|----------|-----|-----|-----|-----|-----|-----|
| Stripe | ✓ | ✓ | ✓ | ✓ | ✗ | ✓ |
| PayPal | ✓ | ✓ | ✓ | ✗ | ✗ | ✓ |
| Coinbase | ✓ | ✓ | ✗ | ✗ | ✗ | ✗ |

---

### 4. Crypto Payment + Affiliate Split Settlement

**Conflicts:** #14, #15, #16 - Splitting crypto payments between affiliator and co-seller

**Resolution:**
- **PRD-006 (Direct Crypto Settlement):** Smart contract handles splits
- **PRD-008 (Affiliate):** Both parties can connect wallets
- Contract logic:
  ```solidity
  if (merchantWallet != address(0)) sendTo(merchantWallet)
  else sendTo(platformEscrow)
  
  if (affiliateWallet != address(0)) sendTo(affiliateWallet)
  else sendTo(platformEscrow)
  ```
- Atomic transaction - all succeed or all revert

**Key Decision:**
- If either party has no wallet → Both fall back to platform credit
- Prevents partial payment failures

---

### 5. Onchain Inventory + Affiliate Compatibility

**Conflicts:** #23, #24, #27 - Recorded products in affiliate network

**Resolution:**
- **PRD-010 (Onchain Inventory):** Only Affiliator can record their products
- **PRD-008 (Affiliate):** Co-sellers cannot record affiliate products
- Affiliator must set commission before recording

**Edge Case:**
- If Affiliator changes commission after Co-seller import but before recording:
  - New commission applies
  - Co-seller notified of change
  - Can remove product if unsatisfactory


**Conflicts:** #25, #29, #72 - Wallet requirements for different features

**Resolution:**
- **PRD-010 (Onchain Inventory):** Wallet locked per network after first recording
- **PRD-006 (Direct Crypto Settlement):** Can use same locked wallet
- **PRD-011 (Wallet Management):** Clear UI showing locked wallets
- One wallet per network across all features

**Decision Matrix:**
| Feature | Requires Wallet | Locks Wallet |
|---------|----------------|--------------|
| Record Product | Yes | Yes |
| Direct Settlement | Yes (optional) | No |
| Create Wallet | N/A | N/A |

---

### 7. Credit Balance Multi-Currency Handling

**Conflicts:** #1, #7, #31 - Credit in different currencies

**Resolution:**
- **PRD-007 (Shop Currency):** Track credits by currency
- Display format:
  ```
  Credit Balances:
  - EUR: €300.00
  ```
- Cannot mix currencies (no auto-conversion)
- Withdrawal requires selecting currency
- Debts tracked separately per currency

---

### 8. Order Cancellation with On-Chain Settlement


**Resolution:**
- **Policy Decision:** No automatic refunds for crypto payments
- **Reason:** Blockchain transactions are irreversible
- **Process:**
  1. Customer requests refund
  2. Merchant manually processes (if agreed)
  3. Platform facilitates communication
  4. If merchant unresponsive → Platform credit to customer (goodwill)
- Clear terms shown at checkout: "Crypto payments are final"

**Affiliate Order Cancellations:**
- If order cancelled before fulfillment:
  - Reverse credits (if not withdrawn)
  - If already withdrawn → Account shows negative balance
  - Merchant must repay to restore positive balance

---

### 9. Mixed Cart Scenarios

**Conflicts:** #43, #44, #45 - Cart with regular + affiliate + POD products

**Resolution:**
  - POD: Printful cost deducted

**Payment Splitting:**
- Single customer payment
- Platform disburses to multiple parties
- Each party gets separate line item in transaction

---

### 10. Currency Display vs. Settlement Currency

**Conflicts:** Request 5 (Local Currency Display) vs. PRD-007 (Shop Currency)

**Resolution:**
- **PRD-007 (Shop Currency):** Settlement in shop's base currency
- Clear messaging:
  - Storefront: "$99 USD (≈ €91) - Displayed prices are approximate"
  - Checkout: "All payments processed in USD"
- No confusion about actual transaction currency

---

## Integration Architecture

### System Dependencies Map

```
┌─────────────────────────────────────────────────────────────┐
│                    CORE INFRASTRUCTURE                       │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │   Currency   │  │   Wallets    │  │    Orders    │      │
│  │    System    │  │   (Circle/   │  │   Management │      │
│  │   (PRD-007)  │  │    Xion)     │  │              │      │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘      │
└─────────┼─────────────────┼─────────────────┼──────────────┘
          │                 │                 │
          ▼                 ▼                 ▼
┌─────────────────────────────────────────────────────────────┐
│                    FEATURE MODULES                           │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │   Affiliate  │  │   Onchain    │  │  Direct      │      │
│  │   Network    │  │   Inventory  │  │  Settlement  │      │
│  │   (PRD-008)  │  │   (PRD-010)  │  │  (PRD-006)   │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │    Local     │  │   Bulk       │  │   Page       │      │
│  │   Currency   │  │   Import     │  │   Builder    │      │
│  │   Display    │  │   (PRD-009)  │  │   (PRD-004)  │      │
│  │   (PRD-005)  │  │              │  │              │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
└─────────────────────────────────────────────────────────────┘
```

### Data Flow: Complex Order with Multiple Features

```
Customer Checkout (Mixed Cart)
    │
    ├─ Regular Product ($50)
    │   └─ Merchant A: $49 (after 2% fee)
    │
    ├─ Affiliate Product ($100)
    │   ├─ Affiliator (Merchant B): $83
    │   ├─ Co-seller (Merchant C): $15 (commission)
    │   └─ Platform: $2 (fee)
    │
    └─ Recorded Product ($75) [Onchain]
        ├─ Merchant D: $73.50
        ├─ Platform: $1.50 (fee)
        └─ NFT Transfer recorded on blockchain

Payment Processing:
    │
    ├─ Fiat: Platform receives $225 → Disburses per splits
    └─ Crypto: Smart contract receives $225 → Auto-splits

Settlement:
    ├─ If wallet connected → Direct to wallet
    └─ If no wallet → Credit balance
```

---

## Implementation Sequencing

### Phase 1: Foundation (Weeks 1-4)
1. **PRD-007:** Shop Currency Configuration
   - Enables all other currency-dependent features
   - Lock mechanism prevents future conflicts

2. **PRD-011:** Noncustodial Wallet Management
   - Required for crypto features
   - Wallet infrastructure must be ready first

### Phase 2: Core Commerce (Weeks 5-8)
3. **PRD-009:** Bulk Product Import
   - Helps merchants onboard faster
   - No major dependencies

4. **PRD-004:** Full Store Page Customization
   - Improves merchant experience
   - Independent of other features

5. **PRD-001:** Merchant Dashboard Onboarding
   - Introduces merchants to new features
   - Should be ready before marketing

### Phase 3: Advanced Features (Weeks 9-12)
6. **PRD-008:** Affiliate Network
   - Depends on: Currency (USD requirement), Wallets
   - Test with stable foundation

7. **PRD-006:** Direct Crypto Settlement
   - Depends on: Wallets, Affiliate (for splits)
   - Requires smart contract deployment

8. **PRD-010:** Onchain Inventory
   - Depends on: Wallets (for gas), Currency stability
   - Can run parallel to settlement

### Phase 4: Enhancement (Weeks 13-16)
9. **PRD-005:** Local Currency Display
   - Customer-facing enhancement
   - Display layer only (lower risk)

10. **PRD-003:** AEO and SEO Discovery
    - Marketing enhancement
    - No blocking dependencies

11. **PRD-002:** Remove Trial Plan
    - Business model change
    - Do last to minimize disruption during development

---

## Testing Strategy for Conflicts

### Integration Test Scenarios

**Scenario 1: Full Affiliate Flow**
```
Setup:
- Merchant A (Affiliator): USD currency, wallet connected
- Merchant B (Co-seller): USD currency, wallet connected
- Product: Recorded on blockchain

Flow:
1. Merchant A records product on Polygon
2. Merchant B imports product
3. Customer buys with USDC on Polygon
4. Verify:
   - Payment splits correctly
   - Both merchants receive to wallets
   - Transaction recorded on-chain
   - NFT ownership transfers
```

**Scenario 2: Currency Change Prevention**
```
Setup:
- Merchant: EUR currency, 1 active product

Flow:
1. Try to change currency to USD
2. Verify:
   - Blocked with clear message
   - "Create new shop" option presented
   - Affiliate activation blocked until USD
```

**Scenario 3: Mixed Cart Checkout**
```
Setup:
- Cart: Regular product + Affiliate product + Recorded product

Flow:
1. Customer checks out with crypto
2. Verify:
   - Smart contract handles all splits
   - Each party gets correct amount
   - Gas fees handled correctly
   - All transactions recorded
```

---

## Monitoring & Alerts

### Critical Metrics to Track

1. **Currency Conflicts:**
   - Failed currency changes (should be 0 after lock implementation)
   - Multi-currency credit balance issues

2. **Affiliate Settlement Failures:**
   - Split payment failures
   - Wallet connection issues at checkout

3. **Onchain Recording Issues:**
   - Failed recording transactions
   - Gas estimation errors
   - Wallet lock violations

4. **Mixed Cart Errors:**
   - Checkout failures with mixed product types
   - Incorrect commission calculations

### Alert Conditions

- **P0:** Smart contract split failures
- **P1:** Wallet lock violations (security issue)
- **P2:** Currency validation bypass attempts
- **P3:** High error rates on bulk import

---

## Conclusion

The 11 PRDs have been designed with awareness of their interdependencies. Key architectural decisions:

1. **Currency as Foundation:** Shop currency is immutable after key activities, preventing cascade issues
2. **Wallet Consistency:** One wallet per network across all features
3. **Atomic Operations:** Payment splits either fully succeed or fully fail
4. **Clear Fallbacks:** Missing wallets → Credit, incompatible providers → Disabled
5. **Explicit Blocking:** Features validate requirements before allowing activation

By following the implementation sequence and respecting the dependency map, the features can be deployed without the 68 conflicts materializing in production.

---

**Next Steps:**
1. Review this document with engineering team
2. Validate smart contract architecture for splits
3. Confirm currency lock enforcement strategy
4. Approve implementation sequence
5. Begin Phase 1 development
