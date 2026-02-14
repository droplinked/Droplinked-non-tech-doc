# Droplinked Conflict Solutions - Executive Summary

**Date:** 2026-02-12  
**Document:** Solutions for 68 Conflict Scenarios  
**Approach:** Systematic resolution with trade-offs

---

## SOLUTIONS BY CATEGORY

### **SHOP CURRENCY (7 Scenarios)**

**1. Credit Balance After Currency Change**  
**Solution:** Multi-currency balance tracking  
- Keep original currency balances separate
- Show normalized total in current base currency
- Allow withdrawal in original or converted currency

**2. Printful Integration with Non-USD Currency**  
**Solution:** Merchant bears risk with 3% buffer  
- Printful cost converted to shop currency at order time
- 3% exchange rate buffer added to protect merchant
- Real-time pricing with clear profit calculation

**3. Payment Provider Currency Support**  
**Solution:** Pre-validation with alternatives  
- Show impact before currency change
- Offer alternative payment providers
- Graceful degradation (disable unsupported methods)

**4. Existing Products After Currency Change**  
**Solution:** Grandfather strategy with bulk conversion option  
- Default: Keep products in original currency
- Offer bulk conversion tools
- Manual review option for each product

**5. Pending Orders During Currency Change**  
**Solution:** Immutable order currency  
- Orders complete in original currency
- Added to multi-currency balance
- No conversion of existing orders

**6. Negative Balance in Different Currency**  
**Solution:** Automatic cross-currency offset  
- USD credit can cover EUR debt
- Real-time rate at offset time
- Transparent transaction record

**7. Multi-Currency Credit Display**  
**Solution:** Normalized display with full breakdown  
- Show total in base currency (approximate)
- Display all balances with earning periods
- Include conversion rates used

---

### **AFFILIATE + CURRENCY (6 Scenarios)**

**1. Affiliate Requires USD But Shop Changes Currency**  
**Solution:** Allow all currencies with normalization  
- Remove USD-only requirement
- Automatic currency conversion
- 7-day advance notice to Co-sellers

**2. Mixed Currency Affiliate Order**  
**Solution:** Automatic normalization to Affiliator currency  
- Commission calculated in Affiliator's base currency
- Converted to Co-seller's currency at transaction time
- Rate locked at checkout

**3. Co-seller Currency Change After Import**  
**Solution:** Dynamic conversion at transaction time  
- Use Co-seller's current currency (not import-time)
- Real-time conversion
- Transparent rate display

**4. Non-USD Shop Tries to Activate Affiliate**  
**Solution:** Allow with currency warning  
- Remove currency restrictions
- Show notice about automatic conversion
- Support all major currencies

**5. Currency Change With Active Co-sellers**  
**Solution:** Graceful transition with 14-day notice  
- Products remain active
- Commission in new currency
- Impact analysis provided

**6. Commission Calculation Timing**  
**Solution:** Lock rate at checkout  
- Exchange rate locked for 24 hours
- Protects both parties from volatility
- Clear display of locked rate

---

### **CRYPTO + AFFILIATE (4 Scenarios)**

**1. Crypto Payment for Affiliate Product**  
**Solution:** Smart Contract Payment Router  
- Single transaction splits to both wallets
- Atomic operation (all or nothing)
- Both wallets must be connected

**2. Mixed Cart with Crypto Payment**  
**Solution:** Multi-line item router  
- Each line item tracked separately
- POD costs sent to Printful
- Single customer transaction

**3. Partial Payment Failure**  
**Solution:** Atomic transactions prevent partial failures  
- Smart contract ensures all-or-nothing
- If failure occurs, escalate to support
- Fallback to platform credit

**4. Crypto Payment Network Mismatch**  
**Solution:** Network validation + bridge option  
- Block mismatched networks
- Offer bridge for advanced users
- Clear alternative payment options

---

### **ORDER CANCELLATION + CRYPTO (5 Scenarios)**

**1. Cancelled Order with Crypto Payment**  
**Solution:** 7-day escrow period  
- Funds held in smart contract
- Refund available during escrow
- Post-escrow: platform mediation

**2. Cancelled Affiliate Order**  
**Solution:** Clawback system with debt tracking  
- Attempt recovery from balances
- Track deficits for future deduction
- Transparent communication

**3. Partial Refund for Crypto Payment**  
**Solution:** Percentage-based refund  
- Calculate refund proportionally
- Send crypto back to customer wallet
- Record on blockchain

**4. Dispute with On-Chain Settlement**  
**Solution:** Platform dispute resolution  
- Investigation period
- If merchant at fault: platform covers or bans
- If customer at fault: no refund

**5. Unshipped Product with On-Chain Settlement**  
**Solution:** Escrow prevents this  
- 7-day escrow gives time to verify shipping
- Post-escrow disputes handled case-by-case
- Reputation system tracks fulfillment

---

### **ONCHAIN INVENTORY + AFFILIATE (5 Scenarios)**

