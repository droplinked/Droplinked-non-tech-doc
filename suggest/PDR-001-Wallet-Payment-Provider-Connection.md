# PDR-001: Wallet & Payment Provider Connection System

**Status:** Draft  
**Author:** Product Management  
**Date:** 2026-02-12  
**Version:** 1.0  
**Related:** PDR-002 (Currency), PDR-003 (Affiliate), PDR-005 (Non-Custodial)

---

## 1. Executive Summary

This PDR defines the comprehensive wallet and payment provider connection system for Droplinked, enabling merchants to receive payments directly through traditional payment providers (PayPal, Stripe) and cryptocurrency wallets. The system serves as the foundation for all monetary flows in the platform and must handle complex scenarios including multi-currency support, affiliate commission splits, and cross-network crypto settlements.

### Key Decisions
1. **Dual-track architecture**: Separate but unified flows for fiat (custodial/credit) and crypto (direct settlement)
2. **Network-specific wallet registry**: Merchants can connect different wallets per blockchain network
3. **Dynamic payment method availability**: Real-time validation based on currency, location, and merchant configuration
4. **Smart contract routing**: Crypto affiliate payments split automatically via on-chain logic

---

## 2. Problem Statement

### Current Pain Points
1. **Fragmented payout experience**: Merchants need separate setups for fiat and crypto
2. **Limited payment provider options**: No flexibility in choosing regional providers
3. **Complex affiliate settlements**: No clear mechanism for splitting crypto payments between merchants
4. **Currency mismatches**: Payment providers don't support all currencies, causing checkout failures
5. **Wallet management confusion**: Unclear which wallet receives which payments

### User Stories

**As a merchant**, I want to:
- Connect my PayPal/Stripe account to receive fiat payments directly
- Connect multiple crypto wallets (one per network) to receive token payments
- See all my connected payment methods in one dashboard
- Understand which payment methods are active based on my shop currency
- Receive affiliate commissions directly to my preferred wallet

**As a customer**, I want to:
- See available payment methods based on the shop's configuration
- Pay with my preferred method (credit card, PayPal, crypto wallet)
- Trust that my payment will reach the correct merchant

**As a platform**, I need to:
- Route payments correctly to merchant wallets or credit balances
- Handle affiliate commission splits transparently
- Support multiple currencies without breaking payment flows
- Maintain compliance with financial regulations

---

## 3. Scope

### In Scope ✅
- **Fiat Payment Providers**:
  - Stripe (credit cards, bank transfers)
  - PayPal (wallet, credit cards)
  - Regional providers (to be added: iDEAL, Alipay, etc.)
  
- **Crypto Payment Methods**:
  - Direct wallet connection (MetaMask, WalletConnect, Coinbase Wallet)
  - EVM networks: Ethereum, Polygon, BSC, Arbitrum, Base
  - Stablecoins: USDC, USDT, DAI
  - Native tokens: ETH, MATIC, BNB
  
- **Core Features**:
  - Wallet/provider connection flow
  - Connection status management
  - Default payment method selection
  - Payment method availability logic
  - Disconnection and rotation procedures
  
### Out of Scope ❌
- Non-EVM chains (Bitcoin, Solana, Cardano) - Phase 2
- Fiat off-ramps (crypto → bank) - Integration with third-party
- Subscription/recurring billing - Separate PDR
- Multi-signature wallet support - Future enhancement
- Hardware wallet specific flows - Use generic WalletConnect

---

## 4. Architecture & Design

### 4.1 High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    MERCHANT DASHBOARD                           │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐ │
│  │ Connect PayPal  │  │ Connect Stripe  │  │ Connect Wallet  │ │
│  │   (OAuth)       │  │   (OAuth)       │  │  (Web3)         │ │
│  └────────┬────────┘  └────────┬────────┘  └────────┬────────┘ │
└───────────┼────────────────────┼────────────────────┼──────────┘
            │                    │                    │
            ▼                    ▼                    ▼
