# PRD-007: Shop Currency Configuration

---

## Feature Specification

- **Feature Title:** Shop Currency Configuration
- **Feature ID:** IAA-CUR-007
- **Category:** Shop Builder | Settings
- **Actors:** Merchant, Platform
- **Channel:** Web Dashboard, Shopfront, Checkout
- **Status:** Defined
- **Owner:** Product Management
- **Linked Ticket:** Ticket 1 - Shop Currency Configuration

---

## Part 1: Human-Readable Spec

### Problem Statement

Merchants need the ability to choose a display currency for their shop so that all product listings, cart, checkout, and order confirmations display consistently in their selected currency. This improves customer experience and enables market reach in different regions. The currency choice must be made early and becomes immutable after key business activities to prevent accounting complexity.

### User Stories

- As a merchant, I want to set my shop's currency in settings so that all my products display in my preferred currency.
- As a merchant, I want the shop currency to be the primary currency for all transactions so that there's no confusion with customers.
- As a merchant, I want to be warned if I change currency and some payment providers don't support it so that I can make an informed decision.
- As a merchant, I want the currency to be locked after I create my first product so that I don't accidentally break my accounting.

### Key User Journeys

**Journey 1: Currency Selection in Settings**
```
Merchant Creates Shop
    ↓
Navigates to Shop Settings
    ↓
Selects Currency Tab
    ↓
Selects Primary Currency (e.g., EUR)
    ↓
System Shows Compatible Payment Providers
    ↓
Confirms Selection
    ↓
Currency Locked After First Product Created
```

**Journey 2: Payment Provider Compatibility Check**
```
Merchant Attempts to Change Currency to TRY
    ↓
System Checks Connected Payment Providers
    ↓
Detects PayPal Doesn't Support TRY
    ↓
Shows Warning: "PayPal will be disabled"
    ↓
Merchant Can Proceed or Cancel
    ↓
If Proceeds → PayPal Automatically Disabled
```

**Journey 3: Currency Lock Enforcement**
```
Merchant Tries to Change Currency After First Product
    ↓
System Checks: Products Exist? Yes
    ↓
Shows Message: "Currency locked after first product"
    ↓
Suggests: Create new shop for different currency
    ↓
Change Option Disabled
```

### Scope

#### ✅ In Scope:
- Currency selection in shop settings (NOT during onboarding/registration)
- 50+ supported currencies (major global currencies)
- Payment provider compatibility checking
- Automatic disabling of incompatible payment methods
- Currency lock after first product creation
- Currency lock after affiliate network activation
- Clear warnings about payment provider impacts
- Shopfront price display in shop currency
- Checkout processing in shop currency
- Order confirmation in shop currency
- Currency symbol and formatting localization
- Note: Platform subscription plans are priced in USD regardless of shop currency

#### ❌ Out of Scope:
- Multi-currency shops (one shop = one currency)
- Currency conversion for customers (separate feature)
- Historical exchange rate tracking
- Currency hedging or risk management tools
- Cryptocurrency as shop currency (stablecoins only)
- Automatic currency switching based on customer location
- Currency-based pricing rules
- Parallel currency accounting

### Acceptance Criteria

- ☑ Merchant selects currency in shop settings (separate from onboarding)
- ☑ 50+ major currencies available for selection
- ☑ Currency formatting follows locale standards (e.g., €1.234,56 for EUR)
- ☑ All product prices display in shop currency
- ☑ Cart and checkout display prices in shop currency
- ☑ Order confirmations show amounts in shop currency
- ☑ When changing currency, system checks payment provider compatibility
- ☑ Incompatible payment providers are listed with warnings
- ☑ If merchant proceeds, incompatible providers auto-disable
- ☑ Currency becomes locked (immutable) after first product is created
- ☑ Currency becomes locked after affiliate network is activated
- ☑ Attempting to change locked currency shows clear explanation
- ☑ Currency displayed prominently in shop settings
- ☑ API responses include currency code for all price data
- ☑ Platform subscription billing remains in USD regardless of shop currency

### Technical Notes

- Store currency in shop settings table
- Use ISO 4217 currency codes (USD, EUR, GBP, etc.)
- Formatting via Intl.NumberFormat with locale
- Currency change validation before commit
- Soft lock (enforced by business logic, not database constraint)
- Payment provider compatibility matrix
- Audit log for currency changes (before lock)

### Dependencies
- Shop creation/onboarding flow
- Payment provider integration system
- Product management system
- Affiliate network system
- Shopfront rendering
- Checkout calculation engine
- Order management system

---
