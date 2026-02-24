# Local Currency Display

### Feature ID:

**[IAA-CUR-005]**

### Title:

**Local Currency Display for Customers**

### Category:

**Shopfront | International Commerce** | **Actors**: Merchant, Customer | **Channel**: Shopfront, Checkout

### Status:

**Defined**

### Owner:

Behdad

---

## PART 1: HUMAN-CENTRIC LAYER

---

### 1) **Summary**

**Problem/Value:**
When merchants operate stores in one currency (e.g., USD), customers from other countries see prices only in that currency, creating friction in the shopping experience. International customers want to see prices in their local currency for better understanding and trust.

**Desired Outcome:**

- Merchant can enable/disable local currency display in shop settings
- When enabled, currency automatically detected from customer IP location
- Prices display in dual format: "Primary: $99.00 USD (≈ 363.66 AED)"
- Exchange rates update automatically every 24 hours
- Works on mobile responsive design

---

### 2) **Scope – In / Out**

**In Scope:**

- **Merchant Settings**
    - Merchant toggle to enable/disable local currency display in settings
    - Auto-configuration based on merchant region
- **Currency Detection**
    - Automatic currency detection via IP geolocation (when enabled)
    - Support for 50+ major currencies
    - Session persistence of customer currency detection
- **Dual Currency Display**
    - Dual currency display: shop base currency + customer local currency
    - Real-time exchange rate conversion display
    - Exchange rate updates (daily auto-refresh)
    - Exchange rate timestamp display ("Rates updated: Today")
    - Approximate indicator (≈) for converted prices
- **Graceful Fallbacks**
    - Graceful fallback if geolocation fails (shows base currency only)

**Out of Scope:**

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

---

### 3) **Key User Journeys**

---

**Journey 1: Merchant Enables Local Currency Display**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Merchant | Opens Shop Settings | Settings page loads |
| 2 | Merchant | Navigates to Regional Settings | Regional settings shown |
| 3 | Merchant | Toggles "Enable Local Currency Display" | Toggle activated |
| 4 | System | Auto-Configures Based on Merchant Region | Default settings applied |
| 5 | System | Sets Primary Display | Shop Base Currency |
| 6 | System | Sets Secondary Display | Customer's Local Currency (based on IP) |
| 7 | Merchant | Saves Settings | Changes reflected on shopfront |

---

**Journey 2: Customer Sees Dual Currency Display**

| Step | Actor | Action | System Response |
| --- | --- | --- | --- |
| 1 | Customer | Visits Shop from UAE (or any location) | System detects IP location |
| 2 | System | System Detects IP Location | Location identified: United Arab Emirates |
| 3 | System | Displays Prices in Dual Format | Primary: $99.00 USD (Shop Currency) / Secondary: ≈ 363.66 AED (Customer Local Currency) |
| 4 | Customer | Can See Both Currencies Simultaneously | Both prices visible on all product pages |
| 5 | Customer | Proceeds to checkout | Checkout clearly states "All prices in [Shop Currency]" |

---

### 4) **Business Acceptance Criteria (BAC)**

| ID | Criteria |
| --- | --- |
| **Merchant Settings** |  |
| BAC-1 | Merchant can enable/disable local currency display in shop settings (default: disabled) |
| BAC-2 | Auto-configuration based on merchant's region/country |
| **Currency Detection & Display** |  |
| BAC-3 | When enabled, currency automatically detected from customer IP location |
| BAC-4 | Prices display in dual format: "$99.00 USD (≈ 363.66 AED)" |
| BAC-5 | Exchange rates update automatically every 24 hours |
| BAC-6 | Support for 50+ major currencies |
| **UX & Transparency** |  |
| BAC-7 | "Approximate" disclaimer shown near converted prices |
| BAC-8 | Exchange rate source and last update shown in footer |
| BAC-9 | Checkout page clearly states "All prices in [Shop Currency]" |
| BAC-10 | Works on mobile responsive design |
| **Fallback** |  |
| BAC-11 | Graceful fallback if geolocation fails (shows base currency only) |
| BAC-12 | Performance impact < 50ms per page load |
| BAC-13 | No manual currency selector dropdown (system handles automatically) |