┌─────────────────────────────────────────────────────────────────┐
│              PAYMENT PROVIDER INTEGRATION LAYER                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │              Provider Connection Manager                  │  │
│  │  - OAuth flow handling                                    │  │
│  │  - Credential encryption                                  │  │
│  │  - Webhook configuration                                  │  │
│  │  - Connection health monitoring                           │  │
│  └──────────────────────────────────────────────────────────┘  │
│  ┌──────────────────────┐  ┌────────────────────────────────┐ │
│  │   Fiat Providers     │  │      Crypto Providers          │ │
│  │   - Stripe API       │  │  - Wallet Connection Manager   │ │
│  │   - PayPal API       │  │  - Network Registry            │ │
│  │   - Webhook handlers │  │  - Address validation          │ │
│  └──────────────────────┘  └────────────────────────────────┘ │
└──────────────────────────────┬──────────────────────────────────┘
                               │
            ┌──────────────────┼──────────────────┐
            ▼                  ▼                  ▼
   ┌────────────────┐ ┌────────────────┐ ┌────────────────┐
   │  STRIPE        │ │    PAYPAL      │ │  SMART CONTRACT│
   │  CONNECT       │ │   MERCHANT     │ │    ROUTER      │
   │                │ │                │ │                │
   │ Platform gets  │ │ Platform gets  │ │ Handles split  │
   │ payment,       │ │ payment,       │ │ payments for   │
   │ transfers to   │ │ transfers to   │ │ affiliates     │
   │ merchant       │ │ merchant       │ │ automatically  │
   └────────────────┘ └────────────────┘ └────────────────┘
```

### 4.2 Data Model

```typescript
// Merchant Payment Profile
interface MerchantPaymentProfile {
  merchantId: string;
  defaultCurrency: CurrencyCode; // USD, EUR, etc.
  
  // Fiat Providers
  fiatProviders: {
    stripe?: StripeConnection;
    paypal?: PayPalConnection;
    // Future: [providerId]: ProviderConnection
  };
  
  // Crypto Wallets (one per network)
  cryptoWallets: {
    [networkId: string]: {
      walletAddress: string;
      walletType: 'eoa' | 'smart_contract';
      connectedAt: Date;
      lastVerified: Date;
      status: 'active' | 'disconnected' | 'error';
    }
  };
  
  // Payment Method Preferences
  preferences: {
    defaultFiatProvider: 'stripe' | 'paypal' | null;
    defaultCryptoNetwork: string | null;
    autoConvertCrypto: boolean;
    settlementPreference: 'immediate' | 'batch' | 'credit';
  };
  
  // Validation & Compliance
  validationStatus: {
    kycStatus: 'pending' | 'verified' | 'rejected';
    taxInformation: TaxInfo | null;
    payoutSchedule: 'daily' | 'weekly' | 'monthly';
  };
}

// Stripe Connection
interface StripeConnection {
  accountId: string;
  accountType: 'standard' | 'express' | 'custom';
  chargesEnabled: boolean;
  payoutsEnabled: boolean;
  defaultCurrency: CurrencyCode;
  supportedCurrencies: CurrencyCode[];
  capabilities: string[];
  onboardingComplete: boolean;
}

// PayPal Connection
interface PayPalConnection {
  merchantId: string;
  email: string;
  accountStatus: 'active' | 'limited' | 'suspended';
  supportedCurrencies: CurrencyCode[];
  primaryCurrency: CurrencyCode;
  webhookConfigured: boolean;
}

