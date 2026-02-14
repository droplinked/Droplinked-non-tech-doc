# PDR-002: Shop Currency System

**Status:** Draft  
**Author:** Product Management  
**Date:** 2026-02-12  
**Version:** 1.0  
**Related:** PDR-001 (Wallet/Payment), PDR-003 (Affiliate)

---

## 1. Executive Summary

The Shop Currency System enables merchants to define their base accounting currency, affecting pricing display, payment processing, affiliate commissions, credit balances, and financial reporting. This PDR addresses the 15+ currency-related conflicts identified in the Feature Conflicts Report and establishes best practices inspired by Shopify Markets, Stripe, and Amazon.

### Key Decisions
1. **Currency display ≠ settlement currency**: Customers can view prices in local currency while settlement happens in merchant's base currency
2. **Grandfathered pricing**: Existing products maintain original currency when shop changes currency
3. **Real-time conversion at transaction time**: Exchange rates locked at checkout, not at product creation
4. **Currency lock for active affiliates**: Shops with active Co-sellers have restricted currency change capabilities
5. **Multi-currency credit tracking**: Platform tracks balances per currency, normalizes for reporting

---

## 2. Problem Statement

### Current Pain Points
1. **Currency change breaks affiliate relationships**: Affiliator changes to EUR, Co-seller expectations break
2. **Payment provider mismatches**: Merchant switches to TRY, PayPal becomes unavailable
3. **Credit balance confusion**: $500 credit becomes ambiguous when shop switches to EUR
4. **Existing product pricing**: Products created at $100 - what happens after currency change?
5. **Pending order complications**: 5 pending orders in USD, merchant switches to GBP
6. **Subscription pricing**: Monthly fee in USD, shop now in EUR - what charge?
7. **Coupon value ambiguity**: $10 off coupon in a EUR shop

### User Stories

**As a merchant**, I want to:
- Set my shop's base currency to match my business operations
- Accept payments in multiple currencies without confusion
- Change currency when my business needs evolve (with clear impact warnings)
- Have my credit balance properly valued regardless of currency changes
- See all historical transactions normalized to my current currency for reporting
- Maintain affiliate relationships even if my currency changes

**As a customer**, I want to:
- See prices in my local currency (or at least understand the value)
- Pay in my preferred currency when possible
- Understand exchange rates and fees transparently

**As an affiliator**, I want to:
- Know that my Co-sellers' currencies won't break commission calculations
- Receive commissions in my base currency regardless of customer payment currency

---

## 3. Scope

### In Scope ✅
- **Base Shop Currency**: Primary accounting currency for the shop
- **Display Currency**: Currency shown to customers (can differ from base)
- **Payment Currency**: What customer actually pays in
- **Currency Conversion**: Real-time exchange rate application
- **Multi-currency Support**: Top 50 global currencies
- **Currency Change Workflow**: Safe process with impact analysis
- **Credit Balance Tracking**: Per-currency balance management
- **Historical Data**: Normalized reporting across currency changes

### Out of Scope ❌
- Cryptocurrency as base currency (covered in PDR-001) - Phase 2
- Dynamic currency switching during checkout (future enhancement)
- Hedging/FX risk management (merchant responsibility)
- Multi-currency product pricing (single base currency per shop) - Phase 2

---

## 4. Architecture & Design

### 4.1 Currency Concepts

```
┌─────────────────────────────────────────────────────────────────┐
│                    CURRENCY LAYERS                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  LAYER 1: DISPLAY (What customer sees)                         │
│  ├── Determined by: Customer location, manual selection         │
│  ├── Can differ from: Base currency                             │
│  └── Example: German customer sees €85 on US-based shop         │
│                                                                 │
│  LAYER 2: BASE (Merchant's accounting currency)                │
│  ├── Set by: Merchant in shop settings                          │
│  ├── Affects: Pricing, credit, reporting, affiliate payouts     │
│  └── Example: Merchant sets USD as base currency                │
│                                                                 │
│  LAYER 3: PAYMENT (What customer actually pays)                │
│  ├── Determined by: Payment method capabilities                 │
│  ├── Conversion: At checkout using real-time rate               │
│  └── Example: Customer pays €85 via Stripe (converted to $100)  │
│                                                                 │
│  LAYER 4: SETTLEMENT (What merchant receives)                  │
│  ├── Always in: Base currency (or crypto equivalent)           │
│  ├── Conversion: Handled by payment provider                    │
│  └── Example: Merchant receives $95 (after fees)                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 4.2 Data Model

```typescript
// Shop Currency Configuration
interface ShopCurrencyConfig {
  shopId: string;
  
  // Base Currency (accounting)
  baseCurrency: CurrencyCode; // 'USD', 'EUR', 'JPY', etc.
  baseCurrencyChangedAt: Date | null;
  baseCurrencyChangeHistory: CurrencyChangeEvent[];
  
