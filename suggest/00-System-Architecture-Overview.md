# Droplinked System Architecture Overview

## Executive Summary

This document provides a comprehensive architectural overview of the Droplinked e-commerce platform, specifically addressing the integration of five core features and their interdependencies. The design prioritizes **payment routing flexibility**, **currency consistency**, and **transparent ownership tracking** while learning from best practices of Shopify, Stripe Connect, Amazon Associates, and OpenSea.

---

## Core Features Matrix

| Feature | Primary Goal | Key Stakeholders | Complexity Level |
|---------|-------------|------------------|------------------|
| **Wallet & Payment Provider Connection** | Enable direct merchant payouts | Merchants, Customers | Medium |
| **Shop Currency System** | Multi-currency pricing support | Merchants | High |
| **Affiliate Network** | Cross-merchant product selling | Affiliators, Co-sellers | Very High |
| **Product NFT Recording** | Blockchain product certification | Merchants, Customers | High |
| **Non-Custodial Wallet Management** | Self-custody crypto payments | Merchants | High |

---

## Critical Interdependencies & Conflict Zones

### Zone 1: Currency + Affiliate Network (CRITICAL)
**The Problem:**  
Affiliate commissions require currency consistency between Affiliator and Co-seller shops. If Affiliator is USD and Co-seller switches to EUR, commission calculations break.

**The Solution:**  
- **Currency Lock During Active Affiliations**: When products are actively being affiliated, currency changes are blocked or require partner notification
- **Real-time Conversion at Transaction Time**: Commissions calculated at order time using current exchange rates
- **Base Currency Normalization**: All affiliate transactions settle in Affiliator's base currency

**Learned from:** Amazon Associates (currency normalization), Shopify Markets (currency display vs settlement)

---

### Zone 2: Payment Provider + Currency Support (HIGH)
**The Problem:**  
PayPal doesn't support all currencies (e.g., TRY). Merchant changes currency, payment method becomes invalid.

**The Solution:**  
- **Dynamic Payment Method Validation**: Real-time check of payment provider currency support
- **Graceful Degradation**: Unsupported payment methods hidden at checkout, not broken
- **Migration Wizard**: Currency change shows impact analysis before confirmation

**Learned from:** Stripe (dynamic payment method availability), PayPal (currency limitations documentation)

---

### Zone 3: Crypto Payment + Affiliate Split (CRITICAL)
**The Problem:**  
Customer pays $100 USDC. Commission is 15% ($15 to Co-seller, $85 to Affiliator). Single transaction can't split to two wallets.

**The Solution:**  
- **Smart Contract Payment Router**: Single customer transaction → platform smart contract → automatic split to both merchants
- **Escrow + Settlement Flow**: Platform receives funds first, then executes split settlements
- **Atomic Transactions**: All-or-nothing settlement (prevents partial payment scenarios)

**Learned from:** Stripe Connect (destination charges), Superfluid (money streaming), Gnosis Safe (multi-sig splits)

---

### Zone 4: NFT Recording + Product Mutability (MEDIUM)
**The Problem:**  
Recorded products become immutable (NFT on blockchain), but Affiliators may need to change commission rates.

**The Solution:**  
- **Separation of Concerns**: Product metadata (immutable) vs Business rules (mutable)
- **Commission as Configurable Parameter**: Stored off-chain or in upgradeable contract
- **Version Control**: Product updates create new NFT versions with lineage tracking

**Learned from:** OpenSea (collection vs token metadata), EIP-721/1155 standards

---

### Zone 5: Non-Custodial Wallets + Multi-Network (HIGH)
**The Problem:**  
Merchant connects wallet on Polygon. Customer pays on Ethereum. Merchant doesn't have Ethereum wallet configured.

**The Solution:**  
- **Network-Agnostic Wallet Registry**: Merchants can connect multiple wallets across networks
- **Cross-Chain Payment Routing**: Platform accepts any supported network, handles bridging/settlement
- **Default Wallet Per Network**: Clear UX showing which wallet receives which network's payments