// Crypto Wallet Connection
interface CryptoWalletConnection {
  networkId: string; // 'ethereum', 'polygon', etc.
  chainId: number; // 1, 137, 56, etc.
  walletAddress: string;
  verificationProof: string; // Signed message proving ownership
  connectionMethod: 'metamask' | 'walletconnect' | 'coinbase';
  status: 'connected' | 'disconnected' | 'network_mismatch';
}
```

### 4.3 Connection Flows

#### 4.3.1 Stripe Connect Flow (Fiat)

```
┌─────────────┐     ┌──────────────┐     ┌─────────────────┐
│  Merchant   │────►│   Dashboard  │────►│  "Connect       │
│             │     │              │     │   Stripe"       │
└─────────────┘     └──────────────┘     └────────┬────────┘
                                                  │
                     ┌────────────────────────────┘
                     ▼
            ┌────────────────────┐
            │  Stripe OAuth      │
            │  Onboarding        │
            │  (Express Account) │
            └─────────┬──────────┘
                      │
         ┌────────────┼────────────┐
         ▼            ▼            ▼
   ┌──────────┐ ┌──────────┐ ┌──────────┐
   │ Business │ │   Bank   │ │  Verify  │
   │  Info    │ │  Account │ │  Identity│
   └──────────┘ └──────────┘ └──────────┘
                      │
                      ▼
            ┌────────────────────┐
            │  Return to         │
            │  Droplinked        │
            │  with auth code    │
            └─────────┬──────────┘
                      │
                      ▼
            ┌────────────────────┐
            │ Platform stores    │
            │ - Account ID       │
            │ - Capabilities     │
            │ - Currency support │
            └────────────────────┘
```

**Key Design Decisions:**
- Use **Stripe Express Accounts** for fastest onboarding (merchant doesn't need separate Stripe account)
- Support **Standard Accounts** for merchants with existing Stripe accounts
- Platform handles all webhook configuration automatically
- Real-time capability checking before showing Stripe as available option

#### 4.3.2 PayPal Merchant Flow (Fiat)

```
┌─────────────┐     ┌──────────────┐     ┌─────────────────┐
│  Merchant   │────►│   Dashboard  │────►│  "Connect       │
│             │     │              │     │   PayPal"       │
└─────────────┘     └──────────────┘     └────────┬────────┘
                                                  │
                     ┌────────────────────────────┘
                     ▼
            ┌────────────────────┐
            │ PayPal OAuth       │
            │ (permissions:      │
            │  payments,         │
            │  refund,           │
            │  transaction_read) │
            └─────────┬──────────┘
                      │
                      ▼
            ┌────────────────────┐
            │ Merchant grants    │
            │ permissions        │
            └─────────┬──────────┘
                      │
                      ▼
            ┌────────────────────┐
            │ Return with        │
            │ - Merchant ID      │
            │ - Email            │
            │ - Account status   │
            └─────────┬──────────┘
                      │
                      ▼
            ┌────────────────────┐
            │ Validate currency  │
            │ support against    │
            │ shop currency      │
            └────────────────────┘
```

**Key Design Decisions:**
- Request minimal permissions (read transactions, process payments, issue refunds)
- Validate currency support immediately upon connection
- Show clear warnings if PayPal doesn't support merchant's shop currency
- Support both PayPal Business and Personal accounts (with limitations)

#### 4.3.3 Crypto Wallet Connection Flow

```
┌─────────────┐     ┌──────────────┐     ┌─────────────────────────┐
│  Merchant   │────►│   Dashboard  │────►│  "Connect Crypto        │
│             │     │              │     │   Wallet"               │
└─────────────┘     └──────────────┘     └────────┬────────────────┘
                                                  │
                     ┌────────────────────────────┘
                     ▼
            ┌─────────────────────────┐
            │  Select Network:        │
            │  - Ethereum             │
            │  - Polygon              │
            │  - BSC                  │
            │  - Arbitrum             │
            │  - Base                 │
            └──────────┬──────────────┘
                       │
                       ▼
            ┌─────────────────────────┐
            │  Select Wallet:         │
            │  - MetaMask             │
            │  - WalletConnect        │
            │  - Coinbase Wallet      │
            │  - Phantom (future)     │
            └──────────┬──────────────┘
                       │
                       ▼
            ┌─────────────────────────┐
            │  Wallet SDK initiates   │
            │  connection request     │
            └──────────┬──────────────┘
                       │
                       ▼
            ┌─────────────────────────┐
            │  Merchant approves      │
            │  connection in wallet   │
            └──────────┬──────────────┘
                       │
                       ▼
            ┌─────────────────────────┐
            │  Sign verification      │
            │  message (proves        │
            │  ownership)             │
            └──────────┬──────────────┘
                       │
                       ▼
            ┌─────────────────────────┐
            │  Store in database:     │
            │  - Address              │
            │  - Network              │
            │  - Verification proof   │
            │  - Connection timestamp │
            └─────────────────────────┘
