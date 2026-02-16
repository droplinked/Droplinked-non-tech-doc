# Shop Currency Configuration

### Feature ID:

**[IAA-CUR-007]**

### Title:

**Shop Currency Configuration**

### Category:

**Shop Builder | Settings** | **Actors**: Merchant, Platform | **Channel**: Web Dashboard, Shopfront, Checkout

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
Merchants need the ability to choose a display currency for their shop so that all product listings, cart, checkout, and order confirmations display consistently in their selected currency. This improves customer experience and enables market reach in different regions. The currency choice must be made early and becomes immutable after key business activities to prevent accounting complexity.

**Desired Outcome:**

- Merchant selects currency in shop settings (separate from onboarding)
- 50+ major currencies available for selection
- Currency formatting follows locale standards (e.g., €1.234,56 for EUR)
- When changing currency, system checks payment provider compatibility
- Currency becomes locked (immutable) after first product is created
- Currency becomes locked after affiliate network is activated

---

### 2) **Scope – In / Out**

**In Scope:**

- **Currency Selection**
    - Currency selection in shop settings (NOT during onboarding/registration)
    - 50+ supported currencies (major global currencies)
    - Currency symbol and formatting localization
    - Shopfront price display in shop currency
    - Checkout processing in shop currency
    - Order confirmation in shop currency
- **Payment Provider Compatibility**
    - Payment provider compatibility checking
    - Automatic disabling of incompatible payment methods
    - Clear warnings about payment provider impacts
- **Currency Locking**
    - Currency lock after first product creation
    - Currency lock after affiliate network activation
    - Clear explanation when attempting to change locked currency
    - Soft lock (enforced by business logic, not database constraint)
- **Display & Configuration**
    - Currency displayed prominently in shop settings
    - API responses include currency code for all price data
    - Note: Platform subscription plans are priced in USD regardless of shop currency

**Out of Scope:**

- Multi-currency shops (one shop = one currency)
- Currency conversion for customers (separate feature)
- Historical exchange rate tracking
- Currency hedging or risk management tools
- Cryptocurrency as shop currency (stablecoins only)
- Automatic currency switching based on customer location
- Currency-based pricing rules
- Parallel currency accounting

---

### 3) **Key User Journeys**

---

**Journey 1: Currency Selection in Settings**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Creates Shop | Shop created |
| 2 | Merchant | Navigates to Shop Settings | Settings page loads |
| 3 | Merchant | Selects Currency Tab | Currency options shown |
| 4 | Merchant | Selects Primary Currency (e.g., EUR) | Currency selected |
| 5 | System | Shows Compatible Payment Providers | Provider compatibility list |
| 6 | Merchant | Confirms Selection | Currency saved |
| 7 | System | Currency Locked After First Product Created | Lock applied when applicable |

---

**Journey 2: Payment Provider Compatibility Check**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Attempts to Change Currency to TRY | Currency change initiated |
| 2 | System | Checks Connected Payment Providers | Compatibility check runs |
| 3 | System | Detects PayPal Doesn't Support TRY | Incompatibility found |
| 4 | System | Shows Warning: "PayPal will be disabled" | Warning displayed |
| 5 | Merchant | Can Proceed or Cancel | Options presented |
| 6 | Merchant | If Proceeds | PayPal Automatically Disabled |

---