**1. Recorded Product Added to Affiliate**  
**Solution:** Commission separate from product data  
- Product metadata immutable (on-chain)
- Commission rate stored off-chain (mutable)
- No conflict between recording and affiliate

**2. Affiliated Product Then Recorded**  
**Solution:** Recording doesn't affect affiliate terms  
- Product becomes immutable
- Commission remains editable
- Co-sellers notified of NFT verification

**3. Wallet Lock Conflicts**  
**Solution:** Feature-specific permissions  
- Recording locks wallet per network
- Can add additional wallets for other features
- Clear permission management

**4. NFT Verification on Co-seller Shop**  
**Solution:** Show verification badge  
- NFT verification visible on Co-seller storefront
- Links to blockchain proof
- Builds customer trust

**5. Recorded Product Exported to Wallet**  
**Solution:** Export doesn't affect sales  
- NFT transferred to merchant wallet
- Product remains available for sale/affiliate
- Ownership tracked on-chain

---

### **ONCHAIN INVENTORY (5 Scenarios)**

**1. Bulk Recording Fee Ownership**  
**Solution:** Merchant pays, bulk discount applies  
- Merchant pays all gas fees
- 20% discount for bulk (50+ items)
- Failed transactions: gas lost, no NFT created

**2. Wallet Lock on Multiple Networks**  
**Solution:** Per-network lock only  
- Wallet locked only on specific network
- Can use different wallets on other networks
- Clear network-specific permissions

**3. Product Edit After Recording**  
**Solution:** Re-record as new version  
- Original NFT remains (historical record)
- New NFT created with corrections
- Link between versions in metadata

**4. Network Congestion During Recording**  
**Solution:** Queue system with gas price alerts  
- Queue transactions during congestion
- Alert when gas prices drop
- Option to speed up with higher gas

**5. Insufficient Gas Balance**  
**Solution:** Pre-validation + funding options  
- Check balance before starting
- Suggest gas funding options
- Clear error messages

---

### **PAYMENT PROVIDER + CURRENCY (3 Scenarios)**

**1. Currency Change Disables Payment Method**  
**Solution:** Option B - Auto-disable with warning  
- Don't block currency change
- Show clear warning
- Suggest alternatives

**2. Checkout with Disabled Payment Method**  
**Solution:** Hide unavailable methods  
- Don't show disabled methods
- If accessed directly: clear error message
- Suggest alternatives

**3. Stripe Currency Limitations**  
**Solution:** Stripe handles multi-currency  
- Stripe supports 135+ currencies
- Automatic conversion
- No separate flows needed

---

### **AFFILIATE NETWORK (7 Scenarios)**

**1. Commission Rate Change Timing**  
**Solution:** Rate locked at checkout  
- Commission rate from time of checkout
- Not from import time
- Protects both parties

**2. Affiliator Disables Affiliate**  
**Solution:** Grace period for pending orders  
- 7-day grace period
- Existing orders fulfilled
- New imports blocked

**3. Affiliator Deletes Product**  
**Solution:** Immediate removal with cart notification  
- Remove from Co-seller shops immediately
- Notify customers with product in cart
- Pending orders: fulfillment or refund

**4. Affiliator Changes Price**  
**Solution:** Price locked at checkout  
- Customer pays price at checkout time
- Not price at cart addition
- Real-time sync to Co-sellers

**5. Co-seller Removes Import**  
**Solution:** Allow checkout if in cart  
- Don't remove from active carts
- Complete order normally
- Remove from storefront only

**6. Role Across Multiple Shops**  
**Solution:** Role per shop, not per user  
- Can be Affiliator in Shop A, Co-seller in Shop B
- User-level roles don't exist
- Shop-level flexibility

**7. Affiliator Tries to Import**  
**Solution:** Allow with new shop  
- Create separate shop for Co-selling
- Or pause Affiliator status temporarily
- Clear error with workaround

---

### **MIXED CART (3 Scenarios)**

**1. Coupon with Mixed Cart**  
**Solution:** Show clear exclusion message  
- Display which items excluded
- Calculate discount on eligible items only
- Clear UX explanation

**2. Mixed Cart Checkout Flow**  
**Solution:** Line-item payout routing  
- Each item routed separately
- Regular → Merchant
- Affiliate → Split
- POD → Printful

**3. Shipping Calculation Mixed Cart**  
**Solution:** Item-level shipping display  
- Show shipping per item
- Each item ships from respective source
- Customer pays combined shipping

---

### **ORDER PROCESSING (5 Scenarios)**

**1. Merchant Credit Insufficient**  
**Solution:** Graceful checkout error  
- Generic error: "Payment processing issue"
- Don't expose internal credit system
- Alert merchant separately

**2. Order with Zero Total**  
**Solution:** Allow $0 orders  
- Commission = $0
- Co-seller gets nothing (fair)
- Track as marketing cost