```

**Key Design Decisions:**
- **One wallet per network**: Clear separation, no confusion about which address receives which payments
- **Signature verification**: Merchant must sign a message proving ownership (prevents typos, fraud)
- **Address validation**: Checksum validation before storing
- **Network lock**: Once set, changing wallet on same network requires special flow (prevents mistakes)

---

## 5. Conflict Resolution & Tradeoffs

### Conflict 1: Multiple Wallets vs Single Wallet
**Problem**: Should merchants connect one wallet for all networks or different wallets per network?

**Tradeoff Analysis**:
| Approach | Pros | Cons |
|----------|------|------|
| Single wallet (multi-network) | Simple UX, one address to remember | Limited wallet support, complex routing | 
| One wallet per network | Flexible, any wallet type, clear separation | More setup, potential for mistakes |

**Decision**: One wallet per network
- Merchants can use best wallet for each network (e.g., MetaMask for ETH, different wallet for Polygon)
- Clear UX: "Your Polygon wallet: 0x123..."
- Reduces risk of sending payments to wrong chain

### Conflict 2: Direct Settlement vs Credit System
**Problem**: Should crypto payments go directly to merchant wallet or through platform credit?

**Tradeoff Analysis**:
| Approach | Pros | Cons |
|----------|------|------|
| Direct settlement (this PDR) | Immediate access, no platform risk | Complex for affiliates, gas fees |
| Credit system | Easy affiliate splits, platform control | Delayed access, counterparty risk |

**Decision**: Support both (merchant choice)
- Default: Direct settlement for single-merchant orders
- Affiliate orders: Smart contract split (direct to both wallets)
- Fallback: Credit system if wallet not connected or transaction fails

### Conflict 3: Payment Provider Currency Mismatch
**Problem**: Merchant changes shop currency to TRY, but PayPal doesn't support TRY.

**Resolution**:
```
IF merchant_changes_currency:
  FOR each_connected_provider:
    IF provider.supports(new_currency):
      provider.status = 'active'
    ELSE:
      provider.status = 'disabled_currency_mismatch'
      notify_merchant(
        provider_name,
        unsupported_currencies,
        suggested_alternatives
      )