**Journey 3: Currency Lock Enforcement**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Tries to Change Currency After First Product | Change attempted |
| 2 | System | Checks: Products Exist? Yes | Products detected |
| 3 | System | Shows Message: "Currency locked after first product" | Lock message displayed |
| 4 | System | Suggests: Create new shop for different currency | Alternative suggested |
| 5 | System | Change Option Disabled | Change button disabled |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria |
| --- | --- |
| **Currency Selection** | |
| BAC-1 | Merchant selects currency in shop settings (separate from onboarding) |
| BAC-2 | 50+ major currencies available for selection |
| BAC-3 | Currency formatting follows locale standards (e.g., €1.234,56 for EUR) |
| BAC-4 | All product prices display in shop currency |
| BAC-5 | Cart and checkout display prices in shop currency |
| BAC-6 | Order confirmations show amounts in shop currency |
| **Payment Provider Compatibility** | |
| BAC-7 | When changing currency, system checks payment provider compatibility |
| BAC-8 | Incompatible payment providers are listed with warnings |
| BAC-9 | If merchant proceeds, incompatible providers auto-disable |
| **Currency Locking** | |
| BAC-10 | Currency becomes locked (immutable) after first product is created |
| BAC-11 | Currency becomes locked after affiliate network is activated |
| BAC-12 | Attempting to change locked currency shows clear explanation |
| **Configuration & API** | |
| BAC-13 | Currency displayed prominently in shop settings |
| BAC-14 | API responses include currency code for all price data |
| BAC-15 | Platform subscription billing remains in USD regardless of shop currency |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-16 | Behdad | Converted from PRD-007 to Feature format | Documentation structure |
| 2026-02-01 | Product Management | Initial PRD creation | New feature request |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **Shop Currency** | Primary currency used for all shop transactions and display |
| **Currency Lock** | Business rule preventing currency changes after certain actions |
| **ISO 4217** | International standard for currency codes |
| **Locale Formatting** | Displaying currency according to regional conventions |
| **Soft Lock** | Enforced by business logic rather than database constraint |

---

### B2) Exhaustive Functional Logic

---

### **FL-1: Currency Configuration**

```
SUPPORTED_CURRENCIES = [
    { code: 'USD', name: 'US Dollar', symbol: '$', locale: 'en-US' },
    { code: 'EUR', name: 'Euro', symbol: '€', locale: 'de-DE' },
    { code: 'GBP', name: 'British Pound', symbol: '£', locale: 'en-GB' },
    { code: 'JPY', name: 'Japanese Yen', symbol: '¥', locale: 'ja-JP' },
    { code: 'AUD', name: 'Australian Dollar', symbol: 'A$', locale: 'en-AU' },
    { code: 'CAD', name: 'Canadian Dollar', symbol: 'C$', locale: 'en-CA' },
    { code: 'CHF', name: 'Swiss Franc', symbol: 'CHF', locale: 'de-CH' },
    { code: 'CNY', name: 'Chinese Yuan', symbol: '¥', locale: 'zh-CN' },
    { code: 'INR', name: 'Indian Rupee', symbol: '₹', locale: 'hi-IN' },
    { code: 'BRL', name: 'Brazilian Real', symbol: 'R$', locale: 'pt-BR' },
    { code: 'AED', name: 'UAE Dirham', symbol: 'د.إ', locale: 'ar-AE' },
    // ... 40+ more currencies
]

FUNCTION getCurrencyConfig(currency_code):
    RETURN FIND SUPPORTED_CURRENCIES WHERE code == currency_code

FUNCTION formatPrice(amount, currency_code):
    currency = getCurrencyConfig(currency_code)
    
    formatter = Intl.NumberFormat(currency.locale, {
        style: 'currency',
        currency: currency_code,
        currencyDisplay: 'symbol'
    })
    
    RETURN formatter.format(amount)

FUNCTION validateCurrencyChange(shop_id, new_currency):
    shop = GET shop WHERE id == shop_id
    
    // Check if currency is locked
    IF isCurrencyLocked(shop_id):
        RETURN {
            valid: false,
            reason: 'Currency is locked',
            lock_reason: getLockReason(shop_id)
        }
    
    // Check payment provider compatibility
    connected_providers = GET payment_providers WHERE shop_id == shop_id AND active == true
    incompatible_providers = []
    
    FOR each provider IN connected_providers:
        IF NOT provider.supports_currency(new_currency):
            incompatible_providers.push({
                provider: provider.name,
                current_status: 'active',
                action: 'will be disabled'
            })
    
    RETURN {
        valid: true,
        incompatible_providers: incompatible_providers,
        requires_confirmation: incompatible_providers.length > 0
    }
```

---

### **FL-2: Currency Lock Logic**