---

### 📜 Change Log

| Date | Author | Description of Changes | Reason |
| --- | --- | --- | --- |
| 2026-02-16 | Behdad | Converted from PRD-005 to Feature format | Documentation structure |
| 2026-02-01 | Product Management | Initial PRD creation | New feature request |

---

---

## PART 2: AI-CENTRIC LAYER (Functional Logic Depth)

---

### B1) Definitions & Glossary

| Term | Definition |
| --- | --- |
| **Base Currency** | Shop's primary currency for transactions |
| **Local Currency** | Customer's detected currency based on location |
| **IP Geolocation** | Determining physical location from IP address |
| **Exchange Rate** | Conversion rate between two currencies |
| **Dual Display** | Showing both base and local currency prices |

---

### B2) Exhaustive Functional Logic

---

### **FL-1: IP Geolocation Logic**

```
FUNCTION getCustomerLocation(ip_address):
    // Use MaxMind GeoIP2 or similar service
    location_data = geolocation_service.lookup(ip_address)

    IF location_data.success:
        RETURN {
            country_code: location_data.country_code,
            country_name: location_data.country_name,
            region: location_data.region,
            city: location_data.city
        }
    ELSE:
        // Fallback
        RETURN null

FUNCTION getCurrencyByCountry(country_code):
    currency_map = {
        'US': 'USD',
        'GB': 'GBP',
        'EU': 'EUR',
        'AE': 'AED',
        'JP': 'JPY',
        // ... 50+ currencies
    }

    RETURN currency_map[country_code] OR null
```

---

### **FL-2: Exchange Rate Management**

```
FUNCTION fetchExchangeRates():
    // Run daily via cron job
    rates = exchange_rate_api.getLatestRates(
        base_currency: 'USD',
        target_currencies: SUPPORTED_CURRENCIES
    )

    // Store in cache/database
    STORE rates IN cache WITH ttl = 24_hours

    RETURN rates

FUNCTION convertPrice(amount, from_currency, to_currency):
    rates = GET rates FROM cache

    IF NOT rates:
        rates = fetchExchangeRates()

    // Convert to USD first if needed
    IF from_currency != 'USD':
        amount_in_usd = amount / rates[from_currency]
    ELSE:
        amount_in_usd = amount

    // Convert to target currency
    IF to_currency != 'USD':
        converted_amount = amount_in_usd * rates[to_currency]
    ELSE:
        converted_amount = amount_in_usd

    RETURN {
        amount: round(converted_amount, 2),
        rate: rates[to_currency],
        last_updated: rates.timestamp
    }
```

---

### **FL-3: Price Display Logic**

```
ON PageLoad:
    shop = getCurrentShop()
    customer = getCurrentCustomer()

    // Check if feature enabled
    IF NOT shop.settings.enable_local_currency:
        DISPLAY prices in shop.base_currency only
        RETURN

    // Detect customer location
    customer_ip = getClientIP()
    location = getCustomerLocation(customer_ip)

    IF location:
        local_currency = getCurrencyByCountry(location.country_code)

        IF local_currency AND local_currency != shop.base_currency:
            // Store in session
            SET session.local_currency = local_currency

            // Enable dual display
            ENABLE dual_currency_display = true
        ELSE:
            ENABLE dual_currency_display = false
    ELSE:
        // Geolocation failed
        ENABLE dual_currency_display = false
        LOG geolocation_failure

FUNCTION formatDualPrice(base_amount, base_currency, local_amount, local_currency):
    base_formatted = formatCurrency(base_amount, base_currency)
    local_formatted = formatCurrency(local_amount, local_currency)

    RETURN `
        <span class="price-primary">${base_formatted} ${base_currency}</span>
        <span class="price-secondary">(≈ ${local_formatted} ${local_currency})</span>
    `

FUNCTION formatCurrency(amount, currency):
    formatter = Intl.NumberFormat(getLocale(currency), {
        style: 'currency',
        currency: currency,
        minimumFractionDigits: 2
    })

    RETURN formatter.format(amount)
```