```

**UX Design**:
- Show impact preview before currency change confirmation
- Display disabled providers with clear explanation
- Offer to help connect alternative providers
- Don't block currency change, but warn about consequences

---

## 6. Integration with Other Features

### 6.1 Affiliate Network Integration

**Challenge**: Customer buys affiliate product. Payment must split between Affiliator and Co-seller.

**Solution - Smart Contract Router**:
```solidity
// Simplified logic
function processAffiliatePayment(
  address customer,
  address affiliatorWallet,
  address coSellerWallet,
  uint256 totalAmount,
  uint256 commissionRate // e.g., 1500 for 15%
) external payable {
  uint256 commission = (totalAmount * commissionRate) / 10000;
  uint256 affiliatorShare = totalAmount - commission;
  
  // Transfer to affiliator
  payable(affiliatorWallet).transfer(affiliatorShare);
  
  // Transfer commission to co-seller
  payable(coSellerWallet).transfer(commission);
  
  emit AffiliatePaymentSplit(
    orderId,
    affiliatorWallet,
    coSellerWallet,
    affiliatorShare,
    commission
  );
}
```

**Flow**:
1. Customer pays full amount to smart contract
2. Contract automatically splits to both merchant wallets
3. Platform fee taken before split (calculated off-chain, verified on-chain)
4. Both merchants receive funds simultaneously
5. No platform custody required

### 6.2 Shop Currency Integration

**Validation Matrix**:

| Shop Currency | Stripe | PayPal | Crypto |
|---------------|--------|--------|--------|
| USD | ✅ | ✅ | ✅ |
| EUR | ✅ | ✅ | ✅ |
| GBP | ✅ | ✅ | ✅ |
| JPY | ✅ | ❌ | ✅ |
| TRY | ✅ | ❌ | ✅ |
| BRL | ✅ | ✅ | ✅ |

**Dynamic Availability**:
```javascript
// Payment method availability logic
function getAvailablePaymentMethods(shop, cart) {
  const methods = [];
  
  // Check Stripe
  if (shop.connectedProviders.stripe?.supportedCurrencies.includes(shop.currency)) {
    methods.push('stripe');
  }
  
  // Check PayPal
  if (shop.connectedProviders.paypal?.supportedCurrencies.includes(shop.currency)) {
    methods.push('paypal');
  }
  
  // Check Crypto - always available if wallet connected
  if (Object.keys(shop.cryptoWallets).length > 0) {
    methods.push('crypto');
  }
  
  return methods;
}
```

### 6.3 Non-Custodial Wallet Integration

See PDR-005 for full details. Key integration points:
- Non-custodial wallets use same connection flow
- Additional verification steps for security
- Merchant maintains full control of private keys
- Platform never holds funds

---

## 7. Security & Compliance

### 7.1 Security Measures

1. **Credential Encryption**:
   - All API keys encrypted at rest (AES-256)
   - Stripe account IDs stored, not API keys
   - OAuth tokens refreshed automatically

2. **Wallet Verification**:
   - Signature required to prove ownership
   - Checksum validation on all addresses
   - Re-verification required for high-value changes

3. **Webhook Security**:
   - Stripe webhooks verified with signature
   - PayPal webhooks verified with certificate
   - Idempotency keys prevent duplicate processing

4. **Access Control**:
   - Only shop owners can modify payment settings
   - 2FA required for wallet disconnection
   - Audit log of all payment configuration changes

### 7.2 Compliance

1. **KYC/AML**:
   - Stripe handles KYC for fiat accounts
   - Crypto wallets: merchant self-certifies ownership
   - Suspicious activity monitoring

2. **Tax Reporting**:
   - Track all payouts for 1099/K reporting
   - Crypto payments tracked for capital gains
   - Merchant dashboard shows tax-relevant summaries

3. **Data Privacy**:
   - Wallet addresses considered public info (blockchain)
   - Bank account details encrypted and access-restricted
   - GDPR-compliant data export

---

## 8. Error Handling & Edge Cases

### 8.1 Common Error Scenarios

| Scenario | Error Type | Handling |
|----------|-----------|----------|
| Wallet connection rejected | User action | Show friendly message, allow retry |
| Invalid wallet address | Validation | Checksum error, prevent save |
| Stripe account restricted | Provider | Disable Stripe, notify merchant |
| PayPal currency unsupported | Configuration | Hide PayPal, suggest alternatives |
| Network congestion (crypto) | External | Queue transaction, notify customer |
| Insufficient gas balance | User | Estimate gas, warn before transaction |
| Wallet disconnected mid-checkout | Session | Save cart, prompt reconnection |

### 8.2 Recovery Procedures

**Lost Wallet Access**:
1. Merchant reports lost wallet access
2. Platform freezes crypto payments for that network
3. New wallet connection required
4. Pending payments held in credit until new wallet connected

**Provider Account Suspended**:
1. Webhook notification from provider
2. Platform immediately disables provider
3. Merchant notified via email + dashboard
4. Alternative payment methods promoted

**Failed Affiliate Split**:
1. Smart contract emits failure event
2. Platform receives webhook
3. Funds held in escrow contract
4. Support ticket auto-created
5. Manual resolution or refund

---

## 9. User Experience Design

### 9.1 Dashboard - Payment Methods Page

```
┌─────────────────────────────────────────────────────────────┐
│  💳 Payment Methods                                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Fiat Payments                                               │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  Stripe                    [Connected ✅]              │  │
│  │  Account: acct_xxx...                                   │  │
│  │  Currencies: USD, EUR, GBP                             │  │
│  │  Status: Active                                         │  │
│  │  [Configure]  [Disconnect]                              │  │
│  └───────────────────────────────────────────────────────┘  │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  PayPal                    [Connected ✅]              │  │
│  │  Email: merchant@example.com                            │  │
│  │  Currencies: USD, EUR, GBP, JPY                        │  │
│  │  ⚠️ TRY not supported (shop currency)                  │  │
│  │  [Configure]  [Disconnect]                              │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                              │
│  Crypto Wallets                                              │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  Ethereum (ETH)            [Connected ✅]              │  │
│  │  Address: 0x742d...9f2a                                  │  │
│  │  Network: Mainnet                                       │  │
│  │  Connected: Jan 15, 2026                                │  │
│  │  [View on Etherscan]  [Change Wallet]                   │  │
│  └───────────────────────────────────────────────────────┘  │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  Polygon (MATIC)           [Not Connected]             │  │
│  │  [Connect Wallet]                                        │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                              │
│  [+ Add Payment Method]                                      │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### 9.2 Connection Success/Failure States