```
FUNCTION isCurrencyLocked(shop_id):
    shop = GET shop WHERE id == shop_id
    
    // Check for products
    product_count = COUNT products WHERE shop_id == shop_id
    IF product_count > 0:
        RETURN true
    
    // Check for affiliate activation
    IF shop.affiliate_activated:
        RETURN true
    
    RETURN false

FUNCTION getLockReason(shop_id):
    shop = GET shop WHERE id == shop_id
    
    product_count = COUNT products WHERE shop_id == shop_id
    IF product_count > 0:
        RETURN 'Currency cannot be changed after products are created. You have ' + product_count + ' product(s).'
    
    IF shop.affiliate_activated:
        RETURN 'Currency cannot be changed after Affiliate Network is activated.'
    
    RETURN null

FUNCTION lockCurrency(shop_id, reason):
    shop = GET shop WHERE id == shop_id
    
    // Soft lock - store reason for UI display
    shop.currency_locked = true
    shop.currency_locked_at = NOW
    shop.currency_locked_reason = reason
    
    SAVE shop
    
    // Log for audit
    CREATE AuditLog({
        shop_id: shop_id,
        action: 'currency_locked',
        reason: reason,
        timestamp: NOW
    })

// Automatic lock triggers
ON ProductCreate:
    shop_id = new_product.shop_id
    IF NOT isCurrencyLocked(shop_id):
        lockCurrency(shop_id, 'First product created')

ON AffiliateActivate:
    shop_id = activated_shop.id
    IF NOT isCurrencyLocked(shop_id):
        lockCurrency(shop_id, 'Affiliate network activated')
```

---

### **FL-3: Payment Provider Compatibility**

```
PAYMENT_PROVIDERS = {
    'stripe': {
        supported_currencies: ['USD', 'EUR', 'GBP', 'JPY', 'AUD', 'CAD', ...],
        features: ['cards', 'wallets', 'bank_transfer']
    },
    'paypal': {
        supported_currencies: ['USD', 'EUR', 'GBP', 'AUD', 'CAD', 'JPY', ...],
        features: ['paypal_balance', 'cards', 'bank']
    },
    'adyen': {
        supported_currencies: ['USD', 'EUR', 'GBP', 'JPY', ...],
        features: ['cards', 'wallets', 'local_methods']
    }
    // ... other providers
}

FUNCTION getProviderCompatibility(provider_name, currency_code):
    provider = PAYMENT_PROVIDERS[provider_name]
    
    IF NOT provider:
        RETURN { compatible: false, reason: 'Unknown provider' }
    
    IF currency_code IN provider.supported_currencies:
        RETURN { compatible: true }
    ELSE:
        RETURN { 
            compatible: false, 
            reason: currency_code + ' not supported by ' + provider_name 
        }

FUNCTION processCurrencyChange(shop_id, new_currency, confirm_disable = false):
    validation = validateCurrencyChange(shop_id, new_currency)
    
    IF NOT validation.valid:
        RETURN error(validation.reason)
    
    // Check if confirmation needed
    IF validation.requires_confirmation AND NOT confirm_disable:
        RETURN {
            status: 'confirmation_required',
            message: 'Some payment providers will be disabled',
            affected_providers: validation.incompatible_providers
        }
    
    // Proceed with change
    shop = GET shop WHERE id == shop_id
    old_currency = shop.currency
    shop.currency = new_currency
    shop.currency_changed_at = NOW
    shop.currency_changed_from = old_currency
    
    SAVE shop
    
    // Disable incompatible providers
    IF validation.incompatible_providers.length > 0:
        FOR each provider_info IN validation.incompatible_providers:
            provider = GET payment_provider 
                WHERE shop_id == shop_id 
                AND name == provider_info.provider
            
            provider.active = false
            provider.disabled_reason = 'Incompatible with currency ' + new_currency
            provider.disabled_at = NOW
            
            SAVE provider
    
    // Log change
    CREATE AuditLog({
        shop_id: shop_id,
        action: 'currency_changed',
        old_currency: old_currency,
        new_currency: new_currency,
        disabled_providers: validation.incompatible_providers,
        timestamp: NOW
    })
    
    RETURN success({
        new_currency: new_currency,
        disabled_count: validation.incompatible_providers.length
    })
```

---

### **FL-4: API Integration**