  // Display Currency Settings
  displayCurrency: {
    mode: 'auto' | 'fixed'; // auto = based on customer location
    fixedCurrency: CurrencyCode | null; // if mode = 'fixed'
    allowCustomerOverride: boolean;
  };
  
  // Supported Currencies (for payment)
  supportedCurrencies: CurrencyCode[];
  
  // Exchange Rate Preferences
  exchangeRate: {
    provider: 'xe' | 'open_exchange_rates' | 'custom';
    markupPercentage: number; // e.g., 2.5% for conversion fee
    roundingRules: RoundingConfig;
  };
  
  // Affiliate Currency Lock
  affiliateCurrencyLock: {
    locked: boolean;
    reason: 'active_co_sellers' | 'manual' | null;
    unlockableAt: Date | null;
  };
}

// Currency Change Event (audit trail)
interface CurrencyChangeEvent {
  fromCurrency: CurrencyCode;
  toCurrency: CurrencyCode;
  changedAt: Date;
  changedBy: string;
  impactSummary: {
    productsAffected: number;
    activeOrders: number;
    affiliatesAffected: number;
    paymentMethodsDisabled: string[];
  };
  productStrategy: 'convert' | 'grandfather' | 'manual_review';
}

// Multi-Currency Credit Balance
interface MerchantCreditBalance {
  merchantId: string;
  
  // Balances stored per currency
  balances: {
    [currencyCode: string]: {
      amount: number;
      lastUpdated: Date;
      source: 'sales' | 'refunds' | 'adjustments' | 'affiliate_commissions';
    }
  };
  
  // Normalized total (for display)
  totalNormalized: {
    amount: number;
    currency: CurrencyCode; // shop's current base
    exchangeRateUsed: number;
    calculatedAt: Date;
  };
  
  // Payout preferences
  payoutPreferences: {
    autoConvertToBase: boolean;
    minimumPayoutAmount: number;
    preferredPayoutCurrency: CurrencyCode;
  };
}

// Product Pricing (currency-aware)
interface ProductPricing {
  productId: string;
  shopId: string;
  
  // Original pricing
  originalPrice: number;
  originalCurrency: CurrencyCode;
  createdAt: Date;
  
  // Current pricing (may differ if shop changed currency)
  currentPrice: number;
  currentCurrency: CurrencyCode;
  
  // Pricing strategy for this product
  pricingStrategy: 'converted' | 'grandfathered' | 'manual';
  
  // Price history
  priceHistory: PriceChangeEvent[];
}

// Exchange Rate Cache
interface ExchangeRateCache {
  baseCurrency: CurrencyCode;
  targetCurrency: CurrencyCode;
  rate: number;
  fetchedAt: Date;
  expiresAt: Date;
  provider: string;
}
```

### 4.3 Currency Support Matrix

| Currency | Code | Stripe | PayPal | Affiliate Support | Notes |
|----------|------|--------|--------|-------------------|-------|
| US Dollar | USD | ✅ | ✅ | ✅ | Universal |
| Euro | EUR | ✅ | ✅ | ✅ | SEPA support |
| British Pound | GBP | ✅ | ✅ | ✅ | Stripe supports |
| Japanese Yen | JPY | ✅ | ✅ | ✅ | No decimal places |
| Canadian Dollar | CAD | ✅ | ✅ | ✅ | |
| Australian Dollar | AUD | ✅ | ✅ | ✅ | |
| Swiss Franc | CHF | ✅ | ✅ | ✅ | |
| Chinese Yuan | CNY | ✅ | ❌ | ✅ | PayPal limited |
| Indian Rupee | INR | ✅ | ✅ | ✅ | Stripe India specific |
| Brazilian Real | BRL | ✅ | ✅ | ✅ | |
| Turkish Lira | TRY | ✅ | ❌ | ✅ | PayPal unsupported |
| Mexican Peso | MXN | ✅ | ✅ | ✅ | |
| Singapore Dollar | SGD | ✅ | ✅ | ✅ | |
| Hong Kong Dollar | HKD | ✅ | ✅ | ✅ | |
| Swedish Krona | SEK | ✅ | ✅ | ✅ | |
| New Zealand Dollar | NZD | ✅ | ✅ | ✅ | |
| South Korean Won | KRW | ✅ | ✅ | ✅ | |
| Norwegian Krone | NOK | ✅ | ✅ | ✅ | |
| Danish Krone | DKK | ✅ | ✅ | ✅ | |
| Polish Zloty | PLN | ✅ | ✅ | ✅ | |
| UAE Dirham | AED | ✅ | ❌ | ✅ | |

---

## 5. Core Workflows

### 5.1 Currency Change Workflow

```
┌─────────────────────────────────────────────────────────────────┐
│              CURRENCY CHANGE IMPACT ANALYSIS                    │
└─────────────────────────────────────────────────────────────────┘