**Success State**:
- Green checkmark animation
- "Wallet connected successfully!"
- Show connected address (truncated)
- "You'll receive payments on Ethereum Mainnet"
- Option to set as default

**Failure States**:
- User rejected: "Connection cancelled. You can try again anytime."
- Wrong network: "Please switch to Ethereum Mainnet in your wallet"
- Timeout: "Connection timed out. Check your wallet and try again."
- Invalid address: "Invalid wallet address. Please check and try again."

---

## 10. Implementation Roadmap

### Phase 1: Foundation (Weeks 1-4)
- [ ] Stripe Connect integration
- [ ] PayPal OAuth integration
- [ ] Basic wallet connection (MetaMask)
- [ ] Connection status management
- [ ] Dashboard UI

### Phase 2: Multi-Network Support (Weeks 5-8)
- [ ] Polygon, BSC wallet support
- [ ] WalletConnect integration
- [ ] Network switching UX
- [ ] Address validation across networks

### Phase 3: Smart Contract Integration (Weeks 9-12)
- [ ] Payment router contract deployment
- [ ] Affiliate split logic
- [ ] On-chain settlement flow
- [ ] Gas optimization

### Phase 4: Advanced Features (Weeks 13-16)
- [ ] Additional fiat providers (iDEAL, etc.)
- [ ] Automated webhook handling
- [ ] Advanced error recovery
- [ ] Analytics and reporting

### Phase 5: Optimization (Weeks 17-20)
- [ ] Performance tuning
- [ ] Security audit
- [ ] Compliance verification
- [ ] Documentation

---

## 11. Success Metrics

1. **Connection Success Rate**: >95% of connection attempts succeed
2. **Active Payment Methods**: Average 2.5 methods per merchant
3. **Checkout Success**: >98% of checkouts complete successfully
4. **Settlement Time**: 
   - Fiat: <24 hours (based on provider)
   - Crypto: <2 minutes (block confirmation)
5. **Error Resolution**: <5% of connections require support intervention

---

## 12. Open Questions

1. Should we support crypto-to-fiat auto-conversion at settlement?
2. How do we handle gas fee subsidies for small transactions?
3. What's the policy for merchant wallet address changes?
4. Should we support hardware wallet-specific UI flows?
5. How do we handle layer 2 networks (Optimism, Arbitrum) with different finality times?

---

## 13. References

- [Stripe Connect Documentation](https://stripe.com/docs/connect)
- [PayPal Merchant API](https://developer.paypal.com/docs/api/overview/)
- [EIP-155: Simple Replay Attack Protection](https://eips.ethereum.org/EIPS/eip-155)
- [WalletConnect v2.0 Spec](https://docs.walletconnect.com/2.0/)
- Shopify Markets currency handling patterns

---

*Last Updated: 2026-02-12*  
*Next Review: After Phase 1 completion*