```
// Middleware to inject currency into all API responses
FUNCTION injectCurrencyMiddleware(request, response, next):
    shop = getShopFromRequest(request)
    
    // Add currency to response headers
    response.setHeader('X-Shop-Currency', shop.currency)
    
    // Wrap JSON responses to include currency
    original_json = response.json
    response.json = function(data):
        IF isObject(data) AND NOT data._currency_injected:
            data._currency_injected = true
            data.shop_currency = shop.currency
        
        original_json.call(response, data)
    
    next()

// Format all prices in responses
FUNCTION formatPricesInResponse(data, currency):
    IF isArray(data):
        RETURN MAP data TO formatPricesInResponse(item, currency)
    
    IF isObject(data):
        formatted = {}
        FOR each key, value IN data:
            IF key ENDS_WITH '_price' OR key ENDS_WITH '_amount':
                formatted[key] = {
                    value: value,
                    formatted: formatPrice(value, currency),
                    currency: currency
                }
            ELSE IF isObject(value) OR isArray(value):
                formatted[key] = formatPricesInResponse(value, currency)
            ELSE:
                formatted[key] = value
        
        RETURN formatted
    
    RETURN data
```

---

### B3) Behavioral Flow

```
[Merchant Opens Settings]
    ↓
[Selects Currency Tab]
    ↓
[Checks Lock Status]
    ├─ Locked → [Show Lock Message]
    │             ↓
    │         [Explain Reason]
    │             ↓
    │         [Suggest New Shop]
    │
    └─ Unlocked → [Show Currency Options]
                    ↓
                [Select New Currency]
                    ↓
                [Check Provider Compatibility]
                    ├─ All Compatible → [Apply Change]
                    │
                    └─ Some Incompatible → [Show Warning]
                                            ↓
                                        [List Affected Providers]
                                            ↓
                                        [Request Confirmation]
                                            ↓
                                        [Merchant Confirms]
                                            ↓
                                        [Disable Incompatible]
                                            ↓
                                        [Apply Currency Change]
                                            ↓
                                        [Log Change]
```

---

### B4) State Transitions

| Current State | Event | New State | Side Effects |
| --- | --- | --- | --- |
| Unlocked | Currency selected | Changed | Currency updated, audit logged |
| Unlocked | First product created | Locked | Lock applied, reason logged |
| Unlocked | Affiliate activated | Locked | Lock applied, reason logged |
| Locked | Change attempted | Blocked | Error shown, reason displayed |
| Changed | Provider incompatible | Providers Disabled | Incompatible providers turned off |

---

### B5) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** Invalid currency code | Reject, show supported list |
| **EC-2:** All providers incompatible | Block change, require at least one provider |
| **EC-3:** Change fails mid-process | Rollback, notify merchant |
| **EC-4:** API response too large | Paginate, include currency in each page |
| **EC-5:** Currency not in supported list | Show "Coming Soon" message |
| **EC-6:** Lock reason lost | Default to "Currency locked" message |
| **EC-7:** Concurrent change attempts | Lock row, queue second request |
| **EC-8:** Formatting unsupported locale | Fall back to standard format |

---

### B6) Data Requirements

**Shop Currency Settings:**

| Field | Type | Description |
| --- | --- | --- |
| shop_id | UUID | Reference to shop |
| currency | String | ISO 4217 currency code |
| currency_locked | Boolean | Whether currency can be changed |
| currency_locked_at | DateTime | When locked |
| currency_locked_reason | String | Why it's locked |
| currency_changed_at | DateTime | Last change timestamp |
| currency_changed_from | String | Previous currency |

**Audit Log:**

| Field | Type | Description |
| --- | --- | --- |
| shop_id | UUID | Reference to shop |
| action | String | 'currency_changed', 'currency_locked', etc. |
| details | JSON | Additional context |
| timestamp | DateTime | When action occurred |

---

### B7) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | Select valid currency | Currency saved successfully |
| TC-2 | Create first product | Currency locked automatically |
| TC-3 | Attempt change when locked | Error message shown |
| TC-4 | Change with incompatible provider | Warning displayed |
| TC-5 | Confirm with incompatible provider | Provider disabled, currency changed |
| TC-6 | API price response | Includes currency and formatted price |
| TC-7 | Affiliate activation | Currency locked |
| TC-8 | Format EUR price | Uses € symbol and comma separator |
| TC-9 | Format JPY price | No decimal places |
| TC-10 | View audit log | All changes tracked |