**3. Payment Method Change During Checkout**  
**Solution:** Recalculate on method change  
- Update commission calculations
- Show merchant share preview
- Block if would go negative

**4. Affiliate Order Customer Info**  
**Solution:** Show to Co-seller with restrictions  
- Co-seller sees customer info
- Cannot contact directly
- Platform mediates if needed

**5. Fulfillment Status Sync**  
**Solution:** Real-time sync with timeout  
- Status syncs automatically
- If no update in 7 days: alert Co-seller
- Escalation path

---

### **COMMISSION CALCULATION (4 Scenarios)**

**1. Commission on Discounted Price**  
**Solution:** Commission on original price  
- Document clearly
- Co-seller benefits from full commission
- Affiliator absorbs discount impact

**2. Referral Share + Affiliate**  
**Solution:** Referral on final profit  
- After affiliate commission
- After platform fees
- On net amount only

**3. Droplinked Commission Stacking**  
**Solution:** Full transparency with breakdown  
- Show merchant complete breakdown
- Commission calculator tool
- Clear before activation

**4. Negative Commission Edge Case**  
**Solution:** Block low-margin products  
- Warn if margin < 20%
- Suggest higher prices
- Allow override with acknowledgment

---

### **TECHNICAL EDGE CASES (6 Scenarios)**

**1. Browser Close During Recording**  
**Solution:** State recovery system  
- Check blockchain for pending txs
- Resume from last completed step
- Email notification of status

**2. Transaction Failure After Broadcast**  
**Solution:** Retry with same gas  
- Don't charge gas again
- Show failure reason
- Offer retry or refund

**3. Double Recording Attempt**  
**Solution:** Deduplication check  
- Check for pending transactions
- Show "already recording" warning
- Prevent duplicate submissions

**4. Product Already Recorded**  
**Solution:** Block with update option  
- Prevent duplicate NFTs
- Offer "re-record as new version"
- Link old and new versions

**5. Wallet Disconnected During Recording**  
**Solution:** Graceful pause  
- Save progress
- Prompt reconnection
- Resume from saved state

**6. Network Unavailable**  
**Solution:** Status indicator + fallback  
- Show network health
- Disable unhealthy networks
- Retry automatically when available

---

### **CREDIT & PAYOUT (4 Scenarios)**

**1. Credit vs Payout Decision**  
**Solution:** Merchant preference setting  
- Choose default: credit or direct
- Override per transaction
- Clear in settings

**2. Coinbase Commerce Default**  
**Solution:** Clear communication  
- Explain why credit only
- Show expected timeline
- Alternative: connect wallet for direct

**3. Wallet Payment Wrong Chain**  
**Solution:** Hold in platform wallet  
- Queue until correct wallet connected
- Notify merchant
- Auto-convert if wallet never connected

**4. Affiliate Commission in Credit**  
**Solution:** Clear terms on signup  
- Show minimum withdrawal
- Explain accumulation
- Dashboard shows progress

---

### **NOTIFICATION & COMMUNICATION (4 Scenarios)**

**1. Currency Change Notification**  
**Solution:** Single summary email  
- One comprehensive email
- Dashboard notification
- Breakdown of all impacts

**2. Commission Change Notification**  
**Solution:** Email + Dashboard banner  
- Email to all affected Co-sellers
- Banner in dashboard
- 7-day advance notice

**3. Product Removal Notification**  
**Solution:** Immediate multi-channel  
- Email to Co-sellers
- Remove from storefronts
- Cart items: notify customers

**4. Failed Recording Notification**  
**Solution:** Technical + friendly  
- Friendly: "Recording failed"
- Technical details in expandable section
- Clear next steps

---

## IMPLEMENTATION PRIORITIES

### **Phase 1 (Critical - Week 1-2)**
1. Multi-currency balance tracking
2. Smart contract payment router
3. Currency change validation
4. Affiliate commission locking

### **Phase 2 (High - Week 3-4)**
5. Escrow system for crypto
6. Clawback system for cancellations
7. Network validation
8. Wallet lock management

### **Phase 3 (Medium - Week 5-6)**
9. Grandfather product migration
10. Notification system
11. Dispute resolution
12. Bulk recording optimization

### **Phase 4 (Low - Week 7-8)**
13. Advanced features
14. Performance optimization
15. Edge case handling
16. Documentation

---

## KEY TRADE-OFFS SUMMARY

| Conflict | Option Chosen | Rationale |
|----------|--------------|-----------|
| Credit Balance | Multi-currency tracking | Transparency, no loss |
| POD Currency | Merchant risk + 3% buffer | Fair, protects platform |
| Affiliate Currency | Allow all currencies | Growth, flexibility |
| Crypto Split | Smart contract router | Trustless, efficient |
| Order Cancellation | 7-day escrow | Protection for both |
| NFT Edit | Re-record as version | Immutability maintained |
| Commission Timing | Lock at checkout | Stability, fairness |

---

**Next Steps:** Update PDRs with these solutions integrated.