---

### **FL-4: Checkout Flow**

```
ON CheckoutPageLoad:
    shop = getCurrentShop()

    // Always show base currency disclaimer
    DISPLAY_MESSAGE: "All prices are in ${shop.base_currency}. Final amount may vary based on your payment provider's exchange rate."

    // Show prices in base currency only
    prices = GET cart_prices IN shop.base_currency

    // If dual display was enabled, show note
    IF session.local_currency:
        local_currency = session.local_currency
        DISPLAY_MESSAGE: "Estimated total in ${local_currency}: ≈ ${converted_amount}"
```

---

### B3) Behavioral Flow

```
[Merchant Enables Feature]
    ↓
[Customer Visits Shop]
    ↓
[Detect IP Location]
    ├─ Success → [Get Local Currency]
    │               ↓
    │           [Check Cache for Rates]
    │               ┼─ Miss → [Fetch from API]
    │               └─ Hit → [Use Cached Rates]
    │               ↓
    │           [Convert Prices]
    │               ↓
    │           [Display Dual Prices]
    │               ↓
    │           [Store in Session]
    │
    └─ Fail → [Display Base Currency Only]
                ↓
            [Log Failure]
```

---

### B4) State Transitions

| Current State | Event | New State | Side Effects |
| --- | --- | --- | --- |
| Disabled | Merchant enables | Enabled | Dual display active |
| Enabled | Merchant disables | Disabled | Base currency only |
| No Detection | Customer visits | Detecting | IP lookup initiated |
| Detecting | Location found | Active | Currency identified |
| Detecting | Location failed | Fallback | Base currency only |
| Active | Rate expired | Refreshing | New rates fetched |

---

### B5) Edge Cases & Error Handling

| Edge Case | System Behavior |
| --- | --- |
| **EC-1:** IP geolocation service down | Use base currency only, retry next request |
| **EC-2:** Country has no mapped currency | Use base currency, log for review |
| **EC-3:** Exchange rate API fails | Use last cached rates, retry in 1 hour |
| **EC-4:** Customer uses VPN | Show currency for VPN location |
| **EC-5:** Same currency detected | Hide secondary display |
| **EC-6:** Rate extremely outdated (>48h) | Show warning, use base currency only |
| **EC-7:** JavaScript disabled | Server-side render base currency only |
| **EC-8:** Mobile network IP | May show carrier country, not user location |

---

### B6) Data Requirements

**Currency Configuration:**

| Field | Type | Description |
| --- | --- | --- |
| shop_id | UUID | Reference to shop |
| enable_local_currency | Boolean | Feature enabled/disabled |
| supported_currencies | Array | List of supported currency codes |
| default_fallback | String | Currency to show on failure |

**Exchange Rate Cache:**

| Field | Type | Description |
| --- | --- | --- |
| base_currency | String | Base currency (usually USD) |
| rates | JSON | {currency: rate} pairs |
| timestamp | DateTime | When rates were fetched |
| expires_at | DateTime | Cache expiry (24h) |

**Session Data:**

| Field | Type | Description |
| --- | --- | --- |
| local_currency | String | Detected customer currency |
| country_code | String | Detected country |
| detection_time | DateTime | When detected |

---

### B7) Test Case Hooks

| Test ID | Scenario | Expected Result |
| --- | --- | --- |
| TC-1 | Merchant enables feature | Toggle activated |
| TC-2 | Customer from UAE visits | AED displayed as secondary |
| TC-3 | Customer from US visits | No secondary (same as base) |
| TC-4 | VPN from different country | Shows VPN location currency |
| TC-5 | Geolocation fails | Base currency only displayed |
| TC-6 | Exchange rate expires | Rates refreshed automatically |
| TC-7 | Checkout page load | Base currency disclaimer shown |
| TC-8 | Mobile view | Dual prices mobile-friendly |
| TC-9 | Rate timestamp | "Updated today" shown |
| TC-10 | Performance check | < 50ms added to page load |