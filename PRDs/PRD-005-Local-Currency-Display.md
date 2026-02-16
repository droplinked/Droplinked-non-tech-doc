# PRD-005: Local Currency Display for Customers

---

## Feature Specification

- **Feature Title:** Local Currency Display for Customers
- **Feature ID:** IAA-CUR-005
- **Category:** Shopfront | International Commerce
- **Actors:** Merchant, Customer
- **Channel:** Shopfront, Checkout
- **Status:** Defined
- **Owner:** Product Management
- **Linked Request:** Request 5 - Display store prices as local currency option for customer

---

## Part 1: Human-Readable Spec

### Problem Statement

When merchants operate stores in one currency (e.g., USD), customers from other countries see prices only in that currency, creating friction in the shopping experience. International customers want to see prices in their local currency for better understanding and trust. Without local currency display, merchants miss opportunities with global customers who may abandon carts due to unfamiliar pricing.

### User Stories

- As a merchant, I want to enable local currency display in my shop settings so that international customers can see prices in their familiar currency.
- As a customer, I want to see prices in my local currency alongside the shop's base currency so that I understand the actual cost.
- As a merchant, I want the system to automatically handle currency display based on customer region so that I don't need to configure individual currencies.

### Key User Journeys

**Journey 1: Merchant Enables Local Currency Display**
```
Merchant Opens Shop Settings
    ↓
Navigates to Regional Settings
    ↓
Toggles "Enable Local Currency Display"
    ↓
System Auto-Configures Based on Merchant Region
    ↓
Primary Display: Shop Base Currency
    ↓
Secondary Display: Customer's Local Currency (based on IP)
    ↓
Save → Changes Reflect on Shopfront
```

**Journey 2: Customer Sees Dual Currency Display**
```
Customer Visits Shop from UAE
    ↓
System Detects IP Location: United Arab Emirates
    ↓
Prices Display in Dual Format:
    ├─ Primary: $99.00 USD (Shop Currency)
    └─ Secondary: ≈ 363.66 AED (Customer Local Currency)
    ↓
Customer Can See Both Currencies Simultaneously
    ↓
Checkout Processes in Shop's Base Currency Only
```

### Scope

#### ✅ In Scope:
- Merchant toggle to enable/disable local currency display in settings
- Automatic currency detection via IP geolocation (when enabled)
- Dual currency display: shop base currency + customer local currency
- Real-time exchange rate conversion display
- Support for 50+ major currencies
- Exchange rate updates (daily auto-refresh)
- Auto-configuration based on merchant region
- Session persistence of customer currency detection
- Exchange rate timestamp display ("Rates updated: Today")
- Approximate indicator (≈) for converted prices

#### ❌ Out of Scope:
- Multi-currency checkout (payment still in shop's base currency)
- Currency conversion at payment level
- Cryptocurrency display options
- Historical exchange rate charts
- Hedging or rate locking for customers
- Wholesale/bulk pricing in different currencies
- Automatic price rounding rules per currency
- Currency-based pricing discrimination
- Tax calculation in local currency
- Shipping cost currency conversion

### Acceptance Criteria

- ☑ Merchant can enable/disable local currency display in shop settings (default: disabled)
- ☑ When enabled, currency automatically detected from customer IP location
- ☑ Prices display in dual format: "$99.00 USD (≈ 363.66 AED)"
- ☑ Exchange rates update automatically every 24 hours
- ☑ Auto-configuration based on merchant's region/country
- ☑ "Approximate" disclaimer shown near converted prices
- ☑ Exchange rate source and last update shown in footer
- ☑ Checkout page clearly states "All prices in [Shop Currency]"
- ☑ Works on mobile responsive design
- ☑ Graceful fallback if geolocation fails (shows base currency only)
- ☑ Performance impact < 50ms per page load
- ☑ No manual currency selector dropdown (system handles automatically)

### Technical Notes

- Use IP geolocation service (MaxMind GeoIP2 or similar)
- Exchange rate API integration (XE, Fixer.io, or Open Exchange Rates)
- Client-side JavaScript for instant conversion display
- Cache exchange rates for 24 hours
- Store customer preference in cookie (30-day expiry)
- Exchange rate precision: 4 decimal places for accuracy
- Format currencies using Intl.NumberFormat API

### Dependencies

- IP geolocation service
- Exchange rate API provider
- Shopfront header component
- Product pricing display system
- Checkout page
- Merchant settings panel
- Session/cookie management

---
