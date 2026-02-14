# Feature Competitor Analysis: Shop Currency

## Executive Summary

This document analyzes how major e-commerce platforms handle shop currency settings, identifying best practices and competitive gaps for Droplinked's implementation.

---

## Competitor Analysis

### 1. Shopify

**How They Handle Shop Currency:**
- **Primary Currency Model**: Single base currency per store
- **Currency Selection**: Set at store level in Settings > Store details
- **Price Behavior**: Historical prices remain in original currency; new prices use selected currency
- **No Auto-Conversion**: Changing currency does NOT convert existing product prices
- **Shopify Markets**: Allows multi-currency display with automatic conversion for international customers
- **Payment Support**: 130+ currencies supported through Shopify Payments

**Key Features:**
- Base currency determines how prices are stored in database
- Multi-currency pricing requires Shopify Payments activation
- Localized pricing with rounding rules per market
- Currency selector widget for storefront
- Automatic exchange rate updates (manual override available)

**BACs (Business Acceptance Criteria):**
1. Base currency can be changed but requires careful consideration
2. Previous orders maintain original currency for accounting
3. Product prices remain in original entry currency
4. Reports can be viewed in store currency or converted to other currencies
5. Shopify Markets enables localized pricing while keeping base currency stable

**Strengths:**
- Clear separation between base currency and display currency
- Robust multi-currency support through Markets
- Automatic FX rate updates with manual control
- Accounting accuracy maintained

**Weaknesses:**
- Requires Shopify Payments for full multi-currency features
- Changing base currency can cause confusion
- No bulk price conversion tool

---

### 2. WooCommerce

**How They Handle Shop Currency:**
- **Primary Currency Model**: Single base currency (default: USD)
- **Currency Selection**: WooCommerce > Settings > General > Currency
- **Price Storage**: All prices stored in base currency
- **Extensions**: Multi-currency support requires plugins (WooCommerce Multi-Currency, Currency Switcher)

**Key Features:**
- Simple dropdown currency selection
- Thousands of currency options via extensions
- Geolocation-based currency detection (with plugins)
- Manual exchange rate entry or API-based updates

**BACs:**
1. Base currency is the "source of truth" for all calculations
2. Extensions handle display currency conversion
3. Payment gateway must support selected currency
4. Currency symbol and position configurable

**Strengths:**
- Highly flexible with extensions
- Open-source allows custom implementations
- Wide range of multi-currency plugins available

**Weaknesses:**
- No native multi-currency support
- Plugin-dependent for advanced features
- Exchange rate management is plugin-specific

---

### 3. BigCommerce

**How They Handle Shop Currency:**
- **Primary Currency Model**: Single base currency per store
- **Currency Selection**: Store Setup > Currencies
- **Multi-Currency**: Native support for up to 12 currencies simultaneously
- **Price Management**: Set prices per currency or use auto-conversion

**Key Features:**
- Display prices in customer's local currency
- Transaction settlement in any supported currency
- Currency-specific payment methods
- Automatic or manual exchange rates
- Currency selector on storefront

**BACs:**
1. Store can display and accept multiple currencies
2. Default currency is the fallback
3. Payment methods filtered by currency support
4. Inventory and catalog remain unified across currencies

**Strengths:**
- True multi-currency out of the box
- Up to 12 currencies without additional cost
- Payment method filtering by currency

**Weaknesses:**
- Limited to 12 currencies on standard plans
- Complex pricing management for multiple currencies
- Currency-specific SEO considerations

---

### 4. Magento / Adobe Commerce

**How They Handle Shop Currency:**
- **Primary Currency Model**: Base currency + multiple display currencies
- **Currency Selection**: Stores > Configuration > Currency Setup
- **Price Management**: Base prices + currency-specific overrides
- **Multi-Store**: Different currencies per store view

**Key Features:**
- Currency rates managed via XML feed or manual entry
- Currency switcher in frontend
- Different currencies per store view
- Support for 200+ currencies

**BACs:**
1. Base currency used for all backend calculations
2. Display currencies converted at runtime
3. Store views can have different default currencies
4. Import service for exchange rates available

**Strengths:**
- Enterprise-grade multi-currency support
- Highly configurable
- Store view architecture for regional currencies

**Weaknesses:**
- Complex setup and management
- Resource-intensive for many currencies
- Requires careful cache management

---

### 5. Squarespace

**How They Handle Shop Currency:**
- **Primary Currency Model**: Single currency per store
- **Currency Selection**: Commerce > Payments > Store Currency
- **Limitations**: Very limited multi-currency support

**Key Features:**
- Simple dropdown selection
- Currency determines payment processor availability
- Basic currency formatting

**BACs:**
1. One currency per store
2. Changing currency requires careful consideration
3. Payment processor must support selected currency

**Strengths:**
- Simple and straightforward
- No confusion for merchants

**Weaknesses:**
- No multi-currency support
- Limited international flexibility

---

## Best Practices Summary

### 1. **Clear Currency Separation**
- Distinguish between base currency (accounting) and display currency (customer)
- Store prices in base currency to maintain consistency
- Convert for display purposes only

### 2. **Historical Price Preservation**
- Never auto-convert existing prices when base currency changes
- Maintain original values for accounting accuracy
- New entries use new currency

### 3. **Exchange Rate Management**
- Automatic rate updates via reliable API
- Manual override capability for specific markets
- Clear indication of rate freshness

### 4. **Payment Provider Compatibility**
- Filter payment methods by currency support
- Clear warnings when currency changes affect payments
- Support multiple payment processors per currency

### 5. **Customer Experience**
- Persistent currency selection across sessions
- Clear currency indication on all prices
- Localized formatting (symbol position, decimal separator)

---

## Droplinked Recommendations

### Immediate Implementation
1. **Single Base Currency Model** (like Shopify)
   - Clear base currency setting
   - All prices stored in this currency
   - Historical prices preserved

2. **Currency Change Warnings**
   - Alert merchants about payment provider limitations
   - Explain that existing prices won't convert
   - Suggest reviewing product catalog

3. **Payment Method Filtering**
   - Disable payment methods that don't support selected currency
   - Clear messaging about compatibility

### Future Enhancements
1. **Multi-Currency Display** (like BigCommerce)
   - Display prices in customer's local currency
   - Still transact in base currency
   - Customer sees estimated price only

2. **Currency-Specific Pricing**
   - Allow different prices per currency
   - Market-based pricing strategy
   - Override auto-conversion

---

## Feature Conflicts Considerations

### Conflict with Affiliate Network
- **Issue**: Affiliate requires both shops to use same currency
- **Shopify Approach**: No affiliate network feature
- **Recommendation**: Enforce currency compatibility checks

### Conflict with Crypto Payments
- **Issue**: Crypto payments often settle in stablecoins (USDC)
- **Recommendation**: Clear separation between fiat currency and crypto settlement

### Conflict with Printful Integration
- **Issue**: Printful costs always in USD
- **Recommendation**: Real-time conversion for POD cost calculation

---

## Conclusion

**Winner**: Shopify's approach of clear base currency with optional multi-currency display offers the best balance of simplicity and flexibility.

**Droplinked Should**: Adopt Shopify's base currency model with clear warnings about payment provider compatibility and historical price preservation. Consider multi-currency display as Phase 2.