**Learned from:** MetaMask (multi-network support), WalletConnect v2 (chain abstraction)

---

## Architectural Principles

### 1. **Payment Abstraction Layer**
All payments flow through a unified abstraction layer that handles:
- Currency conversion at transaction time
- Payment method routing
- Commission splitting
- Settlement to merchant wallets or credit balances

```
┌─────────────────────────────────────────────┐
│         Payment Abstraction Layer           │
├─────────────────────────────────────────────┤
│  ┌─────────────┐      ┌──────────────────┐ │
│  │   Fiat      │      │      Crypto      │ │
│  │ (Stripe,    │      │ (Wallet Direct,  │ │
│  │  PayPal)    │      │  Smart Contract) │ │
│  └──────┬──────┘      └────────┬─────────┘ │
│         │                      │           │
│         └──────────┬───────────┘           │
│                    ▼                       │
│         ┌─────────────────────┐            │
│         │ Commission Engine   │            │
│         │ - Split calculation │            │
│         │ - Currency conv     │            │
│         └──────────┬──────────┘            │
│                    ▼                       │
│         ┌─────────────────────┐            │
│         │ Settlement Router   │            │
│         │ - Direct wallet     │            │
│         │ - Credit balance    │            │
│         └─────────────────────┘            │
└─────────────────────────────────────────────┘
```

### 2. **Currency Normalization Strategy**
- **Display Currency**: What customer sees (can be different per customer/session)
- **Shop Currency**: Merchant's base accounting currency
- **Settlement Currency**: Actual currency of payment
- **Affiliate Currency**: Always Affiliator's shop currency (normalized)

### 3. **Wallet Connection Modes**
| Mode | Use Case | Control | Complexity |
|------|----------|---------|------------|
| **Custodial (Credit)** | Fiat payments, new merchants | Platform holds funds | Low |
| **Connected Wallet** | Crypto payments, Web3 natives | Direct settlement | Medium |
| **Non-Custodial** | Full self-sovereignty | Merchant controls everything | High |

### 4. **Affiliate Network State Machine**
```
┌──────────────┐     Import      ┌──────────────┐
│   Affiliator │◄────────────────│   Co-seller  │
│   Products   │                 │   Products   │
└──────┬───────┘                 └──────┬───────┘
       │                                │
       │ Commission Rate Change         │ Price Sync
       │ (Notifies Co-sellers)          │ (From Affiliator)
       ▼                                ▼
┌──────────────┐                 ┌──────────────┐
│   Sale       │                 │   Customer   │
│   Occurs     │────────────────►│   Purchase   │
└──────┬───────┘                 └──────┬───────┘
       │                                │
       └────────────┬───────────────────┘
                    ▼
           ┌────────────────┐
           │ Commission     │
           │ Split:         │
           │ - Platform fee │
           │ - Co-seller %  │
           │ - Affiliator % │
           └────────────────┘
```

---

## Best Practices from Industry Leaders

### From **Shopify Markets**:
- ✅ Currency conversion at checkout time (not at product creation)
- ✅ Local currency display with "approximate" indicators
- ✅ Payment method availability based on customer location AND merchant settings

### From **Stripe Connect**:
- ✅ Destination charges (platform receives payment, splits to merchants)
- ✅ On_behalf_of charging (merchant appears as charge owner)
- ✅ Transfer grouping for reduced fees

### From **Amazon Associates**:
- ✅ Standardized commission structures by category
- ✅ 24-hour cookie window for attribution
- ✅ Cross-border affiliate support with currency normalization

### From **OpenSea**:
- ✅ Lazy minting (gasless listing until purchase)
- ✅ Collection-level royalties (configurable)
- ✅ Multi-chain support with clear network indicators