Step 1: Merchant initiates change
┌──────────────────────────────────────────────────────────────┐
│ Change shop currency from USD to EUR?                        │
│                                                              │
│ ⚠️ This will affect:                                         │
│                                                              │
│ 📦 Products (1,247)                                          │
│    • All existing products will be grandfathered in USD     │
│    • New products will use EUR                               │
│                                                              │
│ 🛒 Active Orders (5)                                         │
│    • These will complete in USD                              │
│    • Payouts remain in USD                                   │
│                                                              │
│ 🤝 Affiliate Network                                         │
│    • 12 Co-sellers affected                                  │
│    • They will be notified of the change                     │
│    • Future commissions calculated in EUR                    │
│                                                              │
│ 💳 Payment Methods                                           │
│    ✅ Stripe will continue working                           │
│    ❌ PayPal will be DISABLED (doesn't support EUR)         │
│       • Consider connecting an alternative                   │
│                                                              │
│ 💰 Credit Balance                                            │
│    • Current: $2,450.00                                      │
│    • Will be preserved in USD                                │
│    • Future earnings in EUR                                  │
│    • You'll have multi-currency balance                     │
│                                                              │
│ 📊 Reporting                                                 │
│    • Historical data stays in original currencies            │
│    • Reports will normalize to EUR                           │
│                                                              │
│ [Review Details]  [Cancel]  [Confirm Change]                 │
└──────────────────────────────────────────────────────────────┘

Step 2: System validations
IF active_co_sellers > 0:
  - Notify all Co-sellers 7 days in advance
  - Allow grace period for objections
  - Provide commission rate lock option

IF payment_providers_affected > 0:
  - Show alternative providers
  - Offer setup assistance
  - Don't block change, but require acknowledgment

Step 3: Execute change
- Update shop.baseCurrency
- Create CurrencyChangeEvent record
- Send notifications to all stakeholders
- Update affiliate commission calculations
- Flag payment methods for review

Step 4: Post-change
- Merchant receives confirmation email
- Co-sellers receive notification
- Dashboard shows new currency
- Reports updated with conversion rates
```

### 5.2 Affiliate Currency Normalization

**The Challenge**: Affiliator (USD) has Co-seller (EUR) selling their products. Customer pays in JPY. How is commission calculated?

**Solution: Three-Layer Conversion**

```
Customer pays: ¥10,000 JPY
         │
         ▼ (Real-time exchange rate at checkout)
Order total: $75.50 USD (Affiliator's base)
         │
         ▼ (Commission calculation)
Commission: 15% = $11.33 USD → €10.20 EUR (Co-seller's base)
         │
         ▼
Co-seller receives: €10.20 credit
Affiliator receives: $64.17 credit
```

**Key Rules**:
1. **Order value** always normalized to Affiliator's base currency
2. **Commission** calculated in Affiliator's base currency
3. **Co-seller payout** converted to Co-seller's base currency
4. **Exchange rate** locked at checkout time (not payout time)
5. **Rate source**: XE.com or Open Exchange Rates (configurable)

### 5.3 Credit Balance Multi-Currency Management

```typescript
// Credit balance display logic
function displayCreditBalance(merchant) {
  const balances = merchant.creditBalances; // { USD: 2450, EUR: 300 }
  const baseCurrency = merchant.shop.baseCurrency; // EUR
  
  // Calculate normalized total
  let totalInBase = 0;
  for (const [currency, amount] of Object.entries(balances)) {
    if (currency === baseCurrency) {
      totalInBase += amount;
    } else {
      totalInBase += convertCurrency(amount, currency, baseCurrency);
    }
  }
  
  return {
    // Show normalized total prominently
    total: {
      amount: totalInBase,
      currency: baseCurrency,
      approximate: false
    },
    // Show breakdown
    breakdown: balances,
    // Conversion info
    conversionInfo: {
      ratesUsed: getExchangeRates(Object.keys(balances), baseCurrency),
      calculatedAt: new Date()
    }
  };
}

// Example display:
// "Your Balance: €2,532.50"
// "Includes: $2,450.00 + €300.00"
// "Exchange rate: 1 USD = 0.90 EUR (updated 2 min ago)"
```

### 5.4 Product Pricing Strategy

When shop currency changes, three strategies for existing products:

**Strategy A: Grandfather (Recommended)**
```
Product created: $100 USD
Shop changes to EUR
Product remains: $100 USD (grandfathered)
New products: €90 EUR (at current rate)

Pros: No surprise price changes, existing customers unaffected
Cons: Mixed currency catalog
```

**Strategy B: Auto-Convert**
```
Product created: $100 USD
Shop changes to EUR
Product becomes: €90 EUR (converted at change time rate)

Pros: Consistent currency throughout catalog
Cons: Price changes may surprise customers, affiliate commissions affected
```

**Strategy C: Manual Review**
```
All products flagged for review
Merchant must manually update each
Unreviewed products: Hidden or marked "price pending"

Pros: Full merchant control
Cons: High friction, products unavailable during review
```

**Decision**: Default to Grandfather with option to bulk convert

---

## 6. Conflict Resolution

### Conflict 1: Affiliate Currency Mismatch

**Scenario**: Affiliator (USD) and Co-seller (EUR) are actively affiliated. Co-seller changes currency to GBP.

**Resolution**:
```
IF co_seller.changes_currency AND has_active_imports:
  // Don't block, but handle gracefully
  
  FOR each_imported_product:
    // Option 1: Continue with real-time conversion
    commission_calculation = {
      base: affiliator_currency, // Still USD
      payout: new_co_seller_currency // Now GBP
    }
    
    // Option 2: Lock existing at old rate, new at new rate
    IF product_imported_before_change:
      use_historical_rate_for_commission
    ELSE:
      use_current_rate
  
  // Notify affiliator
  send_notification(
    to: affiliator,
    subject: "Co-seller currency change may affect commissions",
    content: explain_conversion_approach
  )
```

**Best Practice**: 
- Shopify allows this with real-time conversion
- Amazon Associates normalizes to program currency
- **Our approach**: Real-time conversion with rate locking at transaction time

### Conflict 2: Payment Provider Currency Unsupported

**Scenario**: Merchant changes to TRY. PayPal doesn't support TRY.

**Resolution**:
```
ON currency_change:
  affected_providers = []
  
  FOR provider IN merchant.connectedProviders:
    IF NOT provider.supports(new_currency):
      provider.status = 'disabled_currency_mismatch'
      provider.disable_reason = 'Currency not supported'
      affected_providers.push(provider.name)
  
  IF affected_providers.length > 0:
    show_modal({
      title: "Payment Methods Affected",
      content: `The following providers don't support ${new_currency}:`,
      list: affected_providers,
      alternatives: suggestAlternativeProviders(new_currency),
      actions: ["Continue Anyway", "Connect Alternatives First"]
    })
```

**UX Design**:
- Show warning banner in payment settings
- "PayPal unavailable - TRY not supported"
- Button: "Find alternative providers"
- Don't hide the disabled provider - show why it's disabled

### Conflict 3: Subscription Pricing Currency Change

**Scenario**: Merchant on $99/month plan changes currency to EUR.

**Resolution**:
```
IF merchant_has_subscription:
  subscription = getSubscription(merchant)
  
  IF subscription.currency != new_currency:
    // Option 1: Convert subscription price
    new_price = convertCurrency(subscription.price, subscription.currency, new_currency)
    subscription.update({
      price: new_price,
      currency: new_currency,
      effective_date: next_billing_cycle
    })
    
    // Option 2: Keep in original currency (complex accounting)
    // Not recommended
    
  notify_merchant(
    "Your subscription price will change from $99.00 to €89.10 " +
    "starting next billing cycle (March 1, 2026)"
  )
```

**Decision**: Convert at next billing cycle with advance notice

### Conflict 4: Coupon Value Across Currencies

**Scenario**: $10 off coupon created in USD shop. Shop changes to EUR. Coupon still valid?

**Resolution**:
```
// Coupon has two possible modes
interface Coupon {
  code: string;
  
  // Mode 1: Fixed amount
  fixedDiscount: {
    amount: number;
    currency: CurrencyCode; // Locked at creation
  };
  
  // Mode 2: Percentage (currency agnostic)
  percentageDiscount: number;
}

// At checkout:
function calculateCouponDiscount(coupon, cartTotal, shopCurrency) {
  IF coupon.type === 'percentage':
    return cartTotal * coupon.percentage;
  
  IF coupon.type === 'fixed_amount':
    IF coupon.currency === shopCurrency:
      return coupon.amount;
    ELSE:
      // Convert coupon value to shop currency
      return convertCurrency(coupon.amount, coupon.currency, shopCurrency);
}
```

**Best Practice** (from Shopify):
- Fixed amount coupons maintain original currency value
- Converted at checkout time
- Clear display: "$10.00 off (~€9.00)"

### Conflict 5: Pending Orders During Currency Change

**Scenario**: 5 pending orders in USD. Merchant switches to GBP.

**Resolution**:
```
ON currency_change:
  pending_orders = getPendingOrders(merchant);
  
  FOR order IN pending_orders:
    // Orders remain in original currency
    order.currency = UNCHANGED;
    order.settlement_currency = order.original_currency;
    
    // Merchant payout happens in original currency
    // Converted to merchant's credit balance
    addToCreditBalance(
      merchant,
      order.merchantShare,
      order.original_currency // Keep as USD
    );
  
  notify_merchant(
    `${pending_orders.length} pending orders will complete in ${old_currency}. ` +
    "Your credit balance will track multiple currencies."
  );
```

**Rule**: Pending orders are immutable - they complete in original currency

---

## 7. Integration Points

### 7.1 Wallet & Payment Provider (PDR-001)

**Currency Validation**:
```javascript
// When merchant connects payment provider
function validateProviderCurrencySupport(provider, shopCurrency) {
  const supported = provider.supportedCurrencies;
  
  IF supported.includes(shopCurrency):
    return { valid: true };
  ELSE:
    return {
      valid: false,
      error: `${provider.name} does not support ${shopCurrency}`,
      supportedCurrencies: supported,
      alternatives: getProvidersSupporting(shopCurrency)
    };
}
```

**Checkout Integration**:
```javascript
// Display price to customer
function displayPrice(product, customerLocation) {
  const shopCurrency = product.shop.baseCurrency;
  const customerCurrency = detectCurrency(customerLocation);
  
  IF customerCurrency === shopCurrency:
    return {
      price: product.price,
      currency: shopCurrency,
      formatted: formatCurrency(product.price, shopCurrency)
    };
  ELSE:
    // Show price in customer's currency with approximation
    const converted = convertCurrency(product.price, shopCurrency, customerCurrency);
    return {
      price: converted,
      currency: customerCurrency,
      formatted: formatCurrency(converted, customerCurrency),
      originalPrice: product.price,
      originalCurrency: shopCurrency,
      exchangeRate: getRate(shopCurrency, customerCurrency),
      note: `Converted from ${formatCurrency(product.price, shopCurrency)}`
    };
}
```

### 7.2 Affiliate Network (PDR-003)

**Commission Calculation**:
```javascript
function calculateAffiliateCommission(order, affiliator, coSeller) {
  // Step 1: Normalize order total to affiliator's base currency
  const orderInAffiliatorCurrency = convertCurrency(
    order.total,
    order.currency,
    affiliator.shop.baseCurrency
  );
  
  // Step 2: Calculate commission in affiliator's currency
  const commissionRate = getCommissionRate(order.product);
  const commissionAmount = orderInAffiliatorCurrency * commissionRate;
  
  // Step 3: Convert commission to co-seller's currency
  const commissionInCoSellerCurrency = convertCurrency(
    commissionAmount,
    affiliator.shop.baseCurrency,
    coSeller.shop.baseCurrency
  );
  
  return {
    affiliatorReceives: orderInAffiliatorCurrency - commissionAmount,
    affiliatorCurrency: affiliator.shop.baseCurrency,
    coSellerReceives: commissionInCoSellerCurrency,
    coSellerCurrency: coSeller.shop.baseCurrency,
    exchangeRate: getRate(affiliator.shop.baseCurrency, coSeller.shop.baseCurrency),
    rateLockedAt: new Date()
  };
}
```

### 7.3 Subscription & Credit System

**Credit Balance Aggregation**:
```sql
-- SQL example for reporting
SELECT 
  merchant_id,
  SUM(CASE 
    WHEN currency = 'USD' THEN amount * usd_rate
    WHEN currency = 'EUR' THEN amount * eur_rate
    ELSE amount
  END) as total_in_base_currency
FROM merchant_credit_balances
WHERE merchant_id = ?
GROUP BY merchant_id;
```

---

## 8. Edge Cases & Error Handling

### Edge Case 1: Extreme Exchange Rate Volatility

**Scenario**: TRY/USD rate changes 20% between order and settlement.

**Resolution**:
```
IF exchange_rate_volatility > 10%:
  // Lock rate at checkout for 24 hours
  order.exchangeRateLocked = true;
  order.rateLockExpires = now + 24 hours;
  
  IF settlement_after_lock_expires:
    // Use average rate over lock period
    use_average_rate(order.created_at, order.rateLockExpires);
    notify_merchant("Rate volatility affected this settlement");
```

### Edge Case 2: Currency No Longer Supported

**Scenario**: Merchant using ZWL (Zimbabwe Dollar). Currency suspended.

**Resolution**:
- Immediate notification to merchant
- Grace period (30 days) to change currency
- All new orders default to USD
- Existing balances convertible to new currency

### Edge Case 3: Rounding Issues

**Scenario**: JPY has no decimals. Converting $10.99 → ¥1,647.345.

**Resolution**:
```
ROUNDING_RULES = {
  'JPY': { decimals: 0, rule: 'round_up' },
  'KRW': { decimals: 0, rule: 'round_nearest' },
  'default': { decimals: 2, rule: 'round_nearest' }
}

function roundForCurrency(amount, currency) {
  const rule = ROUNDING_RULES[currency] || ROUNDING_RULES.default;
  return Math[rule.rule](amount * Math.pow(10, rule.decimals)) / Math.pow(10, rule.decimals);
}
```

### Edge Case 4: Zero or Negative Exchange Rates

**Scenario**: API returns invalid rate (0 or negative).

**Resolution**:
```
function validateRate(rate) {
  IF rate <= 0 OR rate > 1000000:
    // Use cached rate
    return getCachedRate();
  
  IF rate_age > 1_hour:
    // Stale rate, trigger refresh
    refreshRatesAsync();
  
  return rate;
}
```

---

## 9. User Experience

### 9.1 Shop Settings - Currency Section

```
┌─────────────────────────────────────────────────────────────┐
│  💱 Shop Currency                                            │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Base Currency (Accounting)                                 │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  [USD ▼] US Dollar                                     │  │
│  │  This is your shop's primary currency. All pricing,   │  │
│  │  reporting, and settlements use this currency.        │  │
│  │  ⚠️ Changing this affects your entire shop.          │  │
│  │  [Change Currency]                                     │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                              │
│  Currency Display Settings                                  │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  Display prices to customers:                          │  │
│  │  (•) In their local currency (auto-detect)            │  │
│  │  ( ) In shop's base currency (USD)                    │  │
│  │  ( ) In fixed currency: [EUR ▼]                       │  │
│  │                                                        │  │
│  │  ☑️ Show approximate conversion to customer's currency │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                              │
│  Exchange Rate Settings                                     │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  Rate provider: [XE.com ▼]                             │  │
│  │  Conversion markup: [2.5%]                             │  │
│  │  Last updated: 2 minutes ago                           │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                              │
│  Supported Payment Currencies                               │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  Customers can pay in these currencies:                │  │
│  │  ☑️ USD (US Dollar)                                    │  │
│  │  ☑️ EUR (Euro)                                         │  │
│  │  ☑️ GBP (British Pound)                                │  │
│  │  ☐ JPY (Japanese Yen) [Requires payment provider]     │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### 9.2 Currency Change Impact Modal

```
┌─────────────────────────────────────────────────────────────┐
│  ⚠️ Change Currency from USD to EUR?                        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  📊 Impact Summary                                           │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  Products:        1,247 (grandfathered)               │  │
│  │  Active Orders:   5 (complete in USD)                 │  │
│  │  Co-sellers:      12 (will be notified)               │  │
│  │  Credit Balance:  $2,450.00 (preserved)               │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                              │
│  ❌ Payment Methods That Will Be Disabled                    │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  • PayPal (TRY not supported)                         │  │
│  │                                                        │  │
│  │  [Find Alternative Providers]                         │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                              │
│  ✅ What Stays the Same                                     │
│  • Stripe continues working                                │
│  • Affiliate relationships remain active                   │
│  • Historical reports available                            │
│                                                              │
│  📅 Timeline                                                │
│  • Co-sellers notified: Immediately                        │
│  • Change effective: Immediately                           │
│  • New products: Use EUR                                   │
│                                                              │
│  [Cancel]                              [Confirm Change]     │
└─────────────────────────────────────────────────────────────┘
```

### 9.3 Credit Balance Display

```
┌─────────────────────────────────────────────────────────────┐
│  💰 Your Balance                                             │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Total: €2,532.50 EUR                                       │
│  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│                                                              │
│  Breakdown:                                                  │
│  • $2,450.00 USD                    → €2,205.00            │
│  • €300.00 EUR                      → €300.00              │
│  • £25.00 GBP                       → €27.50               │
│                                                              │
│  Exchange rates (updated 2 min ago):                        │
│  1 USD = 0.90 EUR                                           │
│  1 GBP = 1.10 EUR                                           │
│                                                              │
│  [Withdraw Funds]  [View Transactions]                      │
└─────────────────────────────────────────────────────────────┘
```

---

## 10. Implementation Roadmap

### Phase 1: Foundation (Weeks 1-4)
- [ ] Multi-currency data model
- [ ] Exchange rate integration (XE/Open Exchange Rates)
- [ ] Base currency configuration
- [ ] Top 20 currency support

### Phase 2: Display & Conversion (Weeks 5-8)
- [ ] Currency display logic
- [ ] Checkout conversion
- [ ] Customer location detection
- [ ] Price formatting by locale

### Phase 3: Change Workflow (Weeks 9-12)
- [ ] Currency change impact analysis
- [ ] Grandfathering system
- [ ] Notification system
- [ ] Payment provider validation

### Phase 4: Advanced Features (Weeks 13-16)
- [ ] Multi-currency credit tracking
- [ ] Historical reporting with normalization
- [ ] Affiliate currency normalization
- [ ] Bulk product currency conversion

### Phase 5: Optimization (Weeks 17-20)
- [ ] Exchange rate caching
- [ ] Performance optimization
- [ ] Edge case handling
- [ ] Compliance auditing

---

## 11. Success Metrics

1. **Currency Coverage**: 50+ supported currencies
2. **Conversion Accuracy**: 99.9% accurate rate application
3. **Change Success**: >95% of currency changes complete without support
4. **Affiliate Compatibility**: 0% affiliate breakages due to currency
5. **Checkout Success**: <1% failures due to currency issues
6. **Credit Accuracy**: 100% balance accuracy across currency changes

---

## 12. Open Questions

1. Should we allow cryptocurrency as a base shop currency?
2. How do we handle countries with currency controls (China, Venezuela)?
3. Should merchants be able to set different prices per currency?
4. How often should we refresh exchange rates (real-time vs hourly)?
5. Do we need to support cryptocurrency exchange rate volatility differently?

---

## 13. References

- [Shopify Markets Currency Handling](https://help.shopify.com/en/manual/markets)
- [Stripe Multi-Currency](https://stripe.com/docs/currencies)
- [Open Exchange Rates API](https://openexchangerates.org/)
- [XE Currency API](https://www.xe.com/xecurrencydata/)
- [Amazon Associates Currency](https://affiliate-program.amazon.com/help/node/topic/GP38PYB33SWRG4JY)

---

## Appendix A: Conflict Scenario Solutions

This appendix provides specific solutions for currency-related conflicts identified in the Feature Conflicts Report.

### A.1 Shop Currency Conflicts

#### Scenario 1: Credit Balance After Currency Change
**Problem:** Shop changes base currency from USD to EUR. $500 credit balance - convert or track multi-currency?

**Solution: Multi-Currency Balance Tracking**
- Keep original currency balances separate (don't convert)
- Display as "Multi-currency balance" with breakdown
- Show normalized total in current base currency
- Withdrawals happen in original currency or converted at merchant's choice

**Implementation:**
```typescript
// Credit balance displays as:
Your Balance: €2,205.00 EUR (approximate)

Breakdown:
• $2,450.00 USD (earned Jan-Jun 2026)
• €300.00 EUR (earned Jul-Sep 2026)

Conversion rate: 1 USD = 0.90 EUR
```

**Trade-off:** Transparency vs simplicity. Chosen: Transparency.

---

#### Scenario 2: Printful Integration with Non-USD Currency
**Problem:** Printful prices in USD. Shop currency EUR. Who bears exchange rate risk?

**Solution: Merchant Risk with 3% Buffer**
- Merchant bears exchange rate risk (they set prices)
- Platform adds 3% buffer to Printful cost conversion
- Real-time pricing at order time
- Clear profit calculation display

**Example:**
```
Your selling price: €50.00
Printful cost: $20.00 USD → €18.54 EUR (with 3% buffer)
Your profit: €31.46

⚠️ Exchange rate risk disclosed to merchant
```

**Trade-off:** Merchant protection vs platform risk. Chosen: Shared responsibility.

---

#### Scenario 3: Payment Provider Currency Support
**Problem:** PayPal doesn't support all currencies. How to notify during currency change?

**Solution: Pre-Validation with Alternatives**
- Validate all connected providers before currency change
- Show clear impact analysis
- Offer alternative providers
- Don't block change, just warn

**UX:**
```
Change Currency to Turkish Lira (TRY)?

⚠️ PayPal will be DISABLED (doesn't support TRY)

Alternatives:
• Stripe (supports TRY) ✅
• Cryptocurrency ✅
• Local Turkish providers

[Continue Anyway] [Find Alternatives]
```

**Trade-off:** Flexibility vs safety. Chosen: Flexibility with clear warnings.

---

#### Scenario 4: Existing Products After Currency Change
**Problem:** Products priced at $100 USD. Shop changes to EUR. Convert or grandfather?

**Solution: Grandfather Strategy (Default)**
- Keep existing products in original currency
- New products use new currency
- Offer bulk conversion tools
- Manual review option available

**Options:**
1. **Grandfather (Default)** - No surprise price changes
2. **Bulk Convert** - Convert all at current rate
3. **Manual Review** - Review each product

**Trade-off:** Consistency vs stability. Chosen: Stability (no surprises).

---

#### Scenario 5: Pending Orders During Currency Change
**Problem:** 5 pending orders in USD. Merchant changes to GBP. How to handle payouts?

**Solution: Immutable Order Currency**
- Orders remain in currency at creation time
- Payouts happen in order currency
- Added to multi-currency balance
- No conversion of existing orders

**Trade-off:** Complexity vs predictability. Chosen: Predictability.

---

#### Scenario 6: Negative Balance in Different Currency
**Problem:** $500 USD credit. Shop now EUR. Merchant owes €50. Can USD cover EUR debt?

**Solution: Cross-Currency Automatic Offset**
- Yes, USD credit can cover EUR debt
- Real-time exchange rate at offset time
- Automatic conversion
- Transparent transaction record

**Trade-off:** Automation vs manual control. Chosen: Automation with transparency.

---

#### Scenario 7: Multi-Currency Credit Display
**Problem:** 6 months USD ($500), 3 months EUR (€300). How to display total balance?

**Solution: Normalized Display with Full Breakdown**
- Show normalized total in base currency (approximate)
- Display all balances with earning periods
- Include conversion rates used
- Mark as approximate

**Trade-off:** Simplicity vs accuracy. Chosen: Accuracy with transparency.

---

### A.2 Affiliate + Currency Conflicts

#### Scenario 1: Affiliate Requires USD But Shop Changes Currency
**Problem:** Affiliator changes to EUR. Co-sellers have imported products. What happens?

**Solution: Allow All Currencies with Normalization**
- Remove USD-only requirement
- Allow any currency combination
- Commission calculated in Affiliator's current currency
- Converted to Co-seller's currency at transaction time
- 7-day advance notice to Co-sellers

**Trade-off:** Restrictions vs flexibility. Chosen: Flexibility.

---

#### Scenario 2: Mixed Currency Affiliate Order
**Problem:** Affiliator (USD), Co-seller (EUR), validation bypassed. Customer pays €100. Product $100 USD.

**Solution: Automatic Normalization**
- Commission calculated on Affiliator's product price ($100 USD)
- Converted to Co-seller's currency (€13.50 EUR)
- Rate locked at transaction time
- Log validation bypass for investigation

**Trade-off:** Strict validation vs graceful handling. Chosen: Graceful handling.

---

#### Scenario 3: Co-seller Currency Change After Import
**Problem:** Co-seller imports at USD, changes to GBP. How to calculate commission?

**Solution: Dynamic Conversion at Transaction Time**
- Use Co-seller's current currency (not import-time)
- Real-time conversion at checkout
- Rate locked at transaction time
- Transparent display

**Trade-off:** Historical rate vs current rate. Chosen: Current rate (fairness).

---

#### Scenario 4: Non-USD Shop Tries to Activate Affiliate
**Problem:** EUR shop tries to activate affiliate. Currently blocked with warning.

**Solution: Allow with Currency Notice**
- Remove currency restrictions
- Show notice about automatic conversion
- Support all major currencies
- Normalize commissions automatically

**Trade-off:** Restrictions vs growth. Chosen: Growth (remove barriers).

---

#### Scenario 5: Currency Change With Active Co-sellers
**Problem:** Affiliator has 10 Co-sellers, changes USD to JPY. What happens to imports?

**Solution: Graceful Transition with 14-Day Notice**
- Products remain active (no disruption)
- 14-day advance notice for major changes
- Commission in new currency (normalized)
- Impact analysis provided to Co-sellers

**Trade-off:** Speed vs preparation. Chosen: Preparation.

---

#### Scenario 6: Commission Calculation Timing
**Problem:** Co-seller (EUR), Affiliator (USD). Order in EUR. When to convert? What if rate changes?

**Solution: Lock Rate at Checkout Time**
- Exchange rate locked at checkout
- 24-hour validity period
- Protects both parties from volatility
- Clear display of locked rate

**Trade-off:** Current rate vs locked rate. Chosen: Locked rate (stability).

---

## A.3 Summary of Trade-offs

| Conflict | Solution | Trade-off Made |
|----------|----------|----------------|
| Credit Balance | Multi-currency tracking | Transparency over simplicity |
| POD Currency | Merchant risk + 3% buffer | Shared responsibility |
| Payment Provider | Pre-validation with alternatives | Flexibility over restriction |
| Existing Products | Grandfather strategy | Stability over consistency |
| Pending Orders | Immutable order currency | Predictability |
| Negative Balance | Auto-offset | Automation with transparency |
| Multi-currency Display | Normalized with breakdown | Accuracy over simplicity |
| Affiliate Currency | Allow all currencies | Flexibility over restriction |
| Mixed Currency | Auto-normalization | Graceful handling |
| Co-seller Change | Dynamic conversion | Current rate (fairness) |
| Non-USD Affiliate | Allow with notice | Growth over restriction |
| Active Co-sellers | 14-day notice | Preparation over speed |
| Commission Timing | Lock at checkout | Stability over current rate |

---

*Last Updated: 2026-02-12*  
*Next Review: After Phase 2 completion*