### From **Coinbase Commerce**:
- ✅ Real-time exchange rate locking during checkout
- ✅ Overpayment/underpayment handling
- ✅ Webhook-based confirmation system

---

## Tradeoff Decisions Summary

### Decision 1: Currency Change During Active Affiliations
**Option A**: Block currency change until affiliations closed  
**Option B**: Allow change with partner notification  
**Option C**: Auto-convert existing affiliate products  

**Chosen: B + C Hybrid**  
- Notify all Co-sellers of upcoming change
- Existing products remain in original currency (grandfathered)
- New imports use new currency
- Commission calculations use real-time rates

### Decision 2: Crypto Payment Split Mechanism
**Option A**: Customer sends two transactions (complex UX)  
**Option B**: Platform receives all, executes internal transfers (centralized)  
**Option C**: Smart contract router (decentralized, gas efficient)  

**Chosen: C (Smart Contract Router)**  
- Single customer transaction
- Trustless split execution
- Transparent on-chain
- Gas cost borne by platform (merchant fee covers it)

### Decision 3: NFT Recording Mutability
**Option A**: Immutable forever (pure blockchain)  
**Option B**: Upgradeable proxy contracts  
**Option C**: Off-chain business rules + on-chain certificate  

**Chosen: C (Hybrid Approach)**  
- Product metadata immutable on-chain
- Commission rates, pricing stored off-chain or in separate config
- Allows business flexibility while maintaining authenticity proof

### Decision 4: Wallet Network Support
**Option A**: One wallet per merchant (simple)  
**Option B**: One wallet per network (flexible)  
**Option C**: Multi-network wallets (complex, future)  

**Chosen: B (One wallet per network)**  
- Clear UX: "Your Polygon wallet", "Your Ethereum wallet"
- Customer can pay on any supported network
- Platform handles routing to correct wallet

---

## Implementation Phases

### Phase 1: Foundation (Weeks 1-4)
- Payment Abstraction Layer
- Wallet connection framework
- Basic currency support (USD, EUR, GBP)

### Phase 2: Affiliate Core (Weeks 5-8)
- Affiliate network v1
- Commission calculation engine
- Currency normalization for affiliates

### Phase 3: Crypto Integration (Weeks 9-12)
- Smart contract payment router
- Non-custodial wallet support
- Multi-network wallet registry

### Phase 4: NFT & Advanced (Weeks 13-16)
- Product recording system
- Immutable product certification
- Cross-chain settlements

### Phase 5: Optimization (Weeks 17-20)
- Performance tuning
- Edge case handling
- Developer APIs

---

## Success Metrics

1. **Payment Success Rate**: >98% of checkout attempts complete successfully
2. **Affiliate Activation**: <5 minute setup time for Co-sellers
3. **Currency Coverage**: Support for top 20 global currencies
4. **Crypto Settlement**: <2 minutes from payment to wallet receipt
5. **NFT Recording**: <30 seconds gas estimation and transaction submission

---

## Risk Mitigation

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Currency volatility during affiliate split | High | Medium | Lock exchange rates at checkout time |
| Smart contract vulnerabilities | Medium | Critical | Multiple audits, bug bounties, insurance |
| Payment provider API changes | Medium | High | Abstraction layer isolates changes |
| Merchant wallet loss | Low | High | Clear warnings, backup options, support process |
| Network congestion | High | Medium | Dynamic gas pricing, retry logic |

---

## Conclusion

This architecture prioritizes **flexibility without fragility**. By separating concerns (display vs settlement vs certification) and using industry-proven patterns (Stripe Connect's routing, Shopify's currency handling, OpenSea's NFT standards), we create a system that can handle the complex interactions between features while maintaining a simple merchant experience.

**Key Takeaway**: The complexity of interactions requires a unified payment abstraction layer and clear currency normalization rules. Individual features should be built with awareness of their impact on the broader ecosystem.

---

*Next Steps: Review individual PDRs for each feature's detailed implementation.*
